## Introduction
本项目旨在基于ReChorus框架实现MGDCF模型的复现。
MGDCF模型是一种基于图神经网络的推荐系统模型，其主要思想是将用户的和项目之间的关系建模为图，并通过图神经网络学习用户的兴趣图表示，从而预测用户对项目的喜好程度。
MGDCF通过应用马尔科夫图神经网络（MGDN）和 InfoBPR 损失函数，可以有效地捕捉用户和项目之间的复杂关系，并学习到用户的兴趣图表示，提升了原有模型的性能。
## code
本项目基于ReChorus框架实现了MGDCF模型的复现。
`MGDCF.py`文件位于`ReChorus/src/models/general`目录下，实现了MGDCF模型的初始化、数据编码、前向传播、损失函数计算等功能。
`MGDCFBase`类实现了MGDCF模型的基本结构，包括邻接矩阵、关系矩阵等，并通过`MGDCFEncoder`类实现对数据的编码处理。
`MGDCF`类继承自`MGDCFBase`类，其中的`loss`函数通过InfoBPR损失和l2损失来实现模型损失的计算。 
`MGDCFEncoder`类实现了对用户和项目的特征编码，包括用户特征编码、项目特征编码等。通过运用马尔科夫图神经网络（MGDN）提升了模型的性能。
可以直接下载本项目的`ReChorus`文件夹，并运行我们下面给出的代码，即可复现MGDCF模型。也可以只下载`MGDCF.py`文件，将其放在下载好的ReChorus框架中（具体位置为`ReChorus/src/models/general`），然后运行我们下面给出的代码。
## requirement
```
python == 3.10.4
torch == 2.5.0+cu118
numpy == 1.22.0
ipython == 8.10.0
jupyter == 1.0.0
tqdm == 4.66.1
pandas == 1.4.4
scikit-learn == 1.1.3
scipy == 1.7.3
```
## training
由于硬件条件受限，我们仅在cpu环境下进行训练。如果有gpu环境，可以将 `--gpu`后面的参数设为0，使用gpu进行训练。
训练的数据位于 `ReChorus/data`，我们提供两个数据集进行TOP-K训练任务，分别是`Grocery_and_Gourmet_Food`和`MIND_Large/MINDTOPK`。输出放在`ReChorus/log`目录下。
下面是我们对不同模型的训练命令：
```cd src```<br>
对于`Grocery_and_Gourmet_Food`数据集：<br>
MGDCF:```python main.py  --gpu -1 --model_name MGDCF  --emb_size 64 --lr 1e-3 --l2 1e-6 --dataset Grocery_and_Gourmet_Food  --alpha 0.1 --beta 0.9 --n_layers 3```<br>
LightGCN:```python main.py --gpu -1 --model_name LightGCN --emb_size 64 --n_layers 3 --lr 1e-3 --l2 1e-8 --dataset 'Grocery_and_Gourmet_Food'```<br>
BPRMF:```python main.py --gpu -1 --model_name BPRMF --emb_size 64 --lr 1e-3 --l2 1e-6 --dataset 'Grocery_and_Gourmet_Food'```<br>
对于`MIND_Large/MINDTOPK`数据集：<br>
MGDCF:```python main.py  --gpu -1 --model_name MGDCF  --emb_size 64 --lr 1e-3 --l2 1e-6 --dataset MIND_Large\MINDTOPK  --alpha 0.1 --beta 0.9 --n_layers 2 ```<br>
LightGCN:```python main.py --gpu -1 --model_name LightGCN --emb_size 64 --n_layers 3 --lr 1e-3 --l2 1e-8 --dataset MIND_Large\MINDTOPK```<br>
BPRMF:```python main.py --gpu -1 --model_name BPRMF --emb_size 64 --lr 1e-3 --l2 1e-6 --dataset MIND_Large\MINDTOPK```<br>
所有训练均在Windows环境下进行，训练时间较长，请耐心等待。
## parameter
可能要用到的参数：
gpu $\Rightarrow$ gpu的编号: 使用该编号的gpu进行训练
model_name $\Rightarrow$ 模型名称: 使用该模型进行训练
dataset $\Rightarrow$ 数据集名称: 使用该数据集进行训练
lr $\Rightarrow$ 学习率: 学习率
l2 $\Rightarrow$ l2正则化系数: l2正则化系数
n_layers $\Rightarrow$ 图神经网络层数: 图神经网络层数
alpha $\Rightarrow$ $\alpha$系数: 仅在MGDCF模型中使用
beta $\Rightarrow$ $\beta$系数: 仅在MGDCF模型中使用

---------------------------------------------------------
ReChorus框架代码地址：```https://github.com/THUwangcy/ReChorus```
如果在运行代码过程中遇到什么问题，请在企业微信上联系我们。