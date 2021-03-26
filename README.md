# YOLOv5 학습하기   
## 1. Dataset 구조 변경 및 data.yaml 업로드하기.   
<img src=https://user-images.githubusercontent.com/81284736/112667377-87409400-8ea0-11eb-9e13-71107ac657a5.jpg>   

[Create-Custom-Dataset-for-YOLOv5](https://github.com/Cl0udContainer/Create-Custom-Dataset-for-YOLOv5)를 실행한 후 Dataset의 대략적인 모습이다.   
<img src=https://user-images.githubusercontent.com/81284736/112668306-b4da0d00-8ea1-11eb-989e-9bdb17820160.jpg><img src=https://user-images.githubusercontent.com/81284736/112668762-39c52680-8ea2-11eb-9212-123d6e8e1ef0.jpg>   
이를 **train**과 **val**폴더를 만들고, **7:3 혹은 6:4**의 비율로 나누고, 각 폴더에 **images**와 **labels**폴더를 생성하여 이미지와 텍스트 파일을 분리한다.   
Dataset을 압축한 뒤, Google Drive에 업로드한다.   
레포지토리에 있는 data.yaml을 Google Drive에 업로드한다.
***   
## 2. YOLOv5 학습하기.   
[Yolov5 깃허브 링크](https://github.com/ultralytics/yolov5.git)에 보면 YOLOv5의 여러 정보를 알 수 있다.   
yolov5에는 크게 **YOLOv5s, YOLOv5m, YOLOv5l, YOLOv5x**의 4가지 모델이 있고, 각 모델은 정확도, FPS, 크기등이 모두 제각각이므로, 용도에 맞는 모델을 선택해야 한다.   
졸업 프로젝트 당시, 모바일 환경에 이식하기 위해 YOLOv5m 모델을 학습하였으나, ios에 이식할 수 없어서 포기한 모델과, 학습에 사용한 Colab Notebook이다.(Andriod는 많은 YOLOv5 프로젝트가 이미 깃헙에 존재하는데, ios는 하나도 없음..)   
따라서 이번 포스트에서는 YOLOv5의 학습과, 결과 확인까지만 진행할 예정이다.
***   
레포지토리에 있는 Colab Notebook을 열고 GPU설정을 해 주어야 한다.   
<img src=https://user-images.githubusercontent.com/81284736/112671439-8100e680-8ea5-11eb-91ca-7f3174f5f4a3.jpg><img src=https://user-images.githubusercontent.com/81284736/112671770-f2409980-8ea5-11eb-827e-713c54489a61.jpg>   
수정 탭에 있는 노트 설정을 누르고, 하드웨어 가속기에서 GPU를 선택하고 저장을 누른다. 이 후 맨 왼쪽에서 **드라이브 마운트**를 클릭하여 Google Drive를 마운트한다.   
이후 Colab Notebook의 셀을 차례대로 실행하면 학습이 진행된다. 여기서 각자의 상황에 따라 명령어를 수정해야 하는 셀을 나열하겠다.   
<pre><code>#Google Drive에 있는 Dataset 불러오기 
!cp /content/drive/My\ Drive/Dataset의 경로 /content</code></pre>   
<pre><code>#data.yaml파일 불러오기
!cp /content/drive/My\ Drive/data.yaml의 경로 /content/dataset</code></pre>   
<pre><code>#yolov5를 학습시키기
%cd /content/yolov5/

!python train.py --img 416 --batch 16 --epochs 200 --data /content/dataset/data.yaml --cfg ./models/yolov5m.yaml --weights yolov5m.pt --name 학습된 가중치의 이름</code></pre>   
<pre><code>#인식할 영상을 Google Drive에서 불러오기
!cp /content/drive/My\ Drive/영상 경로 /content/yolov5</code></pre>   
<pre><code>#영상에 있는 객체 탐지하기#영상에 있는 객체 탐지하기
!python detect.py --weights 학습된 가중치 파일 이름 --img 1280 --conf 0.4 --source /content/yolov5/영상 파일 이름</code></pre>   
<pre><code>#탐지 결과를 Google Drive로 복사하기.
!cp /content/yolov5/inference/output/탐지 결과 영상 파일 이름 /content/drive/My\ Drive/저장하고자 하는 경로</code></pre>   
***   
학습을 진행한 후, 영상에서 객체를 탐지한 모습이다.
<img src=https://user-images.githubusercontent.com/81284736/112675486-8f9dcc80-8eaa-11eb-8626-43fe4537f682.gif>
