# VINS-mono-docker-tutorial

There is a dockerfile on the original repository, but the explanation is unfriendly for beginners.  

I use another docker image, and I make my own tutorial video, just follow it.  

## Requirements  
- [docker](https://www.docker.com/)
- [nvidia-docker](https://github.com/NVIDIA/nvidia-docker)  
- [terminator](https://www.geeksforgeeks.org/terminator-a-linux-terminal-emulator/) (High recommended)  

## Dataset  

- [OpenLORIS-Scene Dataset](https://lifelong-robotic-vision.github.io/dataset/scene.html)  

> If you want to use this dataset, you need to fix some bag files.  
> Please refer to [this issue](https://github.com/lifelong-robotic-vision/OpenLORIS-Scene/issues/15).  

When you make dataset, you have to know where it is (Absolute path will be used).  

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
but I make my own [VINS-Mono-TUM-format](https://github.com/Taeyoung96/VINS-Mono-TUM-format).  

I added config file for OpenLORIS-Scene dataset, and change the file format of `vins_result_loop.csv` like **[TUM RGB-D dataset](https://vision.in.tum.de/data/datasets/rgbd-dataset/file_formats)**. 
