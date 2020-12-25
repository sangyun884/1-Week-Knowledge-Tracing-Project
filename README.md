# 1-Week-Knowledge-Tracing-Project
이 프로젝트는 2020/12/20 ~ 2020/12/26에 진행되었습니다. TF 2.0을 이용하였습니다. 
## 문제 정의
Knowledge Tracing(KT)이란 educational application에서 학습자의 과거 performance를 바탕으로 future performance를 예측하는 domain입니다. 주어진 데이터는 ASSISTments2009의 skill_builder 데이터셋에서 exercise tag와 user correctness 정보만 추출한 것입니다. 학습자의 과거 exercise tag에 대한 correctness 정보를 바탕으로 새로운 문제에 대한 correctness를 binary classification하여 주어진 데이터에서 좋은 성능을 내는 것이 목표입니다.

## 모델 탐색

#### DKT
KT에서 처음으로 딥러닝을 적용한 RNN 기반 모델입니다. 
https://stanford.edu/~cpiech/bio/papers/deepKnowledgeTracing.pdf
#### SAKT
KT에서 처음으로 self attention을 적용한 모델입니다. Exercise id 정보를 필요로 하지만 주어진 데이터에는 존재하지 않습니다.
https://arxiv.org/abs/1907.06837
#### qDKT
Exercise ID 정보를 활용하기 위해 graph Laplacian regularization과 fastText에 기반한 initialization 방법을 이용해서 좋은 결과를 낸 논문입니다. 하지만 주어진 데이터에는 question ID 정보가 존재하지 않습니다.
https://arxiv.org/abs/2005.12442
#### SAINT+
SAINT에 elapsed time과 lag time이라는 feature를 추가적으로 사용해서 성능을 올린 논문입니다. 이 데이터에는 존재하지 않는 feature입니다.
https://arxiv.org/abs/2010.12042
#### SAINT
기존의 attention 기반 모델들의 attention layer가 너무 shallow하다고 지적하며, encoder-decoder 구조에서 exercise와 response의 embedding이 각각 encoder와 decoder로 분리되어 들어가게 하여 여러 attention layer를 쌓을 수 있게 해서 좋은 결과를 얻은 논문입니다. 또한 exercise embedding을 만들 때 feature를 잘 선택하면 성능이 좋다고 주장합니다. 하지만 주어진 데이터에서는 그러한 feature를 사용할 수 없습니다.
https://arxiv.org/abs/2002.07033
#### PEBG
Question difficulty를 비롯한 side information을 이용해서 좋은 결과를 얻은 논문입니다. 주어진 데이터에서는 그런 정보가 존재하지 않습니다.
https://arxiv.org/abs/2012.05031
#### AKT
https://arxiv.org/abs/2007.12324
Monotonic attention과 Rasch model을 사용해서 성능을 높였습니다. 주어진 데이터에 question difficulty가 없으므로 Rasch model은 사용할 수 없고, 그럴 경우 ASSISTments2009에서 DKT보다 낮은 성능을 보입니다. 그것과는 별개로 다양한 baseline 모델들의 실험 결과를 제공하는데, 아래와 같습니다.


![image](https://user-images.githubusercontent.com/71681194/102733002-7b17d880-437f-11eb-8dd3-0d14a99a9c21.png)


표를 보면, DKT가 나온지 오래됐음에도 불구하고 AKT에 Rasch를 사용한 모델을 제외한 모든 baseline보다 좋은 성능을 보이고 있습니다. 원 논문에서 주장한 0.86보다는 낮은 0.817의 AUC를 기록하고 있습니다. 또한, 최근에 나온 논문들의 흐름을 살펴보면 exercise embedding을 만들 때 데이터셋의 rich feature들을 적극적으로 활용하여 좋은 성능을 보인다고 주장합니다. 같은 tag를 가진 exercise라도 각 문제가 모두 동일한 것이 아니며, difficulty나 id, position 등의 feature를 사용하면 더 좋은 성능을 얻을 수 있습니다. 하지만 주어진 데이터셋은 그러한 정보가 존재하지 않으므로, 이 문제에 가장 적합한 모델은 DKT입니다.

## 결과
![4](https://user-images.githubusercontent.com/71681194/103141375-d1e23100-4736-11eb-94be-02a0168f60c3.JPG)
### AUC : 0.82
본 논문에서는 0.86의 AUC를 주장했지만, 이후에 DKT를 reproduce한 논문들에서는 그보다 낮은 성능을 보고하고 있습니다.
![table1](https://user-images.githubusercontent.com/71681194/103141412-503ed300-4737-11eb-919b-1c82cee405a4.png)

Context-Aware Attentive Knowledge Tracing에서 재현한 DKT의 벤치마크 결과는 위와 같습니다.
https://arxiv.org/abs/2007.12324
![table2](https://user-images.githubusercontent.com/71681194/103141419-6f3d6500-4737-11eb-9cb4-6fc20c233cd6.png)

Going Deeper with Deep Knowledge Tracing에서 재현한 DKT의 벤치마크 결과는 위와 같습니다.
https://eric.ed.gov/?id=ED592679

위의 두 논문의 결과를 잘 재현했다고 할 수 있습니다.
