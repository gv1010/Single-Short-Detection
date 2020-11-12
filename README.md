** SINGLE SHOT DETECTION

** Source code to the problem is in google colab.(link shared in the mail)

** Code can be executed following the instructions in the Notebook

**Data Preperation:

	*Worked on the data downloaded from the link provided(not the github). Used the annotations from the github repo which is combined annotations for test and train data.
	
	*The annotations provided were not adhering to the SSD annotations input in tensorflow

	* Annotations from github were:
		eg. (image name, number of products, x (left top), y(left top), width(bounding box), height(bounding box), class)
		
	* The above format is converted to:
		eg. (image name, width, height, class, xmin, ymin, xmax, ymax)
		
		*xmin, ymin are same as x, y from github annotations.
		
		*xmax is calculated using
		
		#xmax = xmin + 0.95*(widht of bounding box)
		
	*Have taken xmin+width of boouding box, as the boxes are adjacent to each other, there was a slight overlap in the bouding box region. To avoid that i took 0.95 time the width.
		
		#ymax = ymin + height of bounding box
		
	**Note:
		width -- full image width(not bounding box)
		height -- full image height(not bounding box)


	**Number of Classes:
		*In the given data we have 11 products of similar shape and sizes.Our task is to predict whether there is a product or not in an image.

		*Since, we want to count the number of items, all we need to do is to classify the bounding box as empty or not. If it's not empty we would count it. So, I have put all 11 products under one class and 'empty space', as another  class(by default).
		
		
** Training:

	** SSD Inception V2 with 1 class randomly sampled trained data(23k steps)
		*Train Data : 100 Randomly Sampled trained data
		*Data_Augmentation : random_horizontal_flip, ssd_random_crop
		*Anchor box on feature cell : 1,  with aspect ratio 2:1.
		*Feature maps : 6
		
	**Results:
		
		Average Precision(AP) @IoU=0.50:0.95 | area=   all | maxDets=100 ] = 0.655
		Average Precision(AP) @IoU=0.50      | area=   all | maxDets=100 ] = 0.987
		Average Precision(AP) @IoU=0.75      | area=   all | maxDets=100 ] = 0.823
		Average Precision(AP) @IoU=0.50:0.95 | area= small | maxDets=100 ] = -1.00
		Average Precision(AP) @IoU=0.50:0.95 | area=medium | maxDets=100 ] = -1.00
		Average Precision(AP) @IoU=0.50:0.95 | area= large | maxDets=100 ] = 0.655
		Average Recall   (AR) @IoU=0.50:0.95 | area=   all | maxDets=  1 ] = 0.020
		Average Recall   (AR) @IoU=0.50:0.95 | area=   all | maxDets= 10 ] = 0.199
		Average Recall   (AR) @IoU=0.50:0.95 | area=   all | maxDets=100 ] = 0.727
		Average Recall   (AR) @IoU=0.50:0.95 | area= small | maxDets=100 ] = -1.00
		Average Recall   (AR) @IoU=0.50:0.95 | area=medium | maxDets=100 ] = -1.00
		Average Recall   (AR) @IoU=0.50:0.95 | area= large | maxDets=100 ] = 0.727
		
	** train data sampled executing RandomSampleOf100Images.py 
		*please check the code to input source and destination locations
		
	*Apart from this model, I trained on the whole data with 11 calsses on mobileNet with one extra data augmentions, with single class on mobileNetV2 and inceptionV2 with full data and two extra data augmentions, though they were taking time, but was giving a decent mAPs, ARs which could have been improved with further training.

	
**Inference:(this process can be run direclty executing the cells in the colab notebook)
	*We save the trained model in frozen inference graph.For every image run on the inference graph will generate output having detection_scores, detection_boxes, and detection_class.
	
	*To count the number of products in the image we set the Score threshold to 0.5. Any score in the detection_scores for each image will be counted as product present if its value is greater than 0.5 .
	
	*The output json file is generated in the asked format.
		
			
		
			
				
