## Set of Podman and Singularity definition files to build container images

> Singularity is a container solution created for scientific and application driven workloads. Singularity containers can be used to package entire scientific workflows, software and libraries, and even data. This means that you don’t have to ask your cluster admin to install anything for you - you can put it in a Singularity container and run.

> Podman is a daemonless container engine for developing, managing, and running OCI Containers on your Linux System.

Both container engines support the reusage of docker images.

### Singularity

When using singularity as runtime, be sure to have created and set predefined folders for building and pulling:

```
[create these folders]


export SINGULARITY_TMPDIR=${HOME}/.singularity/tmp
export SINGULARITY_LOCALCACHEDIR=${HOME}/.singularity/local_cache
```

#### keras + tensorflow

1. Move to the definiton file (`definiton.def`) folder:

`cd ml_keras_tf`

2. Build the image:

`singularity build --fakeroot ml_keras_tf.sif definition.def`

3. Once it's ready, run the container:

*Example: running container with default runscript (in this case jupyter notebook)*

`singularity run --nv  ml_keras_tf.sif

*Example: running container with a custom command*

`singularity exec --nv ml_keras_tf.sif jupyter notebook --no-browser`

> NOTE: By default singularity binds the directories `$HOME`, `/tmp`, `/proc`, `/sys`, and `/dev`. So that you can modify the files wher your permissions (as user) apply.

*Example: running a command from inside the container --in shell mode--*

`singularity shell --nv --bind data:/data ml_keras_tf.sif`
`jupyter notebook --no-browser`

This will open a shell inside the container. Additionally, we can add extra bindings using `--bind`. In this example, the folder `data` in our current working directory is bind to the folder `/data` in the containers file system.

> For more information refer to singularity user's guide https://sylabs.io/guides/3.4/user-guide/quick_start.html

### Modifying the container image

You can do so by: mounting the container in `sandbox` mode and rebuilding once it is ready; using the `writable` mode; or by modifying the `definition.def` file and rebuilding the image.

> For more information refer to singularity user's guide https://sylabs.io/guides/3.4/user-guide/quick_start.html

