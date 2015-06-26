---
layout: post
title:  "AngularJS 양방향 Binding의 원리"
date:   2015-02-10 18:53:36
categories: jekyll update
---

AngularJS의 양방향 Binding의 원리

Javascript MVC 프레임워크의 필수 기능을 하나 꼽으라면 Model과 View간의 Binding일 것이다.
이 Binding 덕분에 귀찮은 UI조작을 하지 않아도 된다.(물론 jQuery가 상당히 단순화 시켜주긴 했지만 그래도 귀찮다.)
 
보통 이 Binding을 구현하기 위해서는 이벤트 시스템이 필요한데, 데이터를 반영할 시점을 알아야 하기 때문이다.
일반적으로 HTML의 입력 엘리먼트들은 사용자의 입력이 있을 경우 이벤트를 발생시키며 이 이벤트 리스너에서 데이터를 반영하는 코드를 작성하게 된다.
 

이것은 View의 변경을 Model에 반영하는 과정이다.
그럼 Model의 변경을 View에 반영하는 것은 어떤가?
어떤식으로든 Model의 변경이 있을 경우 이 Model이 변경되었다는 이벤트를 발생시켜야, 그 이벤트 리스너에서 데이터를 View에 반영하는 코드를 작성할 수 있을 것이다.
 
AngularJS의 특징이 바로 양방향 Binding을 지원하는지만 Model 객체는 일반 javascript Object를 사용할 수 있다는 점이다.
그럼 AngularJS는 plain javascript object로 이것을 어떻게 구현했을까?
 
Binding에 관해서 AngularJS의 전략은 조금 특별하다.
AngularJS가 선택한 포인트는 Model을 변경했을때 어떻게 이벤트를 발생시키도록 할까가 아니라, 언제 Model이 변경되는가 이다.
 
Model이 변경된다는 것은 사실 따져보면 프로그램이 수행되어 코드상으로 Model 객체에 값을 세팅하게되는 상황을 말한다.
그런데 브라우저에서 돌아가는 이런 웹 어플리케이션에서 이 javascript 코드가 수행되는 상황은 언제인가?
 
1) HTML 엘리먼트의 이벤트 핸드러가 작동할 때
2) Ajax 통신의 결과가 도착했을때
3) timer에 의해 발생된 tick 이벤트의 핸들러가 작동할 때
 가만히 생각해 보면 위의 3가지 외에는 javascript 코드가 작동하는 경우가 없다는 것을 알 수 있을 것이다.(HTML5에서 지원되는 멀티스레드 관련은 여기서 논외로 하자.)

AngularJS는 이 3가지 경우에 대해 Model을 View에 반영시키는 코드가 작동하도록 구성되어 있다.
 
1) ng-click, ng-mousedown 등등의 핸들러
2) $http의 success, error 핸들러
3) $timer
 위의 경우에 관해서 AngularJS가 Model을 View에 반영시키도록 하는 과정을 digest loop라고 하는데, 

<img src="/images/digestloop.jpeg" />

위 그림에서 보듯이 scope에 등록된 watch가 Model을 View에 반영하는 코드가 되고, 이 watch들을 실행시켜주는 과정이 주기적으로 실행되기 때문에 digest loop라고 한다. 
 
그런데 이 digest loop를 작동시키는 것은 결국 AngularJS이므로 다시말해 AngularJS가 모르게 위의 3가지 경우를 처리하게 되면 digest loop가 작동하지 않게된다.
예를 들어 어떤 HTML 엘리먼트에 대해 ng-clic이 아니라 jQuery의 이벤트 핸들러를 사용하게 되면 Model이 View에 반영되지 않는다.


즉,
{% highlight javascript %}
$scope.btnClick = function()
    {
        $scope.text = "btn1 click"; //변경 내용이 화면에 나타남
    }
 
    jQuery( "#btn2" ).click( function()
    {
        $scope.text = "btn2 click"; //변경 내용이 화면에 나타나지 않음
    });
{% endhighlight %}
{% highlight javascript %}
<body ng-controller="fooCtrl">
    <div>{{text}}</div>
    <input id="btn1" type="button" value="버튼" ng-click="btnClick()" />
    <input id="btn2" type="button" value="버튼" />
</body> 
{% endhighlight %}

btn1에서 사용한 것 처럼 ng-click (AngularJS에서 데이터가 언제 변화 될 수 있을지대해 고려 된 이벤트)에서 변화시킨 text에 변화에 뷰에 반영하지만. jQuery에 이벤트 핸들러를 통해서, 변경된 text값에 대해서는 뷰에 반영시키지않는다,
즉 기존 옵저버 패턴처럼 데이터에 변화가 있으면 뷰에 반영하기 보다는 이벤트 자체에 옵저버를 등록하고, 이벤트가 발생했을때 해당 데이터 변화에 따른 view를 반영 시켜주는 것이다.


[출처] AngularJS의 양방향 Binding의 원리 - digest loop|작성자 쫌조
