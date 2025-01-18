# Context Aggregation Network

[![arXiv](https://badgen.net/badge/arXiv/2111.11057/red?cache=300)](https://arxiv.org/abs/2111.11057)
[![License](https://badgen.net/github/license/yeliudev/CATNet?label=License&color=cyan&cache=300)](https://github.com/yeliudev/CATNet/blob/main/LICENSE)

本存储库维护了论文《遥感图像实例分割：聚焦多尺度上下文聚合方法》的官方实现代码。

<p align="center"><img width="850" src="https://raw.githubusercontent.com/yeliudev/CATNet/main/.github/model.svg"></p>

### 安装

 请参考我们所使用的以下环境设置。如果在自动安装过程中遇到任何问题，你可以自行安装这些软件包。 

- CUDA 11.8
- CUDNN 8.7.0.84
- Python 3.11.3
- PyTorch 2.0.1
- [MMEngine](https://github.com/open-mmlab/mmengine) 0.7.4
- [MMCV](https://github.com/open-mmlab/mmcv) 2.0.0
- [MMDetection](https://github.com/open-mmlab/mmdetection) 3.0.0
- [NNCore](https://github.com/yeliudev/nncore) 0.3.6

### 从源代码安装

1. 从 GitHub 克隆存储库。

```
git clone https://github.com/yeliudev/CATNet.git
cd CATNet
```

2. 安装依赖项。

```
pip install -r requirements.txt
```

3. 设置环境变量。

```
export PYTHONPATH=$PWD:$PYTHONPATH
```

## 入门指南

### 下载并准备数据集

1. 下载预处理后的数据集。

- [iSAID](https://huggingface.co/yeliudev/CATNet/resolve/main/datasets/isaid-9d62d4ad.zip)
- [DIOR](https://huggingface.co/yeliudev/CATNet/resolve/main/datasets/dior-b162132d.zip)
- [NWPU VHR-10](https://huggingface.co/yeliudev/CATNet/resolve/main/datasets/vhr-79ccc9f3.zip)
- [HRSID](https://huggingface.co/yeliudev/CATNet/resolve/main/datasets/hrsid-4e02052e.zip)

2. 按照以下结构准备文件。

```
CATNet
├── configs
├── datasets
├── models
├── tools
├── data
│   ├── dior
│   │   ├── Annotations
│   │   ├── ImageSets
│   │   └── JPEGImages
│   ├── hrsid
│   │   ├── annotations
│   │   └── images
│   ├── isaid
│   │   ├── train
│   │   ├── val
│   │   └── test
│   └── vhr
│       ├── annotations
│       └── images
├── README.md
├── setup.cfg
└── ···
```

### 训练模型

运行以下命令使用指定的配置训练模型。

```
mim train mmdet <path-to-config> --gpus 4 --launcher pytorch
```

> 如果在 iSAID 数据集上出现 “内存不足” 错误，请取消注释数据集代码中的 [L22-L24](https://github.com/yeliudev/CATNet/blob/main/datasets/isaid.py#L22:L24) 并再次尝试。这将过滤掉一些包含超过 1000 个对象的图像，大大降低内存成本。

### 测试模型并评估结果

运行以下命令测试模型并评估结果。

```
mim test mmdet <path-to-config> --checkpoint <path-to-checkpoint> --gpus 4 --launcher pytorch
```

## 模型库

我们在此提供多个预训练模型。所有模型均使用 4 个 NVIDIA A100 GPU 进行训练，并使用数据集的默认指标进行评估。

<table>
  <tr>
    <th rowspan="2">Dataset</th>
    <th rowspan="2">Model</th>
    <th rowspan="2">Backbone</th>
    <th rowspan="2">Schd</th>
    <th rowspan="2">Aug</th>
    <th colspan="2">Performance</th>
    <th rowspan="2">Download</th>
  </tr>
  <tr>
    <th>BBox AP</th>
    <th>Mask AP</th>
  </tr>
  <tr>
    <td align="center" rowspan="2">
      <a href="https://arxiv.org/abs/1905.12886">iSAID</a>
    </td>
    <td align="center">
      <a href="https://github.com/yeliudev/CATNet/blob/main/configs/isaid/cat_mask_rcnn_r50_3x_isaid.py">CAT Mask R-CNN</a>
    </td>
    <td align="center">ResNet-50</td>
    <td align="center">3x</td>
    <td align="center">&cross;</td>
    <td align="center">45.1</td>
    <td align="center">37.2</td>
    <td align="center">
      <a href="https://huggingface.co/yeliudev/CATNet/resolve/main/checkpoints/cat_mask_rcnn_r50_3x_isaid-384df911.pth">model</a> |
      <a href="https://huggingface.co/yeliudev/CATNet/resolve/main/checkpoints/cat_mask_rcnn_r50_3x_isaid.json">metrics</a>
    </td>
  </tr>
  <tr>
    <td align="center">
      <a href="https://github.com/yeliudev/CATNet/blob/main/configs/isaid/cat_mask_rcnn_r50_aug_3x_isaid.py">CAT Mask R-CNN</a>
    </td>
    <td align="center">ResNet-50</td>
    <td align="center">3x</td>
    <td align="center">&check;</td>
    <td align="center">47.7</td>
    <td align="center">39.2</td>
    <td align="center">
      <a href="https://huggingface.co/yeliudev/CATNet/resolve/main/checkpoints/cat_mask_rcnn_r50_aug_3x_isaid-1e5351dd.pth">model</a> |
      <a href="https://huggingface.co/yeliudev/CATNet/resolve/main/checkpoints/cat_mask_rcnn_r50_aug_3x_isaid.json">metrics</a>
    </td>
  </tr>
  <tr>
    <td align="center" rowspan="4">
      <a href="https://arxiv.org/abs/1909.00133">DIOR</a>
    </td>
    <td align="center">
      <a href="https://github.com/yeliudev/CATNet/blob/main/configs/dior/catnet_r50_3x_dior.py">CATNet</a>
    </td>
    <td align="center">ResNet-50</td>
    <td align="center">3x</td>
    <td align="center">&cross;</td>
    <td align="center">74.0</td>
    <td align="center">—</td>
    <td align="center">
      <a href="https://huggingface.co/yeliudev/CATNet/resolve/main/checkpoints/catnet_r50_3x_dior-5cb86542.pth">model</a> |
      <a href="https://huggingface.co/yeliudev/CATNet/resolve/main/checkpoints/catnet_r50_3x_dior.json">metrics</a>
    </td>
  </tr>
  <tr>
    <td align="center">
      <a href="https://github.com/yeliudev/CATNet/blob/main/configs/dior/catnet_r50_aug_3x_dior.py">CATNet</a>
    </td>
    <td align="center">ResNet-50</td>
    <td align="center">3x</td>
    <td align="center">&check;</td>
    <td align="center">78.2</td>
    <td align="center">—</td>
    <td align="center">
      <a href="https://huggingface.co/yeliudev/CATNet/resolve/main/checkpoints/catnet_r50_aug_3x_dior-6ec5fae1.pth">model</a> |
      <a href="https://huggingface.co/yeliudev/CATNet/resolve/main/checkpoints/catnet_r50_aug_3x_dior.json">metrics</a>
    </td>
  </tr>
  <tr>
    <td align="center">
      <a href="https://github.com/yeliudev/CATNet/blob/main/configs/dior/cat_rcnn_r50_3x_dior.py">CAT R-CNN</a>
    </td>
    <td align="center">ResNet-50</td>
    <td align="center">3x</td>
    <td align="center">&cross;</td>
    <td align="center">75.8</td>
    <td align="center">—</td>
    <td align="center">
      <a href="https://huggingface.co/yeliudev/CATNet/resolve/main/checkpoints/cat_rcnn_r50_3x_dior-044be4c7.pth">model</a> |
      <a href="https://huggingface.co/yeliudev/CATNet/resolve/main/checkpoints/cat_rcnn_r50_3x_dior.json">metrics</a>
    </td>
  </tr>
  <tr>
    <td align="center">
      <a href="https://github.com/yeliudev/CATNet/blob/main/configs/dior/cat_rcnn_r50_aug_3x_dior.py">CAT R-CNN</a>
    </td>
    <td align="center">ResNet-50</td>
    <td align="center">3x</td>
    <td align="center">&check;</td>
    <td align="center">80.6</td>
    <td align="center">—</td>
    <td align="center">
      <a href="https://huggingface.co/yeliudev/CATNet/resolve/main/checkpoints/cat_rcnn_r50_aug_3x_dior-89845304.pth">model</a> |
      <a href="https://huggingface.co/yeliudev/CATNet/resolve/main/checkpoints/cat_rcnn_r50_aug_3x_dior.json">metrics</a>
    </td>
  </tr>
  <tr>
    <td align="center" rowspan="2">
      <a href="https://doi.org/10.1016/j.isprsjprs.2014.10.002">NWPU<br>VHR-10</a>
    </td>
    <td align="center">
      <a href="https://github.com/yeliudev/CATNet/blob/main/configs/vhr/cat_mask_rcnn_r50_6x_vhr.py">CAT Mask R-CNN</a>
    </td>
    <td align="center">ResNet-50</td>
    <td align="center">6x</td>
    <td align="center">&cross;</td>
    <td align="center">71.0</td>
    <td align="center">69.3</td>
    <td align="center">
      <a href="https://huggingface.co/yeliudev/CATNet/resolve/main/checkpoints/cat_mask_rcnn_r50_6x_vhr-d38af93b.pth">model</a> |
      <a href="https://huggingface.co/yeliudev/CATNet/resolve/main/checkpoints/cat_mask_rcnn_r50_6x_vhr.json">metrics</a>
    </td>
  </tr>
  <tr>
    <td align="center">
      <a href="https://github.com/yeliudev/CATNet/blob/main/configs/vhr/cat_mask_rcnn_r50_aug_6x_vhr.py">CAT Mask R-CNN</a>
    </td>
    <td align="center">ResNet-50</td>
    <td align="center">6x</td>
    <td align="center">&check;</td>
    <td align="center">72.4</td>
    <td align="center">70.7</td>
    <td align="center">
      <a href="https://huggingface.co/yeliudev/CATNet/resolve/main/checkpoints/cat_mask_rcnn_r50_aug_6x_vhr-599b2304.pth">model</a> |
      <a href="https://huggingface.co/yeliudev/CATNet/resolve/main/checkpoints/cat_mask_rcnn_r50_aug_6x_vhr.json">metrics</a>
    </td>
  </tr>
  <tr>
    <td align="center" rowspan="2">
      <a href="https://doi.org/10.1109/access.2020.3005861">HRSID</a>
    </td>
    <td align="center">
      <a href="https://github.com/yeliudev/CATNet/blob/main/configs/hrsid/cat_mask_rcnn_r50_6x_hrsid.py">CAT Mask R-CNN</a>
    </td>
    <td align="center">ResNet-50</td>
    <td align="center">6x</td>
    <td align="center">&cross;</td>
    <td align="center">70.9</td>
    <td align="center">57.6</td>
    <td align="center">
      <a href="https://huggingface.co/yeliudev/CATNet/resolve/main/checkpoints/cat_mask_rcnn_r50_6x_hrsid-198d5409.pth">model</a> |
      <a href="https://huggingface.co/yeliudev/CATNet/resolve/main/checkpoints/cat_mask_rcnn_r50_6x_hrsid.json">metrics</a>
    </td>
  </tr>
  <tr>
    <td align="center">
      <a href="https://github.com/yeliudev/CATNet/blob/main/configs/hrsid/cat_mask_rcnn_r50_aug_6x_hrsid.py">CAT Mask R-CNN</a>
    </td>
    <td align="center">ResNet-50</td>
    <td align="center">6x</td>
    <td align="center">&check;</td>
    <td align="center">72.0</td>
    <td align="center">59.6</td>
    <td align="center">
      <a href="https://huggingface.co/yeliudev/CATNet/resolve/main/checkpoints/cat_mask_rcnn_r50_aug_6x_hrsid-0da9459c.pth">model</a> |
      <a href="https://huggingface.co/yeliudev/CATNet/resolve/main/checkpoints/cat_mask_rcnn_r50_aug_6x_hrsid.json">metrics</a>
    </td>
  </tr>
</table>


