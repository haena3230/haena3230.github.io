---
title: MachineLearning-scikit learn
categories: [etc]
comments: true
---

`빅데이터` 과목을 수강하며, 머신러닝의 실습으로 `scikit learn` 라이브러리를 사용했습니다. 해당 라이브러리에 대한 기본적인 내용을 정리해 두기 위해 작성합니다.

## 기본 기능

머신러닝에 사용되는 지도/`비지도 학습 알고리즘을 제공하는 파이썬 라이브러리로, `Numpy`,`Pandas`,`Matplotlib` 와 같이 널리 쓰이는 기술을 기반으로 합니다.

1. 지도학습, 비지도 학습 모듈
2. 모델 선택 및 평가 모듈
3. 데이터 변환 및 데이터 불러오기 위한 모듈
4. 계산 성능 향상을 위한 모듈

오픈소스로 공개되어 있고, 샘플 데이터 셋이 부속되어 있어 머신러닝을 배우기 시작할 때 사용해보기 좋은 라이브러리 입니다.

## 설치

jupyter notebook를 사용한다면

```jsx
pip install scikit-learn
```

anaconda를 사용한다면

```jsx
conda install scikit-learn
```

을 통해 설치합니다.

## Cheat-sheet

scikit learn을 사용할 때, 자신이 하고 싶은 분석(분류/회귀/클러스터링 등)에 대해 선택할 때, 다음의 그림을 참고하여 적합한 모델을 선택할 수 있습니다.

![https://s3-us-west-2.amazonaws.com/secure.notion-static.com/a96eb755-29c7-483a-8c5f-9f32370c77d4/Untitled.png](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/a96eb755-29c7-483a-8c5f-9f32370c77d4/Untitled.png)

(출처 : [https://scikit-learn.org/stable/tutorial/machine_learning_map/](https://scikit-learn.org/stable/tutorial/machine_learning_map/))

# 분석 종류

- Classification(분류)

주어진 데이터가 어느 클래스에 속하는지 판별

- Regression(회귀)

전달된 데이터를 바탕으로 값을 예상

- Clustering(클러스터링)

절달된 데이터를 어떤 규칙에 따라 나누는 것

- Dimensional reduction(차원감소)

데이터의 차원 수가 많은 경우 학습 효율이 떨어지므로, 차원을 감소시키는 전처리 진행

- Model selection
- Preprocessing

# 학습세트와 평가세트

머신러닝을 통해 생성한 분류 모델이 얼마나 정확한지 확인하는 과정은 중요합니다. 지도학습에서는 이미 레이블링 된 데이터가 있기 때문에 그것을 활용하여 알고리즘의 정확도를 테스트할 수 있습니다.

- 학습 세트 (Training Set)
- 검증 세트(Validation Set)
- 평가 세트(Test Set)

학습세트는 말 그대로 **알고리즘이 학습할 데이터**입니다.

학습세트를 통해 모델을 학습시키고 이후에 검증세트를 통해 **모델의 예측/분류 정확도**를 계산할 수 있습니다.

`정확도`가 만능의 지표는 아니기 때문에 `정밀도` 와 `재현율` 그리고 정확률과 정밀도의 조화 평균인 `F1` 점수를 확인하는 방법도 있습니다.

### (1) 세트를 나누는 방법

학습세트 vs 검증세트를 어떤 비율로 분할할지 판단하기가 쉽지 않습니다. 학습 세트가 너무 작으면 알고리즘이 효과적으로 학습하기에 충분치 않을 수 있고, 검증 데이터가 너무 작으면 이를 통해 계산한 정확도(Accuracy), 정밀도(precision), 재현율(recall), F1 점수가 서로 차이가 많이 나서 신뢰하기 어려울 수 있습니다.

일반적으로 **전체 데이터 중 80%를 학습으로, 20%를 검증으로 사용하는 것이 좋다**고 합니다.

나누는 방법은 scikit-learn에서 친절하게 제공합니다.

```python
**from** sklearn.model_selection **import** train_test_split
training_data, validation_data , training_labels, validation_labels = train_test_split**(**x, y, train_size=0.8, test_size=0.2**)**
```

### (2) **교차 검증 (N-Fold Cross-Validation)**

데이터가 충분치 않을 경우 80/20으로 나누면 여전히 많은 양의 분산이 발생합니다. 이에 대한 한 가지 해결책은 **N-Fold 교차 검증**을 수행하는 것, 즉 **전체 프로세스를 N번 수행하고 모든 수행에서 나온 정확도의 평균을 구하는 방법입니다.**

예를 들어, 10번 교차 검증을 한다고 하면 아래와 같은 그림으로 표현할 수 있습니다.

![http://hleecaster.com/wp-content/uploads/2019/12/10fold.jpg](http://hleecaster.com/wp-content/uploads/2019/12/10fold.jpg)

[출처](<[http://hleecaster.com/ml-training-validation-test-set/](http://hleecaster.com/ml-training-validation-test-set/)>)

당연히 이**렇게 구한 정확도의 평균 값이 아무래도 모델의 평균 성능을 더 잘 나타낸다**고 볼 수 있습니다.

그리고 이걸 일일이 하기 귀찮으니 scikit-learn에서는 아예 이런 기능을 제공하고 있기도 합니다.

```python
**from** sklearn.model_selection **import** KFold
```

### (3) **모델 성능 개선 및 평가 세트(Test Set)**

모델의 파라미터를 세부적으로 튜닝하면 그 모델의 성능을 향상시킬 수 있습니다. 예를 들어, K-최근접 이웃(K-Nearest Neighbors) 알고리즘에서는 K를 늘리거나 줄일 때 정확도가 어떻게 달라지는지 확인할 수 있습니다. (참고: [K-최근접 이웃 (K-Nearest Neighbor) 파이썬 코드 예제](http://hleecaster.com/ml-knn-example))

만약 모델의 성능이 어느정도 만족스럽다면 **평가 세트(Test Set)**를 넣어볼 수 있습니다. 이건 **실제 데이터로,** 어떤 예측/분류가 일어날지 궁금한 값을 만들어 넣어줄 수도 있고, 새롭게 얻은 데이터일 수도 있으며, 애초에 모델을 생성하기 전에 일부러 따로 떼어놓은 데이터일 수도 있습니다. 어떻게 보면 **검증 세트(Validation Set)와 비슷하지만, 모델을 구축하거나 튜닝할 때 포함된 적 없다는 점에서 차이가 있습니다.**

평가 세트(Test Set)에서 모델이 예측/분류해준 값과 실제 값을 비교해서 정확도(Accuracy), 정밀도(precision), 재현율(recall), F1 점수를 계산해봄으로써 **알고리즘이 현실 세계에서 얼마나 잘 수행되는지** 이해할 수 있게 됩니다.

# 패키지 내 모듈

## **[Linear Regression](http://scikit-learn.org/stable/modules/generated/sklearn.linear_model.LinearRegression.html)**

**Import and create the model**

```python
**from** sklearn.linear_model **import** LinearRegression
your_model = LinearRegression**()**
```

**Fit**

```python
your_model.fit**(**x_training_data, y_training_data**)**
```

- `.coef_`: contains the coefficients
- `.intercept_`: contains the intercept

**Predict**

```python
predictions = your_model.predict**(**your_x_data**)**
```

- `.score()`: returns the coefficient of determination R²

---

## **[Naive Bayes](http://scikit-learn.org/stable/modules/generated/sklearn.naive_bayes.MultinomialNB.html#sklearn.naive_bayes.MultinomialNB))**

**Import and create the model**

```python
**from** sklearn.naive_bayes **import** MultinomialNB
your_model = MultinomialNB**()**
```

**Fit**

```python
your_model.fit**(**x_training_data, y_training_data**)**
```

**Predict**

```python
# Returns a list of predicted classes - one prediction for every data point
predictions = your_model.predict(your_x_data)

# For every data point, returns a list of probabilities of each class

probabilities = your_model.predict_proba(your_x_data)
```

---

## **[K-Nearest Neighbors](http://scikit-learn.org/stable/modules/generated/sklearn.neighbors.KNeighborsClassifier.html#sklearn.neighbors.KNeighborsClassifier)**

**Import and create the model**

```python
**from** sklearn.neigbors **import** KNeighborsClassifier
your_model = KNeighborsClassifier**()**
```

**Fit**

```python
your_model.fit**(**x_training_data, y_training_data**)**
```

**Predict**

```python
# Returns a list of predicted classes - one prediction for every data point
predictions = your_model.predict(your_x_data)

# For every data point, returns a list of probabilities of each class

probabilities = your_model.predict_proba(your_x_data)
```

---

## **[K-Means](http://scikit-learn.org/stable/modules/generated/sklearn.cluster.KMeans.html)**

**Import and create the model**

```python
**from** sklearn.cluster **import** KMeans
your_model = KMeans**(**n_clusters=4, init='random'**)**
```

- `n_clusters`: number of clusters to form and number of centroids to generate
- `init`: method for initialization
  - `k-means++`: K-Means++ [default]
  - `random`: K-Means
- `random_state`: the seed used by the random number generator [optional]

**Fit**

```python
your_model.fit**(**x_training_data**)**
```

**Predict**

```python
predictions = your_model.predict**(**your_x_data**)**
```

---

그리고 데이터를 학습 세트와 시험 세트로 분리하여 `overffiting`을 방지하고 학습한 뒤 테스트를 해야합니다.

### **[Training Sets and Test Sets](http://scikit-learn.org/stable/modules/generated/sklearn.model_selection.train_test_split.html)**

```python
**from** sklearn.model_selection **import** train_test_split
x_train, x_test, y_train, y_test = train_test_split**(**x, y, train_size=0.8, test_size=0.2**)**
```

- `train_size`: the proportion of the dataset to include in the train split
- `test_size`: the proportion of the dataset to include in the test split
- `random_state`: the seed used by the random number generator [optional]

---

이번엔 학습시킨 모델이 얼마나 괜찮은 성능을 보여주는지 확인하는 방법입니다.

## **[Validating the Model](http://scikit-learn.org/stable/modules/classes.html#sklearn-metrics-metrics)**

**Import and print accuracy, recall, precision, and F1 score:**

```python
**from** sklearn.metrics **import** accuracy_score, recall_score, precision_score, f1_score
print(accuracy_score(true_labels, guesses))

print(recall_score(true_labels, guesses))

print(precision_score(true_labels, guesses))

print(f1_score(true_labels, guesses))
```

**Import and print the confusion matrix**

```python
**from** sklearn.metrics **import** confusion_matrix
print**(**confusion_matrix**(**true_labels, guesses**))**
```

자세한 내용은 [해당](<[http://hleecaster.com/ml-accuracy-recall-precision-f1/](http://hleecaster.com/ml-accuracy-recall-precision-f1/)>) 포스팅을 참고

---

[공식 사이트 연습문제](<[https://scikit-learn.org/stable/tutorial/basic/tutorial.html](https://scikit-learn.org/stable/tutorial/basic/tutorial.html)>)
