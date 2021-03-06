# Cyclical Video Captioning

## Installation

```shell
# create conda env
conda create -n cyclical python=3.7

# activate the enviorment
conda activate cyclical

# install addtional packages
pip install -r requirements.txt

# install pytorch (change the version of cudatoolkit according to your setup)
conda install pytorch torchvision cudatoolkit=10.2 -c pytorch
# (optional) if you want to install pytorch on laptop without GPUs
conda install pytorch torchvision -c pytorch

# install torchtext
pip install torchtext
```

## Download everything

Simply run the following command to download all the data and pre-trained models (total 216GB):

```shell
bash tools/download_all.sh
```

## Local development on MacOS

If you are just like me, who likes to enjoy the fast speed that local development brings you, this code is prepared to make local code development much easier for you. 
It allows you to develop your next research idea on a laptop (e.g., MacBook) with limited computational resources and storage (i.e., the storage space is too small on a laptop).

To do this, download the dummy dataset from Dropbox link [dummy_dataset_files.zip](https://www.dropbox.com/s/1ew6rkt2nl58t3g/dummy_dataset_files.zip?dl=1) (340 MB).

After you unzip the `dummy_dataset_files.zip` file, move both `dummy_rgb_motion_1d` and `dummy_fc6_feat_100rois` under `data/anet/`.
In this way, you can run the code on your laptop without GPUs for local code development.

## Think you met a bug?

### PyTorch older than 1.3

Older PyTorch version (<1.3) does not support the usage of converting Tensor to bool type by `.bool()`.

```shell
AttributeError: 'Tensor' object has no attribute 'bool'
```

### Pillow on older PyTorch

Pillow released version 7.0.0 early 2020, and this would break on older PyTorch versions (PyTorch < 1.4.0).

```shell
# if you see this error
ImportError: cannot import name 'PILLOW_VERSION'

# install pillow with version older than 7.0.0
pip install "pillow<7"
```

### Segmentation fault (core dumped)

I have observed that there is a segmentation fault issue when training with 4 2080 Ti GPUs. There is no issue at all when training with either 1, 2, or 3 GPUs (I only have 4 GPUs to test on when code releasing). This error exists at least between PyTorch v1.1 to PyTorch v1.5.

Code however is working fine with Titan X GPUs, thus we are suspecting the bug is coming from CUDA and specifially from CUDA > 10.0.

This issue might be related to this opened issuei in pytorch: [pytorch/pytorch#31906](https://github.com/facebookresearch/dlrm/issues/42).

### Use torch.save instead of pickle.dump

Currently, I still follow [GVD](https://github.com/facebookresearch/grounded-video-description) to use `pickle.dump`.
As a result, it will show a warning at the moment.

```shell
warnings.warn("pickle support for Storage will be removed in 1.5. Use `torch.save` instead", FutureWarning)
```

Feel free you change it to `torch.save` to avoid the warnings.
