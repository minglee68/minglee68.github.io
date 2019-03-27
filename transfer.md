---
title: Transfer Learning
---

# Transfer Learning
**우리가 산학 연구를 하면서 받을 수 있는 데이터는 새로운 모델을 처음부터 학습시키기엔 양이 부족하기 때문에, 이미 학습된 모델을 가지고 추가로 학습시키는 Transfer Learning을 사용하게 될 것이다.**
  
## Transfer Learning이란?
기존에 만들어진 모델을 사용해서, 새로운 모델을 만들 때에 학습을 더 빠르게 하고, 예측 확률을 더 높이는 방법이다. 이것을 사용하는 이유는 크게 3가지로 나뉜다.
1. Convolution Network를 사용하는 경우엔 대체적으로 영상인식을 하는 경우인데, 이미 많은 모델들이 만들어져있고, 비슷한 문제를 해결한 모델들도 많기 때문에
2. 최근에 나오는 모델들은 매우 복잡한데, 이런 복잡한 모델을 돌리기 위해선 많은 GPU와 많은 시간이 소요되는데, 이미 학습된 모델에 추가로 학습시키면 그 학습비용으 크게 감소되기 때문에
3. 처음부터 학습을 시키려면 많은 Trial-and-Error가 필요하기 때문에  
한마디로 이미 영상인식을 충분히 잘하는 모델들이 많고, 그 모델들을 바탕으로 추가적으로 학습을 시킨 모델들이 더 좋은 결과를 많이 냈기 때문에 사용한다.  
하지만 만약에 문제가 기존의 영상인식과 거리가 먼 문제라면, Transfer Learning을 사용하기엔 적합하지 않다.  
  
## Transfer Learning의 종류
Transfer Learning 안에서도 종류가 크게 두 가지로 나뉘어지는데, 하나는 전체적으로 재학습시키는 Fine-Tuning이고, 다른 하나는 모델의 일부분만 학습시키는 Freezing이다.  
  
### Fine-Tune
Fine-Tuning은 모델의 모든 weight(가중치)들을 update시키는 Transfer Learning 방식이다. 이 방식은 주로 데이터가 많은 경우에 사용된다. 만약 데이터가 부족한데 전체 모델에 대해서 Backpropagation을 진행하면, 그 적은 데이터에 금방 Over-fitting이 될 가능성이 높다.  
  
![Sample Image 17](/images/sample17.jpg)
  
#### Over-fitting
Over-fitting이란 주로 같은 Dataset에 대해서 너무 많이 학습을 시키면 그 Dataset에 대한 예측확률은 계속 올라가지만, 결과적으로 그 Dataset에 대해서만 예측할 수 있는 모델이 되어버려서 다른 Data를 넣었을 경우에 정답을 찾지 못하는 경우를 얘기한다.
  
![Sample Image 18](/images/sample18.png)
  
### Freezing
Freezing이란 모델 중에서 어느 일정 부분만 재학습시키는 Transfer Learning이다. 일반적으로 데이터가 부족할 때에 Freezing을 많이 쓴다. Data가 많고 부족함의 기준은 주로 사용하게 될 Pre-trained Model이 먼저 학습한 Dataset의 크기에 따라서 달라진다. 만약 Semantic Segmentation을 하는 DeepLab v3+를 생각하면, PASCAL VOC 2012 Dataset에 대해서 이미 학습이 된 모델을 사용하게 된다면 몇 만장의 데이터 가지고 Fine-Tuning을 하게 되면 학습이 잘 안 된다. 그런 경우에는 주로 모델의 Hidden Layer는 그대로 놔두고, Classification 부분만 새로 학습을 시키는 경우가 많다. 이런 작업을 Hidden Layer는 Freeze 시켜놓는다 해서 Freezing 기법이라 부른다.  
  
![Sample Image 19](/images/sample19.jpeg)
  
