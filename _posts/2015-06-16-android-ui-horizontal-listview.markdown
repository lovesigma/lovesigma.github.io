---
layout: post
title:  "android ui horizontal listview"
date:   2015-06-16 11:00:36
categories: android 
--- 
- android horizontal listview
 <br>
android에서 listview는 많이 쓰는 기능중 하나 일 것인다.
listview는 일반적인 목록을 나열할때 많이 쓴다.
하지만 아쉽게도 가로로 나열 해주는 listview는 제공해 주지 않는다.
이럴땐 동적으로 LinearLayout에 view를 추가시켜주는 방법인데,
간략하게 말하자면 LinearLayout에 종류가 두가지인데, 하나는 세로로 추가되는 LinearLayout을 
parent로 넣고 가로로 추가되는 LinearLayout 계속 추가해주는 방식이다.
 - LinearLayout(세로로 추가되는)
 	- LinearLayout(가로로 추가되는)
 		- viewComponent
 		- viewComponent
 		- viewComponent
  - LinearLayout(세로로 추가되는)
 	- LinearLayout(가로로 추가되는)
 		- viewComponent
 		- viewComponent
 		- viewComponent
이런식이다.
이것을 코드로 나타내면
{% highlight javascript %}
    public static void makeMultilineLinearLayout(Context context, LinearLayout parent, List<View> views, int maxWidth){
        parent.removeAllViews();

        int tmpMaxWidth = maxWidth;
        LinearLayout linearLayout = new LinearLayout(context);
        LinearLayout.LayoutParams params = new LinearLayout.LayoutParams(ViewGroup.LayoutParams.FILL_PARENT, ViewGroup.LayoutParams.WRAP_CONTENT);
        params.setMargins(0,5,0,5);
        linearLayout.setLayoutParams(params);
        parent.addView(linearLayout);
        for(View view : views){
            view.measure(View.MeasureSpec.UNSPECIFIED, View.MeasureSpec.UNSPECIFIED);
            tmpMaxWidth = tmpMaxWidth - (view.getMeasuredWidth() + 5); //5 마진

            if(tmpMaxWidth <= 0){
                tmpMaxWidth = maxWidth - (view.getMeasuredWidth() + 5); // 5 마진
                linearLayout = new LinearLayout(context);
                linearLayout.setLayoutParams(params);
                parent.addView(linearLayout);
            }
            linearLayout.addView(view);
        }

    }
}
{% endhighlight %}
과 같은데 
여기서 받은 인자 3개는 세로로 추가되는 parent linearlayout과 해당 컨텐스트 , 가로 사이즈이다.
또 눈여겨 볼 코드는 view.measure 인데.
어떤 view에 크기를 알고 싶을때,
{% highlight javascript %}
 view.getWidth();
 view.getHeight();
{% endhighlight %}
 을 많이쓰는데 해당 방법은 onDraw가 불리지 않았을때는 0을 리턴한다.
 onDraw전에 크기를 알고 싶을때는 
{% highlight javascript %}
 view.measure(MeasureSpec.UNSPECIFIED, MeasureSpec.UNSPECIFIED); 
 view.getMeasuredWidth();
 view.getMeasuredHeight();
{% endhighlight %}
 다음과 같이 할 수 있다. (measure에 옵션은 추가적으로 더있음.)




