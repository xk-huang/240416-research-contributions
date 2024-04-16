# DATA

## BTCV

The instructions of downloading BTCV https://github.com/Project-MONAI/research-contributions/tree/main/UNETR/BTCV#dataset


BTCV https://www.synapse.org/#!Synapse:syn3193805/wiki/217752
Account: shiokihuang@gmail.com xk-huang
Click "Data", and hit "Join".

Download with commands: https://help.synapse.org/docs/Downloading-Data-Programmatically.2003796248.html
Installation https://python-docs.synapse.org/tutorials/configuration/
```
pip install synapseclient[pandas, pysftp]
```

Configure token: "Your Account" -> "Account Settings" -> "Personal Access Tokens" https://python-docs.synapse.org/tutorials/authentication/#use-synapseconfig

The dataset split from the authors https://drive.google.com/file/d/1t4fIQQkONv7ArTSZe4Nucwkk1KfdUDvW/view?usp=sharing



The structure:
```shell
.
├── Abdomen
│   ├── Abdomen.zip
│   ├── RawData.zip
│   ├── Reg-Training-Testing.zip
│   ├── Reg-Training-Training.zip
│   └── SYNAPSE_METADATA_MANIFEST.tsv
├── averaged-testing-images
│   ├── DET0000301_avg.nii.gz

│   ├── DET0044901_avg.nii.gz
│   └── SYNAPSE_METADATA_MANIFEST.tsv
├── averaged-training-images
│   ├── DET0000101_avg.nii.gz

│   ├── DET0044601_avg.nii.gz
│   └── SYNAPSE_METADATA_MANIFEST.tsv
├── averaged-training-labels
│   ├── DET0000101_avg_seg.nii.gz

│   ├── DET0044601_avg_seg.nii.gz
│   └── SYNAPSE_METADATA_MANIFEST.tsv
├── Cervix
│   ├── CervixRawData.zip
│   ├── CervixRegData.zip
│   ├── Cervix.zip
│   └── SYNAPSE_METADATA_MANIFEST.tsv
└── SYNAPSE_METADATA_MANIFEST.tsv


78G	Abdomen
39G	Abdomen/Abdomen.zip
1.6G	Abdomen/RawData.zip
15G	Abdomen/Reg-Training-Testing.zip
23G	Abdomen/Reg-Training-Training.zip
4.0K	Abdomen/SYNAPSE_METADATA_MANIFEST.tsv

33M	averaged-testing-images
39M	averaged-training-images
384K	averaged-training-labels

47G	Cervix
2.1G	Cervix/CervixRawData.zip
22G	Cervix/CervixRegData.zip
24G	Cervix/Cervix.zip
4.0K	Cervix/SYNAPSE_METADATA_MANIFEST.tsv

40K	SYNAPSE_METADATA_MANIFEST.tsv
```
It seems that `Raw` contains the 3D data, while `Reg` contains the huge amount of data that are chunked into slides.
The sizes are 1.6G vs. 15/23G. in `Abdomen`.


### Preprocessing

Besides the readme of UNETR https://github.com/Project-MONAI/research-contributions/tree/main/UNETR/BTCV#dataset
Check the tutorials from MONAI for details https://github.com/Project-MONAI/tutorials/blob/main/3d_segmentation/unetr_btcv_segmentation_3d_lightning.ipynb, https://github.com/Project-MONAI/tutorials/blob/main/3d_segmentation/unetr_btcv_segmentation_3d.ipynb

Unzip `Abdomen/RawData.zip`.

Then make directories and move the data around.
- Training and validation: `imagesTr` and `labelsTr`.
- Testing: `imagesTs`.

Script:
```shell
mkdir -p unetr_btcv/{imagesTr,labelsTr,imagesTs}

unzip /path/to/zip -d unetr_btcv

mv unetr_btcv/RawData/Training/img/*.nii.gz unetr_btcv/imagesTr
mv unetr_btcv/RawData/Training/label/*.nii.gz unetr_btcv/labelsTr
mv unetr_btcv/RawData/Testing/img/*.nii.gz unetr_btcv/imagesTs
```


### Data Card
- Target: 13 abdominal organs including 1. Spleen 2. Right Kidney 3. Left Kideny 4.Gallbladder 5.Esophagus 6. Liver 7. Stomach 8.Aorta 9. IVC 10. Portal and Splenic Veins 11. Pancreas 12.Right adrenal gland 13.Left adrenal gland.
- Task: Segmentation
- Modality: CT
- Size: 30 3D volumes (24 Training + 6 Testing)
	- Index 11-20 are missing.

### Location at VLAA@UCSC

Location on VLAA@UCSC
My download dataset is at `vila-4u-9:/data3/xiaoke/data/unetr_btcv_data`.
I have back it up at `/HDD_data_storage_2u_1/xiaoke/data/unetr_btcv_data`.
- `rsync -avP /data3/xiaoke/data/unetr_btcv_data /HDD_data_storage_2u_1/xiaoke/data/`

To sync up to new machine
```shell
rsync -avP /HDD_data_storage_2u_1/xiaoke/data/unetr_btcv_data /path/to/data
```

Difference with Sucheng's BTCV
`/HDD_data_storage_2u_2/rsc/Medical/BTCV`, uses different train-val split, but the test split is the same as UNETR. The size is about 1.4GB, which is the same as the original data.