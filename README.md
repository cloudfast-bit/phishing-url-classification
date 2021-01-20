# Phishing-url-classification

## Contents 
### [01. Exploratory Data Analysis](https://github.com/hojisu/phishing-url-classification/blob/master/01-Exploratory-Data-Analysis/01-Exploratory-Data-Analysis.ipynb)
### [02. Preprocessing Feature Engineering](https://github.com/hojisu/phishing-url-classification/blob/master/02-Preprocessing-Feature-Engineering/02-Preprocessing-Feature-Engineering.ipynb)
### [03. Machine Learning Modeling & CNN](https://github.com/hojisu/phishing-url-classification/tree/master/03-Machine-Learning-Modeling%20%26%20CNN)
### [04. Final Model CAE & DanseNet](https://github.com/hojisu/phishing-url-classification/blob/master/04-Final-Model-CAE-DenseNet/05-Build-CAE-DanseNet-Model.ipynb)
### [05. Model Evaluation](https://github.com/hojisu/phishing-url-classification/blob/master/05-Model-Evaluation/06-Model_Evaluation.ipynb)
### [06. Phishing URL Detection PPT](https://github.com/hojisu/phishing-url-classification/blob/master/06-Phishing-URL-Detection-PPT/Phishing-URL-Detection-PPT.pdf)


## 데이터 
기관측된 피싱 URL 15,000 건, 정상 URL 45,000 건 사용

## Preprocessing & Feature Engineering
- URL로부터 문자 수준의 특징만을 추출하기 위해 각 문자는 UTF-8 인코딩 아래에서 고유한 유니코드 값으로 대체되어 One-hot 인코딩
- URL 문자 길이의 평균을 고려하여 최대 100자의 정수 시퀀스를 추출


## Machine Learning Modeling & Bulid CNN
- Logistic Regression, MultinomialNB, DecisionTreeClassifier, RandomForestClassifier, GradientBoostingClassifier, XGBClassifier 모델링
- MLP, CNN 모델링
- 모델별 10겹 교차검증을 통해 Recall을 비교 

## Final Model : CAE & DenseNet
- 공간특징추출에는 정상 URL 데이터로만 Convolutional Auto Encoder를 학습 및 인코더와 디코더를 통해 문자 수준의 정상 URL 특징을 압축 및 재구축
- 정상 URL의 특징만을 학습한 컨볼루션 오토인코더 모형에 피싱 URL이 입력되면 높은 오류를 가진 재구축 URL을 출력
- 재구축 오류 기반 피싱 URL 탐지 컨볼루션 신경망을 구축하여 재구축 된 URL의 문자 수준 특징을 간단한 신경망에 의해 정상/피싱 URL로 분류하였습니다.

## Model Evaluation
- 타 기계 학습과 제안한 모형의 Recall 비교 
  - 제안한 모델이 0.88로 성능이 가장 우수함
- Data Imbalance 대응 평가 수행을 위해 Test data의 피싱 데이터 비율을 증가시키며 performance 측정
  - 제안한 모형의 Accuracy가 0.96으로 성능이 가장 우수함을 확인


References
- Gao Huang, Zhuang Liu, Laurens van der Maaten, Kilian Q. Weinberger(2016). : Densely Connected Convolutional Networks.
- Jonathan Masci, Ueli Meier, Dan Cire¸san, and J¨urgen Schmidhuber(2011). : Stacked Convolutional Auto-Encoders for Hierarchical Feature Extraction
- Jaime Zabalza, Jinchang Ren, Jiangbin Zheng, Huimin Zhao, Chunmei Qing, Zhijing Yang,Stephen Marshall(2015). : Novel Segmented Stacked AutoEncoder for - Effective Dimensionality Reduction and Feature Extraction in Hyperspectral Imaging
