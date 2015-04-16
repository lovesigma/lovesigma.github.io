---
layout: post
title:  "AngularJS 자바스크립트 상속(더글라스 크락포드의 자바스크립트)"
date:   2015-03-13 11:00:36
categories: AngularJS
---
상속
상속은 프로그래밍에 큰 역할을 하는 기능이다.
자바스크립트는 상속을 prototype을 이용하여 하는데,
 
사용법은 다음과 같다





{% highlight javascript %}
var Cat = function(name) { 
  this.name = name;
  this.saying= '야옹'
}
 
//상속 
Cat.prototype = new Mammal();
 
Cat.prototype.getName = function(){
  //구현
}
}
{% endhighlight %}
이러한 상속은 좀 보기가 좋지 않다.
이 전글에서 method라는 메소드를 사용하여, 메소드추가를 쉽게 했던 것처럼
상속도 그렇게 만들수 있다.
 
-상속을 관계를 정의하는 메소드-
{% highlight javascript %}
Function.method('inherits', function(Parent){
  this.prototype = new Parent();
  return this;
});
{% endhighlight %}
위 코드를 통해
{% highlight javascript %}
var Cat = function(name) { 
  this.name = name;
  this.saying= '야옹'
}.inherits(Mammal);
{% endhighlight %}
인수가 많은 객체
가끔 생성자가 많은 인수를 필요로하는 생성자가 있다.
이러한 생성자에 경우에는 인수에 순서를 외우고 있거나, 계속해서 확인해야하는 불편한점이 있다.
그래서 자바스크립트에서는 인수를 따로 받지않고 객체를 받는 방법을 선택한다.
이렇게 할 경우 인수에 순서를 외울 필요도 없으며, 때에 따라 필요없는 값을 생략도 가능하다.
 
함수형클래스방식
 
이러한 클래스에 가장 큰 문제는 name이라는 변수가 무조건 public 하다는 것이다.
만약 어떠한 변수가 외부에서 바뀌지 않기를 원한다면 다음과 같은 패턴을 사용해서, private 속성을 유지 시킬수있다.
{% highlight javascript %}
var mammal = function(spec){
  var that = {};
  that.get_name(){
    return spec.name
  }
}
var myMammal = mammal({name : 'kun'});
{% endhighlight %}
이로써 외부에서는 name값을 가져올수는 있지만 변경이 불가능하게 만들수 있습니다.
함수형 패턴에 상속은 다음과 같이 할 수 있습니다.
{% highlight javascript %}
var cat = function(spec){
  var that = mammal(spec);
  //cat에서 추가 / 오버라이드 하고 싶은 구현....
  return that
}
{% endhighlight %}
함수형 패턴에서는 오버라이드 메소드 말고 부모 메소드에 접근할수 있는 방법도 있다
{% highlight javascript %}
Object.method('super',function(name){
  var that = this,
  method = this[name];
  return function(){
    return method.applay(that, arguments);
  }
});
{% endhighlight %}
만약 cat 생성자에서 부모 getName메소드를 호출할수 있게 해주고 싶다면
{% highlight javascript %}
var cat = function(spec) {
  var that = {}
  super_get_name = that.superior('getName');
}
{% endhighlight %}