# Official RCS-YOLO
This is the source code for the paper, "RCS-YOLO: A Fast and High-Accuracy Object Detector for Brain Tumor Detection" accepted by the 26th International Conference on Medical Image Computing and Computer Assisted Intervention ([MICCAI 2023](https://conferences.miccai.org/2023/en/)), of which I am the first author.

## Model
The model configuration (i.e., network construction) file is rcs-yolo.yaml in the directory [./cfg/training/](https://github.com/mkang315/RCS-YOLO/tree/main/cfg/training). The [RepVGG](https://openaccess.thecvf.com/content/CVPR2021/papers/Ding_RepVGG_Making_VGG-Style_ConvNets_Great_Again_CVPR_2021_paper.pdf)/[RepConv](https://arxiv.org/pdf/2207.02696.pdf) [ShuffleNet](https://openaccess.thecvf.com/content_ECCV_2018/papers/Ningning_Light-weight_CNN_Architecture_ECCV_2018_paper.pdf) based [One-Shot Aggregation](https://openaccess.thecvf.com/content_CVPR_2020/papers/Lee_CenterMask_Real-Time_Anchor-Free_Instance_Segmentation_CVPR_2020_paper.pdf) (RCS-OSA) module file is rcsosa.py in the directory [./models/](https://github.com/mkang315/RCS-YOLO/tree/main/models), which is the unique module we proposed.

#### Training

The hyperparameter setting file is hyp_training.yaml in the directory [./data/](https://github.com/mkang315/RCS-YOLO/tree/main/data).

###### Single GPU training
```
python train.py --workers 8 --device 0 --batch-size 32 --data data/br35h.yaml --img 640 640 --cfg cfg/training/rcs-yolo.yaml --weights '' --name rcs-yolo --hyp data/hyp_training.yaml
```

###### Multiple GPU training
```
python -m torch.distributed.launch --nproc_per_node 4 --master_port 9527 train.py --workers 8 --device 0,1,2,3 --sync-bn --batch-size 128 --data data/br35h.yaml --img 640 640 --cfg cfg/training/rcs-yolo.yaml --weights '' --name rcs-yolo --hyp data/hyp_training.yaml
```

#### Testing

The model weights we pretrained on the brain tumor detection was saved as best.pt in the directory [./runs/train/exp/weights/](https://github.com/mkang315/RCS-YOLO/tree/main/runs/train).
```
python test.py --data data/br35h.yaml --img 640 --batch 32 --conf 0.001 --iou 0.65 --device 0 --weights runs/train/exp/weights/best.pt --name val
```

## Evaluation
We trained and evaluated RCS-YOLO on the dataset [Br35H :: Brain Tumor Detection 2020](https://www.kaggle.com/datasets/ahmedhamada0/brain-tumor-detection). The .txt format annotations in the folder dataset-Br35H are coverted from original json format. We used 500 images of which in the ’train’ folder were selected as the training set, while the other 201 images in the ’val’ folder as the testing set. <br />

The screenshots of evaluation results are in the directory [./runs/val/](https://github.com/mkang315/RCS-YOLO/tree/main/runs/val).

## Suggested Citation
Our manuscript has been accepted for publication. Please cite our paper if you use code from this repository:
> Plain Text

- Springer *Lecture Notes in Computer Science (LNCS)* Style</br>
Kang, M., Ting, C.-M., Ting, F. F., Phan, R. C.-W.: Rcs-yolo: A fast and high-accuracy object detector for brain tumor detection. In: X. Xxxx et al. (Eds.) Medical Image Computing and Computer-Assisted Intervention – MICCAI 2023: 26th International Conference, Vancouver, Canada, October 8–12, 2023, Proceedings, Part ?. Lecture Notes in Computer Science (LNCS), vol 00000, in press. Springer, Cham (2023). [https://doi.org/10.1007/978-3-000-00000-0\_0](https://doi.org/10.1007/978-3-000-00000-0\_0)</br>
<sup>**NOTE:** MICCAI conference proceedings are part of the book series LNCS and use the LNCS citation style.</sup>

- *Nature* Style</br>
Kang, M., Ting, C.-M., Ting, F. F. & Phan, R. C.-W. Rcs-yolo: A fast and high-accuracy object detector for brain tumor detection. In *Medical Image Computing and Computer-Assisted Intervention – MICCAI 2023: 26th International Conference, Vancouver, Canada, October 8–12, 2023, Proceedings, Part ?* (eds. Xxxx, X. et al.) in press (Springer, 2023).</br>

- *IEEE* Style</br>
M. Kang, C.-M. Ting, F. F. Ting, and R. C.-W. Phan, "Rcs-yolo: A fast and high-accuracy object detector for brain tumor detection," in *Proc. Int. Conf. Med. Image Comput. Comput. Assist. Interv. (MICCAI)*, Vancouver, BC, Canada, Oct. 8–12, 2023, in press.</br>
<sup>**NOTE:** City of Conf., Abbrev. State, Country, Month & day(s) are optional.</sup>

> BibTeX Format</br>
```
@inproceedings{kang2023rcsyolo,
  author = {Kang, Ming and Ting, Chee-Ming and Ting, Fung Fung and Phan, Raphaël C.-W.},
  title = {Rcs-yolo: A fast and high-accuracy object detector for brain tumor detection},
  editor = {Xxxx, X. et al.},
  booktitle = {Medical Image Computing and Computer-Assisted Intervention – MICCAI 2023: 26th International Conference, Vancouver, Canada, October 8--12, 2023, Proceedings, Part ?},
  series = {Lecture Notes in Computer Science (LNCS), vol 00000},
  year = {2023},
  pages = {},
  publisher = {Springer},
  note = {in press},
  doi= {10.1007/978-3-000-00000-0\_0},
  url = {https://doi.org/10.1007/978-3-000-00000-0\_0}
}
```
```
@inproceedings{kang2023rcsyolo,
  author = "Ming Kang and Chee-Ming Ting and Fung Fung Ting and Raphaël C.-W. Phan",
  title = "Rcs-yolo: A fast and high-accuracy object detector for brain tumor detection",
  editor = "X. Xxxx et al.",
  booktitle = "Proc. Int. Conf. Med. Image Comput. Comput. Assist. Interv. (MICCAI)",
  series = "Lecture Notes in Computer Science (LNCS), vol. 00000"
  address = "Vancouver, BC, Canada, Oct. 8--12",
  year = "2023",
  pages = "",
  publisher = "Cham, Germany: Springer",
  note = "in press",
  doi = "10.1007/978-3-000-00000-0\_0",
  url = "https://doi.org/10.1007/978-3-000-00000-0\_0",
}
```
<sup>**NOTE:** Please remove some optional *BibTeX* fields, for example, `address`, `publisher`, and so on, while the *LaTeX* compiler produces an error. Author names may be manually modified if not automatically abbreviated by the compiler under the control of the .bst file.</sup>

## License
RCS-YOLO is released under the GNU General Public License v3.0. Please see the [LICENSE](https://github.com/mkang315/RCS-YOLO/blob/main/LICENSE) file for more information.

## Acknowledgement
Many utility codes of our project reference the codes of [YOLOv7](https://github.com/WongKinYiu/yolov7), [RepVGG](https://github.com/DingXiaoH/RepVGG), [ShuffleNet](https://github.com/megvii-model/ShuffleNet-Series), and [VoVNetV2](https://github.com/youngwanLEE/vovnet-detectron2) repositories.
