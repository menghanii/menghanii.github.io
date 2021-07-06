---
title: 도커로 Jupyterhub 설치하고 GPU 연결하기
categories: []
tags: []
math: true
comments: true
---
# 도커로 Jupyterhub 설치하고 GPU 연결하기

서버조교 화이팅

## 1. Docker 설치 및 서비스 등록

https://docs.docker.com/engine/install/centos/

## 2. Nvidia-Docker 설치 및 설치확인

https://docs.nvidia.com/datacenter/cloud-native/container-toolkit/install-guide.html#docker

## 3. GPU 할당하여 jupyterhub container 실행 및 nvidia-smi 확인

https://gldmg.tistory.com/148

```
# Host Machine의 Command Line에서
 docker run -it --gpus all -v /home:/home -v /etc/:/jup_etc/ -d -p 8000:8000 --name jupyterhub jupyterhub/jupyterhub jupyterhub
```

```
# bash 접속하여 nvidia-smi 확인
docker exec -it jupyterhub bash
nvidia-smi
```

```
apt-get update
apt-get install -y vim
apt-get install -y htop
apt-get install -y wget
apt-get install -y build-essential
apt-get install -y ubuntu-drivers-common
apt-get install -y git

# graphic driver 관련된 건 깔다가 난리날 수 있음. 돌이킬 수 없는...ㅠㅠ
```

```
pip install --upgrade pip
pip install notebook jupyterlab numpy pandas sklearn tqdm xgboost lightgbm catboost
pip install tensorflow-gpu
pip install keras
pip3 install torch==1.8.1+cu111 torchvision==0.9.1+cu111 torchaudio==0.8.1 -f https://download.pytorch.org/whl/torch_stable.html

```



## 4. 유저 등록 - 어떻게 하면 host에 있는 유저를 가져올 수 있을까?

https://github.com/jupyterhub/jupyterhub/issues/535 참고!!!

(매번 etc/passwd etc/shadow etc/group을 자동 복사하는 스크립트를 만들어줘야 함.)

```
The direct "mount" (-v /etc/passwd:/etc/passwd -v /etc/shadow:/etc/shadow) of the authentication files only works if you have a non-changing set of users. If you like to perform a useradd or passwd without needing to restart the jupyterhub docker container, i propose the following workaround:

mount the whole /etc/ foldert into the container, e.g. -v /etc/:/jup_etc/
create a copy_script that copies the files into the /etc folder in the container cp -f /jup_etc/passwd /etc/passwd; cp -f /jup_etc/shadow /etc/shadow; cp -f /jup_etc/group /etc/group
run the copy_script at startup of the jupyerhub docker container and every time when /etc/passwd, /etc/group, /etc/shadow have changed (inside the container with docker exec -it "containername" bash)
```



## 5. 남은 작업

1) admin user 권한 주기 (왜 ysda로는 접속이 안되는거지?)

2) passwd 바꿀때마다 자동으로 docker에서도 바뀌게 하기 : 이것과 관련해서는 일단 5초마다 한번씩 cp passwd가 실행되도록 스크립트를 작성함.(https://zetawiki.com/wiki/%EB%A6%AC%EB%88%85%EC%8A%A4_%EB%AC%B4%ED%95%9C%EB%B0%98%EB%B3%B5_%EC%8A%A4%ED%81%AC%EB%A6%BD%ED%8A%B8_repeat.sh)

```
echo 'while true; do cp -f /jup_etc/passwd /etc/passwd; cp -f /jup_etc/shadow /etc/shadow; cp -f /jup_etc/group /etc/group; sleep 5; done' > passwd_update.sh

cat passwd_update.sh

# 백그라운드 실행
sh passwd_update.sh &
```



## 6. 각종 디버깅

1) opencv(cv2) import 에러

```
ImportError: libGL.so.1: cannot open shared object file: No such file or directory

해결법
apt-get install -y libgl1-mesa-glx
```

```
ImportError: libgthread-2.0.so.0: cannot open shared object file: No such file or directory

해결법
apt-get install -y libglib2.0-0
```

---

unknown group 'messagebus' 에러

https://kingkingokingking.tistory.com/13 참고