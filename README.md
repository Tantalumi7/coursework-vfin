##MedSAM腹部CT分割：集成自适应增强与图形用户界面##
本项目是一个基于 MedSAM (Medical Segment Anything Model) 的腹部CT影像器官分割的完整实现方案。项目复现了 MedSAM 的核心功能，同时引入了创新的数据增强策略，并开发了一套功能完备的图形用户界面（GUI）以实现交互式分割。

##核心贡献
本项目的关键创新与贡献主要体现在以下三个方面：
1、自适应边界框增强策略：
在模型训练脚本 train_update.py 中，我们设计并实现了一种自适应的边界框（Bounding Box）扰动算法。该算法摒弃了固定的像素偏移，转而根据目标器官的尺寸、纵横比等几何特征动态计算扰动幅度。这一策略旨在模拟真实应用中用户可能提供的不精确提示，从而显著提升模型对提示框精确度的鲁棒性。
2、交互式分割图形用户界面 (GUI)：
为了提升模型的实用性与易用性，我们使用 PyQt5 开发了一套图形用户界面（gui.py）。用户可通过该界面加载医学影像切片、通过鼠标拖拽交互式地绘制边界框，并实时获取和可视化模型的分割结果。此外，该工具还支持多标签色彩渲染、分割掩码（Mask）的保存以及撤销操作。
3、自动化数据预处理流程：
项目包含一个高度自动化的数据预处理脚本 (pre_CT_MR.py)，负责将原始的 FLARE22 数据集从 .nii.gz 格式转换为模型训练所需的 .npy 切片格式。该流程集成了CT窗宽窗位调整、无关解剖学标签的移除，以及基于3D和2D连通域分析的噪声过滤，确保了训练数据的高质量与一致性。

##项目结构
MedSAM_v202510/
├── assets/                  # 存放示例图片等静态资源(使用公开数据集，未上传至仓库)
├── gui.py                   # 交互式分割图形界面 (GUI)
├── MedSAM_Inference.py      # 用于单张图像推理的脚本
├── npytopng_grey.py         # 将 .npy 格式数据转换为 .png 图像的工具
├── pre_CT_MR.py             # 自动化数据预处理脚本
├── setup.py                 # 项目安装配置文件
├── train_update.py          # 集成自适应增强的模型训练脚本
├── .gitignore               # 定义Git版本控制的忽略规则
├── requirements.txt         # 项目的Python依赖库列表
└── README.md                # 本项目说明文档

##环境配置与安装
先决条件：
支持 CUDA 的 NVIDIA GPU
Git
Conda
Python 3.12

安装步骤：
1. 克隆仓库：
git clone https://github.com/Tantulumi7/MedSAM_v202510.git
cd MedSAM_v202510
2. 创建并激活虚拟环境：
conda create -n MedSAM python=3.12
conda activate MedSAM
3. 安装依赖：
pip install -r requirements.txt

运行流程：
步骤一：数据与模型准备
1、下载数据集：从 FLARE22 挑战赛官网下载公开数据集。
2、数据存放：解压数据集，将图像和标签分别存放于 data/FLARE22Train/images 和 data/FLARE22Train/labels 目录。
3、下载预训练模型：从 MedSAM 官方 GitHub 仓库下载预训练权重文件 medsam_vit_b.pth。
4、模型存放：创建 work_dir/MedSAM/ 目录，并将下载的 .pth 模型文件放入其中
步骤二：数据预处理
1、运行 pre_CT_MR.py 脚本，进行数据预处理。
步骤三：模型训练
1、运行 train_update.py 脚本，开始模型训练。
步骤四：交互式分割
1、运行 gui.py 脚本，启动图形用户界面进行交互式分割。

