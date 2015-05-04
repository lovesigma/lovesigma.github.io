---
layout: post
title:  "Android Host파일 설정"
date:   2015-05-04 11:00:36
categories: Android 
---

안드로이드를 개발하다 보면, 공인 IP가 아닌 특정, 로컬IP 같은 외부에서 접속하지 못하는 
IP 서버에 접속해야하는 경우가 있다. 
이럴경우 host파일을 조작해 도메인주소를 특정IP로 매핑시켜 접속을 해야하는데,
Android 폰에서는 host파일을 조작하지 못한다.(정상적인 방법으로는..)
이럴때 host파일을 설정하는 방법은 몇가지 있는데, 그중 가장 쉬운방법은 데스크탑에 QuidMan프로그램을 실행시켜 프록시 서버를 여는 것이다. 
폰에서는 와이파이 환경설정에서 내 데스크탑 프록시 서버를 타게 만들어, 내 데스크탑에서 설정한 Host파일 설정을 사용 할 수 있다.
하지만, 위 방법은 특정 app (app내 web app)에서 동작하지 않을 수 있다.
두 번째 방법은 툴에서 제공해주는 버츄얼 머신에 host파일을 조작하는 방법이다.
우선 버츄얼머신을 시작한 후, 데스크탑에 Android SDK가 설치 된 장소로 이동한다.
그후 platform-tools 디렉토리로 들어가면 자신이 시작한 
버츄얼 마신 adb shell로 접속 할 수 있다.
adb쉘로 접속하는 명령어는 다음과 같다.
platform-tools $ ./adb shell
접속을 한 후 이 shell안에서 host파일을 접속 하면 좋겠지만, 
vi 나 vim같은 기능이 없으므로 밖으로 빼서 host파일을 조작해야한다. 
host파일을 빼내기 전에 host파일을 변경 가능 할 수 있도록 mount옵션을 바꾸어야 한다.
root@heneric_x86: mount -o rw,remount /system
그 후 exit 명령어로 shell을 종료 한 다음 
pull 명령어로 host파일을 내려받고, 자신이 원하는 대로 host파일을 조작한 후 
다시 adb에 push를 해주게 되면 해당 버츄얼머신에 바뀐 host파일을 사용 할 수 있다.
