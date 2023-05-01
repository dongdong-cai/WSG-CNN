# WSG-CNN



# The code will be open source soon！！！





## 1、Updates

- [2023-04-20] Added datasets METR-LA, PeMS-Bay

- [2023-04-15] WSG-CNN v1.0 is released



## 2、Features

- Supports 6 mainstream traffic volume prediction datasets, including PeMS03、PeMS04、PeMS07、PeMS08、METR-LA、PeMS-Bay
- Provide test mode, draw prediction curve, save training process



## 3、Used Datasets

We conducted model training and testing on six mainstream traffic datasets, including PeMS03、PeMS04、PeMS07、PeMS08、METR-LA、PeMS-Bay.

| Datasets | Variants | Timesteps | Granularity |      Time Range       | Task Type  |
| :------: | :------: | :-------: | :---------: | :-------------------: | :--------: |
|  PeMS03  |   358    |   26208   |    5min     | 09/01/2018-11/30/2018 | Multi-step |
|  PeMS04  |   307    |   16992   |    5min     | 01/01/2018-02/28/2018 | Multi-step |
|  PeMS07  |   883    |   28224   |    5min     | 05/01/2017-08/31/2017 | Multi-step |
|  PeMS08  |   170    |   17856   |    5min     | 07/01/2016-08/31/2016 | Multi-step |
| METR-LA  |   207    |   34272   |    5min     | 03/01/2012-06/27/2012 | Multi-step |
| PeMS-Bay |   325    |   52116   |    5min     | 01/01/2017-06/30/2017 | Multi-step |



## 4、model training

### 4.1  Requirements

Install the required package first:

```sh
cd WSG-CNN
conda create -n WSG_CNN python=3.7
conda activate WSG_CNN
pip install -r requirements.txt
```



### 4.2  Dataset preparation

To prepare all dataset at one time, you can just run:

```sh
sudo chmod 777 download_datasets.sh
sh download_datasets.sh
```

The data directory structure is shown as follows. 

```
└── PEMS
    ├── METR-LA.csv
    ├── PeMS03.csv
    ├── PeMS04.csv
    ├── PeMS07.csv
    ├── PeMS08.csv
    └── PeMS-Bay.csv
```



### 4.3  Run training code

Explanation and default values of some command line parameters

| Parameter Name |                      Description                       |         Default          |
| :------------: | :----------------------------------------------------: | :----------------------: |
|   output_dir   |                  Dataset output path                   |     data/PEMS/PeMS08     |
|  dataset_path  |                Downloaded dataset path                 | datasets/PEMS/PeMS08.csv |
|  wavelet_name  |              The wavelet family's choice               |           dmey           |
| wavelet_level  |              wavelet decomposition times               |            4             |
|  seq_length_x  |              Input time series dimension               |            12            |
|  seq_length_y  |              output time series dimension              |            12            |
|   yaml_path    | Path to the yaml configuration file for model training |    config/PeMS08.yaml    |



**PeMS03**

```sh
# Data set preprocessing only needs to be performed once
python wavelet_preprocessing.py --output_dir data/PEMS/PeMS03 --dataset_path datasets/PEMS/PeMS03.csv --wavelet_name dmey --wavelet_level 4 --seq_length_x 12 --seq_length_y 12
# traning for PeMS03
python train.py --yaml_path config/PeMS03.yaml
```

**PeMS04**

```sh
# Data set preprocessing only needs to be performed once
python wavelet_preprocessing.py --output_dir data/PEMS/PeMS04 --dataset_path datasets/PEMS/PeMS04.csv --wavelet_name dmey --wavelet_level 4 --seq_length_x 12 --seq_length_y 12
# traning for PeMS04
python train.py --yaml_path config/PeMS04.yaml
```

**PeMS07**

```sh
# Data set preprocessing only needs to be performed once
python wavelet_preprocessing.py --output_dir data/PEMS/PeMS07 --dataset_path datasets/PEMS/PeMS07.csv --wavelet_name dmey --wavelet_level 4 --seq_length_x 12 --seq_length_y 12
# traning for PeMS07
python train.py --yaml_path config/PeMS07.yaml
```

**PeMS08**

```sh
# Data set preprocessing only needs to be performed once
python wavelet_preprocessing.py --output_dir data/PEMS/PeMS08 --dataset_path datasets/PEMS/PeMS08.csv --wavelet_name dmey --wavelet_level 4 --seq_length_x 12 --seq_length_y 12
# traning for PeMS08
python train.py --yaml_path config/PeMS08.yaml
```

**METR-LA**

```sh
# Data set preprocessing only needs to be performed once
python wavelet_preprocessing.py --output_dir data/PEMS/METR-LA --dataset_path datasets/PEMS/METR-LA.csv --wavelet_name dmey --wavelet_level 4 --seq_length_x 12 --seq_length_y 12
# traning for METR-LA
python train.py --yaml_path config/METR_LA.yaml
```

**PeMS-Bay**

```sh
# Data set preprocessing only needs to be performed once
python wavelet_preprocessing.py --output_dir data/PEMS/PeMS-Bay --dataset_path datasets/PEMS/PeMS-Bay.csv --wavelet_name dmey --wavelet_level 4 --seq_length_x 12 --seq_length_y 12
# traning for PeMS-Bay
python train.py --yaml_path config/PeMS_Bay.yaml
```



## 5、Other tools

In addition to model training, we also provide model testing, model prediction curve drawing program

```sh
# model test
python test.py
# model prediction curve drawing
python plot.py
```

![Prediction Curves on Dataset PeMS08](https://github.com/dongdong-cai/resources/blob/main/imgs/PEMS08.png)



## 6、train your own dataset

First make sure that your dataset is a csv file in the following format

|       time       | node 1 | ...  | node N |
| :--------------: | :----: | :--: | :----: |
| 2016/7/1 0:00:00 |   48   | ...  |   64   |
|       ...        |  ...   | ...  |  ...   |

Then run the data preprocessing program, here we recommend using the default wavelet base and wavelet decomposition times

```sh
python wavelet_preprocessing.py --output_dir output_dataset_path --dataset_path path_to_own_dataset/mydata.csv  --wavelet_name dmey --wavelet_level 4 --seq_length_x 12 --seq_length_y 12
```

Follow the yaml file in config to write a yaml configuration file suitable for your own data set，then

```sh
python train.py --yaml_path config/my_dataset.yaml
```




