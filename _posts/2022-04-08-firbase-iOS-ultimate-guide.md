---
layout: single
title: "Firebase with iOS, the ultimate guide"
date: "2022-04-08"
last_modified_at: "2022-04-08"
---

<br>
> Reference의 미디엄 포스트를 제 입맛대로 읽고 해석했습니다. 의역, 오역, 특히 생략된 부분이 있을 수 있으니 원본 포스트를 참고하시는 걸 권장드립니다.

<br>

# Firebase 간단 소개

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

<br>

**Cloud Storage**는 크게 확장이 가능한(exabyte 수준의 크기) 파일 저장소를 제공한다. 엄밀히 말하자면 클라우드 스토리지는 firebase 제품이 아니라 [Google Cloud](https://cloud.google.com/storage/) 제품이다. 하지만 *Firebase와 연동된* Cloud Storage를 이용해서, 클라이언트 SDK를 쓸 수있고 유저는 앱을 이용하여 클라우드 스토리지 "[bucket](https://cloud.google.com/storage/docs/key-terms#buckets)"로부터 파일을 직접 다운로드, 업로드 할 수 있다.<br>
<br>
**Authentication은 security rule을 활용해 앞서 설명한 세개의 제품들과 연동성이 굉장히 좋다.** 이는 클라이언트가 데이터에 대한 접근을 당신이 허용하는 방식으로 강제한다. Authentication을 이용해 로그인한 유저는 자동적으로 indentification token을 제공하게되고 이 토큰을 이용해서 당신이 설정한 룰 대로 데이터의 읽기, 쓰기를 통제할 수 있다. 그리고 만약 유저의 개인정보를 데이터로 저장하고 있다면, 절대 Security Rule과 Firebase Authentication을 써서 데이터 접근을 오용하지 않도록 해야한다. 만약 룰이 너무 약하다면 Firebase가 알려줄 수도 있다.<br>
<br>

  
> 다른 기능들은 내가 실제로 Firebase의 여러 기능들을 사용하게 될 때 내용을 추가
<br>
### **Grow you app** -- attract and retain users

"Grow" 그룹의 제품들은 아래와 같다. <br>
<br>
**Analytics** -- 유저와 유저가 앱을 어떻게 사용하는지 이해한다.<br>
**Predictions** -- 머신 러닝을 적용해서 유저 행동을 예측한다.<br>
**Cloud Messaging** -- 유저에게 메시지와 노티를 보낸다. <br>
**Remote Config** -- 새로운 버전을 배포하지않고 앱을 사용자화 한다; 변화를 감지한다.<br>
**A/B Testing** -- 마케팅, 사용성 실험을 시행한다.<br> 
**Dynamic Links** -- 네이티브 앱 convesion, 사용자 쉐어링, 마케팅 캠페인을 가능하게 한다.<br>
**App Indexing** -- Google Search의 통합을 통해 사용자의 관심을 끈다.<br>
**In-App Messaging** -- Active 사용자에게 메시지를 보내 관심을 끈다.<br>
<br>
<br>
**Google Analytics for Firebase**는 "grow" 제품군의 핵심이다. 유저와 그 행동패턴에 대해 더 알고 싶으면, Analytics가 이를 도울 수 있다. 앱 초기 배포에서 유저 베이스가 어떻게 형성될지 당신만의 아이디어가 있을지 모르나, 사실 그것은 완전히 잘 못될 수 있다. <br>

<img src="/assets/images/7-firebaseUserbase.jpeg" width="90%" height="90%" title="" alt="ERROR:cannot load image"/>  

<figcaption align = "center"><b>from: https://medium.com/firebase-developers/what-is-firebase-the-complete-story-abridged-bcc730c5f2c0</b></figcaption>

<br>

**Firebase Cloud Messaging**로 푸쉬 메시지를 보낼 수 있다. 메시지를 보내는 방법에는 두가지가 있다. 우선 무언가 업데이트 되어서 이 업데이트에 반응하고 싶을 때(예를들어 채팅방의 노티) 당신의 백엔드에 앱을 *ping*하는 코드를 적을 수 있다. 두 번째로, Firebase 콘솔에 메시지를 직접 작성하여 유저들을 *ping*할 수 있다. 오늘 이야기하는 것은 두 번째 케이스, 즉 direct user notificaiton이다.<br>
<br>
Analytics와 Prediction과 통합되어 있기에, 유저 메시지로 특정한 유저군에게 메시지를 보낼 수 있게된다. 이것은 매우 유용한데, 왜냐면 유저들이 클릭할만한 노티를 맞춤 전송할 수 있기 때문이다.

**Firebase In App Messaging**은 유저에게 앱의 핵심기능을 사용하도록 유도하는 사용자화된 메시지를 보내주는 것을 도와준다. FCM과 다른점은 FCM은 서버에서 컨트롤되는(예를들어 Firebase 콘솔) 메시지라는 점이고, FIAM은 앱 내에서 제공된다는 점이다(물론 Firebase Console에서 설정된다). FIAM은 유저가 실제로 앱을 사용할 때 보내지는 것을 보장한다. 하지만 FCM이나 FIAM 둘 다 Analytics와 Predictions과 통합되어 있음은 같다.<br>
<br>
FCM을 남발할 때, 사용자는 관련없는 메시지의 내용과 더불어 노티를 받고싶지 않은 시간대에 메시지를 받아볼 수 있다. 아니면 최악의 경우 실수로 노티를 삭제할지도 모른다. 이런 짜증나느 상황을 개선할 수 있는 것이 FIAM이다. 대개 FIAM은 타이밍이나 내용상 유저가 관련있다고 여길 가능성이 있고 이는 Analytics나 Prediction으로 강화된다.<br>
<br>

> 다른 Grow 기능들은 내가 좀 더 Relevant한 상황이 될 때 추가

<br>

### **Improve your app** -- 안정성과 퍼포먼스

"Improve" 제품군은 아래와 같다.<br>     
<br>
**Test Lab** -- 확장, 자동화 가능한 cloud-hosted 기기 앱 테스트 <br>
**Crashlytics** -- 앱 크래쉬에 관한 명확한, 실행 가능한 인사이트 제공 <br>
**Performance Mornitoring** -- 앱의 퍼포먼스 이슈에관한 인사이트 제공 <br>
<br>
**Firebase Test Lab**은 가상의 Android기기와 더불어 다양한 iOS, Android 기기에 대한 접근을 제공한다. 모바일 앱을 만든다면, 테스트 기기 하나만으로는 부족할 것이다. Test Lab은 당신의 앱을 설치할 수 있는 실제 기기들을 호스팅해서 test suite(Android: Espresso, iOS: XCTest)를 실행할 수 있게 해준다. 이는 실제로 추가의 코딩없이 자동화된 테스트를 할 수 있게 해준다.<br>
<br>
**Firebase Crashlytics**는 최고의 크래쉬 리포팅 툴이다. 그냥 써라. Crashlytics는 Analytis와 통합되어 이런 크래쉬가 유저들의 앱 사용에 영향을 주는지 알려준다. <br>
<br>
**Firebase Performance Monitoring**은 HTTP 리퀘스트, 스타트업 타임, 혹은 다른 코드를 측정해서 유저 입장에서 본 앱의 퍼포먼스 이슈에 대해 인사이트를 제공한다. Firebase 제품은 몇 줄 안되는 코드로 이를 가능하게 하고 결과는 Firebase console에서 바로 볼 수 있다.<br>
<br>
*사용자 입장*에서 제품을 바라보는 것은 중요하다. 개발자는 높은 확률로 빠른 기기와 네트워크를 사용해 앱을 개발한다. 이러한 환경이 적용되는 사람은 전세계에서 보자면 드물 것이다, 누군가는 끊김이 있는 모바일 네트워크와 성능이 좋지않은 기기를 가지고 있을 것이기 때문이다. 유저들이 느낄 진정한 불편함을 느끼기 위해 Performance Monitoring 사용하자, 그러면 다른 어떤 곳에서 당신의 앱이 얼마나 문제가 있는지 보게 될 것이다. OS버전, 그리고 특정 HTTP endpoint가 얼마나 큰 딜레이를 만드는지 알게되어 앱을 최적화하는데 도움이 될 것이다.<br>
<br>



# Reference

- [What is Firebase? The complete story, abridged.](https://medium.com/firebase-developers/what-is-firebase-the-complete-story-abridged-bcc730c5f2c0)
