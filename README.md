# [U-Mamba](https://wanglab.ai/u-mamba.html)

Official repository for U-Mamba: Enhancing Long-range Dependency for Biomedical Image Segmentation.
Welcome to join our [mailing list](https://forms.gle/bLxGb5SEpdLCUChQ7) to get updates.

## Installation 

Requirements: `Ubuntu 20.04`, `CUDA 11.7`

1. Create a virtual environment: `conda create -n umamba python=3.10 -y` and `conda activate umamba `
2. Install [Pytorch](https://pytorch.org/get-started/previous-versions/#linux-and-windows-4) 2.0.1: `pip install torch==2.0.1 torchvision==0.15.2`
3. Install [Mamba](https://github.com/state-spaces/mamba): `pip install causal-conv1d` and `pip install mamba-ssm`
4. Download code: `git clone https://github.com/bowang-lab/U-Mamba`
5. `cd U-Mamba/umamba` and run `pip install -e .`
6. Others dependencies:
    ```bash
    pip install --upgrade git+https://github.com/FabianIsensee/hiddenlayer.git@more_plotted_details #egg=hiddenlayer
    pip install ipython
    ```



sanity test: Enter python command-line interface and run

```bash
import torch
import mamba_ssm
```

![network](https://github.com/bowang-lab/U-Mamba/blob/main/assets/U-Mamba-network.png)



https://github.com/bowang-lab/U-Mamba/assets/19947331/1ac552d6-4ffd-4909-ba31-7b48644fd104




## Model Training
Download dataset [here](https://drive.google.com/drive/folders/1DmyIye4Gc9wwaA7MVKFVi-bWD2qQb-qN?usp=sharing) and put them into the `data` folder. U-Mamaba is built on the popular [nnU-Net](https://github.com/MIC-DKFZ/nnUNet) framework. If you want to train U-Mamba on your own dataset, please follow this [guideline](https://github.com/MIC-DKFZ/nnUNet/blob/master/documentation/dataset_format.md) to prepare the dataset. 

### Preprocessing

```bash
nnUNetv2_plan_and_preprocess -d DATASET_ID --verify_dataset_integrity
```

### Train 2D models

- Train 2D `U-Mamba_Bot` model

```bash
nnUNetv2_train DATASET_ID 2d all -tr nnUNetTrainerUMambaBot
```

- Train 2D `U-Mamba_Enc` model

```bash
nnUNetv2_train DATASET_ID 2d all -tr nnUNetTrainerUMambaEnc
```

### Train 3D models

- Train 3D `U-Mamba_Bot` model

```bash
nnUNetv2_train DATASET_ID 3d_fullres all -tr nnUNetTrainerUMambaBot
```

- Train 3D `U-Mamba_Enc` model

```bash
nnUNetv2_train DATASET_ID 3d_fullres all -tr nnUNetTrainerUMambaEnc
```


## Inference

- Predict testing cases with `U-Mamba_Bot` model

```bash
nnUNetv2_predict -i INPUT_FOLDER -o OUTPUT_FOLDER -d DATASET_ID -c CONFIGURATION -tr nnUNetTrainerUMambaBot --disable_tta
```

- Predict testing cases with `U-Mamba_Enc` model

```bash
nnUNetv2_predict -i INPUT_FOLDER -o OUTPUT_FOLDER -d DATASET_ID -c CONFIGURATION -tr nnUNetTrainerUMambaEnc --disable_tta
```

> `CONFIGURATION` can be `2d` and `3d_fullres` for 2D and 3D models, respectively.


## Paper

```
@article{U-Mamba,
    title={U-Mamba: Enhancing Long-range Dependency for Biomedical Image Segmentation},
    author={Ma, Jun and Li, Feifei and Wang, Bo},
    journal={arXiv preprint arXiv:2401.04722},
    year={2024}
}
```


## Acknowledgements

We acknowledge all the authors of the employed public datasets, allowing the community to use these valuable resources for research purposes. We also thank the authors of [nnU-Net](https://github.com/MIC-DKFZ/nnUNet) and [Mamba](https://github.com/state-spaces/mamba) for making their valuable code publicly available.

