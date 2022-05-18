# VINS-Mono-docker-tutorial

There is a dockerfile on the original repository, but the explanation is unfriendly for beginners.  

I use another docker image, and dataset.  

## Requirements  
- [docker](https://www.docker.com/)
- [nvidia-docker](https://github.com/NVIDIA/nvidia-docker)  
- [terminator](https://www.geeksforgeeks.org/terminator-a-linux-terminal-emulator/) (High recommended)  

## Dataset  

- [OpenLORIS-Scene Dataset](https://lifelong-robotic-vision.github.io/dataset/scene.html)  

> If you want to use this dataset, you need to fix some bag files.  
> Please refer to [this issue](https://github.com/lifelong-robotic-vision/OpenLORIS-Scene/issues/15).  

**When you make dataset, you have to know where it is (Absolute path will be used).**  

```
@inproceedings{shi2019openlorisscene,
    title={Are We Ready for Service Robots? The {OpenLORIS-Scene} Datasets for Lifelong {SLAM}},
    author={Xuesong Shi and Dongjiang Li and Pengpeng Zhao and Qinbin Tian and Yuxin Tian and Qiwei Long and Chunhao Zhu and Jingwei Song and Fei Qiao and Le Song and Yangquan Guo and Zhigang Wang and Yimin Zhang and Baoxing Qin and Wei Yang and Fangshi Wang and Rosa H. M. Chan and Qi She},
    booktitle={2020 International Conference on Robotics and Automation (ICRA)},
    year={2020},
    pages={3139-3145},
}
```

## Clone VINS-Mono repository  

You could use original [VINS-Mono](https://github.com/HKUST-Aerial-Robotics/VINS-Mono) repository,  
but I make my own [VINS-MONO-studying](https://github.com/Taeyoung96/VINS-MONO-studying).  

I added config file for OpenLORIS-Scene dataset, and change the file format of `vins_result_loop.csv` like **[TUM RGB-D dataset](https://vision.in.tum.de/data/datasets/rgbd-dataset/file_formats)**. 


```
git clone https://github.com/Taeyoung96/VINS-MONO-studying.git  
```
**Also, you should know where it is (Absolute path will be used).**  

## Pull the docker image  

I used [icra2018/vins-mono](https://hub.docker.com/r/icra2018/vins-mono) docker image.  

```
docker pull icra2018/vins-mono
```

When the Docker image pull is completed, the output is as follows when input as shown below.  

```
docker images
```
Output:
```
REPOSITORY           TAG                    IMAGE ID       CREATED        SIZE
icra2018/vins-mono   latest                 1dc54987f8fe   2 years ago    4.95GB
```

## Make your own container  

You have to use your own absolute path, when you run the command below.  

```
nvidia-docker run -it -p 8888:8888 -e DISPLAY -w /home/jovyan/catkin_ws/src -v /tmp/.X11-unix:/tmp/.X11-unix \
-v [Absolute path of Dataset]:/dataset \
-v [Absolute path of VINS-Mono repository]:/home/jovyan/catkin_ws/src/ \
--name vins-mono icra2018/vins-mono:latest /bin/bash
```

When you are done, the terminal will be changed like below.  

```
jovyan@e9663ba248d6:~/catkin_ws/src$ 
```
Type ls and you should see the VINS-Mono folder.  
```
jovyan@e9663ba248d6:~/catkin_ws/src$ ls
VINS-Mono
```

Change the directory and build it.  
```
jovyan@e9663ba248d6:~/catkin_ws/src$ cd ..
jovyan@e9663ba248d6:~/catkin_ws$ catkin_make 
```

When you are done you would see the outputs below.  
```
[  3%] Built target benchmark_publisher
[ 23%] Built target camera_model
[ 44%] Built target Calibration
[ 47%] Built target ar_demo_node
[ 53%] Built target feature_tracker
[ 78%] Built target vins_estimator
[100%] Built target pose_graph
...
```

Then run roscore.  

```
jovyan@e9663ba248d6:~/catkin_ws$ roscore  
```

Now you need to connect to the Docker container using the other 3 terminals.  

- 2nd Terminal : launch vins_estimator     
- 3rd Terminal : launch Rviz  
- 4nd Terminal : play rosbag file  

2nd Terminal :  
```
docker exec -it -w /home/jovyan/catkin_ws/ vins-mono /bin/bash
```
When connecting to a docker container,  
The launch file is in [VINS-Mono-TUM-format](https://github.com/Taeyoung96/VINS-Mono-TUM-format).  
```
roslaunch vins_estimator realsense_color_cafe.launch
```

3rd Terminal :  
```
docker exec -it -w /home/jovyan/catkin_ws/ vins-mono /bin/bash
```
When connecting to a docker container,  
```
xhost +local:docker
roslaunch vins_estimator vins_rviz.launch
```

4nd Terminal :   
```
docker exec -it -w /dataset vins-mono /bin/bash  
```
When connecting to a docker container,  
```
rosbag play cafe1-1_with_imu.bag
```
This bag file was created by myself using the OpenLORIS-Scene dataset.  

## Result  

<p align="center"><img src="https://user-images.githubusercontent.com/41863759/158924202-3696a69c-bffe-44ee-86fd-e8f532f9b0bb.JPG" width = "600" ></p>  

## License  

Same License with original VINS-MONO.  

The source code is released under [GPLv3](http://www.gnu.org/licenses/) license.

We are still working on improving the code reliability. For any technical issues, please contact Tong QIN <tong.qinATconnect.ust.hk> or Peiliang LI <pliapATconnect.ust.hk>.

For commercial inquiries, please contact Shaojie SHEN <eeshaojieATust.hk>

