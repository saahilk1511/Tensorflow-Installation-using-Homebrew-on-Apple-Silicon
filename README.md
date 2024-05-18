# Tensorflow-Installation-using-Homebrew-on-Apple-Silicon
Tensorflow Installation with GPU support using Homebrew on Apple Silicon (tested on M1 and M2)

## 1. Install Homebrew
Download and install Homebrew, the package manager for macOS, using the provided command in the Terminal.
```
/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"
```

## 2. Install miniforge
Use Homebrew to install miniforge, a conda package manager for setting up environments.
```
brew install --cask miniforge
```

## 3. Setup your shell
Initialize conda for your shell to enable environment management and restart the terminal.
```
conda init <shell-name>
eg: conda init zsh
```

## 4. Create a Virtual Environment 
Create and activate a new conda virtual environment with Python.
```
conda create -n <env_name> python=<version>
eg: conda create -n tensorflow python=3.9.6
```
### 4.1 Activate Environment
```
conda activate <env_name>
eg: conda activate tensorflow
```

## 5. Install TensorFlow in the Virtual Environment 
Install TensorFlow and its dependencies tailored for Apple Silicon within the virtual environment.
```
conda install -c apple tensorflow-deps
python -m pip install tensorflow-macos
python -m pip install tensorflow-metal
```

## 6. Register environment
Register the newly created environment to the kernel using this command. This links the new **tensorflow** environment to the Jupyter Notebook
```
pip install ipykernel
python -m ipykernel install --user --name tensorflow --display-name "Python 3.9.6 (tensorflow)"
jupyter kernelspec list
```

## 7. Install Jupyter Notebook and common packages
Install Jupyter Notebook and a list of common Python packages for data science and machine learning.
```
conda install notebook -y

pip install numpy  --upgrade
pip install pandas  --upgrade
pip install matplotlib  --upgrade
pip install scikit-learn  --upgrade
pip install scipy  --upgrade
pip install plotly  --upgrade
pip install seaborn

## Optional
pip install polars
pip install keras-tuner --upgrade
pip install xgboost
pip install statsmodels
pip install pyspark

## LightGBM with GPU support
pip install --no-binary lightgbm --config-settings=cmake.define.USE_OPENMP=OFF --config-settings=cmake.define.USE_GPU=ON 'lightgbm>=4.0.0'

## NLP Libraries
pip install nltk
pip install afinn
pip install beautifulsoup4
pip install bs4
pip install gensim
pip install regex
pip install spacy
pip install stop-words 
pip install textacy
```

## 8. Testing the environment
Open a new Jupyter Notebook using this command:
```
jupyter notebook
```

## 9. Check GPU availability
Verify if the GPU is available for TensorFlow and print the versions of installed packages.
``` python
import sys
import tensorflow.keras
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

🚨 Note: The following steps outline how to remove the virtual environment and miniforge. The guide to installing TensorFlow with GPU support ends here. You can skip these next steps.

## 1. Remove Virtual Environment
Deactivate and delete the created conda virtual environment.
```
conda info --envs
deactivate tensorflow
conda remove --name tensorflow --all
```

## 2. Uninstallation miniforge
Remove all traces of miniforge from your system, including configuration files and directories.

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

🚨 Warning: the rm command below is irreversible! Check the output of the echo command above. To make sure you are deleting the correct directory
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

