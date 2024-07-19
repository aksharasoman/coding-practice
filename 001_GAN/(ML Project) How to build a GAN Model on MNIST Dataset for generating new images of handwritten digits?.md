#mlproject 

project title source: https://www.projectpro.io/projects/data-science-projects/machine-learning-projects-in-python

### Resources:
1. [Deep Learning with PyTorch : Generative Adversarial Network](https://www.coursera.org/projects/deep-learning-with-pytorch-generative-adversarial-network#details) 
### Expected learnings:
![[Pasted image 20240610154905.png|500]]

### Let's start ...
The hands on project on[ *Deep Learning with PyTorch : Generative Adversarial Network*](https://www.coursera.org/learn/deep-learning-with-pytorch-generative-adversarial-network/supplement/MhGNK/project-based-course-overview) is divided into 8 tasks.

Objective: Build a DCGAN (deep convolutional GAN) to generate handwritten digits.

### Theory
#### Expected Nature of Loss curves
- **Generator Loss:**
    - Initially high, then decreases as the generator improves, and finally oscillates around a certain value.
- **Discriminator Loss:**
    - Initially low, then increases as the generator improves, and finally oscillates around a certain value (ideally around 0.5 in binary cross-entropy terms).
#### Convergence Phase
- Ideally, the generator and discriminator reach an equilibrium where the generator produces high-quality images that are hard to distinguish from real ones, and the discriminator is about 50% accurate (i.e., it cannot reliably distinguish real from fake images). At this point, both losses oscillate around a stable value.
- However, this balance is often difficult to achieve perfectly in practice. The losses might continue to oscillate or show minor fluctuations.
#### What to Watch For
- **Mode Collapse:** If the generator loss drops sharply and then stabilizes while the discriminator loss also stabilizes, it might indicate that the generator is producing a limited variety of images (mode collapse).
- **Training Imbalance:** If one loss dominates the other (e.g., the generator loss remains very high or the discriminator loss stays very low), it might indicate an imbalance in training. This could be addressed by adjusting hyperparameters, learning rates, or using techniques like one-sided label smoothing or gradient penalties.
### Common Tasks:
#### Task 1: Setup Google Runtime
Google colab (to use GPU)
	Runtime (menu) -> Change Runtime type -> Select 'GPU' as hardware accelerator
Click on "Connect" button in the top right corner of the page.
#### Task 2: Configurations
declare here the common variables or constants used in this project.
`device = 'cuda'`  
	define device as cuda since we are using GPU. In-order to use GPU on pytorch, we have to transfer tensors to the GPU. device variable will be used for that.
`batch_size = 128`
`noise_dim = 64` shape of the random noise vector in the generator
###### optimizer parameters:
`lr = 0.002` learning rate
`beta_1 = 0.5`
`beta_2 = 0.99` for Adam optimizer
######  training variables:
`epochs = 20`
#### Task 3: Load Dataset  (MNIST handwritten)
`from torchvision import datasets, transforms as T`
	import 'datasets' package from torchvision
	
Declare transforms to be performed on each input sample:
`train_augs = T.Compose([T.RandomRotation(-20,+20), T.ToTensor()])
		`ToTensor()` used to convert PIL or numpy images to tensors, it also changes shape to channel-dimension-first format (ie., (h,w,c) - > (c,h,w) )
		Because pytorch used channel-first convention.
	NB: different transforms are available like RandomRotation, verticalFlip etc
	
Load Dataset
`trainset = datasets.MNIST('MNIST/',download = True,train = True, transform = train_augs)`
	There are many datasets in torchvision. 
	first argument: path at which dataset is saved.
		
	
(DEBUG) Let's plot some images

``` {{EXTRA}}
image, label = trainset[5]
plt.imshow(image.squeeze(),cmap='gray')

print('Total number of images in the trainset:',len(trainset))
```

#### Task 4: Load Dataset into Batches
with the help of dataloader

Import Dataloader:
```
from torch.utils.data import DataLoader

trainloader = DataLoader(trainset,batch_size=batch_size,shuffle=True)
```
--- 

(DEBUG) Load one batch from this trainloader:
```
print('Total number of batches in trainloader: ',len(trainloader))

dataiter = iter(trainloader)
images,labels = next(dataiter) #one batch
print(images.shape) 
```
`next()` function is used to iterate over a `DataLoader` in PyTorch. It returns one batch. 

(DEBUG) plot some images from this batch:
```
show_tensor_images(): function used to plot a subset of images from the batch

from torchvision.utils import make_grid
	# *make_grid* is useful to plot multiple images.

def show_tensor_images(tensor_img,num_images = 16,size = (1,28,28)):
	unflat_img = tensor_img.detach().cpu() 
		#move images from gpu to cpu
	img_grid = make_grid(unflat_img[:num_images],nrow=4) 
		# input subset of images & num of rows to make_grid()
	
	plt.imshow(img_grid.permute(1,2,0).squeeze()) 
		# channel dimension has to be moved to the end since that's what matplotlib recognizes
		
	plt.show()
```

### Project Specific Tasks:
DL - discriminator loss
Distr: disdriminator
GL- generator loss
![[IMG_0537.jpeg]]
#### Task 5: Create Discriminator Network
It is a binary classifier:classifies input image as a real image or fake image.
Beforing implementing, let's understand [[Theory behind GAN]].
Import necessary packages:
```
from torch import nn
from torchsummary import summary
```

there is repetitive component in the discriminator network: create a function block for it with variable parameters as inputs.
![[Pasted image 20240701120941.png]]

**Linear Output layer**: we are not using sigmoid output layer: since BCEwithLogitsLoss() has sigmoid and BCE loss in a single layer which provides better stability. Binary cross entropy with logits loss will take raw inputs.
#### Task 6: Create Generator Network
![[Pasted image 20240702111035.png]]

formula for convtranspose2d() output shape computation:
= (Hin-1)* stride + kernel_size
for conv2d(): floor{(Hin-kernel_size)/stride)} + 1
##### My questions:
2. How did you fix the channel numbers, kernel size and stride for the convolutional layers? - read chatgpt answer again
4. Why tanh output layer?
##### ConvTranspose2d()
- used to perform a 2D transposed convolution operation, also known as deconvolution (although it's not a true mathematical inverse of convolution).
- unlike standard convolution, which reduces the spatial dimensions, transposed convolution aims to increase them.
- Essential Building block for tasks like:
	- **Image Upscaling/Super-Resolution:** Increasing the spatial resolution of an image by learning to generate additional details.
	- **Autoencoders:** Learning compressed representations of data (encoding) and then reconstructing it (decoding) using transposed convolutions to recover the original dimensions.
	- **Generative Adversarial Networks (GANs):** The generator network in a GAN often employs transposed convolutions to progressively generate higher-resolution images.
##### BatchNorm2d()
- implements **Batch Normalization** for 2D input data
	- improves the training speed and stability of deep neural networks
	- It is a technique used in training neural networks to:
		- **Accelerate training:** By stabilizing the learning process and improving gradient flow.
		- **Improve gradient flow:** Normalization helps prevent gradients from becoming too small or exploding during backpropagation, leading to smoother training.
		- **Reduce internal covariate shift:** This refers to the change in distribution of activations between layers as the network trains. BatchNorm helps mitigate this issue by *normalizing the activations of each layer across a mini-batch during training*.
	- How?
		1. **Normalization:** During training, for each mini-batch, BatchNorm2d calculates the mean (`mu`) and standard deviation (`sigma`) of the activations across each channel (excluding the batch dimension).
		2. Each activation value (`x`) in the input is transformed using::
			` normalized_x = (x - mu) / (sigma + eps) `
		3. **Learnable Scale and Shift:** The normalized activations are then scaled and shifted using two learnable parameters, `weight` (gamma) and `bias` (beta): ```output = normalized_x * weight + bias ```
			These parameters allow the network to learn how much emphasis to put on each channel after normalization and adjust any potential biases introduced during normalization.
			
		This process essentially forces the activations of each layer to have a consistent distribution (zero mean, unit standard deviation) across mini-batches. This helps subsequent layers receive inputs with a more predictable distribution, making it easier for them to learn and improve their weights.

Answers: [ChatGPT](https://chatgpt.com/share/20a47b01-e2af-4aaf-b3bc-1e84831e8071) ; 
#### Task 7: Create loss function 
real loss & fake loss -> to calculate discriminator loss and generator loss

```
criterion = nn.BCEwithLogitsLoss()
loss = criterion(disc_pred,ground_truth)
```
ground_truth will be vector (same size as disc_pred) of zeros for fake_loss and vector of ones for real_loss calculation.

Discriminator loss: average of real and fake losses 
		fake loss computed with disc_pred of fake image
		real loss computed with disc_pred of real image
Generator loss: real loss with disc_pred of fake image

Nb: disc_pred : output of discriminator (discriminator prob)


#### Task 8: Training Loop

##### Questions":
1. other loss functions?
2. progression of loss values during training
3. stopping criterion? other than fixed number of epochs
### Next Projects 
1. Modify this project to *generate images of fashionable clothes*. 
	1. reference: [Generate Synthetic Images with DCGANs in Keras](https://www.coursera.org/projects/generative-adversarial-networks-keras#outcomes) 
		1. Instead of Keras, code it up in pytorch itself. let's not diverge too much & make my brain confused. 🙂
	2. Dataset: Fashion MNIST images
	3. Start by simply replacing the Dataset and evaluate the performance of the model.
	4. Then check with guided project to if I did it correctly.
2. How to spot a deepfake?
	1. Use a discriminator network to give us a confidence score on whether the input is real or fake?
	2. reference: [Overview ‹ Detect DeepFakes: How to counteract misinformation created by AI — MIT Media Lab](https://www.media.mit.edu/projects/detect-fakes/overview/) ; [\[2406.08651\] How to Distinguish AI-Generated Images from Authentic Photographs](https://arxiv.org/abs/2406.08651)
	
		