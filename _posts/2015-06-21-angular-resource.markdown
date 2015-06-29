---
layout: post
title:  "angular resource"
date:   2015-06-26 11:00:36
categories:  angular
--- 
- IntelliJ 에서 파일 변경 반영하기.

IntelliJ를 통해 톰캣위에 간단한 테스트를 위한 웹페이지를 만들고 있었는데, 한가지 문제가 있었다.
그 문제는 grunt를 사용하여, html이나, css파일이 바뀌면 저절로 파일을 갱신시켜주는 watch를 달았음에도,
tomcat을 껐다 켜여야만, 바뀐정보들이 반영되는 것이었다.
이 문제는 IntelliJ옵션으로 해결이 가능 한 문제였다.
http://stackoverflow.com/questions/19596779/intellij-and-tomcat-changed-files-are-not-automatically-recognized-by-tomcat
위 사이트에 잘 정리 되어있었다.
문제 해결 방법은 다음과 같다.

우선 Run/Debug configuration 을 열고,
war 파일에 exploded설정에서 Server 탭에 on Update action /on framedeactivation에서
'update classes and resources'를 선택하면 된다.
그러면 앱이 쉬는동안 자동으로 바뀐내용을 반영 시켜준다.

- CORS문제 쉽게 해결하기

이 방법은 임시적으로 해결할 수 있는 방법이긴 하지만, 테스트 페이지 같은경우에는 아주 유용하게 쓰일수 있을것같다.
CORS는 기본적으로 크로스 도메인 환경에서 발생하는 문제로, javascript에서 해당 도메인이 아닌 다른 도메인으로 네트워크 요청을 할시 발생 하는문제이다.
이 문제는 보안관련 이슈로 javascript에서 다른도메인으로 요청을 하려면, 서버쪽에서 헤더 정보를 세팅해줘야한다.
하지만, 임시적으로 막을수 있는 plugin이 크롬에서 제공해준다.
https://chrome.google.com/webstore/detail/allow-control-allow-origi/nlfbmbojpeacfghkpbjhddihlkkiljbi
해당 플러그인을 on시키면 아무 제약없이 Ajax요청을 보낼수 있다.
