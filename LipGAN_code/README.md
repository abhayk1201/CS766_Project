LipGAN Source Code
===================

[[Paper]](https://dl.acm.org/doi/10.1145/3343031.3351066)

We illustrate the training and inference pipeline for the LRS2 dataset.

###### LRS2 dataset folder structure
```
data_root (mvlrs_v1)
├── main, pretrain (we use only main folder in this work)
|	├── list of folders
|	│   ├── five-digit numbered video IDs ending with (.mp4)
```

### Preprocess the dataset
We need to do two things: (i) Save the MFCC features from the audio and (ii) extract and save the facial crops of each frame in the video. 
We use Python [Librosa](https://librosa.github.io/librosa/) library to save melspectrogram features and perform face detection using dlib.  
We preprocess the video files by detecting faces using a face detector from dlib. 
CNN Face detection using dlib: [Link](http://dlib.net/files/mmod_human_face_detector.dat.bz2) 

```bash
# Please copy the appropriate LRS2 split's filelist.txt to the filelists/ folder.
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
|	│   |	 ├── mels.npz, audio.wav (melspectrogram and raw audio of the whole video)
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

#### Inference: Generating lip-sync from a single face image
Given an audio, LipGAN generates a correct mouth shape (viseme) at each time-step and overlays it on the input image. The sequence of generated mouth shapes yields a talking face video.
```bash
python batch_inference.py --checkpoint_path <saved_checkpoint> --model residual --face <random_input_face> --audio <guiding_audio_wav_file> --results_dir <folder_to_save_generated_video>

#### More options
python batch_inference.py --help
```

#### Citation
Our work is derivered from this original work. Please cite the following original paper:

```
@inproceedings{KR:2019:TAF:3343031.3351066,
  author = {KR P, Mukhopadhyay R, Philip J, Jha A, Namboodiri V, Jawahar CV.},
  title = {Towards Automatic Face-to-Face Translation},
  booktitle = {Proceedings of the 27th ACM International Conference on Multimedia (MM '19)}, 
  url = {http://doi.acm.org/10.1145/3343031.3351066},
  }
```




