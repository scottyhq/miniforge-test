# Quick test of miniforge vs miniconda

to build:
```
cd miniforge-test
docker build -t miniforge:test .
```

Questions:
### 1) are miniconda and miniforge base environments equivalent in size and structure?
 
* Ran into in Docker build  https://github.com/ContinuumIO/docker-images/issues/140
* Seems like miniforge automatically runs `conda clean` (no pkgs directory) 
```
(base) ubuntu@4249f73bc693:~$ ls miniforge3/
LICENSE.txt  bin  compiler_compat  conda-meta  condabin  envs  etc  include  lib  man  share  shell  ssl  x86_64-conda_cos6-linux-gnu
(base) ubuntu@dd1c6a601f90:~$ du -sh miniforge3/
218M    miniforge3/
---------------------------------------
(base) ubuntu@59f6cde78c20:~$ du -sh miniconda3/
290M	miniconda3/
(base) ubuntu@59f6cde78c20:~$ ls miniconda3/
LICENSE.txt  bin  compiler_compat  conda-meta  condabin  envs  etc  include  lib  pkgs  share  shell  ssl  x86_64-conda_cos6-linux-gnu
```

### 2) Can we share packages from the base image installation?

* Packages are re-downloaded with miniforge b/c no pkgs/
* Seems packages not shared w/ base? https://stackoverflow.com/questions/56232524/should-the-conda-base-environment-be-kept-up-to-date
```
(base) ubuntu@dd1c6a601f90:~$ conda create --name myclone --clone base
Source:      /home/ubuntu/miniforge3
Destination: /home/ubuntu/miniforge3/envs/myclone
The following packages cannot be cloned out of the root environment:
 - conda-forge/linux-64::conda-4.8.2-py37_0
Packages: 36
Files: 1
Downloading and Extracting Packages
.
.
.
(base) ubuntu@912755c01923:~$ du -sh miniforge3/
599M	miniforge3/
(base) ubuntu@912755c01923:~$ conda clean -yaf
Remove all contents from the following package caches?
  - /home/ubuntu/miniforge3/pkgs
(base) ubuntu@912755c01923:~$ du -sh miniforge3/
432M	miniforge3/
---------------------------------------
(base) ubuntu@4963bdb71493:~$ conda create --name myclone --clone base
Source:      /home/ubuntu/miniconda3
Destination: /home/ubuntu/miniconda3/envs/myclone
The following packages cannot be cloned out of the root environment:
 - defaults::conda-4.7.12-py37_0
Packages: 34
Files: 0
Preparing transaction: done
Verifying transaction: done
Executing transaction: done
(base) ubuntu@4963bdb71493:~$ du -sh miniconda3/
489M	miniconda3/
(base) ubuntu@4963bdb71493:~$
(base) ubuntu@4963bdb71493:~$ conda clean -yaf
Remove all contents from the following package caches?
  - /home/ubuntu/miniconda3/pkgs
(base) ubuntu@4963bdb71493:~$ du -sh miniconda3/
396M	miniconda3/
```
