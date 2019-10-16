---
layout: post
title: '[삽질] x570 aorus elite WOL 사용하기 (부제 : 자동화의 시작1)'
date: 2019-10-06
author: cloudjun
tags: wol 삽질
---
컴퓨터를 바꾸기 전, 한참 잘 쓰던 WOL를 컴퓨터를 바꾼뒤 아무리 설정을 해봐도 사용 할 수가 없었는데, 이틀간 설정과 드라이버를 바꿔보며 알아낸 지식을 이렇게 공개합니다.

### 설정하기
X570 Aorus Elite 에서는 메인보드에 기본적으로 WOL가 허용으로 되어 있다보니 따로 메인보드 설정은 건들 필요가 없었다. (혹시나 잊은 설정이 있을까 4번 이상 들락날락 거렸지만 없었음.)

사용하는 랜카드는 인텔 I211를 사용하는데 기본 드라이버 설정으로는 'Wake on Magic Packet' , 'Wake on Pattern Match' 두가지 설정만 가능하게 되어 있다. (전원 관리에서 - 전원을 절약하기 위해 이 장치를 끌 수 있음도 해제한 상태)

![캡처](https://user-images.githubusercontent.com/36251104/66266396-86a4df80-e85f-11e9-83df-406ab3411017.PNG)


하지만 이 상태에서는 WOL를 사용 할 수 없는데, 'Intel PROset Adapter Configuration Utility'을 설치해서 숨겨진 설정을 바꿔줘야한다.

아래에서 먼저 드라이브를 받아준다.

인텔 이더넷 컨트롤러 I211다운로드 
https://downloadcenter.intel.com/ko/product/64403

![그림1](https://user-images.githubusercontent.com/36251104/66266398-8e648400-e85f-11e9-9b0d-cce3a2c74625.png)

정성것 버전에 맞는 드라이버를 설치 한뒤 실행시키면 다음과 같은 화면을 볼 수 있는데

여기서 AdapterSettings 아래를 내리다보면 나오는 '전원 끄기 상태의 Wake on Magic Packet'을 활성화로 바꿔준 다음 하단에 있는 Apply Changes를 눌러 적용시켜준다.

![3](https://user-images.githubusercontent.com/36251104/66266400-9290a180-e85f-11e9-9af9-977e73e81b5f.PNG)


적용뒤 전원을 끄고 실행시켜보면 정상적으로 WOL가 된다.

---

### 여담

이틀간 컴퓨터에게 말 많이 걸었음.

누워서 한걸음 안 움직이고 부팅(/종료)시키겠다는 날먹 생각(!) 에 WOL프로그램 프로토타입 만들어 테스트하는데 
처음에는 내 컴퓨터 WOL 문제가 아니라 내가 짠 프로그램 문제인줄 알고 이상한곳 삽질함

코딩할때도 이정도는 아니였는데 부팅할때마다 애걸복걸 다한듯 엌ㅋㅋㅋㅋ

<img src="https://user-images.githubusercontent.com/36251104/66266401-96bcbf00-e85f-11e9-9d58-bd0988f8b746.PNG" width="300px" />

