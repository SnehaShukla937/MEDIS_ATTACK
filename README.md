# *MedIS* (*Med*ical *I*mage *S*egmentation) ATTACK
## 1. Preface
* This repository provides an implementation of the paper [Exploring the feasibility of adversarial attacks on medical image segmentation](https://link.springer.com/article/10.1007/s11042-023-15575-8), published in [Multimedia Tools and Applications](https://www.springer.com/journal/11042). 

* You can contact me at <shukla.sneha825@gmail.com> to resolve your queries regarding our paper and implementation. 

* If you find this repository helpful for your project or research, please cite this paper as [Citation](https://github.com/SnehaShukla937/MEDIS_ATTACK/tree/main#4-citation).

## 2. Overview
### 2.1  Abstract:
Recent advancements in Deep Learning (DL) based medical image segmentation models have led to tremendous growth in healthcare applications. However, DL models can be easily compromised by intelligently engineered adversarial attacks, which pose a serious threat to the security of life-critical healthcare applications. Thus, understanding the generation of adversarial attacks is essential for designing robust and reliable DL-based healthcare models. To this end, we explore adversarial attacks for medical image segmentation models in this paper. The adversarial attacks are performed by backpropagating the loss function, which minimises the error metrics. However, most of the medical image segmentation models utilise several non-differential loss functions, which obstruct the attack. Consequently, the attacks are performed by surrogate loss functions that are differentiable approximations of the original loss function. However, we observe that different surrogate loss functions behave differently for the same input. Hence, choosing the best surrogate loss function for a successful attack is crucial. Furthermore, these DL models contain non-differentiable layers that obfuscate gradients and obstruct the attack. To mitigate these issues, we introduce an attack, MedIS (Medical Image Segmentation), which utilises parallel fusion for selecting the best surrogate loss function with the least added perturbation. Moreover, our proposed MedIS attack also provides guidelines to tackle non-differentiable layers by replacing them with differentiable approximations. The experiments conducted on several well-known medical image segmentation models employing multiple surrogate loss functions reveal that MedIS outperforms existing attacks on medical image segmentation by providing a higher attack success rate.
### 2.2  Flow Diagram:
![](https://media.springernature.com/full/springer-static/image/art%3A10.1007%2Fs11042-023-15575-8/MediaObjects/11042_2023_15575_Fig1_HTML.png?as=webp) Figure.1: Flow diagram of the proposed medical image segmentation attack, MedIS. Initially, the non-differentiable layers in the threat model are replaced with their differentiable approximations. The resultant model is subsequently attacked using several surrogate loss functions. Eventually, the best surrogate loss function that requires the least perturbation for a successful attack is chosen using parallel fusion.

### 2.3  Qualitative Results:
![](https://media.springernature.com/full/springer-static/image/art%3A10.1007%2Fs11042-023-15575-8/MediaObjects/11042_2023_15575_Fig2_HTML.png?as=webp) Figure.2: Examples depicting successful cases of our proposed, MedIS attack. It shows that adversarial predictions are similar to the target and different from the predictions on clean images, and imperceptible perturbations are introduced in the adversarial images.

## 3. Steps for implementation
### For Polyp Segmentation:
1. Clone the repository <https://github.com/DengPingFan/PraNet.git> and modify the `utils/dataloader.py` file by commenting out the `transforms.Resize((self.testsize, self.testsize))` **(line 92)** and `transforms.Normalize([0.485, 0.456, 0.406],[0.229, 0.224, 0.225])` **(line 94 & 95)**  inside the `class test_dataset`.
2. Download the dataset and pre-trained weight from  <https://github.com/DengPingFan/PraNet.git>.
3. Set the path in `Target2_makeCSV.ipynb` and run it to make a CSV file for each dataset that contains the mean IOU of each image to another image.
4. Set the path and all variables in `MainFile_Polyp_targeted.ipynb` and `MainFile_Polyp_Untargeted.ipynb` and run it. This gives us the attack results calculated up to 10 steps.
5. Set the path of the CSV file (created in step 4) in `FinalResult_untargeted.ipynb` and `FinalResult_targeted.ipynb` and run it to get the final attack success rate and average distortion with and without parallel fusion.

### For Skin-Lesion Segmentation:
1. Clone the repository <https://github.com/DengPingFan/PraNet.git> and modify the `utils/dataloader.py` file by-
   * Commenting out the `transforms.Resize((self.testsize, self.testsize))` **(line 92)** and `transforms.Normalize([0.485, 0.456, 0.406],[0.229, 0.224, 0.225])` **(line 94 & 95)** inside the `class test_dataset`.
   * Adding a new method inside the `class test_dataset`.
       ```
       def load_dataM(self):
            image = self.rgb_loader(self.images[self.index])
            image = self.transform(image).unsqueeze(0)
            gt = self.binary_loader(self.gts[self.index])
            name_img = self.images[self.index].split('/')[-1] 
            name_gt = self.gts[self.index].split('/')[-1] 
            self.index += 1
            return image, gt, name_img, name_gt
       ```
      
2. Download the dataset from  <https://challenge.isic-archive.com/data/>.
3. Perform Transfer Learning by training the <https://github.com/DengPingFan/PraNet.git> architecture using the skin-lesion dataset.
4. Set the path in `Target2_makeCSV.ipynb` and run it to make a CSV file for each skin-lesion dataset that contains the mean IOU of each image to another image.
5. Set the path and all variables in `MainFile_SkinLesion_targeted.ipynb` and `MainFile_SkinLesion_Untargeted.ipynb` and run it. This gives us the attack results calculated up to 10 steps.
6. Set the path of the CSV file (created in step 4) in `FinalResult_untargeted.ipynb` and `FinalResult_targeted.ipynb` and run it to get the final attack success rate and average distortion with and without parallel fusion.

## 4. Citation
If you find this repository helpful for your project or research, please cite this paper as,
```
@article{Shukla2023Exploring,
    title        = {Exploring the feasibility of adversarial attacks on medical image segmentation},
    author       = {Shukla, Sneha and Gupta, Anup Kumar and Gupta, Puneet},
    year         = 2023,
    month        = {Jun},
    day          = 22,
    journal      = {Multimedia Tools and Applications},
    doi          = {10.1007/s11042-023-15575-8},
    issn         = {1573-7721},
    url          = {https://doi.org/10.1007/s11042-023-15575-8}
}
```

## 5. Contact
For any query, kindly mail us at <shukla.sneha825@gmail.com>.
