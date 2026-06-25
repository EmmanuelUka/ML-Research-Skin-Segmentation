# ML-Research---Skin-Segmentation

## Documentation for Skin Disease Segmentation on a multi annotated dataset using a DAST Architecture

- The very basis of this project is reliant on the "IMA++: ISIC Archive Multi-Annotator Dermoscopic Skin Lesion Segmentation Dataset" dataset. This dataset dataset contains 17,684 segmentation masks and 14,967 dermoscopic images, where over 2 thousand of the dermoscopic images have about 2-5 segmentation per image, annotates by 16 different annotators. Given the fact that some of these images have two or more annotators per image, it gives is the prime opportunity to make a comparison of difference and similarities. With this we are able to train a model to predict the ground truth of a skin lesion image given the very differences between annotators.

The model uses a Dual Attention Self Attention Transformer which goes through three stages:
1. Channel Attention (CBAM): recalibrates which of the feature channels matter most for a given image
2. Spatial Attention (CBAM):  highlights which spatial regions of the image contain the lesion
3. Transformer Self-Attention: captures global relationships between all spatial regions simultaneously.


Link to raw dataset:
https://zenodo.org/records/14201693
https://api.isic-archive.com/collections/482/

Link to compiled dataset on Kaggle
https://www.kaggle.com/datasets/emmanueluc322/ima-segmentation-images

Link to Kaggle Notebook
https://www.kaggle.com/code/emmanueluc322/skin-disease-segmentation-annotation/notebook