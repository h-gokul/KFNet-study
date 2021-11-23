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

- Download this [docker image ](NEED TO PROVIDE LINK) 

- run ``` docker load < kfnetv1.tar.gz ```

- Check if the docker image is present by running 

``` docker image ls ```

- ```docker run -it --rm --gpus all -v $(pwd):/workspace/ gokulkfnet/cuda9-cudnn7-tf1.10-gpu:v1 bash```

Note: -it tag means interactive mode. --rm tag means, the container will be automatically deleted after we exit the command line si closed.

In the docker terminal, go to workspace folder ``` cd ../workspace```


run ```python KFNet/eval.py --input_folder ./input --output_folder ./output/fire --model_folder ./models/KFNet/fire --scene fire```

run ```python OFlowNet/eval.py --input_folder ./input --output_folder ./output/fire --model_folder ./models/OFlowNet --scene fire```