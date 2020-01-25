---
layout: post
title: Facenet WIKI 따라하기
image: img/brain.jpg
author: Jaeyoun
date: 2019-12-24T10:03:47.149Z
tags: 
  - machine learning
---

facenet wiki의 예시를 따라해보았습니다.
얼굴 인식 및 분류를 하기 위해서 facenet을 이용해보려고 합니다. 먼저 wiki에 있는 예시를 실행해보면서 코드를 이해하고 적용해볼 생각입니다.

---

https://github.com/davidsandberg/facenet/wiki/Validate-on-LFW


## Prerequisite
먼저 python 환경이 필요합니다. Anaconda를 설치하셔도 좋습니다.
GPU가 있는 머신이 필요합니다.


## Clone
facenet 레파지토리를 클론하고 requirements들을 다운로드 받습니다.

```
git clone https://github.com/davidsandberg/facenet.git
pip install -r requirements.txt
```

## PYTHONPATH
파이썬 패스를 설정해주어야 합니다. 아래에서 […] 은 facenet 디렉토리가 있는 경로를 적어주시면 됩니다. (ex. /home/siisee11/facenet/src)
```
export PYTHONPATH=[...]/facenet/src
```

## Download dataset
[여기](http://vis-www.cs.umass.edu/lfw/lfw.tgz) 에서 데이터를 다운로드 받습니다. 예시에서는 ~/Downloads 에 lfw.tgz 파일을 다운로드 받았습니다.
```
~/Downloads$ wget http://vis-www.cs.umass.edu/lfw/lfw.tgz
```

~/datasets/lfw/raw 에 압축을 풀어줍니다.
```
cd ~/datasets
mkdir -p lfw/raw
tar xvf ~/Downloads/lfw.tgz -C lfw/raw --strip-components=1
```

## Align lfw dataset
다음으로 mtcnn을 이용해서 face detection 및 alignment(이미지 프리프로세싱)를 진행합니다. 다음 스텝에서 160*160 이미지를 사용하기 때문에 이에 맞춰서 이미지를 맞춰서 ~/datasets/lfw/lfw_mtcnnpy_160 에 저장합니다.
```
for N in {1..4}; do \
python src/align/align_dataset_mtcnn.py \
~/datasets/lfw/raw \
~/datasets/lfw/lfw_mtcnnpy_160 \
--image_size 160 \
--margin 32 \
--random_order \
--gpu_memory_fraction 0.25 \
& done
```

이 때, GPU out of memory가 발생한다면 fraction 값을 0.2정도로 낮춰주면 됩니다.
또 다른 에러로,
```allow_pickle=false```
라는 에러가 뜰 때가 있는데, 이는 numpy 버전 때문인데, src/align/detect_face.py 에서 np.load를 찾아서
np.load(..., ..., allow_pickle=True)
마지막에 allow_pickle=True 를 적어주면 된다.