# KFNet- Study

### Install Instructions.
#### Docker setup
Follow this tutorial for installing docker, if not available already.
Run the following commands to install docker.io
'''
sudo apt update
sudo apt install docker.io
sudo systemctl start docker
sudo systemctl enable docker
'''

- Follow the steps to install Nvidia-container-toolkit [here](https://docs.nvidia.com/datacenter/cloud-native/container-toolkit/install-guide.html#docker). If the console output shows the GPU, follow the next steps

- Pull this [docker image ](https://hub.docker.com/r/floydhub/tensorflow/tags?page=2) using this pull command
''' docker pull floydhub/tensorflow:1.11-gpu.cuda9cudnn7-py3_aws.40 '''

- Check if the docker image is present by running 
''' docker image ls '''

- '''docker run -it --rm --gpus all -v $(pwd):/workspace/ floydhub/tensorflow:1.11-gpu.cuda9cudnn7-py3_aws.40 bash'''

Note: -it tag means interactive mode. --rm tag means, the container will be automatically deleted after we exit the command line si closed.

