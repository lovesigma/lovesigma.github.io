---
layout: post
title:  "Angularjs Bindonce [성능이슈]"
date:   2015-02-10 18:53:36
categories: jekyll update
---
<Bindonce>
AngularJS 에 가장 큰 장점 중 하나는 데이터 바인딩이 쉽다는 것이다.
데이터 바인딩을 통해 스크립트와 마크업 코드를 더 가볍게 만들 수 있다.
이러한 데이터 바인딩이 가능한 이유는 angularJS에서 watch를 하고 있기 때문이다.
watch는 데이터에 변화가 있으면, 해당 데이터에 변화가 일어나면 그 데이터를 watch하고 있던 곳에 모두 signal을 주게 된다.
이렇다 보니, watch 덕분에 코드는 깔끔해지지만 성능상에 이슈가 생긴다.
예를들어
{% highlight javascript %}
<ul>
    <li ng-repeat="person in Persons">
        <a ng-href="#/people/{{person.id}}"><img ng-src="{{person.imageUrl}}"></a>
        <a ng-href="#/people/{{person.id}}"><span ng-bind="person.name"></span></a>
        <p ng-class="{'cycled':person.generated}" ng-bind-html-unsafe="person.description"></p>
    </li>
</ul>
{% endhighlight %}
다음과 같은 코드에서 ng 디렉티브를 쓴 곳 모두 watch가 걸려 있다고 볼 수 있다.
만약  Persons Array에 item이 많다면 엄청난 양에 watch가 걸릴 것이다.

이러한 성능 이슈를 해결 할 수 있는 AngularJS 라이브러리인 bindonce가 있다.
bindonce를 사용하게 되면, 다음과 같은 코드가 나오게 된다.

{% highlight javascript %}
<ul>
    <li bindonce ng-repeat="person in Persons">
        <a bo-href="'#/people/' + person.id"><img bo-src="person.imageUrl"></a>
        <a bo-href="'#/people/' + person.id" bo-text="person.name"></a>
        <p bo-class="{'cycled':person.generated}" bo-html="person.description"></p>
    </li>
</ul>
{% endhighlight %}
이 코드를 보게 되면, ng디렉트를 사용하지 않고, bindonce에서 제공해주는 디렉티브를 사용하게 된다.
bindonce에서 제공해주는 디렉티브를 사용하게 되면, 처음 Dom을 만들때 만 해당 데이터에 값을 읽어오고, 
그다음은 그 데이터에 값 변화에 신경쓰지 않는다.
즉 위 코드는 watch가 하나도 생성 되지 않는다.

dom을 구성할 때 데이터 값 변화에 렌더링을 다시 해줘야하는 경우에는 ng 지시자를 사용하고,
데이터값이 바뀌지 않거나, 데이터값이 바뀌어도 view에 표현이 필요 없는 부분은 bindonce를 사용하여, 성능을 올릴 수 있을 것이다.
bindonce는 ng지시자에 거의 모든 부분을 커버할수 있는 bindonce에 지시자를 제공해 준다.
https://github.com/Pasvaz/bindonce 에서 확인 할 수 있다.
