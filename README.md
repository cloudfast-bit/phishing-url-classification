# Phishing-url-classification

## Contents 
### [01. Exploratory Data Analysis](https://github.com/hojisu/phishing-url-classification/tree/master/01-Exploratory-Data-Analysis)
### [02. Preprocessing Feature Engineering](https://github.com/hojisu/phishing-url-classification/tree/master/02-Preprocessing-Feature-Engineering)
### [03. Machine Learning Modeling & CNN](https://github.com/hojisu/phishing-url-classification/tree/master/03-Machine-Learning-Modeling%20%26%20CNN)
### [04. Final Model CAE & DanseNet](https://github.com/hojisu/phishing-url-classification/tree/master/04-Final-Model-CAE-DanseNet)
### [05. Model Evaluation](https://github.com/hojisu/phishing-url-classification/tree/master/05-Model_Evaluation)
### [06. Phishing URL Detection PPT](https://github.com/hojisu/phishing-url-classification/blob/master/06-Phishing-URL-Detection-PPT/Phishing-URL-Detection-PPT.pdf)

***

## 데이터 
오픈소스 웹 디렉터리 저장소로부터 다수의 URL 을 수집하였습니다. 기관측된 피싱 URL 을 데이터베이스화 하는 오픈소스 프로젝트인 Phishtank 로부터 15,000 건의 실제 피싱 URL 을 수집하였고, 웹에 게시된 페이지를 카테고리화 한 데이터베이스를 공유하는 Dmoz-ODP 프로젝트로부터 45,000 건의 정상 URL 을 수집하였습니다.

## Preprocessing & Feature Engineering
URL로부터 문자 수준의 특징만을 추출하기 위해 각 문자는 UTF-8 인코딩 아래에서 고유한 유니코드 값으로 대체되어 One-hot 인코딩이 선행되었습니다. URL 문자 길이의 평균을 고려하여 최대 100자의 정수 시퀀스를 추출하였습니다. 문자에 할당된 정수의 시퀀스를 학습하기 때문에 단어레벨로 임베딩한 URL 벡터 대비 문자 레벨의 저수준 특징까지 고려하였습니다.

## Machine Learning Modeling & Bulid CNN
Logistic Regression, MultinomialNB, DecisionTreeClassifier, RandomForestClassifier, GradientBoostingClassifier, XGBClassifier, MLP, CNN 모델별 10겹 교차검증을 통해 Recall을 비교하였다. 기계학습 모델 중 Recall 성능이 가장 높은 CNN 모델을 Final model과 성능 비교를 하였다. 

## Final Model CAE & DenseNet
공간특징추출에는 정상 URL 데이터로만 Convolutional Auto Encoder를 학습시켰습니다. 인코더와 디코더를 통해 문자 수준의 정상 URL 특징을 압축 및 재구축하였습니다. 정상 URL의 특징만을 학습한 컨볼루션 오토인코더 모형에 피싱 URL이 입력되면 높은 오류를 가진 재구축 URL을 출력합니다. 재구축 오류 기반 피싱 URL 탐지 컨볼루션 신경망을 구축하여 추출된 재구축 URL 문자 수준 특징을 간단한 신경망에 의해 정상/피싱 URL로 분류하였습니다.

## Model Evaluation
성능을 검증하기 위해 타 기계 학습 모델과 재현율을 비교하였고 제안한 모델이 0.91로 가장 높은 재현율을 달성하였습니다. Zero-day 피싱 공격 상황을 가정하여 클래스 불균형에 따른 재현율과 정확도를 평가하기 위해 2가지 실험을 진행하였습니다. 첫 번째로 무작위로 생성되는 피싱 URL의 특성을 반영하기 위해 Input URL에 백색잡음을 추가하는 방법을 고려하였습니다. CNN 정확성은 점점 떨어지는 데 반해 제안한 모델의 정확성은 변함이 없었습니다. 두 번째 실험은 데이터 불균형 문제에 대해 실험을 하였습니다. Test data의 피싱데이터 비율을 증가시키면서 performance 측정하였고 결과는 기존의 수준을 크게 상회하여 제안한 피싱 URL 탐지 모델의 성능을 검증하였습니다.

## 결론 및 향후 연구
Zero-day 특성을 고려한 피싱 URL 의 탐지 방법을 위해 문자 수준의 컨볼루션 오토인코더를 구현하고 정상 URL 의 모형을 학습하였다. 추가로 재구축 오류에 기반한 정상 및 피싱 URL 분류용 컨볼루션 신경망을 제안함으로서 기존 딥러닝 및 기계학습 알고리즘에 대비하여 최고 정확도를 달성하였고, 10 겹 교차검증하였습니다. 
제안하는 방법에서는 정상 URL 모형에서 벗어나는 정도를 가늠하기 위한 피싱공격 점수를 가장 직관적인 형태의 유클리디안 거리로 정의하였으나, 특징공간 상에서의 고찰이 필요하다. 또한 오토인코더의 재구축 픽셀공간 상의 잡음에 대한 베이지안 접근 방식의 딥러닝 모형을 고려해볼만 합니다.

## References
- Gao Huang, Zhuang Liu, Laurens van der Maaten, Kilian Q. Weinberger(2016). : Densely Connected Convolutional Networks.
- Jonathan Masci, Ueli Meier, Dan Cire¸san, and J¨urgen Schmidhuber(2011). : Stacked Convolutional Auto-Encoders for Hierarchical Feature Extraction
- Jaime Zabalza, Jinchang Ren, Jiangbin Zheng, Huimin Zhao, Chunmei Qing, Zhijing Yang,Stephen Marshall(2015). : Novel Segmented Stacked AutoEncoder for - Effective Dimensionality Reduction and Feature Extraction in Hyperspectral Imaging
