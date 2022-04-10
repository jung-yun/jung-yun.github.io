---
layout: single
title: "Firebase with iOS, the ultimate guide"
date: "2022-04-08"
last_modified_at: "2022-04-08"
---

# Firebase 간단 소개
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
프론트 엔드와 백엔드 소프트웨어를 전부 포함하던 전통적인 앱 개발과 다른 것과 양상이 달라진 것이다. 예전에는 프론트엔드는 백엔드에서 노출하는 API 엔드포인트를 사용하기만 하고 백엔드가 실제로 작업을 하면 됐다면, 이제는 firebase 프로덕트로 작업이 클라이언트로 옮겨오게 되면서 전통적인 백엔드는 생략되는 것이다. Administrative한 접근은 [Firebase console](https://console.firebase.google.com/u/0/)을 통해서 제공된다.

 
# Reference

- [What is Firebase? The complete story, abridged.](https://medium.com/firebase-developers/what-is-firebase-the-complete-story-abridged-bcc730c5f2c0)
