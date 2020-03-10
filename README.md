# fastmerkle

A fork of fastmerkle to benchmark how fast GPUs can run iterated SHAs


**Usage**

First, run `./fastmerkle info` (`.\fastmerkle.exe info` on Windows) to get a list of supported OpenCL hardware that you can use. Example output:  
```
Platform #0
  Name:       Apple
  Version:    OpenCL 1.2 (Oct 29 2018 21:43:16)

  Device #0
    Name:     Intel(R) Core(TM) i7-8550U CPU @ 1.80GHz
    Type:     CPU

  Device #1
    Name:     Intel(R) UHD Graphics 617
    Type:     GPU
```
For any device, you have to note down the platform id and the device id. I will be using the Intel UHD Graphics, that's platform #0 and device #1. Don't pick a device of type CPU. It would be possible to run the calculations on CPUs as well, but the code is optimized for GPUs.  

Now, open the config.txt file:  
```ini
# OpenCL platform index
# Run fastmerkle with command 'info' to obtain
# information about available hardware
platform = 0

# OpenCL device to use on specified platform
# Run fastmerkle with command 'info' to obtain
# information about available hardware
device = 1

# Whether or not you want to use
# host pointers when dealing with buffers.
# Setting to 1 can increase or decrease
# performance or even crash the program.
# For integrated GPUs it's worthwhile testing
# it, for dedicated GPUs it might result into
# errors and wrong results.
use_host_ptr = 0

# Number of iterations for benchmarking
iterations = 10
```
Here you'll have to set your platform id and your device id. You can see that I already entered these two identifiers.  
You can keep the other two properties as is.

`./fastmerkle benchmark` (`.\fastmerkle.exe benchmark` on Windows) to run a benchmark. This will take some time.


**How to compile?**  

First off, you need to install OpenCL.  
You can skip this step if you are on macOS, as it ships with OpenCL.

Any of these links below should work. It's recommend to install your GPU vendor's library.  
Download links for OpenCL library:

**NVIDIA**: https://developer.nvidia.com/cuda-downloads  
**AMD**: https://github.com/GPUOpen-LibrariesAndSDKs/OCL-SDK/releases  
**Intel**: https://software.intel.com/en-us/articles/opencl-drivers  

On my Windows VM, I found it pretty easy to use the AMD link.  
If you have installed make and cmake, you can just run `cmake . && make` in the root directory of this repository. You'll get a `fastmerkle` file.  
Alternatively, with gcc:  

**macOS**: `gcc main.c -framework OpenCL -o fastmerkle`  
**Linux** and **Windows (MinGW-w64)**: `gcc main.c -lOpenCL -o fastmerkle`

You might have to set include and library paths.
