---
layout: post
title:  "Android listview"
date:   2015-05-19 11:00:36
categories: Android 
---
- Android ListView
<br>
안도르이드에서 어플리케이션을 만들 때, listview는 필수적 컴포넌트이다.
list를 연결하는 클래스를 adapter클래스라고 하는데, adapter클래는 Android에서 기본적으로
제공해 주고 있다.
만약 리스트 뷰를 커스텀하게 바꾸고 싶다면 baseadapter를 implement한 클래스(사용자 클래스)를 정의 하고 사용하면 된다.
baseadapter를 implement 하려면 몇몇에 메소드를 정의해야하는데 그중 getItem, getSize, getView이다 
여기서 주의해야 할 사항은 getSize에는 리스트에 갯수를 넣어줘야한다. 즉 내가 연결한 listview에 아이템들이 listitems 라면, 사용자 adapter클래스에  listitems를 넣어주고 getSize에 listItems.size() 를 리턴해 주어야 리스트를 잘 연결 시킬수있다.
getItem은 일반적으로 adapter인스턴스에서 getItem으로 많이 쓰이기 때문에, 만약 클릭이벤트와 같이 item에 대한 정보를 알고 싶다면 꼭 listItems.get(position)을 리턴 해주어야 한다.
가장 중요한 메소드는 getView이다.
getView는 list에 한 아이템을 어떻게 보여주는 view를 리턴해주는데, 여기서 주의 해야 할 점은 
성능을 위해 view를 재사용한다는 것이다. 다시 말해 리스트 아이템이 1000개라고 해도 디바이스 크기를 고려하여 view 인스턴스는 여러개 만들지 않고, 내용만 바꿔 주는 것이다.
그래서 다음과 같은 실수를 하게 된다면, 디버깅도 어렵고, 코드상에서 에러를 찾지 못하게된다.
{% highlight javascript %}
if(!listItems.get(position.read)){
	titleTV.setBackground(Color.BLUE);
}
{% endhighlight %}
라고 코드를 짜면 개발자들은 itemlist중 read값이 false값에 대해 리스트뷰에서 제목을 파란색으로 보여주겠지 라고 기대한다. 물론 스크롤이 없거나, 스크롤을 하지 않았을 때는 기대한 대로 볼 수 있다.
하지만 스크롤은 한다면 view를 재사용하기 때문에 이미 파란색으로 세팅해놓은 view에 대해서 default색으로 바꿔 주지 않는다.(코드상 명시되어 있지 않으므로)
이러한 문제를 해결하기 위해 간단히 else문을 넣어 default색을 지정해주면 된다.
{% highlight javascript %}
if(!listItems.get(position.read)){
	titleTV.setBackground(Color.BLUE);
}
else{
	titleTV.setBackground(Color.BLACK);	
}
{% endhighlight %}

기본적으로 xml에 정의되어있는 색이 default색이지만, listview에서는 view를 재사용한다는 점에서 다음과 같은 코드에서 default색을 해줘야 하는것 이다. 


