---
layout: post
title:  "builder pattern"
date:   2015-05-23 11:00:36
categories: pattern 
---
- 빌더패턴
<br>
객체지향언어에서 객체를 생성하는 것은 언어마다 다르지만 대부분에 언어에서 쉽게 객체를 생성 할 수 있다.
일반적으로 객체를 생성하는 방법은 생성자를 이용하여 프로그래머 누구든 쉽게 객체를 생성 할 수 있다.
 
일반적으로 객체 생성 과정에 여러개에 매개변수가 필요하거나, 생성자에 종류가 여러개라면 객체를 생성하는 방법에 대해 고민해 봐야한다.
자주 쓰는 방법으로는
텔레스코핑 생성자이다.
이 방법은 생성자에서 다른 생성자를 부르는 방식인데,
 
예를 들면 
{% highlight javascript %}
class Tellescoping{
	int a;
	int b;
	int c;
	Tellescoping(int a, int b, int c){
		this.a = a;
		this.b = b;
		this.c = c;
	}
 
	Tellescoping(int a){
		this.(a,3,4)
		//여기서 3,4는 디폴트 value이다.
	}
}
{% endhighlight %}
이렇게 하게되면 매개변수가 3개 일경우는 간단하지만 매개변수에 숫자가 늘어날수록 생성자를 만드는 과정과 번거러워지고 많아진다.
또한 가독성이 많이 떨어지게 되므로 매개변수가 많아 질수록 안좋은 코드가 될 확률이 높다.
 
가독성이 좋은 생성자 패턴으로는 자바빈즈 패턴이 있다.
이 방법은 기본 생성자로 객체를 생성하고, 이후 필요한 값들은 setter메소드를 이용해 값을 생성하는것이다.
장점은 생성자를 여러개, 어렵게 만들필요가 없다는 것과 가독성이 좋아진다. (각 멤버변수에 값을 유추할 수 있어짐) 
하지만, 안정성에는 떨어진다. 예를들면 어떤값을 setting 하기 전에 다른 쓰레드에서 해당 값을 참조하게 되면 문제가 생길수 있고,
필수값을 세팅하지 않는 문제도 생길 수 있다.
 
위와 같은 문제점을 해결하기 위해서 빌더 패턴을 사용할 수 있다.
 {% highlight javascript %}
public class 이력서{
     private final int 나이;
     private final int 학점;
     private final int calories;
     private final int fat;
 
     public static class Builder{
        private final int 나이;
        private final int 학점;
 
        private final int 자격증갯수 = 0;
        
 
        public Builder( int 나이, int 학점 ){
            this.나이 = 나이;
            this.학점 = 학점;
        }
 
        public Builder 자격증갯수( int 자격증갯수 ){
            this.자격증갯수 = 자격증갯수;
            return this;
        }
 
        public 이력서 build(){
            return new 이력서( this );
        }
   }
 
   private 이력서( Builder builder ){
         나이 = builder.나이;
         학점 = builder.학점;
         자격증갯수 = builder.자격증갯수;
         
    }
}
 {% endhighlight %}
빌더패턴으로 생성자가 완전히 생성하는 시점에 대한 안정성을 보장 받을 수 있고, 필수 매개변수와 특정 상황에 세팅하는 매개변수를 구분 할 수 있다.
단점은 class마다 builder를 만들어줘야한다는 불편한점이 있다. 따라서 매개변수가 일정 수 이상일때 사용하는것이 바람직하다. 
 



