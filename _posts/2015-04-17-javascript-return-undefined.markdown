---
layout: post
title:  "javascript-return-undefined"
date:   2015-04-17 11:00:36
categories: javascript
---
자바스크립트 코딩을 하다가 객체를 리턴 하는데, 
바꾸 undefined가 리턴되는 버그가 있다.
이 문제는 자바스크립트 문법 문제인 것이다.
다음 코드를 보면

{% highlight javascript %}


<script type="text/javascript">
var returnObject = function(){
	....

	return 
			{'num' : num
			'cnt' : cnt
			}
}
</script>
{% endhighlight %}
returnObject function은 
num,cnt를 멤버로 가지고 있는 object를 리턴을 기대하지만,
undefined가 리턴된다.
그 이유는 자바스크립트 엔진 자체가 줄 바꿈 뒤에는 무조건 ;를 붙이는 것이다.
이 기능 때문에, 다른 언어와달리 ;를 붙이지 않아도, 컴파일이 잘되는것이다.(물론 권장하진 않음)
그렇기 때문에 저코드는 사실상 다음과 같이 컴파일 된다.
{% highlight javascript %}
<script type="text/javascript">
return ;

			{'num' : num
			'cnt' : cnt
			}
			</script>
			</script>
{% endhighlight %}

