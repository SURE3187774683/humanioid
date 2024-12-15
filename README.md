# Humanioid(Windows+Wsl2(Ubuntu20.04)+MobaXterm)

# 1 ISAAC_GYM环境构建——驱动安装和torch配置

## 1.1 配置版本：CUDA+Nvidia_Driver+pytorch

### CUDA Toolkit——11.6（！选择wsl版本）
https://developer.nvidia.com/cuda-12-1-0-download-archive?target_os=Linux&target_arch=x86_64&Distribution=WSL-Ubuntu&target_version=2.0&target_type=deb_local

### NVIDIA Windows Driver_550（驱动可以向下兼容）
https://www.nvidia.com/en-us/drivers/

### Python_3.7.12
Isaac_gym创建的环境rlgpu的python版本是确定的,不能更改

### Pytorch_1.13.0+torchvision+torchaudio(NO pytorch-cuda)(！不要再安装CUDA覆盖了之前的版本)
conda install pytorch==1.13.0 torchvision==0.14.0 torchaudio==0.13.0 -c pytorch -c nvidia
(https://pytorch.org/get-started/previous-versions/)


## 1.2 Some BUG
### Segmentation fault (core dumped)
a.安装：sudo apt install vulkan-tools
检查：vulkaninfo

b.如果进入/usr/share/vulkan/icd.d查看文件信息只有三个文件则执行：

sudo add-apt-repository ppa:kisak/kisak-mesa

sudo apt update

sudo apt upgrade

c.如果vulkaninfo报错（symbol lookup error: /lib/x86_64-linux-gnu/libwayland-client.so.0: undefined symbol: ffi_type_uint32, version LIBFFI_BASE_7.0）

检查依赖关系&&更新库文件：确保您的系统安装了所有必要的依赖库，特别是libffi
sudo apt-get install libffi-dev && sudo apt-get update && sudo apt-get upgrade

创建符号链接并拷贝文件：

sudo ln -s /usr/lib/x86_64-linux-gnu/libffi.so.7 /usr/lib/x86_64-linux-gnu/libffi.so.6

sudo cp /home/sure/miniconda3/envs/rlgpu/lib/libpython3.7m.so.1.0 /usr/lib

设置LD_LIBRARY_PATH：export LD_LIBRARY_PATH=/usr/lib/x86_64-linux-gnu/:$LD_LIBRARY_PATH

## 1.3 bashrc文件配置
### # LIB
export LD_LIBRARY_PATH=/home/sure/miniconda3/envs/rlgpu/lib
export LD_LIBRARY_PATH=/usr/lib/wsl/lib
### # xming
export DISPLAY=10.79.201.2:0.0
### # CUDA path
export PATH=/usr/local/cuda-11.6/bin:$PATH
export LD_LIBRARY_PATH=/usr/local/cuda-11.6/lib64:$LD_LIBRARY_PATH
export CUDA_HOME=/usr/local/cuda-11.6
export LD_LIBRARY_PATH=$LD_LIBRARY_PATH:/home/sure/RL-code/humanplus-main/HST/isaacgym/python/isaacgym/_bindings/linux-x86_64
### # 设置cuda的编号
export CUDA_LAUNCH_BLOCKING=0
export HYDRA_FULL_ERROR=1
### # 删除vulkaninfo报错
export LD_LIBRARY_PATH=/usr/lib/x86_64-linux-gnu/:$LD_LIBRARY_PATH
### # 这有助于减少内存碎片化，确保有足够的连续内存空间分配给大的内存请求
export PYTORCH_CUDA_ALLOC_CONF=max_split_size_mb:50

# 2 有用的帖子
## 2.1 安装教程
https://blog.csdn.net/m0_37802038/article/details/134629194?ops_request_misc=&request_id=&biz_id=102&utm_term=Isaacgym%E6%8A%A5%E9%94%99%20Segmentation%20fault&utm_medium=distribute.pc_search_result.none-task-blog-2~all~sobaiduweb~default-9-134629194.142^v100^pc_search_result_base6&spm=1018.2226.3001.4187

https://blog.csdn.net/wsygbthhhh/article/details/143918730?ops_request_misc=%257B%2522request%255Fid%2522%253A%2522a54726eeae0be86042a6003e0c40c814%2522%252C%2522scm%2522%253A%252220140713.130102334.pc%255Fall.%2522%257D&request_id=a54726eeae0be86042a6003e0c40c814&biz_id=0&utm_medium=distribute.pc_search_result.none-task-blog-2~all~first_rank_ecpm_v1~rank_v31_ecpm-6-143918730-null-null.142^v100^pc_search_result_base6&utm_term=isaacgym%E5%AE%89%E8%A3%85%E6%95%99%E7%A8%8B&spm=1018.2226.3001.4187

https://blog.csdn.net/littlewells/article/details/140179837

https://blog.csdn.net/weixin_44061195/article/details/131830133
## 2.2 CUDA Python tookit pytorch匹配关系
https://pytorch.org/get-started/previous-versions/

https://blog.csdn.net/weixin_41809117/article/details/141246957

https://docs.nvidia.com/cuda/cuda-toolkit-release-notes/index.html#cuda-major-component-versions__table-cuda-toolkit-driver-versions

## 2.3 英伟达驱动卸载安装教程
https://blog.csdn.net/Perfect886/article/details/119109380

