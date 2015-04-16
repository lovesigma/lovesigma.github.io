---
layout: post
title:  "AngularJS 자바스크립트 (더글라스 크락포드의 자바스크립트 핵심가이드1~4장)"
date:   2015-03-18 11:00:36
categories: AngularJS
---

더글라스 크락포드의 자바스크립트 핵심가이드라는 책(1~4장)을 보고, 개발할 때 편하고 코드를 깔끔하게 해주는 팁과
성능을 좋게 유지하는 법, 실수 하기 쉬운법을 정리 해보았다.

<Tip>
-Prototype 사용 간편하게 하기-
자바스크립트는 기본타입에 기능을 추가하는것이 가능하다
오브젝트에 prototype에 메소드를 추가하여, 모든 객체에서 prototype에 추가한 속성을 사용할 수 있습니다.
하지만 추가 할 때마다 (getName이라는 메소드를 추가하고 싶을 때)


{% highlight javascript %}
Object.prototype.getName = function(){
  return this.name;
}
{% endhighlight %}
이런식으로 하게 된다 prototype에 속성을 추가하는 일이 많아진다면,
Function.prototype에 method 메소드를 추가하여 모든 함수에서 이 메소드를 사용하게 만들수 있다
{% highlight javascript %}
Function.prototype.method = function(name function){
  this.prototype[name] = func;
  return this;
};
{% endhighlight %}
이 메소드를 추가함으로써 속성을 추가하기위해 .prototype부분없이 깔끔하게 사용 할 수 있다.
{% highlight javascript %}
Person.method('getName',function(){
  return this.name
})
{% endhighlight %}
<성능개선>
  -메모제이션-
메모제이션이란 어떤 연산을 하기 위한 함수에서, 함수에 연산결과를 빠르게 반환하기 위해 사용하는 기법중 하나이다.
예를들어, 어떤함수에서 값을 받아 그 값을 연산하여 리턴하는 함수라고 가정해보자.
이 함수가 1초에 천번 실행 한다고 가정하고
value에 범위(변할 수 있는 범위가 어느정도 작다고 가정할 때)
해당 함수에
var memo[value]에 연산결과값을 셋팅하는 것이다
이렇게 되면 같은 value값에 대한 연산은 한번만 실행하고 만약 value가 한번이라도 실행 됬으면, memo값에 있는 결과값을 바로 반환해줌으로써,
비교적 메모리를 좀더 쓰지만, 연산 시간을 대폭 줄일 수 있다.
이 방법은 함수 호출 횟수가 많고, value값에 범위가 어느정도 정해져 있을때 유용하게 사용할 수있다.
value값이 너무 폭이 넓으면 메모리 값을 너무 많이 차지하고, 함수 호출 횟수가 적으면 굳이 메모제이션을 사용할 필요가 없는것 이다.

<실수하기 쉬운 부분>
 -클로저 사용에 실수-
클로저에 기본개념부터 살펴보자면,
내부함수에서 자신을 포함하고있는 외부함수에 값을 접근 할 수 있다는 점을 활용하여, 내부 변수를 숨기는 것이 가능하게 한다는 점이다
 
하지만 내부함수에서 외부함수에 값을 전달 받을때는 값을 복사하는것이 아니라 실제 그값을 계속 사용한다는 점을 유의 해야한다.
이것을 이해하지 못하면 다음과 같은 실수를 범할 수 있을 것이다
실수 예제
{% highlight javascript %}
injectClickActionGetNodeNumber = function(nodes){
  var i = 0;
  for (i = 0; i < nodes.length; i += 1){
    nodes[i].onclick =function(i){
      alert(i);
    }
  }
}
{% endhighlight %}
위 함수는 노드배열을 받아서 해당 배열안에 노드들에게 onclick이벤트를 추가 해주는데 클릭을 하게 될때 마다 해당 node에 순서가 alert에 찍히는것을 기대하는 함수이지만,
결과는 무조건 마지막(노드의 개수) 순서만 alert에 찍힐 것이다.
그 이유는 for안에서 i 값을 그 순간에 i 값을 가져가는것이 아니고 i에 변수 자체에 연결되기때문에 마지막 i값을 가지고 있기 때문이다.
더 나은 예
{% highlight javascript %}
injectClickActionGetNodeNumber = function(nodes){
  var i = 0;
  for (i = 0; i < nodes.length; i += 1){
    nodes[i].onclick =function(i){
      return function(e){
        alert(i);
      }
    }(i);
  }
}
{% endhighlight %}
여기서 새로 함수를 정의하고  바로 i 값을 이요하여 함수를 실행 시켰다.
위 함수에서는 정의된 i가 아니라 념거받은 i의 값을 이벤트 핸들러 함수에 연결하여 반환하기 때문에
원하는 결과를 가져올 수 있다.
