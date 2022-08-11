# deep-photo-styletransfer-tf CS523 Final Project

## Team member
* Junan Zhu
* Taowen Dong
* Yuanming Leng

We fork this repo from [here](https://github.com/LouieYang/deep-photo-styletransfer-tf). This is a pure Tensorflow implementation of [Deep Photo Styletransfer](https://arxiv.org/abs/1703.07511).

## Dependencies of this project
* Tensorflow = 1.15
* Numpy
* Pillow
* Scipy
* PyCUDA

## Instruction to run locally
**Please implement this repo on a Windows OS Machine. I tested the following instructions on a M1 Macbook, and it simply could not support the dependancy we need for this project. I don't have other Mac equipment to test, but it will make things easeier if you implement this on a Windows Machine.Thank you!**

### 1. Clone this repo to local

### 2. Download the VGG-19 model weights
The VGG-19 model of tensorflow is adopted from [VGG Tensorflow](https://github.com/machrisaa/tensorflow-vgg) with few modifications on the class interface. The VGG-19 model weights is stored as .npy file and could be download from [Google Drive](https://drive.google.com/file/d/0BxvKyd83BJjYY01PYi1XQjB5R0E/view?usp=sharing&resourcekey=0-Q2AewV9J7IYVNUDSnwPuCA). After downloading, copy the weight file to the **./project/vgg19** directory

### 3. Download Anaconda for setting up enviroment
If you do have Anaconda in your machine, you can proceed to the next step. If not, please download it from [here](https://www.anaconda.com/).

### 4. Setting up enviroment for the project
* Open the Anaconda Prompt
* Run the command to create env
```
conda create --name project python=3.7
```
"project" can be any name, let's use project as example in the following.
* Run the command to activate env
```
conda activate project
```
* Run the command to install tensorflow-gpu
```
conda install -c conda-forge tensorflow-gpu=1.15
```
* Run the command to install pillow
```
conda install -c conda-forge pillow
```
* Run the command to install a estimator
```
pip install tensorflow-estimator==1.15.0
```



## Usage
### Basic Usage
You need to specify the path of content image, style image, content image segmentation, style image segmentation and then run the command

```
python deep_photostyle.py --content_image_path <path_to_content_image> --style_image_path <path_to_style_image> --content_seg_path <path_to_content_segmentation> --style_seg_path <path_to_style_segmentation> --style_option 2
```

*Example:*
```
python deep_photostyle.py --content_image_path ./examples/input/in11.png --style_image_path ./examples/style/tar11.png --content_seg_path ./examples/segmentation/in11.png --style_seg_path ./examples/segmentation/tar11.png --style_option 2
```

### Other Options

`--style_option` specifies three different ways of style transferring. `--style_option 0` is to generate segmented intermediate result like torch file **neuralstyle_seg.lua** in torch. `--style_option 1` uses this intermediate result to generate final result like torch file **deepmatting_seg.lua**. `--style_option 2` combines these two steps as a one line command to generate the final result directly.

`--content_weight` specifies the weight of the content loss (default=5), `--style_weight` specifies the weight of the style loss (default=100), `--tv_weight` specifies the weight of variational loss (default=1e-3) and `--affine_weight` specifies the weight of affine loss (default=1e4). You can change the values of these weight and play with them to create different photos.

`--serial` specifies the folder that you want to store the temporary result **out_iter_XXX.png**. The default value of it is `./`. You can simply `mkdir result` and set `--serial ./result` to store them. **Again, the temporary results are simply clipping the image into [0, 255] without smoothing. Since for now, the smoothing operations need pycuda and pycuda will have conflict with tensorflow when using single GPU**

Run `python deep_photostyle.py --help` to see a list of all options

### Image Segmentation
This repository doesn't offer image segmentation script and simply use the segmentation image from the [torch version](https://github.com/luanfujun/deep-photo-styletransfer). The mask colors used are also the same as them. You could specify your own segmentation model and mask color to customize your own style transfer.


## Examples
Here are more results from tensorflow algorithm (from left to right are input, style, torch results and tensorflow results)

<p align="center">
    <img src='examples/input/in6.png' height='140' width='210'/>
    <img src='examples/style/tar6.png' height='140' width='210'/>
    <img src='examples/final_results/best6_t_1000.png' height='140' width='210'/>
    <img src='some_results/best6.png' height='140' width='210'/>
</p>

<p align="center">
    <img src='examples/input/in7.png' height='140' width='210'/>
    <img src='examples/style/tar7.png' height='140' width='210'/>
    <img src='examples/final_results/best7_t_1000.png' height='140' width='210'/>
    <img src='some_results/best7.png' height='140' width='210'/>
</p>

<p align="center">
    <img src='examples/input/in8.png' height='140' width='210'/>
    <img src='examples/style/tar8.png' height='140' width='210'/>
    <img src='examples/final_results/best8_t_1000.png' height='140' width='210'/>
    <img src='some_results/best8.png' height='140' width='210'/>
</p>

<p align="center">
    <img src='examples/input/in9.png' height='140' width='210'/>
    <img src='examples/style/tar9.png' height='140' width='210'/>
    <img src='examples/final_results/best9_t_1000.png' height='140' width='210'/>
    <img src='some_results/best9.png' height='140' width='210'/>
</p>

<p align="center">
    <img src='examples/input/in10.png' height='140' width='210'/>
    <img src='examples/style/tar10.png' height='140' width='210'/>
    <img src='examples/final_results/best10_t_1000.png' height='140' width='210'/>
    <img src='some_results/best10.png' height='140' width='210'/>
</p>

<p align="center">
    <img src='examples/input/in11.png' width='210'/>
    <img src='examples/style/tar11.png' width='210'/>
    <img src='examples/final_results/best11_t_1000.png' width='210'/>
    <img src='some_results/best11.png' width='210'/>
</p>

## Acknowledgement

* This work was done when Yang Liu was a research intern at *Alibaba-Zhejiang University Joint Research Institute of Frontier Technologies*, under the supervision of [Prof. Mingli Song](http://person.zju.edu.cn/en/msong) and [Yongcheng Jing](http://yongchengjing.com/).

* Our tensorflow implementation basically follows the [torch code](https://github.com/luanfujun/deep-photo-styletransfer).

* We use [martinbenson](https://github.com/martinbenson)'s [python code](https://github.com/martinbenson/deep-photo-styletransfer/blob/master/deep_photo.py) to compute Matting Laplacian.

## Citation
If you find this code useful for your research, please cite:
```
@misc{YangPhotoStyle2017,
  author = {Yang Liu},
  title = {deep-photo-style-transfer-tf},
  publisher = {GitHub},
  organization={Alibaba-Zhejiang University Joint Research Institute of Frontier Technologies},
  year = {2017},
  howpublished = {\url{https://github.com/LouieYang/deep-photo-styletransfer-tf}}
}
```

## Contact
Feel free to contact me if there is any question (Yang Liu lyng_95@zju.edu.cn).
