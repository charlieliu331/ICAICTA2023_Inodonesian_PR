# Punctuation_Restoration


## Source attribution: ##

BERT punctuator Framework code borrowed from [here](https://github.com/attilanagy234/neural-punctuator/)  

Modifications have been made to the notebooks, the data preprocessing, and the model architecture.  

Code modified based on [here](https://github.com/AetherPrior/Multilingual-Sentence-Boundary-detection/tree/main) by Abhinav Rao.

Model available at [https://drive.google.com/file/d/1bC-a0BrbVR3wsh2JZqu9KiMBf7nOK3T-/view?usp=sharing](https://drive.google.com/file/d/1D1T7MrYw3-juo1F8xHoEh-gvYTUaHUZw/view?usp=share_link)
#### Pre-built:  


The dataset folder structure is as follows:  

```
dataset/
|
|-dual/
|   |
|   |-xlm-roberta-base/
|            |-- train_data.pkl
|            |-- valid_data.pkl
|            |-- test_data.pkl
|-en
| |-...
|
|-zh
```
Use the `--data-path` argument to set the data directory:  
```
  --data-path DATA_PATH
                        path to dataset directory
```

## Installation and Usage: ##
### Virtual environment:  
It is **HIGHLY RECOMMENDED** to use a virtualenv or a conda virtual environment for this project:    
1. Setup is fairly easy, open up a terminal and do:  
  - `python -m venv /path/to/root/dire/.../.../venv_name`     or - `conda create --name myenv`
2. Then everytime you want to run the program, just do  
	- `source ./venv/bin/activate` or - `conda activate myenv`

### Preprocess data
Original dataset consists of .TextGrid files. Need to convert and combine them into one .txt file.  
Install the required libraries, convert the files, and then combine the txt files with combine_txts.sh shell script.  
Next, we need to remove --EMPTY-- utterances. 
Last divide the data in to 80:10:10 with balanced distributions for train:valid:test sets.  
Change path_to_files accordingly.  
punc_sym_to_punc.py removes the stamp of each utterance and convert \<c/> and \<s/> to \, and \. Run if needed.
Make sure the texts are in order first.  

First download the dataset from google drive: 
after downloading, move the folder under ./sgh_scripts_datasets/ directory

```
$ pip install -r ./preprocess/preprocess_dataset/requirements.txt
$ cd path/to/dataset_directory
$ python3 TextGrids_to_txts.py
$ . ./preprocess/preprocess_dataset/combine_txts.sh --path path/to/sgh_txt
$ python3 ./preprocess/preprocess_dataset/txtremoveEMPTY.py --data-path path/to/directory/with/all.txt
$ python3 ./train_test_val_split.py --data-path path/to/directory/with/cleaned_all.txt
$ python3 ./punc_sym_to_punc.py --data-path path/to/directory/with/sgh_train/test/val.txt
$ python3 ./create_pkl_dataset_new.py --data-path path/to/directory/with/cleaned_sgh_train/test/val.txt
```
#### Sample code
```
$ pip install -r ./preprocess_TextGrid_data/requirements.txt
$ cd sgh_dataset/
$ python3 ../preprocess_TextGrid_data/TextGrids_to_txts.py --data-path ./TextGrid
$ . ../preprocess_TextGrid_data/combine_txts.sh --path ./txt
$ python3 ../../preprocess_TextGrid_data/txtremoveEMPTY.py --data-path ../
$ python3 ../../preprocess_TextGrid_data/train_val_split.py --data-path ../
$ python3 ../../preprocess_TextGrid_data/punc_sym_to_punc.py --data-path ../
$ python3 ../../preprocess_TextGrid_data/create_pkl_dataset_new.py --data-path ../
```

### Train and Test
Run the `main.py` file:

```  
usage: main.py [-h] [--save-model] [--break-train-loop] [--stage STAGE]
               [--model-path MODEL_PATH] [--data-path DATA_PATH]
               [--num-epochs NUM_EPOCHS]
               [--log-level {INFO,DEBUG,WARNING,ERROR}]
               [--save-n-steps SAVE_N_STEPS] [--force-save]

arguments for the model

optional arguments:
  -h, --help            show this help message and exit
  --save-model          save model
  --break-train-loop    prevent training for debugging purposes
  --stage STAGE         load model from checkpoint stage
  --model-path MODEL_PATH
                        path to model directory
  --train-data-path DATA_PATH
                        path to dataset directory containing train_data.pkl
  --val-data-path DATA_PATH
                        path to dataset directory containing valid_data.pkl
  --eval-type EVAL_TYPE
  			valid/tst validation dataset or test dataset for evaluation
  --num-epochs NUM_EPOCHS
                        no. of epochs to run the model
  --log-level {INFO,DEBUG,WARNING,ERROR}
                        Logging info to be displayed
  --save-n-steps SAVE_N_STEPS
                        Save after n steps, default=1 epoch
  --force-save          Force save, overriding all settings
  --config CONFIG	Path to config directory
  --action ACTION 	train/val training or testing
			
```
#### Sample code
```
#use GPU
$ cd /new_Multilingual-Sentence-Boundary-detection/
#training
$ python3 ./src/main.py --num-epochs 10 --train-data-path /Indonesian/xlm-roberta-base/ --val-data-path Indonesian/xlm-roberta-base/ --model-path /punctuator-model/model_10epoch- --eval-type valid/tst
#testing
$ python3 ./src/main.py --train-data-path /Indonesian/xlm-roberta-base/ --val-data-path /Indonesian/xlm-roberta-base/ --action val --model-path /path/to/model/dir/ --stage model_10epoch-xlm-roberta-base-epoch-1.pth --eval-type valid/tst
```
