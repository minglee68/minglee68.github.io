---
title: CNN (Convolutional Neural Network)
---

# CNN (Convolutional Neural Network)
**이번 프로젝트의 주제인 영상인식에 꼭 필요한 CNN에 대해서 알아본다**  
  
## 영상인식이란?
사람들이 볼 때엔 영상을 그림처럼 보지만, 컴퓨터에겐 단순한 숫자에 불과하다. 이런 숫자가 나열이된 정보를 보고 사람들이 보는 것과 같이 사물을 인식하기 위해선, 일반적인 알고리즘으로는 한계가 있다.  
예를 들자면 아래와 같은 사진이 있다고 가정하자.  
  
![Sample Image 1](/images/sample1.jpg)  
  
우리는 이 사진을 보면, 이것은 코알라의 사진이라고 바로 알 수 있다. 하지만 컴퓨터에겐 그저 1024x768개의 Pixel의 정보일 뿐이다. 하지만 이런 정보를 통해서 우리 사람이 코알라를 인식하듯이 컴퓨터도 코알라로 인식하도록 하는 것이 바로 **영상인식**이다.  
  
영상인식은 쉽지 않지만, 최근의 CNN의 발전을 통해서 충분한 양의 데이터와 적절한 딥러닝 모델만 있으면 사람보다 더 잘 인식하는 모델을 만들 수 있게 되었다.  
  
### Pixel
이미지는 여러개의 Pixel을 행렬같이 나열해 놓은 형식이다. 
  
![Sample Image 2-1](/images/sample2.png)
![Sample Image 2-2](/images/sample2.jpg)
  
한 Pixel이란 위에서 보이는 것과 같이 확대를 했을 때의 한 사각형을 말한다. 이 한 Pixel이 가지고 있는 데이터는 RGB (Red, Green, Blue)의 원색들을 각각 8비트씩 가지고 있다. 첫번째의 코알라 이미지의 크기가 1024x768이면, 컴퓨터가 보는 총 정보는 1024x768x3인 것이다.  
  
## 그렇다면 CNN이란?
CNN이란 Convolutional Neural Network의 줄임말로, Neural Network에 중첩(Convolution)시키는 작업을 넣은 것이다.  
  
### Convolution
Convolution이란 영상처리에서 자주 사용되는 방법인데, 커널(또는 필터)라고 불리는 작은 행렬과 같은 데이터를 영상(이미지)에 중첩시켜서 계산하는 작업이다. 말로하긴 어렵기 때문에, 이미지로 설명을 더 해보겠다. 먼저 아래와 같은 5x5 사이즈의 input과 3x3 사이즈의 커널이 있다고 생각하자.  
  
![Sample Image 3-1](/images/sample3.png)
  
위의 Input이 이미지라고 생각을 했을 때에, 오른쪽의 커널을 왼쪽의 이미지에 올린다. 이 때에 각 element끼리 곱한 뒤 다 더해서 나오는 결과를 커널이 겹쳐진 부분의 가운데 픽셀에 적용시킨다. 이 작업을 Convolution이라고 한다. 그렇게 해서 나온 결과를 Feature Map이라고 한다.  
  
![Sample Image 3-2](/images/sample3_2.png)
  
위와 같이 한 번 적용을 시키고 끝내는 것이 아니라, 한 칸 오른쪽으로 옮겨서 똑같이 다시 실행한다. 이 작업을 끝까지 해서 최종적으로 하나의 Feature Map이 나오도록 한다.  
  
![Sample Image 3-3](/images/sample3_3.png)
  
### Pooling
CNN에는 Convolution Layer 말고도 중요한 Layer가 몇 가지 더 있는데, 그 중 하나가 Pooling Layer이다. Pooling이란 Convolution과 비슷하게 커널의 개념을 가지고 하는 작업인데, 이미지의 크기를 줄이고 싶을 때에 주로 사용되는 작업으로, 영어로는 Downsampling 한다고 한다. 여기에선 대표적인 Max Pooling에 대해 설명하겠다.  
  
Max Pooling이란 입력으로 들어오는 Feature Map에서 중요하다고 생각되는 Feature만 뽑아내기 위한 Pooling이다. 먼저 어느 정도 크기의 구간에서 최대치를 찾는지 커널 사이즈로 지정을 하는데, 아래의 예시에서는 2x2로 지정을 했다 가정한다.  
  
![Sample Image 4](/images/sample4.png)
  
위에서 보면 처음 빨간색이 2x2의 커널에 들어가는 4개의 element들인데, 여기서 최대치인 6만 다음 Feature Map으로 보내주고, 나머지는 버린다.  
  
위와 같은 Max Pooling의 단점은 최대치의 Feature말고는 아예 쓸모없는 정보로 간주하고 버려버리는 데에 있다. 이 점을 극복하기 위해서 2x2의 모든 element를 다 더해버리는 Sum Pooling이나, 모든 element의 평균값을 구해서 Feature Map으로 보내는 Average Pooling 등등 여러가지 대안이 있다.
  
### ReLU
CNN에서 중요한 또 다른 Layer가 바로 ReLU이다. ReLU란 Rectified Linear Unit의 줄임말인데, Neural Network에서 중요한 Non-Linearity를 보장해주는 역할을 한다. 작업 방법은 Input에서 모든 element들 중 0보다 작은 값들은 다 0으로 간주하고, 나머지는 본래 값 그대로 보내주는 작업을 한다.   
  
![Sample Image 5](/images/sample5.png)
  
### Fully Connected Layer
마지막으로 CNN에서 Input에 대한 예측을 하는 Layer가 Fully Connected Layer이다. 본래 이미지는 행렬의 구조를 가지고 있는데, 이것을 한 줄의 배열과 같은 형식으로 바꿔서 백터로 간주한다. 그 백터를 가지고 이 Input이 어떤 카테고리에 가까운지 예측한다.  
  
![Sample Image 6](/images/sample6.png)
  
### CNN 구조 Overview
![Sample Image 7](/images/sample7.png)
  
최종적으로 CNN의 구성을 보면 위와 같다. 먼저 입력의 이미지를 Convolution Layer와 ReLU Layer를 통해서 첫번째 Feature Map을 만들어낸다. 이 Feature Map의 Dimension을 줄이기 위해서 Pooling Layer로 줄인다. 그런 다음 Convolution Layer, ReLU Layer, Pooling Layer를 반복해서 작업을 하다가, 마지막에 Fully Connected Layer를 통해서 입력의 이미지가 어느 카테고리에 들어가는지 맞추는 Classification 작업을 한다.  
  
위의 CNN구조는 제일 기본적인 CNN의 구조이고, 지금은 더욱 더 다양한, 더 성능이 좋은 CNN구조가 많다. 이것에 대해선 나중에 추가 조사를 하도록 하겠다.
