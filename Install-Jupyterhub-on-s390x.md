
Install Jupyterhub on S390x VM.
==================================

This process consists of two main steps: 
1. Creating ruamel.yaml.clib conda package with the help of feedstock.
2. Consuming this package with other dependencies of Jupyterhub during install.


## Creating ruamel.yaml.clib conda package with the help of feedstock 

S390x is a Big Endian CPU arch and ruamel.yaml.clib has an open bug related to this hence we need to apply a patch and build the ruamel.yaml.clib package with the help of feedstock. 
Please follow the [Build process](https://github.com/manogy/ruamel.yaml.clib-feedstock/blob/main/README.md#build-steps) described in the [README.md](https://github.com/manogy/ruamel.yaml.clib-feedstock/blob/main/README.md)   of  https://github.com/manogy/ruamel.yaml.clib-feedstock


## Setup conda env with all required packages to run Jupyterhub
Please make sure Nodejs is available on the VM. In my VM nodejs version 16 is installed.

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
pip install jupyterhub==4.0.2 --no-cache-dir

```

Now Jupyter Hub has been installed, which can be used by the user. 


### Example:

```
09:06:59  |jupyterhub-test|root@manogya1 ~ → uname 
Linux
09:07:03  |jupyterhub-test|root@manogya1 ~ → uname -m 
s390x
09:07:09  |jupyterhub-test|root@manogya1 ~ → jupyterhub
[I 2024-02-07 09:07:18.364 JupyterHub app:2859] Running JupyterHub version 4.0.2
[I 2024-02-07 09:07:18.364 JupyterHub app:2889] Using Authenticator: jupyterhub.auth.PAMAuthenticator-4.0.2
[I 2024-02-07 09:07:18.364 JupyterHub app:2889] Using Spawner: jupyterhub.spawner.LocalProcessSpawner-4.0.2
[I 2024-02-07 09:07:18.364 JupyterHub app:2889] Using Proxy: jupyterhub.proxy.ConfigurableHTTPProxy-4.0.2
[I 2024-02-07 09:07:18.369 JupyterHub app:1664] Loading cookie_secret from /root/jupyterhub_cookie_secret
[I 2024-02-07 09:07:18.423 JupyterHub proxy:556] Generating new CONFIGPROXY_AUTH_TOKEN
[I 2024-02-07 09:07:18.451 JupyterHub app:1984] Not using allowed_users. Any authenticated user will be allowed.
[I 2024-02-07 09:07:18.463 JupyterHub app:2928] Initialized 0 spawners in 0.001 seconds
[I 2024-02-07 09:07:18.467 JupyterHub metrics:278] Found 0 active users in the last ActiveUserPeriods.twenty_four_hours
[I 2024-02-07 09:07:18.468 JupyterHub metrics:278] Found 0 active users in the last ActiveUserPeriods.seven_days
[I 2024-02-07 09:07:18.468 JupyterHub metrics:278] Found 0 active users in the last ActiveUserPeriods.thirty_days
[W 2024-02-07 09:07:18.468 JupyterHub proxy:625] Found proxy pid file: /root/jupyterhub-proxy.pid
[W 2024-02-07 09:07:18.469 JupyterHub proxy:637] Proxy no longer running at pid=4381
[W 2024-02-07 09:07:18.470 JupyterHub proxy:746] Running JupyterHub without SSL.  I hope there is SSL termination happening somewhere else...
[I 2024-02-07 09:07:18.470 JupyterHub proxy:750] Starting proxy @ http://:8000
09:07:18.741 [ConfigProxy] info: Proxying http://*:8000 to (no default)
09:07:18.746 [ConfigProxy] info: Proxy API at http://127.0.0.1:8001/api/routes


```
