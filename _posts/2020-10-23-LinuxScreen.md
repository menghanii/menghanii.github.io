---
title: 리눅스 스크린(screen) 기능 사용하기
tag: [리눅스, 스크린]
categories: [리눅스]
---

## Linux Screen 사용

---

### Screen이란?

![image-20201023002310225](https://user-images.githubusercontent.com/37925813/96893795-0bdc5000-14c6-11eb-8956-4a3cbbf4f64b.png)

Screen은 터미널 세션을 끊고 나서도 다시 사용했었던 터미널 세션에 접속할 수 있게끔 하는 창 관리자이다. 
(https://medium.com/@erwinousy/screen-command-%EC%82%AC%EC%9A%A9%EB%B2%95-linux-mac-62bf5dd23110 참고하였습니다.)
내가 이해한 바로는, 한 번 돌리는데 시간이 엄청 오래 걸리는 파일이 있을 때(ex. 수백 GB에 해당하는 데이터를 py파일을 통해 서버에 적재하는 경우) ①서버에 접속하여 ②스크린을 생성하고 ③스크린에서 파일을 실행한 후 ④ 스크린에서 detach하여 ssh 접속을 종료해도 서버 단에서 해당 파일은 계속해서 돌아가게끔 해주는 기능이다!! (무척무척 유용하다)
심지어 내 로컬 노트북에서 인터넷 연결이 끊겨도, 스크린에서 실행한 파일은 돌아가며, 내 노트북을 끄든 신경을 끄든 어떻게 하든 이미 스크린에서 실행한 파일은 알아서 잘 돌아간다.
스크린 설치하는 방법은 내가 아직 Linux 사용자가 아니라 잘 모르지만,

```
$ sudo apt-get install screen
```

라고 한다.

## 스크린 각종 기능

---

### 스크린 만들기

```
$ screen -S 스크린이름
```

### 스크린 목록 보기

```
$ screen -list
```

### 스크린 접속(attach)

detach 되어 있는 screen에 접속하는 명령어이다.

```
$ screen -r
$ screen -r [session name] # 스크린이 둘 이상일 때
$ screen -x [session name]
```

### 스크린  접속 해제(detach)

파일을 실행하고 있는 스크린에서 떨어져 나오는(detach) 기능으로, 이를 수행한다고 하여 돌아가던 파일이 종료되는 것은 아니다. 

```
ctrl + a , d
```

### 스크린 접속되어 있는 경우 다시 들어가려면?

가끔 내 실수로, attach 되어있는 상태에서 나오는 경우가 있다. 이 경우 "already attached"라며 screen -r 동작이 먹히지 않는다. 이럴 때는

```
$ screen -rd
$ screen -r -d [session name]
```

로 다시 접속할 수 있다.

### 스크린 죽이기(kill)

파일 실행이 끝난 스크린을 없애고 싶으면,

```
$ screen -X -S [session name] kill
```

이상, 어느 정도 스크린을 돌리는데 필요한 것들은 정리한 듯 하다.