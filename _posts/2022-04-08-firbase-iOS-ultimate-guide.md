---
layout: single
title: "Firebase with iOS, the ultimate guide"
date: "2022-04-08"
last_modified_at: "2022-04-08"
---

# Firebase 간단 소개

<br>
> Reference의 미디엄 사이트를 제 입맛대로 해석했습니다. 의역, 오역이 있을 수 있습니다. 

<br>
> Firebase is Google’s mobile application development platform that helps you build, improve, and grow your app.

<br>
<br>
Firebase는 앱을 빌드하고, 개선하고, 키우는데 필요한 toolset이다. 이런 firebase의 툴은 개발자가 원래는 스스로 빌드해야하나 하기실은 많은 일들을 대신해주고 개발자가 본 서비스에 집중할 수 있도록 해준다. firebase는 analytics, authentication, databases, configuration, file storage, push messaging 등의 일을 수행할 수 있다. 이런 서비스들은 클라우드를 호스팅하고 있으며 개발자들은 이를 정말 쉽게 사용할 수 있다.       
<br>
> Hosting: 일반적으로, 웹사이트나 관련 서비스가 저장소나 컴퓨팅 리소스를 저장 및 관리하기 위해 사용하는 외부 서비스를 호스팅이라고 부른다.

<br>      
이렇게 클라우드 기반 호스팅이기 때문에 프로덕트의 백엔드는 구글에 의해서 완전히 유지되고 운영된다. Firebase에 의해 제공되는 클라이언트 SDK로 앱와 firebase 백엔드 서비스는 그 사이의 어떠한 middleware 없이 **직접** 상호작용한다. 따라서, 만약 firebase 데이터베이스 중 하나를 쓴다면, 보통 클라이언트 앱에 데이터베이스 쿼리 코드가 들어가 있다.
<br>
<br>
프론트 엔드와 백엔드 소프트웨어를 전부 포함하던 전통적인 앱 개발과 다른 것과 양상이 달라진 것이다. 예전에는 프론트엔드는 백엔드에서 노출하는 API 엔드포인트를 사용하기만 하고 백엔드가 실제로 작업을 하면 됐다면, 이제는 firebase 프로덕트로 작업이 클라이언트로 옮겨오게 되면서 전통적인 백엔드는 생략되는 것이다. Administrative한 접근은 [Firebase console](https://console.firebase.google.com/u/0/)을 통해서 제공된다.

<img src="/assets/images/4-firebaseSubBackend.png" width="90%" height="90%" title="" alt="ERROR:cannot load image"/>  

<figcaption align = "center"><b>from: https://medium.com/firebase-developers/what-is-firebase-the-complete-story-abridged-bcc730c5f2c0</b></figcaption>

<br>
firebase가 백엔드를 대체한다곤 하지만 백엔드 개발자의 필요성이 완전히 사라지는 것은 아니다. 다양한 이유들로 백엔드는 있어야하고 firebase는 그 백엔드 개발을 도울 수 있다.

<br>
<br>

Firebase의 동작때문에 어떤 이들은 firebase를 "platform as a service" 혹은 "backend as a service"라고 부를지도 모른다. 하지만 이렇게 단순히 정의되는 것이 조금은 맞지 않는것 처럼 느껴진다. Firebase는 firebase다. 아래는 firebase에서 제공하는 프로덕트 목록이다.


<img src="/assets/images/5-firebaseSuiteList.png" width="90%" height="90%" title="" alt="ERROR:cannot load image"/>  

<figcaption align = "center"><b>from: https://medium.com/firebase-developers/what-is-firebase-the-complete-story-abridged-bcc730c5f2c0</b></figcaption>

<br>
<br>

# Firebase 프로덕트 소개

### **Build your app** -- creating the "guts"

Firebase 제품군 중 "build" 그룹에 해당하는 제품들은 아래와 같다. <br>
<br>
**Authentication** -- 유저 로그인과 신원확인 <br>
**Realtime Database** -- 실시간, cloud hosted, NoSQL 데이터베이스 <br>
**Cloud Firestore** -- 실시간, cloud hosted, NoSQL 데이터베이스 <br>
**Cloud Storage** -- 대규모의 확장이 용이한 파일 저장소 <br>
**Cloud Functions** -- "serverless", event driven 백엔드 <br>
**Firebase Hosting** -- 글로벌 웹 호스팅 <br>
**ML Kit** -- 일반적인 머신러닝 작업을 위한 SDK <br>
<br>

**Firebase Authentication**은 유저의 로그인과 신원확인을 담당한다. 이는 firebase의 다른 프로덕트를 설정하는데 초석이 되는 중요한 프로덕트다. 특히 유저마다의 데이터 접근을 제한할 필요가 있다면(사실 대부분의 앱이 그렇게 하겠지만) 더욱 중요하다.<br>
<br>
**Firebase Authentication**의 특징 중 하나는 안전한 로그인을 쉽게 만든다는 것이다. 특히 이것은 혼자서 완벽하게 구현하기란 불가에 가깝다. 그리고 firebase Authentication은 Federated로 -- 페이스북, 트위터, 구글, 깃헙 등의 다양한 indentity provider의 유저 계정을 Firebase Authentication의 유일한 계정으로 링크하는 것-- 참고로 [United Federation of Planets](https://memory-alpha.fandom.com/wiki/United_Federation_of_Planets)는 이것을 사용하도록 권장하고 있다. 앱을 만들때 이 Authentication 기능을 먼저 배워서 적용하는 것을 추천한다. <br>
<br>
**Firebase Realtime Database**와 **Cloud Firestore**는 데이터베이스 서비스다. 이 둘은 비슷하지만 장단점이 있으니 리서치 해볼 것을 추천한다. <br>
<br>
이러한 데이터베이스의 특징은 데이터베이스의 데이터가 바뀌면 ""실시간"" 업데이트를 제공한다는 점이다. 클라이언트 SDK을 이용하여 데이터가 필요한 곳에 "listener"를 셋업할 수 있고 listener는 데이터의 변화가 감지될 때마다 신호를 받습니다. 이 때문에 앱은 폴링(Polling - 하나의 장치 또는 프로그램이 충돌 회피 또는 동기화 처리 등을 목저으로 다른 장치 또는 프로그램의 상태를 주기적으로 검사하여 데이터를 처리하는 것)을 거치지 않고 최신 정보를 앱 화면에 올릴 수 있다. <br>
<br>

<img src="/assets/images/6-firebaseAsRealtimeDB.gif" width="90%" height="90%" title="" alt="ERROR:cannot load image"/>  

<figcaption align = "center"><b>from: https://medium.com/firebase-developers/what-is-firebase-the-complete-story-abridged-bcc730c5f2c0</b></figcaption>




















# Reference

- [What is Firebase? The complete story, abridged.](https://medium.com/firebase-developers/what-is-firebase-the-complete-story-abridged-bcc730c5f2c0)
