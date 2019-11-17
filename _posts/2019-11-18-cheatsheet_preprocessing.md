---
layout: post
title: 데이터 전처리 cheatsheet
tags:
  - Data Analysis
  - study-note
excerpt: 
---

# Data Preprocessing Cheat Sheet
2019-11-12: pandas getting started function application element-wise 전까지 공부함 https://pandas.pydata.org/pandas-docs/stable/getting_started/basics.html#function-application

## Useful References
- encoding: https://www.kaggle.com/lalitsomnathe/data-cleaning-challenge-character-encodings
```python
import chardet

with open("foreigner/foreigner.txt", 'rb') as rawdata:
    result = chardet.detect(rawdata.read(10000))

print(result)
```
- 결측치의 종류와 그에 따른 적절한 처리법: https://eda-ai-lab.tistory.com/14



## Initial Exploring
- 데이터프레임의 기본적인 attribute들: `df.shape`, `df.index`, `df.columns`, `df.dtypes`
- `df.info()` : 칼럼 이름, row 개수, non-null 값 개수, 데이터 형식 등
- `df.Column.describe()`: continuous value exploration. count, mean, std. min, max 등등. categorical에 대해서는 count, unique(unique한 value의 개수), top(가장 많이 등장한 value), freq(가장 높은 빈도수)
- `df.describe(include=['all'])`: 모든 칼럼(continuous, categorical 둘 다) 한꺼번에 describe하기 cf) `df.describe(include=['number'])`, `df.describe(exclude=['number'])`

### continuous
- `df.corr()`: continuous 변수 간 pairwise 상관계수 파악. 다중공선성(=독립변수 간 선형관계가 있는지) 파악. 회귀분석의 기본 전제 조건이다.

### categorical
- `df.Column.value_counts()`: categorical variable의 경우 각 값의 counts를 테이블 형태로 출력   
cf) `sns.countplot(dfColumn)`
- `df.Column.unique()`: list unique values of a column
- `df.Column.nunique()`: unique한 value의 개수

## 변수 검토
### 결측치 처리
- `df.isnull().sum().to_frame('nan_count')` : 칼럼 별 null값의 개수를 데이터 프레임으로 만들기('nan_count'는 칼럼 이름)
- `df[df.isnull().any(axis=1)]` : 결측치를 하나라도 포함한 row 선택하기
- `df.fillna()`: 결측치를 대표값(평균, 중앙값, 최빈값)으로 채우기 `df.fillna(df.mean(), inplace=True)` 
- `df.dropna()`: 결측치를 포함한 row 모두 삭제

### 이상치 처리
- `df.boxplot(column=['Col1', 'Col2'], by='Col3')`: 칼럼 별로 박스플랏 그리기. by=에 categorical column 넣어서 해당 카테고리의 밸류 별로 그려볼 수도 있다
- Z-score가 -3이하(평균-3표준편차)이거나 +3이상(평균+3표준편차)일경우 이상치로 판단
- IQR: Q3+1.5\*IQR 이상이거나 Q1-1.5\*IGA 이하면 이상치로 판단
> IQR이란: Interquartile Range. Q1부터 Q3까지의 거리

## 변수 변환
### bucketing: continuous values into discrete values
- `pd.cut(dfColumn, bins, labels)`: data를 bins를 기준으로 해서 새로운 label 부여
- `pd.qcut(dfColumn, quantilebins)`: pd.qcut(arr, [0, .25, .5, .75, 1]) 이런 식으로 해당하는 quantile을 계산한 이후 이를 bins로 해서 데이터 분할

### 더미변수화
- `DataFrame.select_dtypes(exclude='number').nunique()`: category 변수의 unique value 개수 확인하기(주소, 아이디 이런 것 까지 더미변수화하면 안되니까...)
- `pd.get_dummies(df, prefix=col, columns=[col])`

## DataFrame Handling
### Entire DataFrame Handling

- 두 개의 칼럼 value를 비교해 특정 조건을 만족시키는 row만 따로 저장하기: `df.loc[]`  
  
> `df[df['event']=='stim']`: pure python   
> `df.loc[df['event']=='stim']`: pandas  
> `df.query("event == 'stim'")`: query문을 스트링 형태로 parameter로 전달  
  
- cell value에 대한 조건으로 데이터 프레임 일부를 불러오기: `df.where(conditional)`
- (conditional on df)`.any()`, `.all()`: df를 칼럼별로 보며 해당 condition을 모든 셀이 만족하는가 / 하나라도 만족하는 셀이 있는가 --> 결과도 칼럼별 True/False [boolean reduction](
https://pandas.pydata.org/pandas-docs/stable/getting_started/basics.html#boolean-reductions)

### functions on Series(row-wise/column-wise)
- list comprehension
- `DataFrame.apply`: df.apply(func)으로 칼럼 하나 씩 함수를 적용하는 방법/df['column_name'].apply(func)처럼 한 칼럼의 셀값에 대해서 함수를 적용하는 방법
    - `df.apply(func)`의 func 자리에 Series methods를 넣을 수 있다
    - `reduce`, `broadcast`, `expand` 등을 통해 result의 형태를 조정할 수 있다
- `DataFrame.agg([func1, func2])`: 여러 개의 함수를 동시에 계산하고자 할 때. `DataFrame.Column.agg()`처럼 하나의 칼럼에만 적용할 수도 있다.

### value에 직접 접근하기
- `df.array`(그대로) / `df.to_numpy()`(dtype coercing): 데이터 프레임 혹은 그 일부를 array 혹은 numpy array로 전환. 공식 문서에서는 이를 `.values`보다 추천함
- cell value를 바꾸기: `df.replace()`

### Element Handling

## stuff of course
- standardization
```python
from sklearn.preprocessing import StandardScaler
scaler = StandardScaler()
data = scaler.fit_transform(data)
```
- label encoder
```python
from sklearn.preprocessing import LabelEncoder
labeler = LabelEncoder()
data = labeler.fit_transform(data)
```
- feature/label divide
- training/test divide

## Machine Learning Flow
1. create a model
2. train a model
3. test a model
4. estimate a model: `sklearn.metrics` 이용
- `classification_report`: precision, recall, f1-score, support
- `confusion_matrix`: 이를 heatmap으로 그리면 빈도를 더 쉽게 파악할 수 있다
