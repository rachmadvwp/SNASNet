# Neural Architecture Search for Spiking Neural Networks (updated)
Pytorch implementation code for [Neural Architecture Search for Spiking Neural Networks], ECCV 2022 (https://arxiv.org/abs/2201.10355)


## Introduction 
Spiking Neural Networks (SNNs) have gained huge attention as a potential energy-efficient alternative to conventional Artificial Neural Networks (ANNs) due to their inherent high-sparsity activation. However, most prior SNN methods use ANN-like architectures (e.g., VGG-Net or ResNet), which could provide sub-optimal performance for temporal sequence processing of binary information in SNNs. To address this, in this paper, we introduce a novel Neural Architecture Search (NAS) approach for finding better SNN architectures. Inspired by recent NAS approaches that find the optimal architecture from activation patterns at initialization, we select the architecture that can represent diverse spike activation patterns across different data samples without training. Moreover, to further leverage the temporal information among the spikes, we search for feed forward connections as well as backward connections (i.e., temporal feedback connections) between layers. Interestingly, SNASNet found by our search algorithm achieves higher performance with backward connections, demonstrating the importance of designing SNN architecture for suitably using temporal information. We conduct extensive experiments on three image recognition benchmarks where we show that SNASNet achieves state-of-the-art performance with significantly lower timesteps (5 timesteps).


## Prerequisites
* Python 3.9    
* PyTorch 1.10.0     
* NVIDIA GPU (>= 12GB)      
* CUDA 10.2 (optional)         

## Getting Started

### Conda Environment Setting
```
$ conda create -n SNASNet 
$ conda activate SNASNet
$ conda install pytorch torchvision torchaudio cudatoolkit=10.2 -c pytorch
$ pip install scipy
```

### Check the availability of CUDA
```
$ python3
>>> import torch
>>> torch.cuda.is_available()
True
```
If the result is False, then CUDA is not available.
You can try this instead.
```
$ conda install pytorch torchvision torchaudio pytorch-cuda=11.8 -c pytorch -c nvidia
```

### Spikingjelly Installation (ref: https://github.com/fangwei123456/spikingjelly)
```
$ git clone https://github.com/fangwei123456/spikingjelly.git
$ cd spikingjelly
$ python setup.py install
```

## Training and testing

* Arguments required for training and testing are contained in ``config.py``` 
* Here is an example of running an experiment on CIFAR100
* (if a user want to skip search process and use predefined architecgtur) A architecture can be parsed by ``--cnt_mat 0302 0030 3003 0000`` format

Example) Architecture and the corresponding connection matrix

<img src="https://user-images.githubusercontent.com/41351363/142759748-50d0e9bf-4654-4831-97eb-5bfb4d30c21e.png"  width="630" height="400"/>


### Training

*  Run the following command

```
python3 search_snn.py  --exp_name 'cifar100_backward' --dataset 'cifar100'  --celltype 'backward' --batch_size 32 --num_search 5000 
```
simple argument instruction

--exp_name: savefile name

--dataset: dataset for experiment

--celltype: find backward connections or forward connections

--num_search: number of architecture candidates for searching

## Testing with pretrained models (CIFAR10 & CIFAR100)

(1) download pretrained parameters 

CIFAR10: ([link][e]) to ```./savemodel/save_cifar10_bw.pth.tar```   

[e]: https://drive.google.com/file/d/1irW6V4MNt0BOkNWAP55X_yjrcIt-ulVK/view?usp=sharing

CIFAR100: ([link][e]) to ```./savemodel/save_cifar100_bw.pth.tar```   

[e]: https://drive.google.com/file/d/1pnS0nFMk2KlxTFeeVT5fYMdTPh_8qn84/view?usp=sharing

(2) The above pretrained model is for 

CIFAR10 architecture ``--cnt_mat 0303 0030 2002 0200``

CIFAR100 architecture ``--cnt_mat 0302 0030 3003 0000``

(3)  Run the following command
```
python3 search_snn.py  --dataset 'cifar10' --cnt_mat 0303 0030 2002 0200 --savemodel_pth './savemodel/save_cifar10_bw.pth.tar'  --celltype 'backward' --second_avgpooling 4
```
```
python3 search_snn.py  --dataset 'cifar100' --cnt_mat 0302 0030 3003 0000 --savemodel_pth './savemodel/save_cifar100_bw.pth.tar'  --celltype 'backward'
```

## Acknowledgement 
Hamming distance measurement codes are referred from: 
https://github.com/BayesWatch/nas-without-training

Spiking Jelly reference: 
https://github.com/fangwei123456/spikingjelly

## Citation
```
@article{kim2022neural,
  title={Neural architecture search for spiking neural networks},
  author={Kim, Youngeun and Li, Yuhang and Park, Hyoungseob and Venkatesha, Yeshwanth and Panda, Priyadarshini},
  journal={arXiv preprint arXiv:2201.10355},
  year={2022}
}
```       

 

