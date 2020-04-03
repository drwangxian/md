Introduction
============

This is the accompanying code for our paper titled *Convolutional Acoustic Model for Music Transcription Inspired by the Harmonic Structure of Pitched Music*, which has been submitted to ISMIR 2020 for review. Here we go through the steps to prepare the environment for running this code. The relative paths mentioned hereafter are relative to your Python environment.

Environment Preparation
=======================

-   Install Dependent Libraries

    |:--|:--|
    |tensorflow|1.13.1|
    |librosa|0.6.2|
    |soundfile|0.10.2|
    |mido|1.2.9|
    |magenta|0.4.0|
    |madmon|0.16.1|
    |mir\_eval|0.5|

-   There is a defect with the function *apply\_sustain\_control\_changes* provided by magenta for translating the effect of sustain pedal into an extended note duration. This function is contained in script *sequences\_lib.py*. We have fixed this defect and uploaded the updated script. Please update the corresponding script after installing magenta. The relative path of this script is *lib/python2.7/site-packages/magenta/music/*.

-   Sparse convolution is not a standard operation coming with Tensorflow so we implemented it ourselves. The code for this operation is contained in script *harmonic\_dense.py*. Add this operation to Tensorflow as per the following steps.

    -   Append all the stuff in *harmonic\_dense.py* to script *layers.py* whose relative path is
        *lib/python2.7/site-packages/tensorflow/contrib/layers/python/layers/*.

    -   At the beginning of *layers.py*, there is a list variable named *\_\_all\_\_*. Append ‘*harmonic\_dense*’ to this variable.

    -   There is a script *\_\_init\_\_.py* controlling the visibility of the operations defined in *layers.py*. We need to make the newly added operation visible. The relative path of this script is
        *lib/python2.7/site-packages/tensorflow/contrib/layers/python/*.
        This is done by adding *@@harmonic\_dense* to the long list of similar terms at the beginning of this script.

-   Add the function for note tracking to the *mir\_eval* library. We will use a customized method to extract note onsets and offsets. This method is not available in mir\_eval. This method has been implemented as a function stored in script *onset\_frame\_transcription\_performance\_fn.py*. Please append the content of this script to script *transcription.py* of mir\_eval. The relative path of transcription.py is *lib/python2.7/site-packages/mir\_eval/*.

Download Datasets
=================

-   MAPS
    Download the MAPS dataset as per the instruction given in the paper below.
    V. Emiya, R. Badeau, and B. David, “Multipitch estimation of piano sounds using a new probabilistic spectral smoothness principle,” *IEEE Transactions on Audio, Speech, and Language Processing*, 18(6), pp. 1643–-1654, 2010.
    You may need to unzip the downloaded file. Then create an environment variable named *maps* pointing to the top folder of this dataset. Make sure the following 9 folders are under this folder.
    *ENSTDkCl\_2/MUS*
    *ENSTDkAm\_2/MUS*
    *AkPnBcht\_2/MUS*
    *AkPnBsdf\_2/MUS*
    *AkPnCGdD\_2/MUS*
    *AkPnStgb\_2/MUS*
    *SptkBGAm\_2/MUS*
    *SptkBGCl\_2/MUS*
    *StbgTGd2\_2/MUS*

-   MAESTRO
    Download the MAESTRO dataset from the link given in the paper below.
    C. Hawthorne, A. Stasyuk, A. Roberts, I. Simon, C. A. Huang, S. Dieleman, E. Elsen, J. H. Engel, and D. Eck, “Enabling factorized piano music modeling and generation with the MAESTRO dataset,” in *7th International Conference on Learning Representations*, ICLR 2019, New Orleans, LA, USA, May 6–9, 2019.
    Note that this dataset has multiple versions. The version we used is v1.0.0. You may need to untar the downloaded file. Then create an environment variable named *maestro* pointing to the top folder of this dataset.

Generate Spectrograms
=====================

-   Mel-Spectrograms
    The scripts that generate mel-spectrograms for MAPS and MAESTRO are *gen\_mel\_for\_maps.py* and *gen\_mel\_for\_maestro.py*, respectively. Before running them, please create two environment variables named *maps\_mel* and *maestro\_mel* pointing, respectively, to the folders where the spectrograms for maps and those for maestro will be stored. These folders will be created automatically if they do not exist beforehand.

-   VQT Spectrograms
    We implemented functions for computing VQT in Matlab and will call them from Python to generate VQT spectrograms. Please follow the steps below to generate VQT spectrograms.

    1.  Install Matlab.

    2.  Follow the instruction at [this link](https://au.mathworks.com/help/matlab/matlab-engine-for-python.html?s_tid=CRUX_lftnav) to install necessary libraries so that we can call Matlab functions from Python.

    3.  The Matlab code for VQT is put in folder *matlab\_code*. Please add this folder to Matlab’s search path.

    4.  The Python scripts that generate VQT spectrograms for MAPS and MAESTRO are *gen\_vqt\_for\_maps.py* and *gen\_vqt\_for\_maestro.py*, respectively. Before running them, please create two environment variables named *maps\_vqt* and *maestro\_vqt* pointing to the folders where the spectrograms will be saved.

Run Experiments
===============

-   Experiments for the Comparison of Acoustic Models
    Run script *comp\_acoustic\_models.py* to get the frame-level performance of an acoustic model. Before doing this, you need to configure some parameters, such as the acoustic model, the dataset for training, and the prefetch ration for the training split. Please refer to the script for details. You can view the results with tensorboard.

-   Experiments for the Performance of Our AMT System
    The are three scripts under folder *note\_tracking*, namely, *frame.py*, *onset.py* and *note.py*, which implements, respectively, the frame-wise pitch detector, the onset detector, and the extraction of note contours. You should run the first two scripts first to get the checkpoints for the two detectors so as to run the third script. Before running the scripts, you need to define some parameters, such as the dataset for training, and the prefetch ratio for the training split. Please refer to the scripts for details. You can view the results with tensorboard.


