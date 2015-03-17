---
layout: post
title:  "AngularJS ng-include performance"
date:   2015-03-17 18:53:36
categories: AngularJS
---

angularJS 지시자중 ng-include는 해당 지시자에 명시된 src(html)을 가져와서 붙여주는 역할을한다.
이를 테면 템플릿같은 html이 그러하다.
하지만 ng-include는 ng-repeat에서 사용하게되면 성능 저하를 초래할 수 있다.
그 이유를 살펴보면 angularjs소스를 보면 알수 있다.


{% highlight javascript %}
(2199) ngInclude: ngIncludeDirective
(2226) ngInclude: ngIncludeFillContentDirective
{% endhighlight %}
ngInclude는 두개에 지시자를 살펴보아야하는데 그 중 ngIncludeFillContentDirective 지시자를 살펴보면 
This directive is called during the $transclude call of the first `ngInclude` directive.
It will replace and compile the content of the element with the loaded template.
We need this directive so that the element content is already filled when
the link function of another directive on the same element as ngInclude
is called.
지시자에 설명 주석이고,
코드는 다음과 같다.
{% highlight javascript %}
var ngIncludeFillContentDirective = ['$compile',
  function($compile) {
    return {
      restrict: 'ECA',
      priority: -400,
      require: 'ngInclude',
      link: function(scope, $element, $attr, ctrl) {
        if (/SVG/.test($element[0].toString())) {
          // WebKit: https://bugs.webkit.org/show_bug.cgi?id=135698 --- SVG elements do not
          // support innerHTML, so detect this here and try to generate the contents
          // specially.
          $element.empty();
          $compile(jqLiteBuildFragment(ctrl.template, document).childNodes)(scope,
              function namespaceAdaptedClone(clone) {
            $element.append(clone);
          }, {futureParentElement: $element});
          return;
        }

        $element.html(ctrl.template);
        $compile($element.contents())(scope);
      }
    };
  }];
{% endhighlight %}
여기서 보면 우선 element.append와 complie이 눈에 띈다.
컴파일은 include에 html에 선언된 지시자를 해당 문맥에 맞게 컴파일 해주는 것이므로 비교적 시간이 오래걸린다.
element.append는 돔을 컨트롤 하는 것이기 때문에 마찬가지로 비교적 시간이 오래 걸린다.
실제로 브라우저에서 성능을 측정해 보아도 ng-include를 사용하게 되면 parse HTML과 addElement를 call하는 횟수가 증가하고 그에 따른 시간도 많이 드는 것을 확인 할 수 있다.