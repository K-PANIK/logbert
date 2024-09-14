# LogBERT: Log Anomaly Detection via BERT
### [ARXIV](https://arxiv.org/abs/2103.04475) 

This repository provides the implementation of Logbert for log anomaly detection. 
The process includes downloading raw data online, parsing logs into structured data, 
creating log sequences and finally modeling. 

![alt](img/log_preprocess.png)

## Configuration
- Linux WSL 5.15.153.1-microsoft-standard-WSL2 #1 SMP Fri Mar 29 23:14:13 UTC 2024 x86_64 x86_64 x86_64 GNU/Linux
- Python 3.10
- PyTorch 1.11.0
```
nvidia-smi # nvidia-smi is the command to check NVIDIA GPU details including version
Sat Sep 14 22:55:57 2024
+-----------------------------------------------------------------------------------------+
| NVIDIA-SMI 560.35.02              Driver Version: 560.94         CUDA Version: 12.6     |
|-----------------------------------------+------------------------+----------------------+
| GPU  Name                 Persistence-M | Bus-Id          Disp.A | Volatile Uncorr. ECC |
| Fan  Temp   Perf          Pwr:Usage/Cap |           Memory-Usage | GPU-Util  Compute M. |
|                                         |                        |               MIG M. |
|=========================================+========================+======================|
|   0  NVIDIA GeForce RTX 3070 Ti     On  |   00000000:01:00.0  On |                  N/A |
|  0%   38C    P8             14W /  310W |     752MiB /   8192MiB |      6%      Default |
|                                         |                        |                  N/A |
+-----------------------------------------+------------------------+----------------------+

+-----------------------------------------------------------------------------------------+
| Processes:                                                                              |
|  GPU   GI   CI        PID   Type   Process name                              GPU Memory |
|        ID   ID                                                               Usage      |
|=========================================================================================|
|    0   N/A  N/A        25      G   /Xwayland                                   N/A      |
+-----------------------------------------------------------------------------------------+
```

## Installation
This code requires the packages listed in requirements.txt.
An virtual environment is recommended to run this code

On macOS and Linux:  
```
sudo apt update; sudo apt install python3-pip # Update package list and install pip for Python 3
python3 -m pip install --user virtualenv
sudo apt install python3.10-venv
python3 -m venv env
source env/bin/activate
pip install -r ./environment/requirements.txt

```

```
pip freeze
asttokens==2.4.1
certifi==2024.8.30
charset-normalizer==2.0.12
cycler==0.12.1
debugpy==1.8.5
decorator==5.1.1
entrypoints==0.4
exceptiongroup==1.2.2
executing==2.1.0
fonttools==4.53.1
idna==3.9
ipykernel==6.9.1
ipython==8.27.0
jedi==0.19.1
joblib==1.4.2
jupyter_client==7.4.9
jupyter_core==5.7.2
kiwisolver==1.4.7
matplotlib==3.5.1
matplotlib-inline==0.1.7
nest-asyncio==1.6.0
numpy==1.22.3
packaging==24.1
pandas==1.4.1
parso==0.8.4
pexpect==4.9.0
pillow==10.4.0
platformdirs==4.3.3
prompt_toolkit==3.0.47
ptyprocess==0.7.0
pure_eval==0.2.3
Pygments==2.18.0
pyparsing==3.1.4
python-dateutil==2.9.0.post0
pytz==2024.2
pyzmq==26.2.0
regex==2022.3.2
requests==2.27.1
scikit-learn==1.5.2
scipy==1.11.4
seaborn==0.11.2
six==1.16.0
stack-data==0.6.3
threadpoolctl==3.5.0
torch==1.11.0
tornado==6.4.1
tqdm==4.63.0
traitlets==5.14.3
typing_extensions==4.12.2
urllib3==1.26.20
wcwidth==0.2.13
```

Reference: https://packaging.python.org/guides/installing-using-pip-and-virtual-environments/


## Experiment
Logbert and other baseline models are implemented on [HDFS](https://github.com/logpai/loghub/tree/master/HDFS), [BGL](https://github.com/logpai/loghub/tree/master/BGL), and [thunderbird]() datasets

### HDFS example

#### Install logs dataset for example HDFS
```shell script
cd ~/
git clone https://github.com/K-PANIK/loghubSystemLogs.git ~/.dataset
ln -s HDFS_2k.log .dataset/HDFS/HDFS.log
```

```shell script
cd logbert
source env/bin/activate
cd HDFS

sh init.sh

# process data
python data_process.py

#run logbert
python logbert.py vocab
python logbert.py train
python logbert.py predict

#run deeplog
python deeplog.py vocab
# set options["vocab_size"] = <vocab output> above
python deeplog.py train
python deeplog.py predict 

#run loganomaly
python loganomaly.py vocab
# set options["vocab_size"] = <vocab output> above
python loganomaly.py train
python loganomaly.py predict

#run baselines

baselines.ipynb
```

### Folders created during execution
```shell script 
~/.dataset //Stores original datasets after downloading
project/output //Stores intermediate files and final results during execution
```
