# U-Net : Convolutional Networks for Biomedical Image Segmentation


> ### 주 목적 : 
> * 적은 양의 훈련 데이터(Valid-DataSet)으로 더 향상된 정확도의 리턴(지역화 정확도 : Localization Accuracy)


> ### 선결 과제 : 
> * 일반적인 사용은 "분류작업" 에 있다.
> * "선결 과제" 에서의 이미지에 대한 Return은 단일 클래스 Label이다.
> * * Localization가 포함된 단일 이미지 Label이어야 한다.

> ### 해결 :
> * "Ciresan" 외 Slide-Pixel 설정에서 네트워크를 훈련 시킨 후 해당 픽셀 주변의 Local Region을 제공하여 픽셀의 클래스레이블을 예측


> ### 장점 : 
> * N-Net는 Localization이 가능하다.
> * * Localization로 각 Layer를 규격화하여 쉽게 쌓을수(수정할수)있다.

> * Patch 측면에서 Train-Datas가 Train-Imgs보다 훨씬 크다.
> * * 학습할때 더 정확한 결과를 낼 수 있다.


> ### 단점 :

> * 패치별로 따로따로 훈련 네트워크를 진행해야 한다.
> * * 패치가 중복되는 경우가 많아서 속도가 상당히 느림

> * Localization Accuracy와 Context 가 반비례한다.
> * * Localization Accuracy를 올리기 위해 Path를 줄이면 Context를 거의 볼 수가 없고,
> * * Context를 보기 위해 Patches를 키우면 지역화 정확도가 줄어든다.
> * * 즉 두가지 Return을 만족하는 적절한 PatchSize를 찾아야 한다.

> * 최근에는 여러 계층의 특징을 고려한 분류 출력을 제안함.
> * * 양호한 Localization Accuracy와 ConText 사용이 가능함.


> ### 설명 : 
> * 해당 논문에서 "Fully Convolutional Network" 라는 보다 우아한 Architecture를 기반으로 한다.
> * * 보다 적은 수의 Valid-Imgs로, 보다 정확한 Localization를 산출한다.
> * * 주요 아이디어는 Pulling 연산자가 UpSampleing 연산자로 대체되는 연속적인 Layer로 Network를 보완하는것.
> * * 이 Layer들은 Return의 해상도를 증가시킨다.


> ### 출력 : 
> * U-Net에서 중요한 점은 UpSampling에서 많은 수의 피쳐 채널이 있어, 네트워크가 Context정보를 고해상도 Layer로 전파할수 있다는 점.
> * * 확장 경로는 수축 경로와 대칭적인 U자형 구조를 산출함.
> * * Localization Layer는 각 Convolutional Network의 유효한 부분의 픽셀만 포함하여 사용한다.
> * * 이미지의 테두리 영역에서 픽셀을 예측하기 위해 입력 이미지를 미러링하여 누락된 Context를 출력합니다.

