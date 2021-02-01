# Haar Cascade

2001년 CVPR ["Rapid Object Detection using a Boosted Cascade of Simple Features"](https://ieeexplore.ieee.org/document/990517) 에서
제안되었으며, OpenCV에서 모델을 제공하고 있습니다.

## 1. Haar-like features
Haar-like feature란 아래 그림과 같은 종류의 필터(CNN에서의 필터와 같습니다)로서, 이미지를 
스캔하면서 해당 feature와의 matching 여부를 봅니다.

필터 적용 값 = (검은색 영역 픽셀의 합) - (흰색 영역 픽셀의 합)  

위에서 구한 값이 특정 Threshold 이상이면 해당 feature가 있다고 판단합니다.

<p align="center">
<img src="https://docs.opencv.org/3.4.1/haar_features.jpg"><br>
출처: <a href="https://docs.opencv.org/3.4.1/d7/d8b/tutorial_py_face_detection.html">
opencv</a>
</p>

아래 그림을 보면 검출에 쓰인 original image와 2 종류의 feature, 그리고 이 feature가 검출된 곳이
표시된 사진이 있습니다.

<p align="center">
<img src="https://docs.opencv.org/3.4.1/haar.png"><br>
출처: <a href="https://docs.opencv.org/3.4.1/d7/d8b/tutorial_py_face_detection.html">
opencv</a>
</p>


## 2. Integral image
근데 이걸 하려면 해당 필터 사이즈의 2차원 배열을 읽어야 하는데 논문에서 쓰인 검출용 sub
window size는 24x24인데 여기서 적용할 수 있는 총 필터의 수는 약 160,000개입니다. 이걸 필터수만큼 반복하면
n^2 까진 아니지만(window size가 제한적이어서) 꽤나 시간이 많이 걸린다. 그래서 쓰이는 게 이
integral image.
######  

![](https://yonghakim.github.io/assets/img/2021-01-19-HaarCascade2.png)
출처: [Comparison of Viola-Jones Haar Cascade Classifier and Histogram of Oriented Gradients (HOG) for face detection](https://iopscience.iop.org/article/10.1088/1757-899X/732/1/012038/pdf)

이 그림이 integral image(적분 영상)을 잘 설명해주고 있습니다. 왼쪽의 배열이 original image
이고, 그 오른쪽의 배열이 integral image입니다. Integral image의 각 셀은 
original image의 원점(좌측상단)부터 해당 셀까지의 모든 픽셀의
합을 값으로 갖습니다. 그림에서는 original 배열에서의 초록색 영역의 합이 integral 배열에서의
한 셀 값으로 나타납니다(1+3+7+12+4+8+0+14+16=65).

이 적분영상을 통하면 임의의 영역에 위치한 사각형 영역의 합을 O(1)에 구할 수 있습니다.  
아래의 예시를 보면,  
![](https://t1.daumcdn.net/cfile/tistory/171A2648505508D421)  
출처: https://jangjy.tistory.com/32  

진한 녹색으로 표시된 D영역 셀들의 합을 구하고 싶다면  
d = A+B+C+D  
c = A+C  
b = A+B  
a = A  
임을 이용해 D = d-b-c+a 의 1회 계산으로 구할 수 있습니다.  
수식은 위키(요기)에 정리가 잘 되어 있으니 참고하시면 좋습니다.

## 3. AdaBoost
[link](Ensemble_Learning.md)보고 오시는 걸 추천

## 4. Cascade


## 참고 사이트
https://www.cs.cmu.edu/~efros/courses/LBMV07/Papers/viola-cvpr-01.pdf  
https://www.codeproject.com/Articles/441226/Haar-feature-Object-Detection-in-Csharp  
https://en.wikipedia.org/wiki/Summed-area_table  
https://iopscience.iop.org/article/10.1088/1757-899X/732/1/012038  
https://jangjy.tistory.com/32  
