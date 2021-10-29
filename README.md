# ATP Scoreboard detection + OCR + player tracking
##### _“To iterate is human, to recurse divine.”_

![alt text](https://i.imgur.com/e2rRGVH.gif "mini")
#   


## Problem set:
For a given input image, the idea is to detect the bounding box coordinates of tennis match scoreboard and perform OCR on the scores and identify the serving player marked.
For this I used: 
- ✨MaskRCNN✨
- ✨Keras_OCR✨

## Transfer learning:

- Convert the given json format to COCO format.
- Used imagenet pretrained weights for transfer learning
- Trained on random 300 frames extracted from the video file.

## Training the model:
Explore data: 
![alt text](https://i.imgur.com/BqHNv2D.png "explore")

## Inference from the trained model:
Detection results:
![alt text](https://i.imgur.com/r8Cv56U.png "results")

## Extracted ROI:
![alt text](https://i.imgur.com/Jf3FAmc.png "results")

## Custom train OCR:
As we need to detect the serving_player too, I am using this little trick.
To identify the serving_player we need to detect the marker near the player name.
![alt text](https://i.imgur.com/NjLm9EB.png "ocr_train")

>My trick is I am treating the serving_player marker as a character for OCR.
>i.e the above contains ```(> Anderson 4 7 4 40)```

## Custom OCR results:
![alt text](https://i.imgur.com/MM0t3Ob.png "results")
> Serving player is identified by the presence of ```>``` in the prediction result.

## Note: Annotation data error
- There is an error in the annotation data of the serving player for the frame group starting from : ```16879```

## Player name correction incase of wrong OCR prediction:
- As we have know the names of the players in advance, I created a small dictionary for refernce which contains the player names. (for our case there are 87 unique names in our list.)
- We compare the predicted name with our dictionary and if it doesn't match we update the predicted result with a closet match using difflib.
- ```corrected_name=difflib.get_close_matches((predicted), words,n=1)[0]```



## Corrected names based on our dictionary index reference example:
![alt text](https://i.imgur.com/mL0zY7i.png "corrected names")


## Player tracking:
- Input:
![alt text](https://i.imgur.com/P1Cpzwu.gif "org_2_pt")

- Result:
![alt text](https://i.imgur.com/xJcplK3.gif "pt")



# 
#
## References
- https://github.com/matterport/Mask_RCNN/
- https://github.com/faustomorales/keras-ocr
- https://keras-ocr.readthedocs.io/en/latest/examples/fine_tuning_recognizer.html
