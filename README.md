## Image Processing 기초

### 09.16
#### Otsu's Algorithm 직접 구현
이미지는 많은 정보를 함축한다. 몇몇 정보는 아무 작업을 거치지 않고, 눈으로만 보아도 알아낼 수 있지만, 전처리를 해야만 보이는 정보들도 있다. Image Processing의 기본 스킬 중 하나는 이미지 이진화이다. 쉽게 말하면 컬러사진을 흑백사진으로 바꾸는 과정이다. 
이미지를 행렬로 본다면, 칼라 이미지는 빨강, 초록, 파랑을 가진 3 채널의 행렬로 생각할 수 있다. 이 3채널을 특정한 규칙에 따라 0(흰색)에서 255(검은색)의 범위 내의 값을 갖는 1차원 행렬로 바꾸는 것이다. opencv 모듈에서는 cv2.threshold, cv2.adaptiveThreshold가 이 기능을 수행한다.
예를 들어, cv2.threshold(img,threshold, v, cv2.THRESH_BINARY)는 각각의 채널의 행렬에 대해 threshold를 넘은 값들에 대해서는 v의 값들로 치환하고 채널의 평균을 내서 1차원의 행렬을 리턴한다.
이진화에 대한 컨셉은 대충 알겠고, 적절한 threshold를 정하는 것이 관건이다. 이 threshold를 정해주는 알고리즘 중 하나가 Otsu's Algorithm이다. cv2.THRESH_OTSU로 편리하게 구현할 수 있지만, 어떠한 원리로 이런 값이 찾아지는지 모른다면, 활용 또한 쉽지 않을 것이다. 직접 구현해보자

기본 컨셉은 이렇다. 이미지의 픽셀값들을 히스토그램으로 나열할 때, threshold를 기준으로 두 개의 분포도가 나올 것이다. threshold 값이 적당하다면, 이 두 분포의 분산은 최소값을 가질 것이라는 게 아이디어다. 코드는 np.hsplit을 사용해, 행렬의 길이를 1부터 255까지로 나누고, 비용함수를 weight1*s1^2+weight2*s2^2로 설정해서, 최소값을 갱신해나간다.
