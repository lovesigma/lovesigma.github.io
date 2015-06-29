---
layout: post
title:  "angular resource"
date:   2015-06-26 11:00:36
categories:  angular
--- 
Angularjs $resource과 uri path value request에 넣지 않기.
 
Angularjs에서 $resourcesm 는 RESTful 클라이언트 애플리케이션 개발을 위한 $http 상위 레벨의 서비스이다.
그렇다 보니 http보다 더 쉽게 header configuration이 가능하다.
 
기본적으로 사용법 자체는 아주 간단하다.
 
restful url에는 url path값이 동적인 경우가 많다.
 {% highlight javascript %}
$resource('http://localhost/test/:type',{
	type : '@type'
},{
    getMethod:{
        method:'GET',
        isArray:true
    }
    postMethod:{
        url:'http://localhost/pleasePosthere',
        method:'POST',
        isArray:false
    }
}
{% endhighlight %}
 
만약여기서 /test/ 뒤에 type이 동적으로 바뀐다고 했을때
var query = {type : 'message' //...추가적인 request 인자들}
TestResource.getMethod(query)와 같이 해주면, type에 message라는 값이 들어가 해당 url로 http요청을한다.
 
여기서 문제가 한가지 있다.
저렇게 사용하게 되면, http request 자체에 type이 따라서 들어간다.
type은 url path용도 뿐이지, request에 포함하고 싶지 않다면,(궅이 뺄 필요가 없으면 상관없음.)
type별로 메소드를 따로 만들어야한다.
 {% highlight javascript %}
getMessage{
        method:'GET',
        isArray:true,
        
    	
}
{% endhighlight %}