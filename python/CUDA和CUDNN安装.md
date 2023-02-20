# CUDA 安装
## 下载安装包
首先打开计算机中的 NVIDIA 控制面板，点开左下角的 `系统设置` , 查看组件信息
![image]('resources/imgs/cuda-01.png', "NVIDIA systemInfo")
根据 NVIDIA CUDA 驱动版本号去[CUDA ToolKit 官网]("https://developer.nvidia.com/cuda-toolkit-archive", "CUDA下载")下载对应版本的 CUDA ToolKit
![image]("resources/imgs/cuda-02.png", "选择对应的操作系统信息就可以下载啦")

## 安装
安装的时候，建议更换默认的系统盘路径
![image]("resources/imgs/cuda-03.png", "安装过程")
同意许可
![image]("resources/imgs/cuda-04.png", "安装过程")
选择自定义(也可以选择默认安装所有组件, 这里选自定义主要是为了查看要安装的组件是些什么)
![image]("resources/imgs/cuda-05.png", "安装过程")
默认是全选的，看有什么不需要的就勾掉，因为现在物理磁盘容量都很大，建议全部安装
![image]("resources/imgs/cuda-06.png", "安装组件")
默认安装位置是在"C:\\Program Files" 目录下，可以自定义到其他目录
如果选择了集成visual studio的组件安装，安装完成后会有如下信息界面:
![image]("resources/imgs/cuda-07.png", "visual studio集成信息")
![image]("resources/imgs/cuda-08.png", "安装完成信息")

## 环境变量配置
安装完成，系统环境变量会多出两个值: CUDA_PATH 和 CUDA_PATH_V11_7, 同样path中也添加了对应的值
![image]("resources/imgs/cuda-09.png", "CUDA_HOME")

可以进入到 `%CUDA_PATH%\extras\demo_suite` 目录下执行 bandwidthTest 和 deviceQuery , 查看结果是否都是 `PASS`
![image]("resources/imgs/cuda-10.png", "test")



