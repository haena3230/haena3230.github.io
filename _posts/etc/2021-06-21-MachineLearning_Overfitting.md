---
title: MachineLearning-Overfitting
categories: [etc]
comments: true
---

`빅데이터` 과목을 수강하며,`파이썬` 의 `scikitLearn` 이라는 머신러닝 라이브러리를 통해 데이터 분석 과제를 진행하게 되었습니다.

데이터셋을 준비하고 전처리하는 과정에서 오버피팅을 막기 위해 분할하는 과정이 필요했고, 오버피팅의 개념을 정리하기 위해 작성했습니다.

머신 러닝을 보면 결과적으로 입력 받은 데이타를 놓고, 데이타를 분류 (`Classification`) 하거나 또는 데이타에 인접한 그래프를 그리는 (`Regression`) , “선을 그리는 작업이다.”

그러면 선을 얼마나 잘 그리느냐가 머신 러닝 모델의 정확도와 연관이 되는데, 다음과 같이 붉은 선의 샘플 데이타를 받아서, 파란선을 만들어내는 모델을 만들었다면 잘 만들어진 모델입니다.

[![graph_1](https://t1.daumcdn.net/cfile/tistory/2614C650583E95AB2A)](https://t1.daumcdn.net/cfile/tistory/2614C650583E95AB2A)

## 언더 피팅

만약에 학습 데이타가 모자라거나 학습이 제대로 되지 않아서, 트레이닝 데이타에 가깝게 가지 못한 경우에는 다음과 같이 그래프가 트레이닝 데이타에서 많이 떨어진것을 볼 수 있는데, 이를 언더 피팅 (under fitting)이라고 합니다.

[![graph_2](https://t1.daumcdn.net/cfile/tistory/213B1950583E95AC32)](https://t1.daumcdn.net/cfile/tistory/213B1950583E95AC32)

## 오버 피팅

오버 피팅은 반대의 경우로, 다음 그림과 같이 트레이닝 데이타에 그래프가 너무 정확히 맞아 들어갈때 발생합니다.

[![graph_3](https://t1.daumcdn.net/cfile/tistory/22250250583E95AE32)](https://t1.daumcdn.net/cfile/tistory/22250250583E95AE32)

샘플 데이타에 너무 정확하게 학습이 되었기 때문에, 샘플데이타를 가지고 판단을 하면 100%에 가까운 정확도를 보이지만 다른 데이타를 넣게 되면, 정확도가 급격하게 떨어지는 문제입니다.

## 오버피팅의 해결

이런 오버피팅 문제를 해결하는 방법으로는 여러가지가 있는데 대표적인 방법으로는

- 충분히 많은 학습 데이타를 넣거나
- 피쳐의 수를 줄이거나
- Regularization (정규화)를 이용하는 방법이 있습니다.

출처: [https://bcho.tistory.com/1148](https://bcho.tistory.com/1148)
