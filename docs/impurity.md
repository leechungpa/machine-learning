# Impurity

decision tree model등 tree model의 경우 분류를 진행할 때 impurity를 가장 최소화 하는 방향으로 분류가 진행된다. 여기서 말하는 impurtiy는 불순도로, 각각의 구분된 영역에서 다른 범주의 카테고리가 얼만큼 있는지, 즉 여러개가 혼합해서 있느지를 의미하는 지표이다. 이러한 불순도를 나타나는 지표중에 하나로 entropy가 있다.

이처럼 impurity가 높다면 분류가 제대로 되지 못한다는 것을 의미하므로 최소화 하는 방향으로 만들고자 한다. 그렇기에 impurity를 tree model의 cost function이 된다고 볼 수 있다. 이러한 impurity에는 여러 종류가 있고 대표적으로 gini impujrity, information gain와 variance reduction 등이 있다.



## Entropy

Gini impurity를 보기 전에 먼저 정보이론에서 나온 entropy를 확인해 보면, 그 공식은 다음과 같다.
$$
Entropy  = -\sum_{i=1}^{K}p_i log_2(p_i)
$$
이 entropy는 분류 이후의 각각의 노드에서 구할 수 있다. k개의 카테고리가 있다고 한다면,  각각의 비율 p를 바탕으로 계산한 값이라고 할 수 있다. 순할(homogneity) 수록 그 값은 0에 가까워 지며, 불순도가 올라갈 경우 그 값은 1에 가까워 진다. 

## Gini impurity

같은 notation을 활용하여 구한 gini impurity의 공식은 아래와 같다.  세부적으로 다르나, 그 공식에 있어서 entropy와 비슷한 형태를 보이기는 한다. (좀더 정확히 말하면 Tsallis Entropy에 따른 impurity라 한다.)
$$
I_G(p)=\sum_{i=1}^K(p_i\sum_{j\neq i}p_j)
$$
수식 자체의 의미를 보면 어떤 집합에서 한 항목을 뽑아 무작위로 라벨을 추정할 때 틀릴 확률을 의미한다. 그래서 위의 entorpy처름 같은 집합에 모두 같은 항목이 있다면 최솟값 0을 가지게 된다. 위의 식을 정리하면 다음과 같이 가능하다.
$$
\sum_{i=1}^K(p_i\sum_{k\neq i}p_k) =
\sum_{i=1}^Kp_i(1-p_i) =
\sum_{i=1}^Kp_i - \sum_{i=1}^Kp_i^2 =
1-\sum_{i=1}^Kp_i^2
$$

따라서 보통 다음과 같이 쓴다.
$$
I_G(p)=1-\sum_{i=1}^Kp_i^2
$$

entorpy와의 차이라면, 최대값으로 0.5를 가진다는 점이 차이가 있다. 사실상 그 형태가 비슷하여 어느 것을 사용해도 비슷하다고 한다. CART에서는 gini impurity를 사용한다

# Information gain

gini inpurity보다 좀더 entropy개념을 직접적으로 사용한 형태로 그 공식은 아래와 같다. (사실 위의 entropy 공식과 동일하다.)
$$
I_E(p)=-\sum_{i=1}^K p_ilog_2 p_i
$$
이 엔트로피를 바탕으로 부모node와 자식 node간의 차이를 의미하는 것이 information gain이 된다. 간략히 식으로 쓰면 아래와 같다.
$$
{\rm Information ~  Gain} = 
{\rm parent's~} I_E(p) - {\rm sum ~ of ~ childern's~} I_E(p)
$$
위의 방식은 C4.5등에서 사용한다.



## 참고자료

[위키피디아](https://en.wikipedia.org/wiki/Decision_tree_learning) [등](https://en.wikipedia.org/)

[medium](https://medium.com/@jason9389/gini-impurity-and-entropy-16116e754b27) [등](https://towardsdatascience.com/gini-index-vs-information-entropy-7a7e4fed3fcb)