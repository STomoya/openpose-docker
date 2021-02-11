# openpose-docker

My Docker image for [OpenPose](https://github.com/CMU-Perceptual-Computing-Lab/openpose).

The original `Dockerfile` is from [here](https://github.com/myoshimi/openpose-docker).

## Changes from orginal

- base image to `nvidia/cuda:11.1.1-cudnn8-devel-ubuntu20.04`.

- install `cmake` via `apt`.

- `qtbase5-dev` not installed.

## Enviornment

Only critical ones.

- Hardware

    ```text
    GPU : GeForce RTX 2060 SUPER
    ```
    Turing architecture.

- OS

    ```text
    Ubuntu 20.04.2 LTS
    ```

- nvidia-driver

    ```text
    nvidia-driver-460
    ```

## Usage

- Build image and wait.

    This will take some time...

    ```console
    docker-compose build
    ```

- Add volumes to `docker-compose.yml`

- Exec

    In terminal.

    ```console
    docker-compose up -d
    docker-compose exec openpose bash
    ```

    In container.  
    `--display 0` is needed.  
    See the docs for the options.

    ```console
    openpose.bin --display 0 <other options>
    ```

- or Run

    `--display 0` is needed.  
    See the docs for the options.

    ```console
    docker-compose run --rm openpose openpose.bin --display 0 <other options>
    ```

## Problems I came across

<details><summary>template</summary><div>

```console
message
```

When.  
Why.  
How to fix.  
What I did.

</div></details>
  
---

```console
nvcc fatal   : Unsupported gpu architecture 'compute_80'
```

Occurred when building the docker image.  
Error by nvidia-toolkit version not compatible with some GPU architectures.  
Use a higher version.  
Changed base image to use CUDA11.1.1 from CUDA10.0.

---

```console
CMake Error: The following variables are used in this project, but they are set to NOTFOUND.
Please set them or make sure they are set and tested correctly in the CMake files:
        CUDA_cublas_device_LIBRARY (ADVANCED)
```

Occurred when building the docker image.  
Error by cmake with versions between 3.8 to 3.11. This is also mentioned in the [official docs](https://cmu-perceptual-computing-lab.github.io/openpose/web/html/doc/md_doc_05_faq.html#cuda_cublas_device_library-not-found).  
Update cmake to >=3.12 or use ubuntu20.04 which defaults cmake to 3.16.  
Changed base image to use ubuntu20.04 from ubuntu18.04

---

```console
Cuda check failed (804 vs. 0): forward compatibility was attempted on non supported HW
```

Occurred when running `openpose.bin` inside the container.  
Error by mismatch of `nvidia-driver` and the installed CUDA version.  
Update to a driver that fits the installed CUDA version.  
Reinstalled `nvidia-driver` to 460 from 450 on my local device.

---

```console
Unable to init server: Could not connect: Connection refused
```

Occurred when running `openpose.bin` inside the container.  
Error because trying to use GUI stuff inside the container.  
Disable GUI.  
Pass `--display 0` option when running.

---

A lot of Warnings when building `caffe` and `openpose`

Occurred when building docker image.  
Warnings because building for every known GPU architectures including deprecated ones.  
It still works.  
Docker cannot access to GPU devices when build. Tried to specify it but did not work.
