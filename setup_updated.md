## Installation

These steps are to get the CNDP training model to run on Windows 10. Last updated by Elbert Nhan on 20/7/2024.

Required dependency is a WSL (Windows Subsytem for Linux), and a CUDA compatible GPU. This guide was created on windows 10 using Ubuntu and a RTX 3070 TI.

Compatible CUDA Version found to be working is 11.8 with the code. This will be the version used in the installation.

Navigate to this [link](https://developer.nvidia.com/cuda-11-8-0-download-archive?target_os=Linux&target_arch=x86_64&Distribution=WSL-Ubuntu&target_version=2.0&target_type=deb_local) to install CUDA 11.8. Settings for install should be Linux, x86_64, WSL_UBUNTU, 2.0, deb, local.

Follow the installation instructions in the linux shell. Note the change in the lsat line that 
**sudo apt-get -y install cuda** has been replaced with **sudo apt-get install cuda-toolkit-11.8**

```sh
wget https://developer.download.nvidia.com/compute/cuda/repos/wsl-ubuntu/x86_64/cuda-wsl-ubuntu.pin
sudo mv cuda-wsl-ubuntu.pin /etc/apt/preferences.d/cuda-repository-pin-600
wget https://developer.download.nvidia.com/compute/cuda/11.8.0/local_installers/cuda-repo-wsl-ubuntu-11-8-local_11.8.0-1_amd64.deb
sudo dpkg -i cuda-repo-wsl-ubuntu-11-8-local_11.8.0-1_amd64.deb
sudo cp /var/cuda-repo-wsl-ubuntu-11-8-local/cuda-*-keyring.gpg /usr/share/keyrings/
sudo apt-get update
sudo apt-get install cuda-toolkit-11.8
```

Once installed, we need to change PATH variables. To do so, open linux shell again.
```sh
nano .bashrc
```

In the file add the two following lines at the end of the file.
```sh
export PATH=/usr/local/cuda-11.8/bin${PATH:+:${PATH}}$
export LD=LIBRARY_PATH=/usr/local/cuda-11.8/lib64${LD_LIBRARY_PATH:+:${LD_LIBRARY_PATH}}
```
Press ctrl+s, then ctrl+x to save and exit respectively.

Navigate to reproduce_env_unbutu.sh in learning-CNDP > CNDP-DQN

Remove pyg_lib from the 7th line if it is present.

Replace what follows in the line wtih -f with https://data.pyg.org/whl/torch-2.3.1+cu118.html. It should look like this.
```sh
pip install torch_scatter torch_sparse torch_cluster torch_spline_conv -f https://data.pyg.org/whl/torch-2.3.1+cu118.html
```
Then save the file. Run the file by typing the following in the terminal.
```sh
bash reproduce_env_unbuntu.sh
```
Run the environment by typing the following.
```sh
conda activate learnCNDP
```
Then install wandb in the created environment.
```sh
pip install wandb
```
To set up the C portion of the code type the following.
```sh
python setup.py build_ext -i
```
Finally run the code by typing the following.
```sh
CUDA_VISIBLE_DEVICES=gpu_id python train.py
```