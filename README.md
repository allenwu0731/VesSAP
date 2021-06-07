## README 

For the Nature Methods submission "[Automated analysis of whole brain vasculature using machine learning](https://www.biorxiv.org/content/10.1101/613257v1)"

Authors:  Johannes C. Paetzold and Giles Tetteh

This code can be executed in a public code ocean capsule at https://codeocean.com/capsule/0511563/tree/v1.

## Table of contents

* [General info](#general-info)
* [Segmenting data](#test)
* [Training a model](#train)
* [Feature extraction](#feats)
* [Dependencies](#depend)

## General info

For each of the described tasks, segmentation, training and feature extraction we have  set up a working example (demo) in the code ocean compute capsule. 

This repository includes four main blocks:

1. A utility library for the Deep Learning part of the work in KERAS including 
	
	a. The complete deep learning library including all networks and utilities
	
	b. Detailed descriptions how to setup this framework (Readme.md).	
	
2.  A data folder containing:
	
	a. A part of the [synthetic dataset](https://github.com/giesekow/deepvesselnet/wiki/Datasets) used to pretrain our network	

	b. An exemplary testing/training dataset and corresponding ground truth annotations

	c. The whole microscopy data can be found here (http://discotechnologies.org/). 

3. A model folder containing:
	
	a. Models trained on sythetic data for 1 and 2 channel network input
	
	b. The fully refined 2 input channel network
	
4. MATLAB scripts for statistical evaluation of features


## Segmenting data

The prediction on our models generates a binary segmentation (threshold=0.5) and a probabilistic prediction.

Here we provide one models (other models too heavy to be added to the repository about 2GB):

* Real model from the Vessap paper for two input channels using the fcn network ('fcn_model.dat').

The following arguments can be passed through the terminal:
```
positional arguments:
  filenames             input filename(s) should follow the sequence for
                        multiple channels

optional arguments:
  -h, --help            show this help message and exit
  --maskFilename MASKFN
                        a mask file to be applied to the predictions
  --output OUTPUT       output folder for storing predictions (default:
                        current working directory)
  --o_probs SUFFIX_PROBS
                        filename suffix for renaming probability output files
                        (default: _probs)
  --o_bins SUFFIX_BINS  filename suffix for renaming binary output files
                        (default: _bins)
  --t THRESHOLD         threshold for converting probabilities to binary
                        (default: 0.5)
  --f FORMAT            NIFTI file format for saving outputs (default:
                        .nii.gz)
  --model MODEL         a saved model file (default: model.dat)
  --preprocess PREPROCESS
                        Whether to apply preprocessing or not (default: 0 =>
                        False)
  --bs BATCH_SIZE       Batch size to apply during prediction (default: 1)
  --cs CUBE_SIZE        Size of cube to be applied during prediction (default:
                        64)
  --hist-cutoff HIST_CUTOFF
                        Cutoff to use when applying histogram cutoff (default:
                        0.99)
 ```

## Training a model

Training recquires a labeled dataset, in this git we provide an exemplary dataset with labels. You can either refine a model, for example our 'synth_model' or retrain a model from a random initialization. Please then specify your training set and your labels and pass them as arguments. Further arguments can be passed through the terminal:
```
  -h, --help            show this help message and exit
  --inputFns INPUTFNS   a text file containing a list of names/path of input
                        data for the traning (one example per line) (default:
                        inputs.txt)
  --labelFns LABELFNS   a text file containing a list of names/path of data
                        label for the traning (one example per line) (default:
                        labels.txt)
  --maskFns MASKFNS     a text file containing a list of names/path of mask
                        data for the traning (one example per line)
  --preprocess          Whether to apply preprocessing or not (default: False)
  --hist-cutoff HIST_CUTOFF
                        Cutoff to use when applying histogram cutoff (default:
                        0.99)
  --initModel MODEL     a path to a model which should be used as a base for
                        the training (default: None)
  -- modelType MODELTYPE
			the model type to train (FCN=0, VNET=1, UNET=2)
                        (default: 0)
  --use_crosshair       Whether to use crosshair filters or not (dfault: False)
  --n_in N_IN           number of input channels (default: 1)
  --n_out N_OUT         number of prediction classes (default: 2)
  --batch-size BATCH_SIZE
                        batch size for training (default: None)
  --cs CUBE_SIZE        Size of cube to be used during training (default: 64)
  --epochs EPOCHS       number of training epochs (default: 1)
  --save-after SAVE_AFTER
                        number of training epochs after which the model should
                        be saved (default: 1)
  --modelFn MODELFN     filename for saving trained models. Note .dat will be
                        appended autmatically (default: model)
  --modelFolder MODEL_FOLDER
                        folder where models will be saved (default: current
                        working directory)
  --lr LEARNING_RATE    learning rate (default: 0.01)
  --decay DECAY         learning rate decay per epoch (default: 0.99)
  --weighted-cost       Whether to use weighted cost or not (default: False)
```

## Feature extraction

The feature extraction extracts the skeleton length, number of bifurcation points, maximum radius and average radius.

#### Use your own data

To extract features from your own images, please have segmented data in an itk-comaptible format first. If you do not have a binary segmentation please run the the [Segmenting data](#test) routine on your images first. The following arguments can be passed through the terminal:
```
positional arguments:
  filenames             input filename(s) should follow the sequence for
                        multiple channels

optional arguments:
  -h, --help            show this help message and exit
  --output OUTPUT       output folder for storing predictions (default:
                        current working directory)
  --no-c                Do not save centerline extraction
  --o_cens SUFFIX_CENS  filename suffix for renaming CENTERLINE output files
                        (default: _cens)
  --no-b                Do not save bifurcation detection
  --o_bifs SUFFIX_BIFS  filename suffix for renaming BIFURCATION output files
                        (default: _bifs)
  --no-r                Do not save radius estimates
  --o_rads SUFFIX_RADS  filename suffix for renaming RADIUS output files
                        (default: _rads)
  --f FORMAT            NIFTI file format for saving outputs (default:
                        .nii.gz)
```
#### Regional Features

To extract features in your own images for a particular region of interest, for example regions from the Allen brain atlas, you can upload images of those regions, segment them using the [Segmenting data](#test) routine and then extract the featues using the feature extraction routine. If you have a dataset which is registered to the Allen brain atlas, the utility scripts in the matlab folder allow the calculation of whole brain statistics. 



## Dependencies

* numpy 
* scipy 
* skimage
* Keras
* Tensorflow
* SimpleITK
* matplotlib
