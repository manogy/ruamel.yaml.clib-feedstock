
Install Jupyterhub on S390x VM.
==================================

This process consists of two main steps: 
1. Creating ruamel.yaml.clib conda package with the help of feedstock.
2. Consuming this package with other dependencies of Jupyterhub.


## Creating ruamel.yaml.clib conda package with the help of feedstock 

S390x is a Big Endian CPU arch and ruamel.yaml.clib has an open bug related to this hence we need to apply a patch and build the ruamel.yaml.clib package with the help of feedstock. 
Please follow the process described in the README of  https://github.com/manogy/ruamel.yaml.clib-feedstock


## Setup conda env with all required packages to run Jupyterhub
Please make sure Nodejs is available on the VM.

Install configurable-http-proxy on the VM globally by running the below command:
```
npm install -g configurable-http-proxy
``` 
Create a new conda env with the desired name

```
conda create -n jupyterhub-test
```

Activate the conda env and install the required packages

```
conda activate jupyterhub-test 

conda install python=3.10.13 cryptography=41.0.7 notebook jupyterlab ipykernel libsodium ruamel.yaml
```


#### Install the ruamel.yaml.clib package which we created in Step 1 

Install the ruamel.yaml.clib package like below, here /root/miniconda3/conda-bld is the channel name which we get during the build process as per the README of https://github.com/manogy/ruamel.yaml.clib-feedstock

```
 conda install ruamel.yaml.clib  -c  /root/miniconda3/conda-bld
```

Install Jupyterhub with help of PIP command

```
pip install jupyterhub --no-cache-dir

```

Now Jupyter Hub has been installed, which can be used by the user. 


