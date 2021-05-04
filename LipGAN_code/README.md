LipGAN Source Code
===================




[[Paper]](https://dl.acm.org/doi/10.1145/3343031.3351066)

Training LipGAN
-------
We illustrate the training pipeline using the LRS2 dataset. Adapting for other datasets would involve small modifications to the code. 
### Preprocess the dataset
We need to do two things: (i) Save the MFCC features from the audio and (ii) extract and save the facial crops of each frame in the video. 

##### LRS2 dataset folder structure
```
data_root (mvlrs_v1)
├── main, pretrain (we use only main folder in this work)
|	├── list of folders
|	│   ├── five-digit numbered video IDs ending with (.mp4)
```
##### Saving the MFCC features
We use MATLAB to save the MFCC files for all the videos present in the dataset. Refer to the [fully_pythonic branch](https://github.com/Rudrabha/LipGAN/tree/fully_pythonic) if you do not want to use MATLAB.  

```bash
# Please copy the appropriate LRS2 train split's filelist.txt to the filelists/ folder. The example below is shown for LRS2.
cd matlab
matlab -nodesktop
>> preprocess_mat('../filelists/train.txt', 'mvlrs_v1/main/') # replace with appropriate file paths for other datasets.
>> exit
cd ..
```

##### Saving the Face Crops of all Video Frames
We preprocess the video files by detecting faces using a face detector from dlib. 
```bash
# Please copy the appropriate LRS2 split's filelist.txt to the filelists/ folder. Example below is shown for LRS2. 
python preprocess.py --split [train|pretrain|val] --videos_data_root mvlrs_v1/ --final_data_root <folder_to_store_preprocessed_files>

### More options while preprocessing (like number of workers, image size etc.)
python preprocess.py --help
```
###### Final preprocessed folder structure
```
data_root (mvlrs_v1)
├── main, pretrain (we use only main folder in this work)
|	├── list of folders
|	│   ├── folders with five-digit video IDs 
|	│   |	 ├── 0.jpg, 1.jpg .... (extracted face crops of each frame)
|	│   |	 ├── 0.npz, 1.npz .... (mfcc features corresponding to each frame)
```

#### Train the generator only
As training LipGAN is computationally intensive, you can just train the generator alone for quick, decent results.  
```bash
python train_unet.py --data_root <path_to_preprocessed_dataset>

### Extensive set of training options available. Please run and refer to:
python train_unet.py --help
```
#### Train LipGAN
```bash
python train.py --data_root <path_to_preprocessed_dataset>

### Extensive set of training options available. Please run and refer to:
python train.py --help
```

License and Citation
----------
The software is licensed under the MIT License. Please cite the following paper if you have use this code:

```
@inproceedings{KR:2019:TAF:3343031.3351066,
  author = {K R, Prajwal and Mukhopadhyay, Rudrabha and Philip, Jerin and Jha, Abhishek and Namboodiri, Vinay and Jawahar, C V},
  title = {Towards Automatic Face-to-Face Translation},
  booktitle = {Proceedings of the 27th ACM International Conference on Multimedia}, 
  series = {MM '19}, 
  year = {2019},
  isbn = {978-1-4503-6889-6},
  location = {Nice, France},
   = {1428--1436},
  numpages = {9},
  url = {http://doi.acm.org/10.1145/3343031.3351066},
  doi = {10.1145/3343031.3351066},
  acmid = {3351066},
  publisher = {ACM},
  address = {New York, NY, USA},
  keywords = {cross-language talking face generation, lip synthesis, neural machine translation, speech to speech translation, translation systems, voice transfer},
}
```




