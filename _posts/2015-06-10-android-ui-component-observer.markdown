---
layout: post
title:  "android ui component observer"
date:   2015-06-10 11:00:36
categories: css 
--- 
- view크기를 측정하는 방법
 <br>
android에서 view에 layout을 잡을때, 컴포넌트들에 크기를 알아야 할 필요가 있을때 가 있다.
예를들면 가로로 컴포넌트를 나열할때, 컴포넌트에 크기를 알아야 
해당 컨포넌트를 다음줄에 위치 시켜야할지 아니면 옆에 위치 시켜야할지 정할 수 있다.
하지만 view 컴포넌트 속성에 따라, widht,height가 0으로 나올때 가 있는데 
이 문제는 대부분이 아직 렌더링이 되지 않아서 크기를 알 수 없는 경우가 있다.
이럴때, 사용할 수 있는 코드를  android에서 제공해준다.
{% highlight javascript %}
LinearLayout layout = (LinearLayout)findViewById(R.id.YOUR_VIEW_ID);
ViewTreeObserver vto = layout.getViewTreeObserver();
vto.addOnGlobalLayoutListener(new OnGlobalLayoutListener() { 
    @Override 
    public void onGlobalLayout() { 
        this.layout.getViewTreeObserver().removeGlobalOnLayoutListener(this); 
        int width  = layout.getMeasuredWidth();
        int height = layout.getMeasuredHeight(); 
 
    } 
});
{% endhighlight %}
해당 코드를 보면 layout에 옵저버를 두어, 해당 컴포넌트가 그려지면 불리는 콜백메소드를 등록할 수있다.
이를 통해, 해당 view가 랜더링 후에 크기를 알 수있다.
주의 해야할점은 callback이 호출된후 옵저버(관찰)를 멈춰야 한다.



