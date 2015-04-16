---
layout: post
title:  "AngularJS pagination 사용법과 겪었던 문제."
date:   2015-03-30 11:00:36
categories: AngularJS
---
Angular bootstrap pagination

페이지네이션 기능은 웹페이지에 하나씩은 꼭 있는 기능중 하나이다.
이러한 일반적인 기능들은, 직접 구현하기보다는 다른 오프소슨에서 제공해주는 것을 사용하는게
시간상으로 절약 되고, 안정성을 보장 받을 수 있다
angularjs bootstap에서도 역시 pagenation을 디렉티브로 제공해 주기 때문에, 쉽게 가져다 쓸 수 있다.
홈페이지에 나와있는 디렉티브 사용법은 다음과 같다.

ng-model  : Current page number. First page is 1.

total-items  : Total number of items in all pages.

items-per-page  (Defaults: 10) : Maximum number of items per page. A value less than one indicates all items on one page.

max-size  (Defaults: null) : Limit number for pagination size.

num-pages readonly (Defaults: angular.noop) : An optional expression assigned the total number of pages to display.

rotate (Defaults: true) : Whether to keep current page in the middle of the visible ones.

direction-links (Default: true) : Whether to display Previous / Next buttons.

previous-text (Default: 'Previous') : Text for Previous button.

next-text (Default: 'Next') : Text for Next button.

boundary-links (Default: false) : Whether to display First / Last buttons.

first-text (Default: 'First') : Text for First button.

last-text (Default: 'Last') : Text for Last button.

사용법은 name별로 어떤 값을 줘야하는지, 어떤 값을 의미하는지를 나타낸다.
여기서 눈여겨 봐야 할 값은 ng-model , total-items, items-per-page
이 세가지가 될 것 이다.

ng-model은 페이지의 값(넘버)이고, total-items는 페이징 할려는 아이템에 갯수이다.
items-per-page는 한 페이지에 보여줄 아이템에 갯수이다.

즉, 5000개에 아이템이 있고(total-items=5000), 한 페이지에 10개 씩(items-per-page) 보여준다면, 페이징 갯수는 500 페이지 까지 나올 것이다.

나 역시도 웹페이지에 페이징이 필요하여, 위 페이지네이션을 가져다 썼는데, 
ng-model에 current page를 url param에 값을 가져와 그 값으로 초기화를 해주고 싶었는데, 계속 1페이지로 설정 되는 문제가 있었다.
원인은, 내가 사용하는 페이지는, 처음에 scope를 할당해 페이지를 만들고, 만들면서 서버에서 데이터를 요청해, total-items값을 알아와 셋해주는 것이었다.
문제는 scope가 처음에 바인딩 될때 total-items에 값이 없기 때문이 었다.
즉, total-items가 0이면 페이징에 갯수는 1페이지 이상이 될 수 없기 때문에 아무리 current페이지 값을 올려도 저절로 1페이지로 셋팅되는 것 이었다.

해결방법은 ng-if에서 total-count>0 일 때 scope를 바인딩 시켜줌으로써 해결 할 수 있었다.
