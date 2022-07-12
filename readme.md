# Guide for docker image with pytorch+cuda
##  preparation
__1.Remove all cuda and driver in the system first.__
  + remove cuda:
    ```
    sudo apt-get --purge remove "*cuda*" "*cublas*" "*cufft*" "*cufile*" "*curand*" \
    "*cusolver*" "*cusparse*" "*gds-tools*" "*npp*" "*nvjpeg*" "nsight*" 
    ```

  + remove driver:
    ```
    sudo apt-get --purge remove "*nvidia*"
    ```
__2. Recommend use conda installation of cuda:__

```
conda install cuda -c nvidia
```
or a specific version:
```
conda install cuda -c nvidia/label/cuda-11.3.0
```
verify cuda:
```
nvcc -V
whereis nvcc
```


__3. Install driver.__

+ check kernel version first:

    ```
    dmesg |tail -4  
    ```

    return stuff like:
    ```
    NVRM: API mismatch: the client has the version 440.59, but
    NVRM: this kernel module has the version 440.33.01.  Please
    NVRM: make sure that this kernel module and all NVIDIA driver
    NVRM: components have the same version.
    ```
+ next install the correspond version of driver matched the kernel version:
    ```
    apt install nvidia-driver-<kernel version>
    ```
+  verify the driver:
    ```
    nvidia-smi
    ```
__4. Install hookup:__
```
apt-get install nvidia-container-runtime
```
## Docker setup
1. use the Dockerfile in this repo and insert your app installation steps.

2. use the requirements.yaml file in this repo to setup the conda python environment and put any packages you like in it.

3. run build-up:
    ```
    docker build -t <name>
    ```

4. verify the image installation:
   ```
   docker run -it --rm --gpus all <name> python -c "import torch;print(torch.cuda.is_available())"
   ```
   ```
    docker run -it --rm --gpus all <name> nvidia-smi 
   ```
   ```
    docker run -it --rm --gpus all <name> nvcc -V
   ```