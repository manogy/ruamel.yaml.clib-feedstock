
Introduction 
================================

Because of Endiannes issues, in Big endian CPU architectures like s390x  ruamel.yaml.clib throws the below error 

```
      File "_ruamel_yaml.pyx", line 701, in ruamel.yaml.clib._ruamel_yaml.CParser.get_single_node
      File "_ruamel_yaml.pyx", line 904, in ruamel.yaml.clib._ruamel_yaml.CParser._parse_next_event
      ruamel.yaml.reader.ReaderError: unacceptable character #x00b0: invalid leading UTF-8 octet
```


This Repo is created with help of  ruamel.yaml.clib-feedstock from conda forge and https://src.fedoraproject.org/rpms/python-ruamel-yaml-clib.
As the later one has Big endian support we have included it as a patch in the feedstock.




Build Steps:
================================


1.  Clone the repo https://github.com/manogy/ruamel.yaml.clib-feedstock.git
      ```
      git clone https://github.com/manogy/ruamel.yaml.clib-feedstock.git
      ```

2.  change the directory to recipe.
    ```
    cd ruamel.yaml.clib-feedstock/recipe
    ```
3.  
    Make sure you are in the base environment of the conda unless you want to use a specific conda environment. 
    If conda-build package is not installed please install it with the below command.
    ```
    conda install conda-build
    ```
4.  To build the conda package run the below command (from the recipe folder).

    ```
    conda build .
    ```

5.  Above command will start the build of the package, once it completes it will generate the last lines of the log similar to the below lines

    ```
        # Automatic uploading is disabled
        # If you want to upload package(s) to anaconda.org later, type:


        # To have conda build upload to anaconda.org automatically, use
        # conda config --set anaconda_upload yes
        anaconda upload \
            /root/miniconda3/conda-bld/linux-s390x/ruamel.yaml.clib-0.2.7-py310ha92ec59_2.tar.bz2
        anaconda_upload is not set.  Not uploading wheels: []

        INFO :: The inputs making up the hashes for the built packages are as follows:
        {
        "ruamel.yaml.clib-0.2.7-py310ha92ec59_2.tar.bz2": {
            "recipe": {
            "c_compiler": "gcc",
            "target_platform": "linux-s390x"
            }
        }
        }


        ####################################################################################
        Resource usage summary:

        Total time: 0:00:53.9
        CPU usage: sys=0:00:00.2, user=0:00:05.3
        Maximum memory usage observed: 200.2M
        Total disk usage observed (not including envs): 2.7K
    ```

Installing ruamel.yaml.clib in your desired conda environment
=============================================================
To Install the working ruamel.yaml.clib you need to run the below command. 

    ```
    conda install ruamel.yaml.clib  -c  /root/miniconda3/conda-bld
    ```
Here /root/miniconda3/conda-bld is the name of channel where ruamel.yaml.clib was created. This name is taken from the last lines of build logs.
