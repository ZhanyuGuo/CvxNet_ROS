# CvxNet Readme

## 环境配置

> 最高只支持到CUDA 11.2, 如果有更高版本的CUDA, 使用低版本的Docker

1. 下载代码

    ```bash
    git clone https://github.com/tensorflow/graphics.git
    ```

### Conda

1. 安装[anaconda](https://www.anaconda.com/download/success)

2. 创建虚拟环境

    ```bash
    conda create -n cvxnet python=3.7
    conda activate cvxnet
    ```

3. 安装依赖

    ```bash
    cd graphics/tensorflow_graphics/projects/cvxnet
    pip install -r requirements.txt
    python ./setup.py build_ext --inplace
    ```

### Docker

1. 安装[Docker](https://docs.docker.com/engine/install/)

2. 下载最高支持版本CUDA对应的tensorflow docker并启动

    ```bash
    sudo docker pull tensorflow/tensorflow:2.11.0-gpu

    sudo docker run --gpus all -itd -v ~/Workspace/graphics:/graphics tensorflow/tensorflow:2.11.0-gpu bash

    sudo docker ps -a
    sudo docker exec -it <container_name> bash
    ```

3. 安装依赖

    ```bash
    cd /graphics/tensorflow_graphics/projects/cvxnet
    pip install -r requirements.txt
    python ./setup.py build_ext --inplace
    ```

## 运行

1. 下载数据集

    ```bash
    cd graphics/tensorflow_graphics/projects/cvxnet
    wget cvxnet.github.io/data/sample_dataset.zip
    unzip sample_dataset.zip -d ./datasets
    rm sample_dataset.zip
    ```

2. 训练

    在`train.py`加入路径
    
    ```python
    import sys
    import os
    file_path = os.path.dirname(os.path.abspath(__file__))
    root_path = os.path.abspath(os.path.join(file_path, '../../..'))
    sys.path.append(root_path)
    print(sys.path)
    ```

    ```bash
    # 2d input
    python train.py --train_dir=./models/rgb --data_dir=./datasets/sample_dataset --image_input

    # 3d input
    python train.py --train_dir=/tmp/cvxnet/models/depth --data_dir=/tmp/cvxnet/sample_dataset --n_half_planes=50
    ```

    使用tensorboard可视化训练过程

    ```bash
    tensorboard --logdir=./models
    ```

3. 测试

    ```bash
    # 2d input
    python eval.py --train_dir=./models/rgb --data_dir=./datasets/sample_dataset --image_input

    # 3d input
    python eval.py --train_dir=./models/depth --data_dir=./datasets/sample_dataset --n_half_planes=50
    ```

## TODO

- [ ] 数据集? 开源的? 自己的? 什么形式?
  
- [ ] 输入输出是什么?

- [ ] 如何转成C++在ROS中使用?