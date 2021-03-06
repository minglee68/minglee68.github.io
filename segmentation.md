---
title: Semantic Segmentation
---

# Semantic Segmentation
**우리의 프로젝트는 단순히 이미지를 보고 그게 무엇의 이미지인지 카테고리만 예측하는 것이 아니라, 실제로 그 카테고리가 어떤 영역에 있는 것인지 예측하는 것이기 때문에, 여기에 사용되는 Semantic Segmentation 기능을 알아본다.**  
  
## Semantic Segmentation이란?
Semantic Segmentation은 영상인식의 분야 중 하나인데, 단순히 이미지가 어떤 이미지인지를 인식하는 데에서 그치는 것이 아니라, 이미지에서 어떤 특정 물체가 어디에 있는지, 정확하게 픽셀 단위로 찾는 것이다. 이것을 이해하기 위해선 아래의 내용들을 단계적으로 알아야한다.  
  
* Classification : 이미지 하나의 전체 입력에 대해서 계산을 하여 이 이미지가 무슨(어떤 클래스의) 이미지인지(ex. Dog, Cat...) 예측하는 것이다.
* Localization / Detection : 이미지를 보면서 그 이미지의 클래스만 알려주는 것이 아니라, 그 클래스의 물체가 이 이미지의 어디에 있는지 위치를 파악하는 것이다.
* Segmentation : Localization을 더욱 더 자세하게, 픽셀 단위로 한다고 생각을 하면 된다. 그래서 Classification과 같이 이미지 전체가 하나의 클래스로 레이블링 되는 것이 아니라, 한 픽셀이 하나의 클래스로 레이블링 되는 것이다.
  
![Sample Image 8](/images/sample8.png)
  
또 Semantic Segmentation을 이해하기 위해선 지금까지 나왔던 영상인식을 위한 여러 딥러닝 모델들을 아는 것이 좋다.  
  
* AlexNet : 2012년 ImageNet Competition을 84.6%의 확률로 이긴 CNN이다. 구조는 5개의 Convolutional Layer, Max Pooling Layer, 그리고 ReLU Layer들과 3개의 Fully Convolutional Layer로 구성되어 있다. 거의 처음으로 ImageNet Competition에 딥러닝 구조로 우수한 결과가 나온 사례이다.
* VGG-16 : 2013년 ImageNet Competition을 92.7%의 확률로 이긴 모델이다. 똑같이 Convolutional Layer를 사용하지만, 커널의 크기를 작게 줄이고 Layer수를 늘려서 더 나은 인식률을 보였다.
* GoogLeNet : 2014년 ImageNet Competition을 93.3%의 확률로 이긴 네트워크이다. VGG보다 더 많은 22개의 Layer로 구성이 되어있는데, 각 Layer가 더 다양한 Convolution을 실행하는 Inception module을 만들어냈다.
* ResNet : 2016년 ImageNet Competition을 96.4%의 확률로 이긴 모델이다. 무려 152개의 Layer로 구성되어 있고, 이때까지 Layer를 늘리면 생기는 문제점을 Skip Connection이라는 방법으로 높은 확률로 해결하는 Residual Block을 제시한 모델이다.
  
![Sample Image 9](/images/sample9.png)
  
구체적으로 설명하자면, 어떤 이미지가 Input으로 들어왔을 때에, 그 이미지의 각 픽셀이 어느 클래스에 속하는지 나타내는 레이블이 된 Segmentation Map을 찾는 것이다. 아래와 같이 이미지가 들어오면, 사람이 있는 부분은 사람으로, 식물이 있는 부분은 식물로 등등 이미지와 같이 레이블링만 출력하는 작업이다.  
  
![Sample Image 10](/images/sample10.png)
  
## Semantic Segmentation의 기본 구조
Semantic Segmentation의 Classification과의 가장 큰 차이점은 각 픽셀마다 해당되는 클래스를 찾은 뒤 입력으로 들어온 이미지의 원본크기에 맞춰서 다시 돌려놔야 된다는 점이 있다. Classification은 Fully Connected Layer로 나온 하나의 값으로 이미지 자체의 클래스를 찾으면 되지만, Semantic Segmentation은 한 픽셀의 클래스를 찾은 뒤 원본에서의 정확한 위치도 찾아야하기 때문에 더 복잡하고 많은 Layer를 통과해야 한다.  
  
위와 같은 작업을 하기 위해서 Semantic Segmentation은 모델을 크게 두 영역으로 나누는데, 하나는 픽셀의 클래스를 찾기 위해 Downsampling을 하는 Encoder 영역이고, 다른 하나는 그 찾은 클래스가 원본의 이미지에서 어느 픽셀에 해당되는지 찾기 위해 Upsampling을 하는 Decoder 영역이다.  
  
![Sample Image 11](/images/sample11.png)
  
## 현재까지 나온 Semantic Segmentation 모델
### Fully Convolutional Network-Based Semantic Segmentation (FCN, 2014.11)  
FCN은 기본적으로 전통적인 CNN과 거의 유사하지만, 차이점이 있다면 위치정보를 아예 없애버리는 Fully Connected Layer를 버리고, Convolutional Layer와 Pooling Layer만 가지고 구성된 모델이라는 점이다. 하지만 여기에선 위치정보가 너무 많이 없어져서 아직 Upsampling의 개념을 잘 사용하지 못 했기 때문에, 결과적으로 나오는 출력이 원본의 영상에 비해 해상도가 매우 낮다는 단점이 있다.  
  
![Sample Image 12](/images/sample12.png)
  
### U-Net (U-Net, 2015.5)  
U-Net은 Encoder - Decoder 형식으로 좋은 결과를 낸 초기의 모델이다. Encoding 과정을 Contracting Path라고 부르고, Decoding 과정을 Expanding Path라고 부르는데, U-Net의 특징은 Encoding 하는 과정에서 max pooling으로 잃어버리는 위치 정보들을 Decoding 과정에서 좀 더 회복시키기 위해서 Encoding 중에 나온 Feature를 복사해서 Decoding 중인 Feature에 붙인다. 또 적은 데이터로도 잘 돌리기 위해서 Data Augmentation을 한다. 단점은 느리고, 아직 위치가 정확하지 않아서 나오는 결과의 해상도가 입력에 비해 낮다는 점이다.  
  
![Sample Image 13](/images/sample13.png)
  

### SegNet (SegNet, 2016.10)  
SegNet은 U-Net과 같이 Encoder - Decoder 형식으로 작성된 모델인데, 이번에는 Encoder의 Pooling Layer에서 Pooling을 한 위치정보를 저장을 해놓고 나중에 Decoder에서 Upsampling을 할 때에 그 정보를 사용한다. 기본적으로 FCN과 비슷하지만, Pooling Index를 저장함으로써 출력의 해상도가 많이 개선되었다.  
  
![Sample Image 14](/images/sample14.png)

### Global Convolutional Network (GCN, 2017.3)  
GCN은 거의 모든 모델들이 작은 커널을 쓰던 때에, Semantic Segmentation은 한 픽셀의 Classification을 위해선 주위의 정보가 더 중요하다고 생각하고, 그런 주위의 정보가 많으면 많을 수록 좋다고 생각해서 커널의 크기를 많이 키워서 작업을 한 모델이다. 이 모델에선 FCN의 개념 뿐만이 아니라, ResNet의 개념도 같이 사용이 되어서 Upsampling을 할 때에 도움이 되도록 했고, Boundary Refinement라는 Residual Structure를 더함으로써 이미지의 정보가 더 정확하고 의미가 있는 이미지 정보로 변환이 되도록 만들었다.  
  
![Sample Image 15](/images/sample15.JPG)
  
### DeepLab (DeepLab v3+, 2018.8)  
DeepLab은 Astrous Spatial Pyramid Pooling을 이용해서 Pooling의 과정에서도 잃어버리는 정보를 최소화 시키도록 노력하고, Depthwise Separable Convolution을 이용해서 더 의미있는 Feature Map을 잘 뽑을 수 있도록 만들어낸 구글의 모델이다. Encoder부분에는 v1/2에서는 Google의 기존의 Inception모델을 사용했었고, 현재 Semantic Segmentation에서의 state of art model인 DeepLab v3+에서는 Xception모델을 사용하고 있다.  
  
![Sample Image 16](/images/sample16.png)
  

