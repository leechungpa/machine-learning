# Categorical Value Encoding

먼저 범주형 자료의 encoding에는 대표적으로 one-hot encoding과 label encoding이 있다. 개인적으로는 one-hot encoding을 선호하며, 다른 방법은 생각하지 않았는데, 이외에도 mean encoding frequency encoding 등 여러가지 방법이 있다고 한다. 먼저 핵심만 정리하면 다음과 같다.

1. one-hot encoding : n개의 범주가 있으면 n-1개의 변수로 만든 것으로, 데이터가 커지는 문제가 있다.

    - binary Encoding :  데이터가 커지는 문제를 막을 수느 있으나, binary encoding 방법을 사용하기보다는 범주 합치기를 더 선호한다. (갑자기 든 생각인데 범주의 수가 많은 경우, binary encoding을 이용하나, encoding을 임의로 하는 것이 아니라 mean encoding처럼 집단별 특성이 encoding norm과 유사하게 encoding 하는 방법이 어딘가에 존재할 것 같다...  이정도 까지가 내가 그나마 생각한 방법이다.)

1. label encoding : n개의 범주별로 각가의 값을 정하고 1개의 변수로 만드는 것으로, 순서형 범주에 적당하나, score의 문제가 발생 할 수 있다. ( ordinal Encoding와 분리해서 보는 경우가 있으나, ordinal 하지 않는 경우 label encoding은 부적절하다고 생각하기에 같은 의미로 간주한다.)

    -  frequency encoding : 각 범주의 빈도수로 label encoding 하는 방법

    - mean encoding (=target encoding) :  각 범주펼 target variable의 평균으로 label encoding 하는 방법

    - Leave One Out Encoding : mean encoding과 동일하나 평균을 구할때, 해당 row의 값을 제외하고 구해 encoding 하는 방법
 
    - Weight of Evidence Encoding : 카드사기와 같이 2개의 결과인 경우 범주별로 log odds를 구해 label encoding 하는 방법

    - Probability Ratio Encoding : Weight of Evidence 와 동일하나, log 취하지 않는 방법


이 외에도 Helmert Encoding,, Hashing Encoding, Backward Difference Encoding, James-Stein Encoding, M-estimator Encoding, Thermometer Encoder 등 다양한 방법이 있다고 하나 비슷하므로 생략한다. 해당 내용은 [Medium](https://towardsdatascience.com/all-about-categorical-variable-encoding-305f3361fd02)과 scikit-learn api를 참고하였다.

## mean encoding에 대하여

왜 내가 이 방법을 처음듣는지 했더니 kaggler들로부터 유행된 방법이라고 한다. 그리고 순서형 범주가 아닌경우 개인적으로 label encoding을 선호하지 않기 때문에 잘 몰랐던 것 같다. 먼저 특징을 보면 다음과 같다.

- 데이터의 크기와 상관없이 encoding 하여 학습에 빠르다

- encoding 자체에서 예측변수를 활용하기에 overfitting의 위험이 매우 높다.

- 앞서 언급한 이유로 regularization 등 overfitting을 막기위한 노력이 필수적이다.



## Weight of Evidence 와 Probability Ratio Encoding에 대하여

두개는 사살상 비슷한 방법으로 특히 WoE의 방법의 경우 logistic regression에 유용하게 사용할 수 있다. 특징은 아래와 같다. 하지만 마찬가지로 overfitting의 위험이 존재한다.

- WoE의 경우 logistic sclae을 통해 사용하기에 좀더 정보를 포함할 수 있다.

- 일종의 group해주는 효과로 sparsely populated discrete values를 비슷하게 모아주는 효과를 보인다.

- WoE의 경우 standardized value 이므로 범주형 변수와 결과변수의 영향을 전체와 결과변수를 비교할 수 있다.

- 하지만, outlier 같이 sparse한 범주를 없애므로 정보를 일을 수 있다.

- 단변량 변수이므로 독립변수간의 상관관계를 고려하지 않는다.

- 그리고  결과변수를 사용하므로 overfitting의 위험이 존재한다.

## 주관적인 label encoding의 overfitting을 막는 방법

각각의 학습 모델에 따라 적당한 overfitting을 막는 방법을 고려해야 한다. 그중 mean encoding의 정수인 [Kaggle](https://www.kaggle.com/dustinthewind/making-sense-of-mean-encoding)에서 설명한 additive smoothing와 cross-validation 방법을 정리하였다.

additive smoothing은 기본적으로 mean을 구할때 주의하는 방법이다. mean은 매우 크거나 작은 outlier에 robust하지 않는다. 이를 weight를 이용하여 outlier의 효과를 줄여 mean을 구해 사용하는 방법이다. 반면, corss-validation 방법은 cross-validation을 하면서 mean을 학습에 사용하지 않은 다는 validation set을 이용해 구하여 적용하는 방식이다.

이외에도 기본적인 L1 L2 regularization이나 dropout 방법등 각각의 주어진 모델에 따라 특수하게 사용 될수 있는 overftting을 막는 방법도 사용하면 더 우수한 결과를 얻을 수 있을 것이다.


## 정리

모델을 만드는 과정은 데이터에 있는 bias를 잘 파악해 model로 만들어 예측가능하게 만드는 것이다. 그러기 위해 data 자체 뿐만 아니라, 그 data를 만들어낸 context를 활용하여 model을 만든다. 사실 그동안 data를 객관적으로 다루고, 좀더 overfitting하지 않게 하기 위해 one-hot encoding만 해온 것 같다. 해당 data의 variable의 context를 이용하여 적절한 encoding을 한다면, 그것이 각론적인 label encoding이라고 할지라도, 오히려 모델 예측이나 설명에 있어서 더 좋은 결과를 얻을 수 있을 것이다. 그것이 요즘 kaggle들이 viral하게 mean encoding을 사용하는 의미로 생각된다.

물론, 이렇게 주관적인 encoding을 한다면 overfitting 을 막기 위해 더욱 조심해야 할 것이다.


