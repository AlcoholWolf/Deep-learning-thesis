# U-Net : Convolutional Networks for Biomedical Image Segmentation
* 리뷰 저자 : 최현우[A.W.] / 논문 저자 : Computer Science Department of the University of Freiburg
------------------------------------------------------------------------------------------------------------------------

### 주 목적 : 
* 적은 양의 훈련 데이터(Valid-DataSet)으로 더 향상된 정확도의 리턴(지역화 정확도 : Localization Accuracy)

------------------------------------------------------------------------------------------------------------------------

> ## 선결 과제
> * 첫번째 선결 과제 : 
> * * 일반적인 사용은 "분류작업"
> * * 이미지에 대한 Return은 Localization가 포함된 단일 Imag-Class-Label
> * 두번쨰 선결 과제 :
> * * 처리한 개체 중 동일한 클래스의 접촉 객체 분리
> * * 첫번쨰 과제에서 분리 한 객체 중 2개의 같은 객체가 서로 붙어있어, 한 객체로 취급된 경우 각각 다른 개체로 인식하기 위함
> * * 편의 상 "접촉한 하나의 클래스를 가진 여러 객체" 를 객체(ᚋ ᚌ ᚍ ᚎ ᚏ)라고 부르겟습니다.

> ### 해결 :
> * "Ciresan" 외 Slide-Pixel 설정에서 Net을 훈련 시킨 후 해당 Pixel 주변의 Local Region을 제공하여 Pixel의 Class-Label을 예측
> * 객체(ᚋ) 의 분리를 진행하기 위해 BackGraund-Label의 손실 함수 중 가장 큰 가중치를 가진 함수의 사용을 제안(권장사항)


> ### 장점 : 
> * N-Net는 Localization 가능
> * * Localization로 각 Layer를 규격화하여 쉽게 쌓을수(수정할수)있음
> * Patch 측면에서 Train-Datas가 Train-Imgs보다 훨씬 큼
> * * 학습시 더 정확한 결과 도출


> ### 단점 :
> * 패치별로 따로 훈련 네트워크를 진행해야 함
> * * 패치가 중복되는 경우가 많아서 속도가 상당히 느림
> * Localization Accuracy와 Context(해상도) 가 반비례함
> * * Localization Accuracy를 올리기 위해 Path를 줄이면 Context를 거의 볼 수가 없음
> * * Context를 보기 위해 Patches를 키우면 Localization Accuracy가 줄어들음
> * * 즉 두가지 Return을 만족하는 적절한 PatchSize를 찾아야 함
> * 최근에는 여러 계층의 특징을 고려한 분류 출력을 제안
> * * 양호한 Localization Accuracy와 ConText 사용이 가능


> ### 설명 : 
> * 해당 논문에서 "Fully Convolutional Network" 라는 보다 우아한 Architecture를 기반
> * * 보다 적은 수의 Valid-Imgs로, 보다 정확한 Localization를 산출
> * * 주요 아이디어는 Pulling 연산자가 UpSampleing 연산자로 대체되는 연속적인 Layer로 Network를 보완
> * * 해당 Layer들은 Return의 해상도를 증가


> ### 출력 : 
> * U-Net에서 중요한 점은 UpSampling에서 많은 수의 피쳐 채널이 있어, 네트워크가 Context정보를 고해상도 Layer로 전파할수 있음
> * * 확장 경로는 수축 경로와 대칭적인 U자형 구조를 산출
> * * Localization Layer는 각 Convolutional Network의 유효한 부분의 픽셀만 포함하여 사용
> * * 이미지의 테두리 영역에서 픽셀을 예측하기 위해 입력 이미지를 미러링하여 누락된 Context를 출력

------------------------------------------------------------------------------------------------------------------------

> ### 이 외 설명 :
> * U-Net의 논문을 참조, 해당 논문의 용어와 풀이를 서술하겟습니다.
> ## 
> * 확률적 경사 하강
> 추가되지 않은 Convolutional Network Layer로 인해 Return-Image가 Input-Imag보다 일정한 테두리 폭 만큼 작다.
> 과적합을 최소화하고, GPU를 최대한 활용하기 위해 큰 배치 크기보다 큰 입력 타일을 선호한다.
> 그렇기에 높은 모멘텀(0.99)를 사용하여 훈련 샘플이 현재의 최적화 단계에서 업데이트를 진행한다.
> * * 에너지 함수
> 에너지 함수는 CrossEntropy-LossFunction와 결합된 최종 맵의 형상에 대해 픽셀 단위로 Soft-MaxFunction으로 계산된다.
------------------------------------------------------------------------------------------------------------------------
![image](https://user-images.githubusercontent.com/102508669/180706716-22f7eaa4-06c0-4602-9584-1565cae2ea5a.png)
------------------------------------------------------------------------------------------------------------------------
> 분리 경계는 형태학적 역산을 사용하여 계산. 다음과 같이 계산됨.
------------------------------------------------------------------------------------------------------------------------
![image](https://user-images.githubusercontent.com/102508669/180706491-c5a574c9-ad93-4c0d-979c-ccff6967b7c7.png)
------------------------------------------------------------------------------------------------------------------------
> * PDF Link : https://drive.google.com/drive/folders/1KlDXgiaoTtIhrl6qW0rS-HrWHf-dOJNl?usp=sharing
> * 참고한 논문 리뷰 : https://medium.com/@msmapark2/u-net-%EB%85%BC%EB%AC%B8-%EB%A6%AC%EB%B7%B0-u-net-convolutional-networks-for-biomedical-image-segmentation-456d6901b28a
> * 추가 설명 : https://www.quantumdl.com/entry/%EB%94%A5%EB%9F%AC%EB%8B%9D%EC%9D%84-%EC%9C%84%ED%95%9C-Atrous-Convolution%EA%B3%BC-UNet-%EA%B5%AC%EC%A1%B0-%EA%B0%84%EB%9E%B5%ED%95%9C-%EC%97%AD%EC%82%AC
> * * 한국어 번역본은 Naver의 Papago로 번역한 것임을 밝힙니다.
> * * 해당 번역본은 오번역이 있을수 있으므로 원본 논문과 비교해가며 읽는걸 추천합니다.
> * * 함수 관련 부분은 제가 더 리뷰하거나 요약할 부분 없이 논문 그 자체로 깔끔하기에, 이상으로 리뷰를 마칩니다.
