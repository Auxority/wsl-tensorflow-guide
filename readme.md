# Tensorflow 2 - Windows with WSL installation

## Windows
1. Install any Windows updates that you might have.

## NVIDIA drivers
1. Visit [this link](https://www.nvidia.com/drivers) or open Geforce Experience.
2. Update your drivers to the latest version.

## NVIDIA CUDA Toolkit
1. Visit [this link](https://developer.nvidia.com/cuda-downloads) to find the latest download of CUDA Toolkit.
2. Install the latest CUDA Toolkit using the installer.
3. Add the following to the system path environment variable ([click here](https://www.howtogeek.com/787217/how-to-edit-environment-variables-on-windows-10-or-11/) for more information).:
    - `C:\Program Files\NVIDIA GPU Computing Toolkit\CUDA\v12.1\bin`
    - `C:\Program Files\NVIDIA GPU Computing Toolkit\CUDA\v12.1\extras\CUPTI\lib64`
    - `C:\Program Files\NVIDIA GPU Computing Toolkit\CUDA\v12.1\include`
    - `C:\Program Files\NVIDIA GPU Computing Toolkit\CUDA\v12.1\libnvvp`
4. Visit [this link](https://docs.nvidia.com/cuda/cuda-installation-guide-microsoft-windows/index.html) for more information about the installation of CUDA Toolkit.

## Zlib
1. Visit [this link](https://www.winimage.com/zLibDll/) to find the latest zLib downloads.
2. Right click the `pre-built zlib DLL` download link for `AMD64/Intel EM64T`, and select `Save link as..` to save the zip file to your computer.
3. Extract the zip file. The folder should contain a `dll_x64` and `static_x64` directory.
4. Copy the extracted folder.
5. Go to `C:\Program Files\` on your computer using File Explorer.
6. Create a new directory here called `Zlib`.
7. Enter the new `Zlib` directory you just created.
8. Paste the extracted folder you have copied.
9. Add the `dll_x64` directory to your system path environment variable ([click here](https://www.howtogeek.com/787217/how-to-edit-environment-variables-on-windows-10-or-11/) for more information).

## cuDNN SDK
1. Visit [this link](https://developer.nvidia.com/rdp/cudnn-download) to find the latest cuDNN download.
2. You will have to create an account in order to download this.
3. Once you are logged in download the latest Windows zip file.
4. Unzip the cuDNN zip file.
5. Open the extracted folder.

    **Important note**: You must replace 8.x and 8.x.y.z with your specific cuDNN version.

6. Create a directory called `NVIDIA` in `C:\Program Files`
6. Copy all files from the `bin` directory to `C:\Program Files\NVIDIA\CUDNN\v8.x\bin`
7. Copy all files from the `include` directory to `C:\Program Files\NVIDIA\CUDNN\v8.x\include`
8. Copy all files from the `lib` directory to `C:\Program Files\NVIDIA\CUDNN\v8.x\lib`
9. Add `C:\Program Files\NVIDIA\CUDNN\v8.x\bin` to your system path environment variable. 
10. Visit [this link](https://docs.nvidia.com/deeplearning/cudnn/install-guide/index.html) for more information about the cuDNN SDK installation.

## Python
1. Visit [this link](https://www.python.org/downloads/windows/) to view Python downloads for Windows.
2. Download the Python 3.10.x installer (just select the most recent one).
3. Run the installer and make sure to use the checkbox to add Python to your PATH.
4. Open your favorite terminal.
5. Run `pip install --upgrade pip` to update pip to the latest version.

## WSL
1. Make sure you are running Windows 11.
2. Open your favorite terminal.
3. Run `wsl --shutdown` to stop all WSL instances.
4. Run `wsl -l | tail -n +2 | awk '{print $1}' | xargs -I {} wsl --unregister {}` to delete all your WSL instances.
5. Run `wsl --install` to install WSL2 if this has not been installed.
6. Run `wsl --install Ubuntu` to install Ubuntu (create a username and password here).

```ps
wsl --shutdown
wsl -l | tail -n +2 | awk '{print $1}' | xargs -I {} wsl --unregister {}
wsl --install
wsl --install Ubuntu
```

7. Open your favorite terminal.
8. Run `wsl` to enter the Ubuntu instance.
9. Copy and paste the following [commands](https://developer.nvidia.com/cuda-downloads?target_os=Linux&target_arch=x86_64&Distribution=WSL-Ubuntu&target_version=2.0&target_type=deb_local) in WSL to install CUDA:

```sh
sudo apt-key del 7fa2af80

wget https://developer.download.nvidia.com/compute/cuda/repos/wsl-ubuntu/x86_64/cuda-keyring_1.0-1_all.deb
sudo dpkg -i cuda-keyring_1.0-1_all.deb

sudo apt-get update && sudo apt-get upgrade

sudo apt-get -y install cuda
```

## Miniconda & Tensorflow
1. Open your favorite terminal.
2. Run `wsl` to enter your Ubuntu instance.
3. Copy and paste the following commands into your WSL instance to install Miniconda and Tensorflow:

```
curl https://repo.anaconda.com/miniconda/Miniconda3-latest-Linux-x86_64.sh -o Miniconda3-latest-Linux-x86_64.sh
bash Miniconda3-latest-Linux-x86_64.sh
source ~/.bashrc

conda create --name tf python=3.9
conda activate tf
conda install -c conda-forge cudatoolkit=11.8.0
python3 -m pip install nvidia-cudnn-cu11==8.6.0.163

mkdir -p $CONDA_PREFIX/etc/conda/activate.d

echo -e '#!/bin/sh\nCUDNN_PATH=$(dirname $(python -c "import nvidia.cudnn;print(nvidia.cudnn.__file__)"))\nexport LD_LIBRARY_PATH=$LD_LIBRARY_PATH:$CONDA_PREFIX/lib/:$CUDNN_PATH/lib' > $CONDA_PREFIX/etc/conda/activate.d/env_vars.sh

pip install --upgrade pip
pip install tensorflow==2.12.*
```

5. Verify the tensorflow installation by running `python3 -c "import tensorflow as tf; print(tf.config.list_physical_devices('GPU'))"` in your WSL instance.
6. For more information visit [this website](https://www.tensorflow.org/install/pip/).

# Usage
1. Open your favorite terminal.
2. Enter `wsl` to enter your Ubuntu instance.
3. Run `conda activate tf` to activate the Tensorflow conda environment.
4. Run `pip install jupyter` to install Jupyter.
5. Run `jupyter notebook --ip=0.0.0.0 --port=8888 --no-browser --allow-root` to start your Jupyter notebook server.
6. Copy the url that is given (e.g. `http://127.0.0.1:8888?token=asfg8976ah8b7sd8`).
7. Open Visual Studio Code
8. Open a Jupyter Notebook in Visual Studio Code (make sure you have the Python and Jupyter extensions installed).
9. Click on `Select Kernel` on the top-right side of the notebook.
10. Click on `Select Another Kernel...`
11. Click on `Existing Jupyter Server...`
12. Click on `Enter the URL of the running Jupyter server`
13. Paste the url that you have copied previously and press Enter.
