---
layout: post
title:  "자바스크립트 온오프믹스 중급과정 교육 정리"
date:   2015-04-20 11:00:36
categories: javascript
---

자바스크립트?
 - 객체지향 프로그래밍 언어

- 스크립팅 언어
  - 코드를 만다는 시점에 컴파일 실행
    느리지 않을까? (생각 해볼 필요가 있다, 클라이언트이기 떄문에 어느정도는…)
  - 이것 때문에 생기는 장점도 많다! , 개발자에 몫 -> 어떻게 이용할 수 있을가..
실행 파일 언어와 환경 다름
  - 설계 구현에 차이 있음,
  - 장점을 활용하여 설계 구현 필요
  - 설계에 반영할 때! 코드를 만나느 시점에 컴파일을 실행한다는 !! 것을 유의!
	exe를 만드는 설계 개념과는 완전히 다르다.

언어 특징
	JIT컴파일
		함수안에 10라인이 있다고 했을때, function안에 전체를 들어올때 컴파일함.
		인터프리터와는 다르다.
	코드를 만나는 시점에 컴파일
		함수 안은 컴파일 하지 않음.(그 안에 함수는 신경쓰지 않는다.)
렌더링
	렌더링 
		자바스크립트 실행 환경 설정
	랜더링 시점
		<scipt></script>를 만났을 때
	렌더링 결과
		오브젝트 생성
		값 초기화
		연산자 설정 등
	빌트인 생성* (만들어 놨다,)
		렌더링 할 때 생성
		즉시 사용 가능
빌트인 * 
	빌트인 유형
		테이터타입,오브젝트,연산자
	빌트인 데이터 타입
		Undefined,Null Boolean ,Number, String
		undefined 과 null의 차이
	빌트인 오브젝트
		Global, Object Function, Array

자바스크립트는 객체지향언어이다
	객체지향언어를 이해해야한다!
		하지만 언어마다 조금씩은 수정된것은 존재함(다중상속같은..?)

자바와의 차이
	자바는 new 자체를 해야 object를 생성하고,접근할 수 있다
	자바스크립트는, new 연산자를 실행하는 것 자체도 object로 하는 것! 차이가 분명히 있다.

데이터 타입 기준
	데이터 타입으로 오브젝트 결정
		'a'.concat('b');
		['a'].concat('b');
	빌트인 타입, 빌트인 오브젝트 개념이 필요하다!

Function 오브젝트 생성 *
	자바스크립트엔진 function()키워드를 만드면 빌트인 Function 오브젝트로 Function 오브젝트 생성
		-인스턴스 개념

	즉! function도 오브젝트이다
		오브젝트는 메소드와 프로퍼티가 존재한다!
		call,apply,bind->function 오브젝트에 메소드!

프리미티브 값(default)
	인스턴스에 () (String number array date)

자바스크립트에서는 Own이라는 단어를 많이 쓰는데, 예를들면 내가만든 프로퍼티, 즉 상속받아서 생긴 프로퍼티가 아닌값을 의미

정적 환경
	function이 만들어질때, 이것이 어디 환경에서 사용될수 있는지 어딘가에 적어놓는다(scope)

함수 선언문과 함수 표현식 왜 두개 다있을까
	자바스크립트는 스크립팅언어이다!
		함수 선언문 먼저 해석. 중요!!
		예를들면
		{% highlight javascript %}
			<script type="text/javascript">
		window.onload = function(){
		var sports = function(){
				debugger;
				var player = 11;
				function soccer(){
					return player;
				}
				var swim = function(){

				}
				soccer();
		}
		sports();
	}
	</script>
		{% endhighlight %}
	다음 소스에서 

	sports()함수를 호출하면
	호출되자마자 debgger가 걸리면서
	브라우저가 디버깅모드로 들어간느데 이시점에서 
	scope val들을 보면, player swim은 undefinned이지만, 
	sports는 fn이 리턴되었다.
	즉 함수를 표현식으로 하지않고, 선언문으로 작성시 무엇보다 먼저 해석되는 것을 알수 있다.


- 엔진 해석 단계 *
	- 함수 선언문 해석
		- function sports(){}
	- 변수 초기화
		- var sports
		- var member
	- 자바스크립트 코드 실행
		- var sports = function(){};
		- var member = 123;

		- 이것때문에 함수 호이스팅이 가능해짐.



- 스코프 설정 시점*
	- Function 오브젝트를 생성할 때
		-함수를 호출할 때 설정하지 않음


- 바인딩 시점
	- 정적 바인딩
		- 초기화 단계에서 바인딩
			- 변수; 초깃값 설정
			- 함수 선언문:Function오브젝트 생성
		- 대부분 함수
	- 동적 바인딩
		- 실행 단계에서 바인딩
		- eval함수, with		

- 실행 콘텍스트
	- Execution Contexts
	- 함수의 실행 영역
		- 함수 코드 해석 결과 저장
		- 함수 코드 실행
- 실행 콘텍스트 단계
	- 준비단계
	- 초기화단계
	- 코드실행

실행 콘텍스트 에서 렉시컬 환경은 부모 컨텍스트만 가지고 있다.!! 만약 2단계 위에 스코프에 접근할려면 낭비이다 -> 설계를 잘하자. 함수안에서 최대한 자기것만쓰자~!
