---
layout: post
title: '[FaaS] NCP에서 서버리스 Cloud Functions 사용해보기'
date: 2019-08-13
author: cloudjun
tags: FaaS NCP 서버리스
---
최근 토이프로젝트로  만든 챗봇들을 개인 서버에 올려 사용하다가 리소스가 슬슬 아까워서 Cloud Functions을 이용한 서버리스를 한번 사용해보기로 했다. 

> ### 챗봇 자랑
>
> 한강물 수온을 가져와서 메세지 해주는 나름 채팅방에 지분 있는 봇.<br>
> 개발이 잘 안되면 한번씩 개발팀 동료들도  온도 물어보고 그런다. ~~더워서 or 추워서 못간다~~
> ![9](https://user-images.githubusercontent.com/36251104/62923434-a7fdc800-bde8-11e9-986a-b22a7eb10b7c.PNG)

AWS 및 Google Firebase 에서도 Cloud Functions를 지원하고 3사 전부 비슷한 무료 정책을 가지고 있으니, 정상적인 범위내의 토이프로젝트라면 맘껏 써도 **무료**로  충분히 커버가 가능하다. 아무거나 골라도 되지만 나는 최근에 자주 사용중인 NCP를 선택했다.

~~NCP 무료란 무료는 다 써보면서... 무료 자원 항상 감사합니다.~~

> 무료구간 ) 월별 1백 만회까지 요청 무료 / 월별 40GB-초 무료

### 기본 개념 
서버리스라는 말 그대로 서버를 직접 관리할 필요 없이 Function만 만들어 주면 된다.
서버를 늘리고 배포 or 구동 할 필요 없이  비즈니스 로직만 구성하면 된다는 점에서 급작스러운 이용자가 늘어나도 따로 대응을 할 필요 없으며, 호출이 없을 경우 비용도 나가지 않는다.

### 사용해보기

https://www.ncloud.com/product/compute/cloudFunctions 에 접속하여 이용 신청하기를 누르고, Console 관리 > Cloud Functions  / Action에 들어간다.

![1](https://user-images.githubusercontent.com/36251104/62923403-9a484280-bde8-11e9-8e47-8515604c88cb.PNG)

이후 내 패키지 생성을 누르고 Package 생성 창에서 패키지 명과 설명을 간단하게 입력해준다. 

![2](https://user-images.githubusercontent.com/36251104/62923412-9e746000-bde8-11e9-9939-89dc637f23a2.PNG)

필자는 여기서 bot이라는 이름을 넣고 패키지를 생성했다.
패키지에 대한 설명을 적으면 추후 각각 액션에 대해 쉽게 알아볼 수 있으니 가능하면 적어주자. 


![3](https://user-images.githubusercontent.com/36251104/62923416-9fa58d00-bde8-11e9-8cce-bee775bb9300.PNG)

이후 Package/Action 탭에서 방금 만든 폴더 모양의 bot을 클릭하고 액션 생성하기 버튼을 눌러준다. 액션 선택은 챗봇을 사용할거기 때문에 Basic Action를 선택한다.

![4](https://user-images.githubusercontent.com/36251104/62923421-a16f5080-bde8-11e9-9377-c98d4e6ef7bc.PNG)

이 화면부터 실제 액션을 등록 할 수 있는데,  네이버에서 지원하는 Node버전은 6  / 8버전을 지원하고 있다. 내가 사용하는 node 버전은 10 버전이지만 8버전에서도 정상적으로 동작하는 코드라서 언어는 nodejs:8로 맞췄다. (추후 10버전을 지원한다고는 하는데 언제인지는 모름 ㅜㅜ ) 액션명도 hangang이라는 이름을 넣어줬다.

모든 액션은 function main()에서부터 시작하기 때문에, 동작하는 코드는 main아래에 넣어줘야한다.
나는 위에 보여준 한강물온도를 알려주는 챗봇을 준비해서 넣었다

```javascript
//묘듈은 NCP에서 기본 지원하는 node8 묘듈이라 이렇게 넣어뒀다.
//타 묘듈 사용시에는 아래에 적힌 묘듈 링크를 확인해볼것.
var rp = require('request-promise') 
 
function main(params) { //최초 시작 지점
    var api_url = 'http://hangang.dkserver.wo.tc/'; //api URL
	var options = {
		uri: api_url
	};
	return rp(options)
	.then(function (repos) {
        console.log('repos :: ', repos);
        let jsonReturn = JSON.parse(repos);
        let prcsnValue = jsonReturn.time + "기준 한강물 온도는 "+ jsonReturn.temp +" 입니다.";
        var jsonObject  = new Object();
        jsonObject.body = prcsnValue;
        return jsonObject;
    })
    .catch(function (err) {
        // API call failed..
        return err;
    });
}
```

### 여기서 잠깐 묘듈은 어떻게 사용하나요?
> NCP에서는 Node6버전과 8버전에서 제공하는 기본 라이브러리가 다른데, 기본 지원 라이브러리는  http://docs.ncloud.com/ko/compute/compute-15-2-1.html 링크 맨 아래 라이브러리 정보에서 확인 할수 있다. 추가 묘듈도 가능하니 한번 쭈욱 정독하길 추천한다.

이후 소스코드 작성이 끝났다면  Action 설정을 눌러준 다음 웹 부분을 체크해준다. 

![5](https://user-images.githubusercontent.com/36251104/62923422-a3391400-bde8-11e9-95b4-6f2b0c8e7f41.PNG)

만든 챗봇이 메모리를 많이 사용하는 작업이 아니기에 Action 메모리도 가장 낮은걸로 설정했다.
디폴트 파라미터는 파라미터가 없는 경우 넘겨주는 파라미터를 설정 할 수 있는데, 따로 보내는 파라미터는 없어서 그냥 공백으로 나뒀다. 혹시 필요하면 json 타입으로 작성해주면 된다.


![6](https://user-images.githubusercontent.com/36251104/62923425-a46a4100-bde8-11e9-9852-55f9dd124001.PNG)

이후 저장 누르고 외부 연결 주소 만들기를 누르면 
API Gateway Url 생성 메뉴가 나오는데, API Gateway 상품을 신청하지 않았다면 신청하고 그 페이지로 돌아오면 된다. 스토리지까지 신규 생성으로 등록해주고 배포를 누르자

![7](https://user-images.githubusercontent.com/36251104/62923430-a59b6e00-bde8-11e9-8108-dc938a63db4c.PNG)

배포 누르면 새롭게 발급된 URL을 확인 할 수 있는데, 다음과 같은 형태로 구성된다.
타입의 경우 json타입을 사용할것이기에 마지막에 {type+} 부분을 /json으로 교체해주면 끝이다.


### 정상 확인하기
![8](https://user-images.githubusercontent.com/36251104/62923432-a6cc9b00-bde8-11e9-9f3b-3bf53809f9f0.PNG)

잘 동작하는 모습을 볼 수 있다<br>
이렇게 서버리스의 첫 발자국 끝.

--------


### 참조 
내용에 적지 않은 부분들도 많습니다. 한번 읽어보시길 추천해요.<br>
http://docs.ncloud.com/ko/compute/compute-15-1.html :: Cloud Functions 사용가이드<br>
http://docs.ncloud.com/ko/compute/compute-15-2-1.html :: node 액션 생성하기<br>
http://docs.ncloud.com/ko/compute/compute-15-2.html :: 로그 액션등<br>
http://docs.ncloud.com/ko/compute/compute-15-7.html :: 웹 액션예제 등 기본 HTML 요청 다루는 문서입니다.<br>