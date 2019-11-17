---
layout: post
title: 머신러닝 플로우 cheatsheet
tags:
  - Data Analysis
  - Machine Learning
  - study-note
excerpt: 
---

## Data transformation
- standardization
```python
from sklearn.preprocessing import StandardScaler
scaler = StandardScaler()
data = scaler.fit_transform(data)
```
- label encoder: categorical 변수의 각 value들에 번호 매기기
```python
from sklearn.preprocessing import LabelEncoder
labeler = LabelEncoder()
data = labeler.fit_transform(data)
```
- feature/label divide
```python
X = df[:, ['Col1', 'Col2', 'Col3']]
# X = df[['Col1', 'Col2', 'Col3']].to_frame()
y = df['Col4'].to_frame()
```
- training/test divide
```python
from sklearn.model_selection import train_test_split
X_train, X_test, y_train, y_test = train_test_split(X, y, test_size=.3, random_state=1)
```

## Feature Selection

## Frameworks
![machine learning map](https://scikit-learn.org/stable/_static/ml_map.png)
### Prediction
:연속적인 값을 예측하라
regression
### Classification
:카테고리가 정해져 있을 때, 아이템이 어떤 카테고리에 속하는가?
logistic regression
### Clustering
:카테고리가 정해져 있지 않을 때(혹은 알지 못할 때), 아이템이 어떤 카테고리에 속하는가?

## Model Selection
: 여러 모델의 유효성을 검증하고, 비교하고, 모델 내의 하이퍼 파라미터를 조정함으로써 최종적으로 활용할 모델을 선택한다.

### cross validation
테스트 데이터에서 성능이 좋은 파라미터를 시험하면 unseen data의 역할을 해야할 데이터가 지식에 노출되고, 더 이상 일반화된 성능을 측정한다고 보기가 어려우므로 validation set을 따로 만들어준다. 이 때 트레이닝 셋의 샘플 수가 줄어드는 것을 막기 위해 cross-validation을 실행하는 것. cf) [sklearn 공식문서](https://scikit-learn.org/stable/modules/cross_validation.html)
```python
from sklern import metrics
from sklearn.model_selection import cross_val_score
scores = cross_val_score(아직fit하지않은learner, X_train, y_train, cv=5, score=원하는metric)
```
### parameter tuning
`GridsearchCV`(grid search + cross validation)으로 하기 cf) [사용법 블로그](https://stackabuse.com/cross-validation-and-grid-search-for-model-selection-in-python/) 참고
1. estimator 최초 구축(fit X)
2. `esimator.get_params()`를 통해 변경할 수 있는 하이퍼 파라미터 확인하기
3. search할 파라미터의 목록 정리 후 만들어뒀던 estimator로 GridSearchCV 오브젝트 생성
```python
parameters = {'kernel':['linear', 'rbf'], 'C':[1, 10]}
model = GridSearchCV(먼저만들어놨던estimator, parameters, cv=5)
```
4. model 학습 후 가장 좋은 파라미터 조합 도출
```python
model.fit(X_train, y_train)
print(model.best_params_) # => {'kernel':'linear', 'C':10}
```


## Model Estimation
: 모델의 성능을 평가한다.
`sklearn.metrics` 이용
### classification
- `classification_report`: precision(_positive로 예측한 것 중 실제로 positive한 것이 얼마나 있는가_), recall(_=sensitivity: positive의 멤버를 올바르게 감지하는가_), f1-score, support
- `confusion_matrix`: 이를 heatmap으로 그리면 빈도를 더 쉽게 파악할 수 있다


