---
layout: single
title: "Firebase with iOS, the ultimate guide"
date: "2022-04-08"
last_modified_at: "2022-04-21"
---

<br>
> Reference의 미디엄 포스트를 제 입맛대로 읽고 해석했습니다. 의역, 오역, 특히 생략된 부분이 있을 수 있으니 원본 포스트를 참고하시는 걸 권장드립니다.

<br>

# Firebase 간단 소개

> Firebase is Google’s mobile application development platform that helps you build, improve, and grow your app.

<br>
Firebase는 앱을 빌드하고, 개선하고, 키우는데 필요한 toolset이다. 이런 Firebase의 툴은 개발자가 원래는 스스로 빌드해야하나 하기실은 많은 일들을 대신해주고 개발자가 본 서비스에 집중할 수 있도록 해준다. Firebase는 analytics, authentication, databases, configuration, file storage, push messaging 등의 일을 수행할 수 있다. 이런 서비스들은 클라우드를 호스팅하고 있으며 개발자들은 이를 정말 쉽게 사용할 수 있다.       
<br>
> Hosting: 일반적으로, 웹사이트나 관련 서비스가 저장소나 컴퓨팅 리소스를 저장 및 관리하기 위해 사용하는 외부 서비스를 호스팅이라고 부른다.

<br>
      
이렇게 클라우드 기반 호스팅이기 때문에 프로덕트의 백엔드는 구글에 의해서 완전히 유지되고 운영된다. Firebase에 의해 제공되는 클라이언트 SDK로 앱와 Firebase 백엔드 서비스는 그 사이의 어떠한 middleware 없이 **직접** 상호작용한다. 따라서, 만약 Firebase 데이터베이스 중 하나를 쓴다면, 보통 클라이언트 앱에 데이터베이스 쿼리 코드가 들어가 있다.

<br>
<br>
프론트 엔드와 백엔드 소프트웨어를 전부 포함하던 전통적인 앱 개발과 다른 것과 양상이 달라진 것이다. 예전에는 프론트엔드는 백엔드에서 노출하는 API 엔드포인트를 사용하기만 하고 백엔드가 실제로 작업을 하면 됐다면, 이제는 firebase 프로덕트로 작업이 클라이언트로 옮겨오게 되면서 전통적인 백엔드는 생략되는 것이다. Administrative한 접근은 [Firebase console](https://console.firebase.google.com/u/0/)을 통해서 제공된다.

<br>
<p align="center">
<img src="/assets/images/4-firebaseSubBackend.png" width="90%" height="90%" title="" alt="ERROR:cannot load image"/>
</p>
  
<figcaption align = "center"><b>from: https://medium.com/firebase-developers/what-is-firebase-the-complete-story-abridged-bcc730c5f2c0</b></figcaption>

<br>
Firebase가 백엔드를 대체한다곤 하지만 백엔드 개발자의 필요성이 완전히 사라지는 것은 아니다. 다양한 이유들로 백엔드는 있어야하고 Firebase는 그 백엔드 개발을 도울 수 있다.

<br>
Firebase의 동작때문에 어떤 이들은 Firebase를 "platform as a service" 혹은 "backend as a service"라고 부를지도 모른다. 하지만 이렇게 단순히 정의되는 것이 조금은 맞지 않는것 처럼 느껴진다. Firebase는 Firebase다. 아래는 Firebase에서 제공하는 프로덕트 목록이다.

<p align="center">
<img src="/assets/images/5-firebaseSuiteList.png" width="90%" height="90%" title="" alt="ERROR:cannot load image"/>
</p>  

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

**Firebase Authentication**은 유저의 로그인과 신원확인을 담당한다. 이는 Firebase의 다른 프로덕트를 설정하는데 초석이 되는 중요한 프로덕트다. 특히 유저마다의 데이터 접근을 제한할 필요가 있다면(사실 대부분의 앱이 그렇게 하겠지만) 더욱 중요하다.<br>
<br>
**Firebase Authentication**의 특징 중 하나는 안전한 로그인을 쉽게 만든다는 것이다. 특히 이것은 혼자서 완벽하게 구현하기란 불가에 가깝다. 그리고 firebase Authentication은 Federated로 -- 페이스북, 트위터, 구글, 깃헙 등의 다양한 indentity provider의 유저 계정을 Firebase Authentication의 유일한 계정으로 링크하는 것-- 참고로 [United Federation of Planets](https://memory-alpha.fandom.com/wiki/United_Federation_of_Planets)는 이것을 사용하도록 권장하고 있다. 앱을 만들때 이 Authentication 기능을 먼저 배워서 적용하는 것을 추천한다. <br>
<br>
**Firebase Realtime Database**와 **Cloud Firestore**는 데이터베이스 서비스다. 이 둘은 비슷하지만 장단점이 있다. 이는 아래에 별도의 섹션으로 구분해서 정리했다. <br>
<br>
이러한 데이터베이스의 특징은 데이터베이스의 데이터가 바뀌면 ""실시간"" 업데이트를 제공한다는 점이다. 클라이언트 SDK을 이용하여 데이터가 필요한 곳에 "listener"를 셋업할 수 있고 listener는 데이터의 변화가 감지될 때마다 신호를 받는다. 이 때문에 앱은 폴링(Polling - 하나의 장치 또는 프로그램이 충돌 회피 또는 동기화 처리 등을 목저으로 다른 장치 또는 프로그램의 상태를 주기적으로 검사하여 데이터를 처리하는 것)을 거치지 않고 최신 정보를 앱 화면에 올릴 수 있다. <br>
<br>


<p align="center">
<img src="/assets/images/6-firebaseAsRealtimeDB.gif" width="90%" height="90%" title="" alt="ERROR:cannot load image"/>  
</p>

<figcaption align = "center"><b>from: https://medium.com/firebase-developers/what-is-firebase-the-complete-story-abridged-bcc730c5f2c0</b></figcaption>

<br>

**Cloud Storage**는 크게 확장이 가능한(exabyte 수준의 크기) 파일 저장소를 제공한다. 엄밀히 말하자면 클라우드 스토리지는 Firebase 제품이 아니라 [Google Cloud](https://cloud.google.com/storage/) 제품이다. 하지만 *Firebase와 연동된* Cloud Storage를 이용해서, 클라이언트 SDK를 쓸 수있고 유저는 앱을 이용하여 클라우드 스토리지 "[bucket](https://cloud.google.com/storage/docs/key-terms#buckets)"로부터 파일을 직접 다운로드, 업로드 할 수 있다.<br>
<br>
**Authentication은 security rule을 활용해 앞서 설명한 세개의 제품들과 연동성이 굉장히 좋다.** 이는 클라이언트가 데이터에 대한 접근을 당신이 허용하는 방식으로 강제한다. Authentication을 이용해 로그인한 유저는 자동적으로 indentification token을 제공하게되고 이 토큰을 이용해서 당신이 설정한 룰 대로 데이터의 읽기, 쓰기를 통제할 수 있다. 그리고 만약 유저의 개인정보를 데이터로 저장하고 있다면, 절대 Security Rule과 Firebase Authentication을 써서 데이터 접근을 오용하지 않도록 해야한다. 만약 룰이 너무 약하다면 Firebase가 알려줄 수도 있다.<br>
<br>

  
> 다른 기능들은 내가 실제로 Firebase의 여러 기능들을 사용하게 될 때 내용을 추가

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

> 다른 Grow 기능들은 후에 추가

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


# Cloud Firestore vs Realtime Database

Firebase는 실시간 데이터 동기화를 지원하며 클라이언트에서 액세스할 수 있는 2가지 클라우드 기반 데이터베이스 솔루션을 제공한다. <br>
<br>
Cloud Firestore는 모바일 앱 개발을 위한 Firebase의 **최신 데이터베이스**로서 실시간 데이터베이스의 성공을 바탕으로 더욱 직관적인 새로운 데이터 모델을 선보이고 있다. 또한 실시간 데이터베이스보다 **풍부하고 빠른 쿼리**와 **원활한 확장성**을 제공한다. <br>
<br>
Realtime Database는 Firebase의 기존 데이터베이스로, 여러 클라이언트에서 실시간으로 상태를 동기화해야 하는 모바일 앱을 위한 효율적이고 지연 시간이 짧은 솔루션이다.<br>
<br>

## 두 솔루션의 공통점

- 배포 및 유지 관리할 서버가 없는 클라이언트 우선 SDK
- 실시간 업데이트
- 무료 등급(이후 사용한 만큼만 비용 지불)

## 두 솔루션의 차이

#### 데이터 모델

Realtime Database와 Cloud Firesotre는 모두 NoSQL 데이터베이스다.<br>
<br>
실시간 데이터베이스는 **데이터를 하나의 큰 JSON 트리로 저장한다.** <br>
- 단순한 데이터를 매우 쉽게 저장한다.
- 복잡한 계층적 데이터를 대규모로 정리하기는 어렵다.<br>
<br>

Cloud Firestore는 **데이터를 문서 컬렉션으로 저장한다.**<br>
- JSON과 매우 비슷하게 단순한 데이터를 문서에 쉽게 저장한다.
- 문서에 있는 하위 컬렉션을 사용하여 복잡한 계층적 데이터를 대규모로 쉽게 정리할 수 있다.
- 비 정규화 및 데이터 평면화가 덜 필요하다.

#### 실시간 및 오프라인 지원

두 제품 모두 모바일 위주의 실시간 SDK를 보유하며 오프라인 대응 앱을 위한 로컬 데이터 저장소를 지원한다.<br>
<br>
실시간 데이터베이스는 **Apple, Android 클라이언트를 위한 오프라인 지원이 있다.**<br>
<br>
Cloud Firestore는 **Apple, Android, 웹 클라이언트를 위한 오프라인 지원이 있다.**<br>
<br>

#### 접속 상태

클라이언트가 온라인인지 오프라인인지를 알면 유용하다. Firebase 실시간 데이터베이스는 클라이언트 연결 상태를 기록하고 클라이언트의 연결 상태가 변경될 때마다 업데이트를 제공할 수 있다.<br>
<br>
실시간 데이터베이스 **접속 상태가 지원된다.**<br>
Cloud Firestore는 **기본적으로 지원되지 않는다**.<br> 
- Cloud Functions를 통해 Cloud Firestore와 실시간 데이터 베이스를 동기화하여 실시간 데이터베이스의 접속 상태 지원을 활용할 수 있다.<br>
<br>

#### 쿼리

쿼리를 통해 두 가지 데이터베이스에서 데이터를 검색, 정렬, 필터링한다.<br>
실시간 데이터베이스는 **제한적인 정렬 및 필터링 기능을 갖춘 딥 쿼리**를 사용한다.<br>
- 쿼리에서 속성을 정렬 또는 필터링할 수 있으며 두 가지를 함께 진행할 수는 없다.
- 기본적으로 깊은 쿼리가 수행되어 항상 전체 하위 트리를 반환한다.
- 쿼리에서 JSON 트리의 개별 리프 노드 값에 이르는 세부 수준의 데이터에 액세스할 수 있다.
- 쿼리에서 색인을 필요로하지 않지만 데이터 세트가 커짐에 따라 특정 쿼리의 성능이 저하된다.

<br>
Cloud Firestore는 **복합 정렬 및 필터링 기능을 갖춘 색인이 생성된 쿼리다.**
- 단일 쿼리에서 속성에 여러 필터를 연결하고 필터링과 정렬을 결합할 수 있다.
- 쿼리가 얕아 특정 컬렉션 또는 컬렉션 그룹의 문서만 반환하며 하위 컬렉션 데이터는 반환하지 않는다.
- 쿼리에서 항상 전체 문서를 반환해야 한다.
- 기본적으로 쿼리가 색인화되며 쿼리 성능이 데이터 세트가 아닌 결과 세트의 크기에 비례한다.<br>
<br>

#### 쓰기 및 트랜잭션

실시간 데이터베이스는 **기본 쓰기 및 트랜잭션 작업**을 할 수 있다.<br>
- 설정 및 업데이트 작업을 통해 데이터를 쓴다.
- 트랜잭션이 특정 데이터 하위 트리에서 원자적으로 수행된다. <br>
<br>
Cloud Firestore는 **고급 쓰기 및 트랜잭션 작업**을 할 수 있다.
- 설정 및 업데이트 작업은 물론 배열 및 숫자 연산자와 같은 고급 변환을 통한 데이터 쓰기 작업니다.
- 트랜잭션이 데이터베이스의 모든 부분에서 데이터를 원자적으로 읽고 쓸 수 있다.<br>
<br>

#### 신뢰성 및 성능

실시간 데이터베이스는 **리전(Region) 내 솔루션이다.** <br>
- 리전 내 구성에서 사용할 수 있다(Available in regional configurations). 데이터베이스는 리전 내 영역별 가용성에 따라 제한된다.
- 지연 시간이 매우 짧아 상태 동기화가 자주 발생할 때 적합하다.<br>

<br>
Cloud Firestore는 **자동으로 확장되는 리전 내 밒 멀티 리전 솔루션이다.**<br>
- 데이터가 서로 다른 리전의 여러 데이터 센터에 위치하므로 글로벌 확장성과 견고한 신뢰성이 보장된다.
- 전 세계적으로 리전 내 또는 멀티 리전 구성이 가능하다.<br>
<br>

#### 확장성

실시간 데이터베이스는 **확장하려면 샤딩(sharding)을 사용해야 한다.**<br>
- 단일 데이터베이스에서 동시 연결 약 200,000개, 초당 쓰기 약 1,000회까지 확장된다. 추가로 확장하려면 데이터를 여러 데이터베이스로 샤딩해야한다.
- 개별 데이터의 쓰기 속도에 적용되는 로컬 제한은 없다.
<br>
Cloud Firestore는 **확장이 자동으로 수행된다.**
- 확장이 완전히 자동으로 수행된다. 현재 확장 한도는 동시 연결 약 1,000,000개, 초당 쓰기 약 10,000회다. 향후 이 한도는 늘어날 수 있다.
- 개별 문서 또는 색인의 쓰기 속도에 적용되는 제한이 있다.<br>
<br>

#### 보안

실시간 데이터베이스의 보안상 특징으로는 **승인과 검증이 분리된 캐스케이딩 규칙 언어가 있다.**<br>
- 모바일 SDK의 읽기 및 쓰기가 실시간 데이터베이스 규칙으로 보호된다.
- 읽기 및 쓰기 규칙이 하위로 전파된다.
- validate 규칙을 사용해 별도록 데이터를 검증한다.
<br>
Cloud Firestore의 보안상 특징으로는 **승인과 검증이 결합된 비캐스케이딩 규칙이 있다.**
- 모바일 SDK의 읽기 및 쓰기가 Cloud Firestore 보안 규칙으로 보호된다.
- 서버 SDK의 읽기 및 쓰기가 [identity and Access Management](https://cloud.google.com/firestore/docs/security/iam?hl=ko)로 보호된다.
- 와일드 카드를 사용하지 않는 한 규칙이 하위로 전파되지 않는다.
- 규칙으로 쿼리를 제한할 수 있다. 쿼리 결과에 사용자가 액세스 할 수 없는 데이터가 포함되어 있으면 전체 쿼리가 실패한다.<br>
<br>
<br>
> 참고: 한 서비스에 두가지를 동시에 사용 가능하다. 



## 둘 중에 무엇을 선택해야 할까?

| |Cloud Firestore|Realtime Database|
|-----------|-----------|-----------|
|데이터베이스의 역할<br>**내 앱에서 데이터베이스를 사용하는 목적은...**|고급 쿼리, 정렬, 트랜잭션|주로 데이터 동기화(기본 쿼리 사용)|
|데이터 작업 <br> **내 앱에서 데이터베이스를 사용하면...**|수백 GB 내지 TB의 데이터가 변경되며, 훨씬 더 많은 빈도록 읽힌다.|수 GB 이하의 데이터가 자주 변경된다.|
|데이터 모델 <br> **원하는 데이터 구조는...**|컬렉션으로 정리된 문서|간단한 JSON 형태의 데이터|
|가용성 <br> **가용성 요구사항은...**|99.999%의 매우 높은 업타임을 보장(예: 전자상거래 앱)|99.95% 이상의 업타임을 보장|
|로컬 데이터에 대한 오프라인 쿼리 <br> **내 앱은 연결이 제한되거나 없는 기기에서 쿼리를 수행해야 하는 빈도가**|높을 때|거의 또는 전혀 없을 때|
|데이터베이스 인스턴스 수 <br> **내 개별 프로젝트에서는**|단일 데이터베이스만 사용하면 될 때|많은 데이터 베이스를 사용해야 할 때(예를 들면, 각 주요 고객별 데이터베이스) 혹은 단일 데이터베이스만 사용하면 될 때|

<br>


# Cloud Firestore vs Cloud Storage

> Cloud Storage for Firebase is built for app developers who need to store and serve user-generated content, such as photos or videos.
<br>

> Use our flexible, scalable NoSQL cloud database to store and sync data for client- and server-side development.
<br>

Cloud Firestore, Cloud Storage와 같이 비슷한 이름을 가지고 있는 프로덕트가 있을때 그 기능을 직관적으로 파악하기가 쉽지 않다. 이런 네이밍에도 분명 납득할만한 논리가 있을테지만 사용하는 입장에서는 아쉬운 마음이 드는건 사실이다. 각설하고 Cloud Firestore와 Cloud Storage의 차이점을 살펴보자면, 이 둘은 사실 서로 비교가 불가능하다. 클라이언트에서 security rules애 따라 접근 가능하다는 사실 말고는 공통적인 부분이 거의 없다.

Cloud Firestore는 데이터를 querying하는데 사용되며 binary data를 저장하는데 쓰이는 일은 거의 없다. Firestore는 쿼리를 사용하기 위한 실제 값, 이를테면 이름, 시간, 그리고 다른 메타데이터를 저장하기 위해 사용된다. Firestore의 문서는 1MB로 사이즈 제한이 있기때문에, Cloud Storage와 같은 대용량의 데이터를 저장하는데 무리가 있다. Firestore는 일반적으로 Cloud Storage보다 데이터를 전송, 저장하는데 큰 비용이 든다(당연하게도 기본적인 저장소의 역할보다 더 많은 일을 하기 때문에).  


Cloud Storage는 filesystem path와 비슷한 path를 이용해서 binary 데이터를 저장하기 위한 용도로만 쓰인다. 이 것은 데이터베이스가 아니며, 쿼리를 쓸 수 없다. 대개 사진, 비디오, PDF, backups/exports, 아니면 큰 크기의 raw data를 저장하기 위한 용도로 쓰인다. 저장하기위한 한 오브젝트 당 5GB의 크기 제한이 있다. Cloud Storage는 아주 싸고 다운로드에 특화되어 빠르다. 



# Reference

- [What is Firebase? The complete story, abridged.](https://medium.com/firebase-developers/what-is-firebase-the-complete-story-abridged-bcc730c5f2c0)
- [데이터베이스 선택: Cloud Firestore 또는 실시간 데이터베이스](https://medium.com/firebase-developers/what-is-firebase-the-complete-story-abridged-bcc730c5f2c0)
https://firebase.google.com/docs/firestore/rtdb-vs-firestore?hl=ko
-[What is the difference between Firebase Storage and Cloud Firestore/Realtime Database?](https://stackoverflow.com/questions/61784913/what-is-the-difference-between-firebase-storage-and-cloud-firestore-realtime-dat)
