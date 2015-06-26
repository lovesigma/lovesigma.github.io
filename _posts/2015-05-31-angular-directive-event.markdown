---
layout: post
title:  "angularjs directive event"
date:   2015-06-01 11:00:36
categories: angular 
---
- angularjs directive와 해당 directive에 이벤트핸들러 추가
<br>
angluarjs 에서 디렉티브는 많이 사용되는 컴포넌트이다.
디렉티브 폴리머 기법으로 이미 많은 디렉티브가 오픈소스로 개발되어서,
직접 개발하지 않아도 자신이 필요한 디렉티브를 쉽게 구해 사용할 수있다.
가끔 디렉티브를 내가 원하는대로 변형해야 하거나, 디렉티브에서 제공해주지 않은 이벤트 핸들러를 등록해야 하는경우가있다.
많은 디렉트브는 용도에 맞게 이벤트핸들러를 오픈해두지만, 가끔은 내가 필요한 용도에 이벤트 핸들러를 등록해줘야 하는경우가있다.
예를들면 해당 디렉티브는 ng-change 이벤트만 오픈시켜 두었는데, 이 디렉티브를 사용하는 개발자가 ng-click 이벤트가 필요 한 경우도
있을 수도 있다.
이럴때 해당 디렉티브에 이벤트를 거는 방법은 제이쿼리 함수를 사용하여 엘리먼트에 동적으로 이벤트를 등록해주면되는데
{% highlight javascript %}
$( "#dataTable tbody" ).on( "click", function() {
  console.log( $( this ).text() );
});
{% endhighlight %}
과 같이 제이쿼리 돔셀렉터에 on 메소드를 통해 이벤트를 등록 시킬 수 있다.
 
여기서 문제는 angular에서 컨트롤러가 바인딩 되는 시점에 돔이 안그려질수 있다.
예를들면 ng-repeat같은 돔을 동적으로 생성하는 엘리먼트들이 그러하다.
이럴 때는 제이쿼리 dom selector가 dom을 찾지 못해 해당 element에 이벤트를 붙히지 못하는 경우가 생긴다.
쉬운방법으로 timeout에 시간차를 둬서 그때 이벤트를 달면 된다.
하지만 timeout은 논리적이지 못하므로(몇 초뒤에 돔이 생길지 모름) 많은 코드에서 지양하는 방법이다
이때 제이쿼리 on이벤트에 인자를 추가하는 방법이있다.
{% highlight javascript %}
$( "#dataTable tbody" ).on( "click", "tr", function() {
  console.log( $( this ).text() );
});
 {% endhighlight %}
이렇게 하게되면 tbody안에 tr이 클릭 되었을때, 버블링을 통해 click이벤트를 받을 수 있다.



