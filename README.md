# Phishing-url-classification

## Contents 
### [01. Exploratory Data Analysis](https://github.com/hojisu/phishing-url-classification/tree/master/01-Exploratory-Data-Analysis)
### [02. Preprocessing Feature Engineering](https://github.com/hojisu/phishing-url-classification/tree/master/02-Preprocessing-Feature-Engineering)
### [03. Machine Learning Modeling & CNN](https://github.com/hojisu/phishing-url-classification/tree/master/03-Machine-Learning-Modeling%20%26%20CNN)
### [04. Final Model CAE & DanseNet](https://github.com/hojisu/phishing-url-classification/tree/master/04-Final-Model-CAE-DanseNet)
### [05. Model Evaluation](https://github.com/hojisu/phishing-url-classification/tree/master/05-Model_Evaluation)

***

## 데이터 
오픈소스 웹 디렉터리 저장소로부터 다수의 URL 을 수집하였다. 기관측된 피싱 URL 을 데이터베이스화 하는 오픈소스 프로젝트인 Phishtank 로부터 15,000 건의 실제 피싱 URL 을 수집하였고, 웹에 게시된 페이지를 카테고리화 한 데이터베이스를 공유하는 Dmoz-ODP 프로젝트로부터 45,000 건의 정상 URL 을 수집하였다.

## Preprocessing & Feature Engineering
URL 으로부터 문자수준의 특징만을 추출하기 위해 각 문자에 ord함수를 사용하여 정수를 할당하고 One-hot 인코딩을 하였습니다. 

## Machine Learning Modeling & Bulid CNN
Logistic Regression, MultinomialNB, DecisionTreeClassifier, RandomForestClassifier, GradientBoostingClassifier, XGBClassifier, MLP, CNN 모델별 10겹 교차검증을 통해 Recall을 비교하였다.
머신러닝 모델과 CNN 모델 중 Recall 성능이 가장 좋은 CNN 모델을 Final model과 성능 비교를 하였다. 

## Final Model CAE & DenseNet
컨볼루션 오토인코더 기반 문자수준 정상URL 특징만을 압축 및 재구축을 통해 학습한다. 피싱 URL 입력에 대해 높은 재구축 오류를 가진 재구축URL을 출력한다. 입력된 정상 URL과 재구축된 URL 사이의 차이는 유클리디안 거리로 계산할 수 있는데, 컨볼루션 오토인코더의 손실함수를 정의하고 경사 하강법에 의해 최소화할 수 있는 신경망의 파라미터를 탐색한다. 오토인코더 기반의 이상탐지모형 연구 분야에서는 간단한 문턱값을 가지고 정상 인스턴스와 비정상 인스턴스를 구별할 수가 없음이 알려져 있다. 직관적으로는 피싱 URL 의 생성과정에서 실제 URL 에 주로 사용되는 문자열을 사용하는 사례로 이해할 수 있다. 따라서 입력된 문자수준 별로 문턱값을 설정할 수 있도록 하는 재구축 데이터에 대한 별도의 신경망이 필요하다 생각하여 DenseNet 모형을 이용한 재구축 오류 기반 피싱 URL 분류용 컨볼루션 신경망을 구축하였다. 

## Model Evaluation
첫번째로 잡음 강건성에 대해 테스트 하였다. 잡음의 가정은 표준정규분포의 시그마제곱에 strength을 더하여 잡음을 생성하였다. CNN 모델은 강건성이 점점 떨어지는데 반해 우리의 모델은 변함이 없음을 확인하였다. 두번째로 Zero-day 피싱공격 상황을 가정하여 클래스 불균형에 따른 기존의 수준을 크게 상회하여 제안하는 방법의 피싱 URL 탐지로서의 성능을 검증하였다

## 결론 및 향후 연구
Zero-day 특성을 고려한 피싱 URL 의 탐지 방법을 위해 문자 수준의 컨볼루션 오토인코더를 구현하고 정상 URL 의 모형을 학습하였다. 추가로 재구축 오류에 기반한 정상 및 피싱 URL 분류용 컨볼루션 신경망을 제안함으로서 기존 딥러닝 및 기계학습 알고리즘에 대비하여 최고 정확도를 달성하였고, 10 겹 교차검증하였다.
제안하는 방법에서는 정상 URL 모형에서 벗어나는 정도를 가늠하기 위한 피싱공격 점수를 가장 직관적인 형태의 유클리디안 거리로 정의하였으나, 특징공간 상에서의 고찰이 필요하다. 또한 오토인코더의 재구축 픽셀공간 상의 잡음에 대한 베이지안 접근 방식의 딥러닝 모형을 고려해볼 만 하다.

## References
- Gao Huang, Zhuang Liu, Laurens van der Maaten, Kilian Q. Weinberger(2016). : Densely Connected Convolutional Networks.
- Jonathan Masci, Ueli Meier, Dan Cire¸san, and J¨urgen Schmidhuber(2011). : Stacked Convolutional Auto-Encoders for -Hierarchical Feature Extraction
- Jaime Zabalza, Jinchang Ren, Jiangbin Zheng, Huimin Zhao, Chunmei Qing, Zhijing Yang,Stephen Marshall(2015). : Novel Segmented Stacked AutoEncoder for - Effective Dimensionality Reduction and Feature Extraction in Hyperspectral Imaging
