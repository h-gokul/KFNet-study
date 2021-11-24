# KFNet- Study
This is a study project on Bayesian State estimation in a deep learning framework. KFNet estimates scene coordinates aka. point clouds using a measurement + process systems and  uses a kalman filter formulation to refine the estimated point clouds. This repository is adapted from this [main codebase](https://github.com/zlthinker/KFNet).

You can either follow the below evaluation procedure to predict the point clouds, or you can download the point clouds that we have already obtained [here](NEED TO PROVIDE LINK)  and skip to the visualization step.

## Predict the point clouds:
  The codebase requires CUDA 9.0 cudnn 7 and tensorflow 1.10-gpu. These are not the latest versions and we have built a docker container that covers all of these required dependancies. For this, however, you need docker to be setup in your system, and its install procedure is explained below/ 
  ### Docker setup:
  
  Follow this tutorial for installing docker, if not installed already. Otherwise skip to the next step.
  Run the following commands to install docker.io
  ```
  sudo apt update
  sudo apt install docker.io
  sudo systemctl start docker
  sudo systemctl enable docker
  ```
  Follow the steps to install Nvidia-container-toolkit [here](https://docs.nvidia.com/datacenter/cloud-native/container-toolkit/install-guide.html#docker). If the console output shows the GPU, contine to the next steps.

  ### Obtaining inference

  Download our [docker image ](NEED TO PROVIDE LINK) 

  - run ``` docker load < kfnetv1.tar.gz ```

  - Check if the docker image is present by running 

  ``` docker image ls ```

  - ```docker run -it --rm --gpus all -v $(pwd):/workspace/ gokulkfnet/cuda9-cudnn7-tf1.10-gpu:v1 bash```

  Note: -it tag means interactive mode. --rm tag means, the container will be automatically deleted after we exit the command line si closed.

  In the docker terminal, go to workspace folder ``` cd ../workspace```

  - Download models from [here](https://drive.google.com/file/d/13KZGz_akJw8iTQW90pgbuw2JAQzV7cG8/view) and paste in ./models folder. Also refer main [repository](https://github.com/zlthinker/KFNet#testing)
  - Download the prepared labels and images as mentioned [here](https://github.com/zlthinker/KFNet#usage) and paste in ./inputs folder

  To predict scene coordinates, 
  - run 
  
    `python KFNet/eval.py --input_folder <input folder> --output_folder <output folder> --model_folder ./models/KFNet/$scene$ --scene $scene$`
  
  Example:
    `python KFNet/eval.py --input_folder ./input --output_folder ./output/fire --model_folder ./models/KFNet/fire --scene fire`
  
  
  Note: this will save the process, measured and filtered scene coordinates.

  To predict optical flow, 
  
  - run 
     `python OFlowNet/eval.py  <input folder> --output_folder <output folder> --model_folder ./models/OFlowNet`
  
  Example:
    `python OFlowNet/eval.py --input_folder ./input --output_folder ./output/fire --model_folder ./models/OFlowNet --scene fire`


## Point Clouds/ Optical Flow Visualization. 

  The jupyter notebook we use here for visualization is written in Python3.7 and requires Open3D( for point cloud visualization) and OpenCV. you can install this by running the below command. 
  
  `pip install -r requirements.txt`
  
  Having all dependancies installed, 
  - check `PointCloudVisualization.ipynb` to visualize the point clouds.
  - Check `OpticalFlowVisualization.ipynb` to visualize the optical flow

## PnP RANSAC + NonLinear Optimization
  Having estimated the point clouds, we can solve for the camera poses using the Perspective-n-Point method with RANSAC and refinement by Non Linear Optimization, which uses a reprojection error based constraint. The dependancies for this section involve a different docker container using Ubuntu 20.04 which we provide [here](https://drive.google.com/drive/folders/1bBZqLPaWx7rn4LpaXmskTNUGgvuJthab?usp=sharing.  Follow the below steps to obtain the pose estimates.

  - Download the above link and unzip all the files.
  - run ``` docker load < PnP_Docker.tar```
  - ``` docker image ls ```
  - ```docker run -it --rm --gpus all -v $(pwd):/workspace/ ubuntu/ubuntu:20.04 bash```
  - unzip PnP.zip && cd PnP
  - unzip PnP_Files.zip
  - Place pnp_input and pnp_new in the main repo.
  - cd pnp-new 

To get camera poses, 
  - run 
  
    `python main.py <path_to_output_file_list> <output_folder> --gt <path_to_ground_truth_pose_list> --thread <32>`
  
  Example:
    `python main.py ../pnp-input/fire/cord_list.txt ./pnp-output/ --gt ../pnp-input/fire/gt_pose_list.txt --thread 32`
  




