# XCASH_ALL_Miner

XCASH_ALL_Miner is a universal Stratum pool miner. This miner supports CPUs, AMD and NVIDIA GPUs and can be used to mine XCASH.

XCASH_ALL_Miner hash 0% dev fee.

## Overview
* [Features](#features)
* [Download](#download)
* [Creating config file](#creating-config-file)
* [Changing Settings](#changing-settings)
* [Usage](#usage)
* [Build](#build)

## Features

- support all common backends (CPU/x86, AMD-GPU and NVIDIA-GPU)
- support all common OS (Linux, Windows and macOS)
- easy to use
  - guided start (no need to edit a config file for the first start)
  - auto-configuration for each backend
- open source software (GPLv3)
- TLS support
- [HTML statistics](doc/usage.md#html-and-json-api-report-configuraton)
- [JSON API for monitoring](doc/usage.md#html-and-json-api-report-configuraton)

## Download

You can find the latest releases and precompiled binaries on GitHub under [Releases](https://github.com/X-CASH-official/XCASH_ALL_Miner/releases).

## Creating config file

If you dont have any config files, it is best to let the software generate them for you. To do this, run the software with no options  
`XCASH_ALL_Miner`

#1 It will first ask you for a local port on your computer, to run the http server to allow you to check your stats on another device on your network.

8000 is usually a good port to use.

#2 Then it will ask you for a pool address:port

#3 It will then ask for the wallet address you want to mine to

#4 Next it will ask for the password. You usually can put x here

#5 Now it will ask for the Rig identifier, which you can leave empty

#6 It will now ask if you want to use SSL

#7 It will now ask if you want to use nicehash

#8 It will now ask if you want to configure a backup pool

At this point, let it generate the config files, and then when it says connected to pool you can close the software.

## Changing settings

You will now have a pools.txt file. This file will allow you to change your address and pool at any time

You will now have a cpu.txt file. This file will allow you to change the settings to try to get the best performance for your CPU.

You might have a nvidia.txt file. This file will allow you to change the settings to try to get the best performance for your NVIDIA GPU.

You might have a amd.txt file. This file will allow you to change the settings to try to get the best performance for your AMD GPU.

## Usage
* To mine using all avaible components for your machine  
`XCASH_ALL_Miner`

* To mine using only the CPU  
`XCASH_ALL_Miner --noAMD --noNVIDIA`

* To mine using only an NVIDIA GPU  
`XCASH_ALL_Miner --noCPU --noAMD`

* To mine using only an AMD GPU  
`XCASH_ALL_Miner --noCPU --noNVIDIA`

## Build
### Linux(Ubuntu)

#1 If you want to mine using AMD, you will need to download the [AMD drivers](https://www.amd.com/en/support)  
Once downloaded, unzip the file and then cd into the folder. Then run  
`sudo dpkg --add-architecture i386 && ./amdgpu-pro-install --opencl=legacy,palclear`  
Press y to install the packages.

#2 If you want to mine using NVIDIA, you will need to install [CUDA toolkit](https://developer.nvidia.com/cuda-downloads?target_os=Linux)  
Choose the .DEB file  
Once downloaded run  
`sudo dpkg -i cuda*.deb`  
It will say the cuda key is not installed, so run the command it gives and then run  
`sudo dpkg -i cuda*.deb && sudo apt update && sudo apt install cuda && export PATH=/usr/local/cuda-10.0/bin${PATH:+:${PATH}}`

#3 Clone the repo and build a static build (remove export AMDAPPSDKROOT=/opt/amdgpu-pro/ && cmake -DOpenCL_INCLUDE_DIR=/usr/lib/x86_64-linux-gnu/ . if not using AMD)  
`git clone https://github.com/X-CASH-official/XCASH_ALL_Miner.git && cd XCASH_ALL_Miner && export AMDAPPSDKROOT=/opt/amdgpu-pro/ && cmake -DOpenCL_INCLUDE_DIR=/usr/lib/x86_64-linux-gnu/ . -DCMAKE_LINK_STATIC=ON -DXMR-STAK_COMPILE=generic . && make install && cd bin`

### Windows
* Install [Visual Studio 2017 Community Edition](https://visualstudio.microsoft.com/downloads/)  
make sure to select `Desktop development with C++` and `VC++ 2017 version 15.4 v14.11 toolset` in Individual Components => Compilers, build tools, and runtimes

* Install [CMAKE](https://cmake.org/download/)

* Download [precompiled binaries](https://github.com/fireice-uk/xmr-stak-dep/releases/download/v2/xmr-stak-dep.zip)

#1 If you want to mine using AMD, you will need to download the [AMD drivers](https://www.amd.com/en/support)  
You will also need the [OCL-SDK](https://github.com/GPUOpen-LibrariesAndSDKs/OCL-SDK/releases)

#2 If you want to mine using NVIDIA, you will need to install [CUDA toolkit](https://developer.nvidia.com/cuda-downloads?target_os=Windows)  

#3 Clone the repo  
`git clone https://github.com/X-CASH-official/XCASH_ALL_Miner.git && cd XCASH_ALL_Miner`

#4 Build  
Close the command prompt window and naviagte into the XCASH_ALL_Miner folder  
Then open a new command prompt window and run  
`"C:\Program Files (x86)\Microsoft Visual Studio\2017\Community\VC\Auxiliary\Build\vcvarsall.bat" x64 -vcvars_ver=14.11`

The run the following to build (replace DIRECTORY_FOR_PRECOMPILED_BINARIES with the directory where you unzipped the precompiled binaries)  
`mkdir build && cd build && set CMAKE_PREFIX_PATH=DIRECTORY_FOR_PRECOMPILED_BINARIES\hwloc;DIRECTORY_FOR_PRECOMPILED_BINARIES\libmicrohttpd;DIRECTORY_FOR_PRECOMPILED_BINARIES\openssl && cmake -G "Visual Studio 15 2017 Win64" -T v141,host=x64 .. && cmake --build . --config Release --target install && cd bin\Release && copy C:\xmr-stak-dep\openssl\bin\* .`
