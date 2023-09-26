```
$ pip install -r ./preprocess_dataset/requirements.txt
$ python3 path/to/TextGrids_to_txts.py --data-path path/to/textgrid/directory
$ . path/to/combine_txts.sh --path path/to/txt
$ python3 path/to/txtremoveEMPTY.py --data-path path/to/directory/with/all.txt
$ python3 path/to/train_test_val_split.py --data-path path/to/directory/with/cleaned_all.txt
$ python3 path/to/punc_sym_to_punc.py --data-path path/to/directory/with/train/test/val.txt
$ python3 path/to/create_pkl_dataset_new.py --data-path path/to/directory/with/cleaned_train/test/val.txt


$ cd /new_Multilingual-Sentence-Boundary-detection/
#training
$ python3 ./src/main.py --num-epochs 10 --train-data-path /xlm-roberta-base/ --val-data-path /xlm-roberta-base/ --model-path /punctuator-model/Indonesian_10epoch- --eval-type valid/tst
#testing
$ python3 ./src/main.py --train-data-path /xlm-roberta-base/ --val-data-path /xlm-roberta-base/ --action val --model-path /path/to/model/dir/ --stage model_10epoch-xlm-roberta-base-epoch-1.pth --eval-type valid/tst
```

sample code
```
$ pip install -r ./preprocess_dataset/requirements.txt
$ cd dataset/
$ python3 ../preprocess_dataset/TextGrids_to_txts.py --data-path ./TextGrid
$ . ../preprocess_dataset/combine_txts.sh --path ./txt
$ python3 ../../preprocess_dataset/txtremoveEMPTY.py --data-path ../
$ python3 ../../preprocess_dataset/train_test_val_split.py --data-path ../
$ python3 ../../preprocess_dataset/punc_sym_to_punc.py --data-path ../
$ python3 ../../preprocess_dataset/create_pkl_dataset_new.py --data-path ../

#use GPU
$ cd /new_Multilingual-Sentence-Boundary-detection/
#training
$ python3 ./src/main.py --num-epochs 10 --train-data-path /xlm-roberta-base/ --val-data-path /xlm-roberta-base/ --model-path /punctuator-model/model_10epoch- --eval-type valid/tst
#testing
$ python3 ./src/main.py --train-data-path /xlm-roberta-base/ --val-data-path /xlm-roberta-base/ --action val --model-path /path/to/model/dir/ --stage model_10epoch-xlm-roberta-base-epoch-1.pth --eval-type valid/tst
```

The names of python scripts are unique, can change the relative path to files accordingly.
TextGrids_to_txts.py has been updated in the latest version, so please use the latest scripts.
