TensorFlow 安裝視硬體有不同的支援方法。如果在沒有 nVidia CUDA 支援的情況下，請安裝 CPU 版本。安裝方式可以透過 pip (Package Installer for Python ) 或是 docker

# 透過 pip 安裝

下面是最間單的安裝方式。

``` pip
# Requires the latest pip  
pip install --upgrade pip  
  
# Current stable release for CPU  
pip install tensorflow
```

但是上述方法會把原本安裝好的 TensorFlow 被覆蓋掉，所以建議改用 Python Virtual Machine 的方法來安裝

``` pip
# 安裝 virtualenv (Python Virtual Machine)
pip install virtualenv

# 建立 virtual machine 使用的環境 (~/tmp/envs)
cd ~
mkdir -p tmp/envs
virtualenv ~/tmp/envs/tenserflow

# 啟動 virtual machine
source ~/envs/tensorflow/bin/activate

# Current stable release for CPU  
pip install tensorflow
```

# 透過 docker 安裝
這是 docker 安裝與執行的命令
``` docker
# Download latest stable image
docker pull tensorflow/tensorflow:latest

# Start Jupyter server
docker run -it -p 8888:8888 tensorflow/tensorflow:latest-jupyter
```

# GPU 支援

> [!NOTE]
> TensorFlow 目前支援 CUDA® architectures 3.5, 5.0, 6.0, 7.0, 7.5, 8.0 and higher 以上版本的 GPU
> 
> 舊版 GPU 或是不支援 PTX code 的 GPU 可能會在開啟 `CUDA_FORCE_PTX_JIT=1` 的情況下，載入之敗 

首先需要先安裝 nVidia driver。可以透過下列命令來確認是否安裝

> nvidia-smi