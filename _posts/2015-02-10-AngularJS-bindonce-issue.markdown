---
layout: post
title:  "Angularjs Bindonce [성능이슈]"
date:   2015-02-10 18:53:36
categories: jekyll update
---
<ng-if>
angularJS 지시자중 ng-if 는 많이 사용하는 지시자중 하나이다.
돔을 형성 할 때 ng-if안에 값에 따라 돔을 형성할지 결정 해준다.
ng-if와 비슷한 성격에 지시자로는 ng-hide와 ng-show가 있는데, 이역시도 값에 따라 view에 보일지 말지 결정한다.
ng-if와 show hide에 가장 큰 차이는 if는 돔 자체를 만들지 않고, show,hide는 돔은 만들지만 해당 돔 속성에 display값을 결정하는것이다.
여담으로 hide와 show에 역할은 같다. true냐 fasle이냐에 차이이다. 해당 돔에 역할을 보고 hide로 할지 show를 할지만 골라 더 가독성을 높히는것 이상에 차이는 없다.
다시 본론으로 넘어가 ng-if는 돔자체를 만들어 주지 않기 때문에, 그안에 있는 ng-controller에 $scope를 만드는지도 결정하는 중요한 역할을 한다.
ng-if에 가장 큰 특징은 새로운 scope를 형성한다는 것이다.
다시 말해, angularjs에서 사용자가 만들지 않는 새로운 scope를 만들어 준다는 것이다.
이것때문에 , 가끔은 원하는 결과를 가져오지 못하는 부분이 생긴다.
다음 예를 살펴보자.
<script>
    function main($scope) {
        $scope.testa = false;
        $scope.testb = false;
        $scope.testc = false;
    }
</script>
<div ng-app >
    <div ng-controller="main">
        
        Test A: {{testa}}<br />
        Test B: {{testb}}<br />
        Test C: {{testc}}<br />
        
        <div>
            testa (without ng-if): <input type="checkbox" ng-model="testa" />
        </div>
        <div ng-if="!testa">
            testb (with ng-if): <input type="checkbox" ng-model="testb" /> {{testb}}
        </div>
        <div ng-if="!someothervar">
            testc (with ng-if): <input type="checkbox" ng-model="$parent.testc" />
        </div>
   </div>
</div>
여기서 ng-if로 감싸여진 div는 새로운 scope를 만든다.
즉 ng-if로 싸여진 ng-model에 데이터는 main scope에 데이터가 아니라는 말이다.
만약 개발자에 의도대로 ng-if에서 main scope데이터로 접근 하고 싶다면, $parent 를 이용해 부모Scope로 접근해야 한다.
