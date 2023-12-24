https://www.docker.com/blog/wsl-2-gpu-support-for-docker-desktop-on-nvidia-gpus/

Draw fractals
The project at https://github.com/jameswmccarty/CUDA-Fractal-Flames uses CUDA for generating fractals. There are two steps to build and run on Linux. Let’s see if we can have it running on Docker Desktop. A simple Dockerfile with nothing fancy helps for that.

# syntax = docker/dockerfile:1.3-labs
FROM nvidia/cuda:11.4.2-base-ubuntu20.04
RUN apt -y update
RUN DEBIAN_FRONTEND=noninteractive apt -yq install git nano libtiff-dev cuda-toolkit-11-4
RUN git clone --depth 1 https://github.com/jameswmccarty/CUDA-Fractal-Flames /src
WORKDIR /src
RUN sed 's/4736/1024/' -i fractal_cuda.cu # Make the generated image smaller
RUN make
And then we can build and run:

$ docker build . -t cudafractal
$ docker run --gpus=all -ti --rm -v ${PWD}:/tmp/ cudafractal ./fractal -n 15 -c test.coeff -m -15 -M 15 -l -15 -L 15
Note that the --gpus=allis only available to the run command. It’s not possible to add GPU intensive steps during the build.