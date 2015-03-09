---
layout: post
title:  "AngularJS watch count"
date:   2015-03-03 16:56:36
categories: jekyll update
---

angularJS watch는 개발자에게 손쉬운 개발과 짧은 코드라인을 제공해준다.
하지만  watch가 많아지고(리스트 일 경우 더 더욱) 그에 따라 성능이 저하되는 경우가 생긴다
전체 watch 갯수를 판단 할 수 있어야하고, 때 에 따라 어디서 watch가 많이 걸려있는지 파악해야한다.
watch count를 알아내는 방법은 다음과 같다.
{% highlight javascript %}
<script type="text/javascript">
(function () {
                var root = angular.element(document.getElementsByTagName('body'));

                var watchers = [];

                var f = function (element) {
                    angular.forEach(['$scope', '$isolateScope'], function (scopeProperty) {
                        if (element.data() && element.data().hasOwnProperty(scopeProperty)) {
                            angular.forEach(element.data()[scopeProperty].$$watchers, function (watcher) {
                                watchers.push(watcher);
                            });
                        }
                    });

                    angular.forEach(element.children(), function (childElement) {
                        f(angular.element(childElement));
                    });
                };

                f(root);

                // Remove duplicate watchers
                var watchersWithoutDuplicates = [];
                angular.forEach(watchers, function(item) {
                    if(watchersWithoutDuplicates.indexOf(item) < 0) {
                        watchersWithoutDuplicates.push(item);
                    }
                });

                console.log(watchersWithoutDuplicates.length);
            })();
</script>
{% endhighlight %}
이 코드에서는 root를 body태그에 걸었지만, css셀렉터를 사용하여 해당 영역 아래에 watch갯수를 확인 할 수 도 있다.

