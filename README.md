# Tensorflow-Installation-using-Homebrew-on-Apple-Silicon
Tensorflow Installation using Homebrew on Apple Silicon (tested on M1 and M2)

## Install Homebrew

```
/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"
```

## Install miniforge
```
brew install --cask miniforge
```

## Setup your shell
```
conda init <shell-name>
eg: conda init  zsh
```

## Create a Virtual Environment 
```
conda create -n <env_name> python=<version>
eg: conda create -n tfd python=3.9.6
```
Activate Environment
```
conda	activate tfd
```

## Install TensorFlow in the Virtual Environment 

```
conda install -c apple tensorflow-deps
python -m pip install tensorflow-macos
python -m pip install tensorflow-metal
```

## Register environment 
```
pip install ipykernel
python -m ipykernel install --user --name tfd --display-name "Python 3.9.6 (tfd)"
jupyter kernelspec list
```

## Install Jupyter Notebook and common packages 

```
conda install notebook -y

pip install numpy  --upgrade
pip install pandas  --upgrade
pip install matplotlib  --upgrade
pip install scikit-learn  --upgrade
pip install scipy  --upgrade
pip install plotly  --upgrade
pip install seaborn
pip install afinn
pip install beautifulsoup4
pip install polars
pip install keras-tuner --upgrade
pip install xgboost
pip install statsmodels
pip install --no-binary lightgbm --config-settings=cmake.define.USE_OPENMP=OFF --config-settings=cmake.define.USE_GPU=ON 'lightgbm>=4.0.0'

NLP Libraries 
pip install nltk
pip install pyspark
pip install regex
pip install spacy
pip install stop-words 
pip install textacy
```

## Testing the environment

Almost there! Let's just make sure everything is setup and works fine! Jump in to a new Jupyter notebook using this command:

```
jupyter notebook
```

## Check GPU availability

``` python
import sys
import tensorflow.keras
import lightgbm as lgb
import pandas as pd
Import polars as pl
import numpy as np
import sklearn as sk
import scipy as sp
import tensorflow as tf
import platform

print(f"Python Platform: {platform.platform()}")
print(f"Tensor Flow Version: {tf.__version__}")
print(f"Keras Version: {tensorflow.keras.__version__}")
print(f"\nPython {sys.version}")
print(f"Pandas {pd.__version__}")
print(f"Polars {pl.__version__}")
print(f"Scikit-Learn {sk.__version__}")
print(f"SciPy {sp.__version__}")
gpu = len(tf.config.list_physical_devices('GPU'))>0
print("GPU is", "available" if gpu else "NOT AVAILABLE")
print(f"\nNumPy Path {np.__path__}")
tf.config.experimental.list_physical_devices()
```

## Remove Virtual Environment
```
conda info --envs
deactivate tfd
conda remove --name tfd --all
```

## Uninstallation miniforge

Any modifications to your shell rc files that were made by Miniforge. Use this first command to see what rc files will be updated
```
conda init --reverse --dry-run
```

Use this next command to take action on the rc files listed above
```
conda init --reverse
```

Temporarily IGNORE the shell message. For changes to take effect, close and re-open your current shell, and CLOSE THE SHELL ONLY AFTER the 3rd step below is completed. Remove the folder and all subfolders where the base environment for Miniforge was installed
```
CONDA_BASE_ENVIRONMENT=$(conda info --base)
echo The next command will delete all files in ${CONDA_BASE_ENVIRONMENT}
```

🚨 Warning: the rm command below is irreversible! check the output of the echo command above. To make sure you are deleting the correct directory
```
rm -rf ${CONDA_BASE_ENVIRONMENT}
```

Any global conda configuration files that are left behind
```
echo ${HOME}/.condarc will be removed if it exists
rm -f "${HOME}/.condarc"
echo ${HOME}/.conda and underlying files will be removed if they exist.
rm -fr ${HOME}/.conda
```

