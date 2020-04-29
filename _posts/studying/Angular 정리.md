# Angular 정리

## Angular JS는

자바스크립트 프레임 워크로 html에 태그 추가해 사용 가능하다

```js
<script src="http://ajax.googleapis.com/ajax/libs/angularjs/1.3.14/angular.min.js"></script>
```

위와같이 스크립트 형태로 추가해 사용할 수 있다.

기본적으로 html에서 확장시켜서 사용할 수 있고, 특징은 앞에 ng가 붙는다는 특징이 있다

## MVC 프레임워크(<http://soomong.net/blog/2014/01/20/translation-ultimate-guide-to-learning-angularjs-in-one-day/>)

- 모델 : 일반적으로 JSon으로 표현되는 애플리케이션의 특정 데이터 구조이다. 뷰가 서버와 통신하기 위해 꼭 필요한 내용
- 뷰 : HTML 혹은 렌더링 된 결과를 말한다. 뷰를 갱신할 모델 데이터를 받아 HTML에서 해당 데이터를 보여주는 것
- 컨트롤러 : 데이터를 조정하는 위치로 일종의 중간 통로이다. 서버와 클라이언트 통신으로 데이터를 변경한다.

## 설정

시작 시에는 `ng-*` 을 통해 앱을 정의

```html
<div ng-app="myApp">
    <div ng-controller="MainCtrl">
    </div>
</div>
```

와 같이 html에 선언한다. 그리고 모듈과 컨트롤러에는

```js
var myApp = angular.module('myApp', []);

myApp.controller('MainCtrl', ['$scope', function ($scope){
    // Controller magic
}]);
```

위와 같이 로직을 담는 방식이다. 이를 여러개를 쓰기 위해선 `myApp.controller('Name',...)` 와 같은 방식으로 로직을 만들면된다.

여기서 원하는 내용을 집어 넣을 때

```html
<div ng-app="myApp">
    <div ng-controller="MainCtrl">
         {{ text }}
    </div>
</div>
```

```js
var myApp = angular.module('myApp', []);

myApp.controller('MainCtrl', ['$scope', function ($scope) {

    $scope.text = 'Hello, Angular fanatic.';

}]);
```

와 같이 진행하면 text값이 출력이된다.

여기서 중요한 `$scope`라는 개념이 나타난다.

### $scope

이 값은 DOM의 현재 요소/영역을 참조하며(this랑 다름) 요소안의 데이터와 로직을 주시하는 관찰 기능이 있다.

DOM을 만드는데 아주 유용한 방식으로 예를 보면

```js
var myApp = angular.module('myApp', []);

myApp.controller('UserCtrl', ['$scope', function ($scope) {

    // user details라는 네임스페이스를 사용하자. DOM에서 알아보기도 좋을 것이다.
    $scope.user = {};
    $scope.user.details = {
      "username": "Todd Motto",
      "id": "89101112"
    };

}]);
```

```html
<div ng-app="myApp">
    <div ng-controller="UserCtrl">
        <p class="username">Welcome, {{ user.details.username }}</p>
        <p class="id">User ID: {{ user.details.id }}</p>
    </div>
</div>
```

위와 같이 사용하면 해당 내용들을 scope를 이용해서 controller에 접근하고 변수처럼 사용하는 것을 볼 수 있다.

이렇게 만들 때 해당 컨트롤러를 정의하는 함수를 전역 함수처럼 절대 만들지 말자.

### 디렉티브

디렉티브는 html태그에 고유한 속성을 추가하고, 혹은 태그를 작성해 html안에 특수한 기능을 할당하고 이를 디렉티브라고 부른다.

디렉티브를 사용하면 쉽게 DOM을 주입하거나 사용자 정의 DOM의 상호작용을 적용할 수 있다.

이를 버튼을 만드는 예제를 통해서 작성해보면,

```html
<!-- 1: 속성으로 정의 -->
<a custom-button>Click me</a>

<!-- 2: 요소로 정의 -->
<custom-button>Click me</custom-button>

<!-- 3: 클래스로 정의(IE 구버전 호환을 위해) -->
<a class="custom-button">Click me</a>

<!-- 4: 주석으로 정의 (데모로는 별로 안좋긴 하다) -->
<!-- directive: custom-button -->
```

와같이 버튼을 생성하는 것을 작성한다. 이를 디렉티브를 이용해 적용하기 위한 디렉티브를 먼저 선언하면,

```js
myApp.directive('customButton',function(){
    return{
        link: function(scope,element,attrs){

        }
    };
});
```

와 같이 작성할 수 있다.

위와같이 작성해서 실행했을 대는 단순히 글씨만 나타나게 된다. 하지만 이를 directive내에서 값의 변화를 주면

```js
myApp.directive('customButton', function(){
    return {
        restrict : 'A',
        replace : true,
        transclude : true,
        template : '<a href="" class="myawesomebutton" ng-transclude>' +
                        '<i class="icon-ok-sign"' + '</a>',
        link: function(scope, element, attrs){

        }
    };
});
```

이를 통해서 Click me를 버튼화 시켜서 클릭할 수 있는 형태로 만들 수 있다. 여기서 각각 directve의 속성의 역할은

- restrict : 어떻게 요소의 사용을 제한할지 다시 생각하는 용도. 'A'는 속성으로만 사용, 'E'는 요소, 'C'는 클래스, 'M'은 주속으로만 사용할 수 있다. 기본 값은 'EA' 이고, 제한은 여러개 걸 수 있다.

- replace : 디렉티브에 정의한 DOM의 마크업을 변경할 수 있음을 의미한다. 

- transclude : 간단하게 말해서 집어넣는 것으로 이를 이용하면 기존의 DOM 내용을 디렉티브 안에 복사할 수 있다. 'Click me'라는 문자열이 렌더링될 때 디렉티브로 옮겨지는 것을 확인 할 수 있다.

- template : 템플릿은 주입할 마크업을 말하며, HTML의 아주 작은 일부분 정의할 때 특히 유용하다.

- templateUrl : template 속성과 비슷하지만 \<script\> 태그 혹은 파일을 지정할 때 사용한다. 이를 통해서 해당 내용을 가져올 수 있다.

### 서비스

서비스는 기능적인 큰차이를 제공하지는 않는다. 기본적으로 싱글톤으로 사용하지만, 객체 리터럴이나 좀 더 복잡한 유즈케이스처럼 더 복잡한 기능은 팩토리를 사용한다.

예제로는 2개의 곱의 서비스이다.

```js
myApp.service('Math', function(){
    this.multiply = function(x,y){
        return x*y;
    };
});

myApp.controller('MainCtrl', ['$scope', function($scope) {
    var a = 12;
    var b = 24;

    var result = Math.multiply(a,b);
}]);
```

를 이용한다. 이를 이용할 때, 서비스의 존재를 모를 수 있기 때문에 의존성을 주입해 주는 방법을 이용한다.

```js
myApp.controller('MainCtrl',['$scope', 'Math', function($scope, Math){
    ...
}]);
```

위와 같이 사용한다.

### 팩토리

팩토리로 서비스를 만드는 건 객체 리터럴을 팩토리안에서 생성하거나 몇가지 메서드를 추가하면 된다.

```js
myApp.factory('Server', ['$http',function($http){
    return {
        get: function(url){
            return $http.get(url);
        },
        post: function(url){
            return $http.post(url);
        },
    };
}]);
```

위와 같이 XHR을 래핑한 코드를 만들어 준다. 작성한 뒤에 컨트롤러에 해당 요소를 집어넣는다.

```js
myApp.controller('MainCtrl', ['$scope', 'Server', function ($scope, Server){
    var jsonGet = 'http://myserver/getURL';
    var jsonPost = 'http://myserver/postURL';
    Server.get(jsonGet);
    Server.post(jsonPost);
}]);
```

### 필터

배열의 데이터와 함께 사용하며 루프 밖에서도 사용이 가능하다. 데이터를 순회하며 조건에 만족하는 데이터를 추리고 싶을 때 필터를 사용한다.

```js
myApp.filter('reverse', function(){
    return function (input, uppercase) {
        var out = '';
        for(var i = 0 ; i< input.length ; i++){
            out = input.charAt(i) + out;
        }
        if(uppercase) {
            out = out.toUpperCase();
        }
        return out;
    }
});

myApp.controller('MainCtrl',['$scope', function($scope){
    $scope.greeting = 'Todd Motto';
}]);

```

```html
<div ng-app="myApp">
    <div ng-controller="MainCtrl">
        <p>No filter: {{ greeting }}</p>
        <p>Reverse: {{ greeting | reverse }}</p>
    </div>
</div>
```

위와 같이 사용해서 filter를 통해서 반복을 진행해 줄 수 있다.

또한, 저런 필터의 경우는 controller안에서 필터를 선언해서 사용할 수 있다.

### 양방향 데이터 바인딩

이를 간단히 말하면 동기화된 데이터 라고 말하면 될 것 같다. 모델을 갱신하면 뷰에 반영되고 뷰를 갱신하면 모델에 반영되는 형태를 말한다.

이는 별다른 작업 없이도 데이터가 동기화 된다는 뜻이다.

이는 input에 `ng-model`을 바인딩하고 값을 입력하면 동시에 모델이 생성되며 갱신되는 것을 보인다.

```html
<div ng-app="myApp">
    <div ng-controller="MainCtrl">
        <input type="text" ng-model="myModel" placeholder="Start typing..." />
        <p>My model data: {{ myModel }}</p>
    </div>
</div>
```

```js
myApp.controller('MainCtrl', ['$scope', function ($scope) {
    // 빈 문자열로 초기화하고 모델 데이터를 읽어온다.
    $scope.myModel = '';
}]);
```

### XHR/Ajax/$http 호출과 JSON바인딩

$http를 통해 서버 데이터에 접근하는 기능을 쉽게 사용할 수 있다. `GET`요청을 보내고 서버에서 데이터를받아오는 예제이다.

```js
myApp.controller('MainCtrl', ['$scope', '$http', function($scope, $http){
    $http({
        method: 'GET',
        url: '//localhost:900/someUrl'
    });
}]);
```

이를 진행하면 콜백을 promise라는 걸 반환하고,  Promise는 성공했을 때와 실패했을 때의 핸들러를 제공한다.

```js
myApp.controller('MainCtrl', ['$scope', '$http', function($scope, $http){
    $http({
        method: 'GET',
        url: '//localhost:900/someUrl'
    })
    .success(function (data, status, headers, config){
        //성공 사례
    })
    .error(function (data, status, headers, config){
        //오류 사례
    });
}]);
```

이를 서버 객체에서 json 형태로 반환하고 이를 저장 시키는 형태로 진행한다면

```json
{
    "user"{
        "name" : "jhpark",
        "id" : "123123123"
    }
}
```

```js
myApp.controller('MainCtrl', ['$scope', '$http', function($scope, $http){
    
    $scope.user ={};

    $scope.user.name = '';
    
    $http({
        method: 'GET',
        url: '//localhost:900/someUrl'
    })
    .success(function (data, status, headers, config){
        //성공 사례
        $scope.user.username = data.user.name;
    })
    .error(function (data, status, headers, config){
        //오류 사례
    });
}]);
```

위와 같은 사례를 통해서 `$scope`내에 있는 username을 데이터로 받아온 name으로 교체가 가능하다.

### 선천적 데이터 바인딩

동적 HTML을 생성해 웹클라이언트에서 하지 못했던 일을 하려는 것이 Angular의 목적이다.

Ajax 요청을 가져와 DOM에 그리는 기능을 구현하는 것을 만든다는 가정해 보자.

```js
myApp.controller('EmailsCtrl',['$scope', function($scope){

    $scope.emails = {};

    $scope.emails.message = [{
        "from": "Steve Jobs",
        "subject": "I think I'm holding my phone wrong :/",
        "sent": "2013-10-01T08:05:59Z"
    },{
        "from": "Ellie Goulding",
        "subject": "I've got Starry Eyes, lulz",
        "sent": "2013-09-21T19:45:00Z"
    },{
        "from": "Michael Stipe",
        "subject": "Everybody hurts, sometimes.",
        "sent": "2013-09-12T11:38:30Z"
    },{
        "from": "Jeremy Clarkson",
        "subject": "Think I've found the best car... In the world",
        "sent": "2013-09-03T13:15:11Z"
    }];
}]);
```

이를 html에서 작성할 필요없이 불러오면 된다.

```html
<ul>
    <li ng-repeat="message in emails.messages">
        <p>From: {{ message.from }}</p>
        <p>Subject: {{ message.subject }}</p>
        <p>{{ message.sent | date:'MMM d, y h:mm:ss a'}}</p>
    </li>
</ul>
```

여기서 `ng-repeat` 을 통해서 리스트의 반복을 진행 했다는 점이 주요하고, ng-* 디렉티브에 대한 추가 공부가 필요하다.

### Scope 함수

scope 함수를 이용하면 미리 함수 내용을 선언 해놓고 가져다 쓸 수 있다.

```js
myApp.controller('MainCtrl', ['$scope', function($scope){
    $scope.deleteEmail = function (index){
        $scope.emails.messages.splice(index, 1)
    };
}]);
```

```html
<a ng-click="deleteEmail($index)">Delete email</a>
```

위와 같이 사용하면 클릭한 이메일이 삭제된다. 그리고 추가적으로 `splice(index, count)`의 경우는 js에서 문서를 대체할 때 사용되는데 이는 삭제처럼 사용이 가능하다. 

### 선언적 DOM 메서드

스크립트 로직으로 작성해 DOM기능을 제공하는 형테로 예시로 간단한 토글 네비게이션을 사용한다.

```html
<a href="" ng-click="toggle =! toggle">Toggle nav</a>
    <ul ng-show="toggle">
        <li>Link 1</li>
        <li>Link 2</li>
        <li>Link 3</li>
</ul>
```

이를 사용하면 클릭을 통해서 해당 내용이 생성되고 안되고를 볼 수 있다.

### 표현식

자바스크립트의 반복문이나 조건문을 대체하여 사용하는 부분이 있다.

기존에

```js
elem.onclick = function(data){
    if(data.length === 0){
        otherElem.innerHTML = 'No data';
    } else{
        otherElem.innerHTML = 'My data';
    }
};
```

라고 작성했던 js코드를 단순히 html내에서 작성해 사용할 수 있다.

```html
<p>{{ data.length > 0 && 'My data' || 'No data' }}</p>
```

위와 같이 작성하면 동적으로 데이터를 갱신한다.

### 동적 뷰와 라우팅

단일 페이지 웹 에플리케이션은 각각으 ㄹ나눠서 내용이 표시되는게 보통이다. 이를 Angular를 통해서 동적뷰를 사용하면 쉽게 설정 할 수 있다. URL기준으로 `$routeProvide`를 통해 특정 뷰를 얻어와 적용하면된다.

```js
myApp.config(['$routeProvider', function($routeProvider){
    $routeProvider
    .when('/', {
        templateUrl: 'views/main.html'
    })
    .when('/emails', {
        templateUrl: 'views/emails.html
    })
    .otherwise({
        redirectTo: '/'
    });
}]);
```

URL이 '/'이면 'main.html'이 주입이 되는 것을 볼수 있다. 이를 다른 값들 도 쉽게 추가할 수 있다. 위와 같이 'emails'를 연결해주는 방버도 있다.

### 전역 static 데이터

페이지에 데이터를 즉시 반영하려면 Angular를 사용하면 렌더링 속도도 빠르게 할 수 있다. 자바 태그를 DOM 안에 넣고 렌더링될 때 서버로부터 데이터를 받는다. 페이지에 Json을 작성해서 컨트롤러에 넣고 즉시 바인딩 하는 방법이다.

```html
<script>
window.globalData = {};
globalData.emails = <javaTagHereToGenrateMessage>;
</script>
```

이를 Angular에서 처리해준다.

```js
myApp.controller('EmailsCtrl', ['$scope', function($scope){
    $scope.emails = {};

    $scope.emails.messages = globalData.emails;

}]);
```

### 압축

코드를 압축하는데에 있어서 앞의 배열에 주입해야하는 의존 관계를 잘 정의해야 한다.

```js
myApp.controller('MainCtrl',
['$scope', 'Dependency', 'Service', 'Factory', 
  // a = $scope
  // b = Dependency
  // c = Service
  // d = Factory

  // $scope 별칭이 사용됨
  a.someFunction = function () {...};

}]);
```

위와 같이 사용이 되므로 순서에 대해서 확실하게 알고 처리해야 한다.

### MVC와 MVVM의 차이점

- MVC : 컨트롤러와 통신, 모델 - 뷰 - 컨트롤러
- MVVM : 기술적으로는 자기자신과 통신하는 선언적 데이터 바인딩. 모델 - 뷰 - 뷰 - 모델, 모델은 뷰와 통신하고, 뷰는 모델과 통신한다.

예를 들어, 다음 데이터를 제공하는 컨트롤러없이도 ng-repeat을 생성하는 예제이다.

```js
<li ng-repeat="number in [1,2,3,4,5,6,7,8,9]">
    {{ number }}
</li>
```

- - -

## 다시 처음부터 개념 돌아보기 (<https://code.angularjs.org/1.7.8/docs/guide/introduction>)

Angular 1.7.8 버전을 사용하는 것을 확인해서 해당 문서를 확인해서 진행한다.

### What is Angular

다이내믹 웹앱을 만드는 프레임워크이며, html을 확장해서 어플리케이션의 컴포넌트를 저 잘 사용하도록 돕는다.

HTML은 정적 언어로 굉장히 명확하다. 이런 상황에서 동적 코드와 정적 코드간의 불일치가 일어날 수 있는데, 이libarary(jQuery), frameowrk(durandal) 와 같은 방식으로 주로 해결 했다.

이를 angular는 다른 방식으로 접근을 하였다. 이를 browser에게 새로운 syntax인 driectives를 통해서 학습시킨다. 예를들어

- Data binding, {{}}
- DOM 형태를 반복하는 것을 제어
- form들을 제공하고 검증까지 제공

### Data Binding

기존의 데이터 바인딩의 경우에는 단방향 바인딩이 이루어졌다. 템플릿과 모델이 merge가 한번 되면 이를 뷰에 전달하는 방식이었다. 이 부분에서 한번 만들어진 것을 변환 시키려면 다시 합치는것을 진행해야 했다.

이를 AngularJs에서는 처음에 브라우저에서 템플릿을 컴파일 해준다. 이 부분에서 변환 된 것이 모델에 바로 적용이 된다. 그리고 모델에서 변환된 부분이 바로 뷰에 다시 전달이 된다. 이를 통해서 바로바로 컴파일된 템플릿 외에는 뷰와 모델이 적용이 바로바로 되게 된다.

이것이 가능한 것은 뷰가 단순히 모델의 투사된 모습이고 컨트롤러는 뷰와 완전히 분리되어 있기 때문이다.

### Understanding Controllers

Angular에서는 컨트롤러를 JavaScript에서 constructor function으로 정의되어 있다. 이는 Scope로 사용할 수 있다. Angular가 Controller를 인스턴스화 시키고 특별한 constructor function을 이용한다.

- ngController directive를 이용해서 새로운 자식 scope를 만들고 해당 Controller에 속한 값을 `$scope`를 사용한다.
- route controller같은 경우는 `$route definition`에 존재한다.
- refular directive나 component directive도 사용된다.

Controller는 scope 오브젝트를 처음 셋업 할때 쓰거나 scope 오브젝트의 행동에 대해 적용할 때 사용한다.

#### Setting up the initial state of a $scope object

scope를 만들어 줄 때 속성값(property)를 지정해서 사용하면 된다.

```js
var myApp = angular.module('myApp',[]);

myApp.controller('GreetingController', ['$scope', function($scope) {
  $scope.greeting = 'Hola!';
}]);
```

의 경우처럼 module을 만들어주고 `GreetingController`라는 컨트롤러를 적용하고, 그 안에 `scope.greeting`이라는 값을 적용해 준다. 그러면 html에서

```html
<div ng-controller="GreetingController">
  {{ greeting }}
</div>
```

을 적용하면 `ng-controller`로 지정한 `GreetingController`값에서 `greeting` 속성 값을 불러온다.

#### Adding Behavior to a Scope Object

앞서 속성값만 만드는 것이 아니라, function을 통해 함수를 사용할 수도 있다.

```js
var myApp = angular.module('myApp', []);

myApp.controller('DoubleController', ['$scope', functions($scope) {
    $scope.double = function(value) { retrun value *2};
}]);
```

이렇게 scope에 function을 만들어주면 이를 그대로 가져와서 사용이 가능하다.

```html
<div ng-controller="DoubleController">
    Two times <input ng-model="num"> equlas {{ double(num) }}
</div>
```

이런 식으로 controller를 다른 컨트롤러로 설정하고 해당 값을 이용해서 사용하면 된다.

#### Simple Spicy Controller Example

각각의 버튼을 만들어 주고, scope값을 이용해 내용을 바꾸도록 지정하는 예시를 진행해 본다.

```js
var myApp = angular.module('spicyApp1',[]);

myApp.controller('SpicyController', ['$scope', function($scope){
    $scope.spice = 'very';

    $scope.chiliSpicy = function(){
        $scope.spice = 'chili';
    };

    $scope.jalapenoSicy = function(){
        $scope.spice = 'jalapeno';
    };
}]);
```

```html
<div ng-controller="SpicyController">
    <button ng-click="chilySpicy()">Chili</button>
    <button ng-click="jalapenoSpicy()">Jalpeno</button>
    <p>The food is {{spice}} spicy!</p>
</div>
```

위의 두가지를 보면, 'SpicyController'라는 컨트롤러에 scope의 속성값으로 spice를 주고 해당 값을 변환시킬 수 있는 함수들을 두개 만들어 놓았다. 이를 활용하기 위해서 버튼으로 해당 값을 작동시킬 수 있게 연결을 해놓았다. 버튼을 만드는 것 역시 `ng-click`이라고 만들어 해당 값이 Angular에서 나온 것을 알 수 있다.

#### Spicy Argument Example

앞서 진행한 사항을 입력을 받아서 처리하는 것으로 바꾸어 보면

```js
var myApp = angular.module('spicyApp2', []);

myApp.controller('SpicyController', ['$scope', function($scope) {
    $scope.customSpice = 'wasabi';
    $scope.spice = 'very';

    $scope.spicy = function(spice) {
        $scope.spice = spice;
    };
}]);
```

```html
<div ng-controller="SpicyController">
 <input ng-model="customSpice">
 <button ng-click="spicy('chili')">Chili</button>
 <button ng-click="spicy(customSpice)">Custom spice</button>
 <p>The food is {{spice}} spicy!</p>
</div>
```

앞의 내용에서 변동 시킨 사항은 spicy라는 값에 미리 값을 집어 넣고 시작을 한다는 부분이다. spicy라는 fucntion에다가 미리 값을 넣고 시작하는 방식이다. 
그리고 `ng-model`을 이용해서 값을 전달 받을 수 있는 부분을 만들어 놓는다. 이 값을 `customSpicy`와 연결 시키고, 두 번째 버튼을 누르면 이 `customSpicy`값을 전달하게 바꾼다.

#### Scope Inheritance

스코프의 상속관계에 대해서 알아보면, 해당 scope는 controller를 div를 기준으로 나눠준다. 더 안에 있는 값일 수록 우선 순위가 높아지게 된다. 
따라서 같은 이름의 scope라 하더라도 상속이 되어 가장 내부에서 사용하는 변수를 이용한다.

#### Testing Controllers

컨트롤러를 테스트 하는 것은 `$rootScope`와 `$controller`를 활용한다.

이는 기존에 있는 describe 함수를 이용하면 사용이 가능하다.

- - -

### Services

Angular의 서비스는 의존성 주입을 대체할 수 있다.

- 어플리케이션 컴포넌트가 해당 서비스에 의존할 때 인스턴스화 시킨다.
- 싱클톤을 이용한다.

#### Using a Service

Angular의 서비스를 사용하기 위해서 컴포넌트들의 의존성으로 추가해야한다.

#### Creating Services

service factory function을 만들 때 자유롭게 이름을 지정할 수 있다. service factory function을 만들 때 하나의 오브젝트나 함수를 만들어 줄 수 있다.

##### Registering Services

서비스는 Module API에서 온 값에 레지스터를 시킨다. 또한 Module factory API를 사용해 서비스를 등록하는 것이다.

단순히 js를 작성한다고 해서 sevice instance를 레지스터 시키는 것이아니고 이 factory function이 불려질 때 instance를 만들어준다.

#### Dependecies

서비스에 자체 종속성이 있고, 컨트롤러에서 종속성을 선언하는 것처럼 팩토리 기능 서명에서도 종속성을 지정할 수 있다.

#### $provice와 함께 레지스터링

configue 함수에 $provide를 적용해 사용할 수 있다.

- - -

### What are Scopes

#### Scope charateristicsc

scope는 `$watch`의 API를 제공해 모듈의 mutation을 감시할 수 있고, `$apply`를 이용해 모델 변경 사항을 외부에서 뷰로 전파할 수 있다.

scope는 child scope와 isolate scope가 있어서 child scope는 상속관계를 가질 수 있고, isolate scope는 아니다.

또한, 표현식이 평가가 되는 컨텍스트를 제공한다. 예를들어 `username` 이라는 표현식이 의미가 없더라도 username이라는 속성이 정의되었는지 판단한다.

#### Scope as Data-Model

controller와 directives가 스코프를 갖고 있지만 서로에게 존재하지 않는다. 이를 통해서 controller에서 directive를 분리 시킨다. dom 에서 작동할 때 이 둘을 따로 판단 할 수 있다는 것은
굉장히 중요한 요소이다. 

만약 하나의 메서드 안에서 프로퍼티를 선언했다면, 해당 메서드를 한번 사용을해야 해당 내용의 속성이 생성이된다.

#### Scope Hierarchies

각각의 Angular 어플리케이션은 하나의 root scope를 갖지만 많은 child scope를 갖는다. 이는 directives가 새로운 scope를 만들어 자식 scope화 시킬 수 있다. 

이름이 동일하더라도 각각의 컨트롤러의 종속성에 따라서 다른 값들이 출력이 된다.

#### Retrieving Scopes from the DOM

디버깅 과정에서 scope 데이터를 찾을 수 있다. root scope가 위치한 곳은 `ng-app`을 통해서 확인할 수 있다. 보통 html 에 위치해 있지만 다른 곳에도 위치할 수 있다. 
이 sopce의 위치를 찾는 방법은 다음과 같다.

1. 원하는 값에 우클릭을 하여 `Inspect element`를 선택한다. 그러면 브라우저 디버거가 선택된 곳이 하이라이팅 된다.
2. 디버거가 최근에 선택한 `$0`값에 접근을 허용한다.
3. 이를 통해 연관된 스코프를 찾는다 `angular.element($0).scope()

#### Scope Events Propagation

emit의 경우는 해당 위치의 상위로 위치한 값들에 event에 대한 값을 전달하며, broadcast같은 경우는 자식값들에 대해서 값을 변화시킬 수 있다.

#### Scope Life Cycle

일반적으로는 브라우저가 이벤트를 받게 되면 자바스크립트에서 콜백이 일어난다.

이때 자바스크립트에서 앵귤러에 대해서 작동을 시키는 것이 아니라 apply 메소드를 실행하면 그때서야 이제 모델이 만들어진다.
예를 들어, ng-click 같은 것을 실행하면, 이 표현식은 apply 메소드 내에서 실행된다.

그 이후에 apply 메소드는 digest를 실행시킨다. digest는 watch의 모든 표현식을 그 전 값과 비교하고 작동시킨다.

이런 체크는 비동기적으로 일어난다. angular가 어떤 username을 바꿨을 때 watch가 바로 변화를 감지하지 않고, watch가 딜레이를 시키고, digest가 나타날 때 발생한다.

즉 순서대로 보면

1. Creation
    - root scope가 `injector`를 이용해 bootstrap을 만든다.
2. Watcher registration
    - 잠시 연결을 했을 때 directive 가 watch들을 스코프에 레지스터링 시킨다. 
3. Model mutation
    - 모델 값에대해 mutation을 적용하기 위해서는 scope.$apply를 이용해서 적용한다.
4. Mutation observation
    - apply의 마지막에 digest를 적용시켜서 모든 child 스코프에 적용시킨다. digest를 도는 동안은 watch의 값들은 모델과의 비교를 하고 바뀌게 된게 있으면 watch listner를 요청한다.
5. Scope destruction
    - child 스코프가 더이상 필요하지 않을때 scope.$destroy() 를 이용해 없앤다.

##### Scopes and Directives

컴파일 과정에서 directive는 둘 중의 한가지 경우에 적용된다.

- 중괄호에 expression이 작성된 directive가 감지되면 watch 메서드를 사용하는 리스너를 등록한다. 이 경우 언제든 expression이 바뀌는 것에 대해 확인할 수 있어야 한다.
- 리스너 directive들의 경우 DOM과 함께 리스너를 등록한다. 그래서 DOM에서 리스너들이 작동하면 directive들도 뷰에 apply메소드를 이용해 적용시켜준다.

##### Directives that Create Scopes

대부분 directive와 스코프 는 서로 상호작용하지만 새로운 스코프 인스턴스를 만들지는 않는다. 하지만, 몇몇 directive들은 자식 스코프를 생성하고 DOM에서 작동할 수 있도록한다.

`isolate` 스코프같은 경우는 상속되지 않고 하나로써 작동한다. 이는 부모 스코프와 구분되고 싶은 directive에서 유용하게 사용가능하다.

##### Controllers and Scope

- 컨트롤러가 스코프를 이용해 컨트롤러 메서드를 템플릿에 전달
- 컨트롤러가 메서드를 정의해서 모델을 변화
- 컨트롤러가 watch들을 모델에 적용, watch가 controller의 사용후 전달

##### Scope `$watch` performance consideration

스코프가 변하는 것에 대해 체크 하는 것은 흔한 작업이다 그리고 이러한 작업이 효율적일 필요성이 있다. 이런 작업은 DOM에서 작동하지 않게해서 느리게 만들지 말아야한다.

##### Scope `$watch` Depth

체크 과정은 3가지 전략에 의해 체크된다. By Reference, By Collection contents, By Value에 의해 적용된다.

- by Reference의 경우 모든 값이 return 될 때 새로운 값으로 바꾸어 준다. 만약 그 값이 오브젝트나 배열이라면 아직 발견되지 않은 내부 값을 바꾸어 준다.
- by Collection contents의 경우 array나 object의 내부에서 변화를 감지한다.(item이 추가되거나 삭제되거나 재정렬 될 때) reference를 보는 것보다 비싼데 이 이유는 컬렉션 컨텐츠를 복사하고 저장하는 것이 필요하기 때문이다. 그렇기 때문에 카피를 줄이는 것이 주요하다.
- by Value의 경우 모든 값들의 변화를 확인한다. 가장 강력한 변화 측정 방법이지만 가장 비싸다. 모든 값들을 봐야하므로 모든 값에 대한 복사가 필요하다.

#### Integration with the browser event loop

브라우저에서 어떻게 AngularJs와 반응하는지에 대한 이벤트 루프는

1. 브라우저가 이벤트를 기다린다.
2. 이벤트가 발생하면 JS 문법에 들어가고 callback이 DOM을 변형시킨다.
3. callback이 발생하면 브라우저가 JS문법을 빠져나오고, DOM의 변화에 맞춰 다시 렌더링한다.

- - -

### Dependecy Injection

Dependency Injection이라는 의존성 주입이란, 개발을 하다보면 생기는 의존성들 때문에 나타난 문제 때문에 발생되었다.

그래서 DI를 통해서 개발을 하면

- Unit Test 용이
- 코드간 재활용성 높아짐
- 객체 간의 이존성을 줄이거나 없앰
- 객체 간 결합도를 낮추며 유연한 코드 생성

이라는 장점을 갖게 된다.

#### Using Dependecy Injection

run이나 config를 통해서 사용할 수 있다.

- Service, Directive, filter, animation들이 주입가능한 팩토리 매서드나 생성자 메서드로 정의 되어있으며, 의존성 주입이 가능하다.
- Controller의 경우는 생성자 함수로 정의되어 있고, 어떤 service, value에도 주입이 가능하다.
- `run` 메서드는 service, value, constans가 주입된 함수를 허용한다. "providers"는 `run` blocks에 주입될 수 없다.
- `config` 메서드는 provicer, constant들이 주입되어 있는 함수를 허용한다.
- `provider` 메서드는 또 다른 `provider`를 주입받을 수 있다. 

##### Factory Methods

이를 정의할 때는

```js
angular.module('myModule', [])
.factory('serviceId', ['depService', function(depService) {
  // ...
}])
.directive('directiveName', ['depService', function(depService) {
  // ...
}])
.filter('filterName', ['depService', function(depService) {
  // ...
}]);
```

와 같이하는 것이 좋다.

##### Module Methods

config와 run을 맞추기 위해서 설정한다.

```js
angular.module('myModule', [])
.config(['depProvider', function(depProvider) {
  // ...
}])
.run(['depService', function(depService) {
  // ...
}]);
```

- - -

##### Controllers

클래스와 생성자 함수를 템플릿에서 제공하는 어플리케이션 값들을 제공한다. 

```js
someModule.controller('MyController', ['$scope', 'dep1', 'dep2', function($scope, dep1, dep2) {
  ...
  $scope.aMethod = function() {
    ...
  }
  ...
}]);
```

서비스와는 달리 여러개의 같은 타입의 컨트롤러가 있을 수 있다.

#### Dependecy Annotation

어노테이션을 이용해 어떤 서비스가 inject되었는지 확인한다. 여기에는 3가지 방법이 있다.

- inline array 어노테이션(추천)
- $inject를 사용
- 함수 매개변수 이름에서 사용

##### Inline Array Annotation

```js
someModule.controller('MyController', ['$scope', 'greeter', function($scope, greeter){

}]);
```

array 형태로 어노테이션을 계속해서 사용한다.

##### `$inject` Property Annotation

선언한 값에 `$inject`를 사용해 직접 주입한다.

```js
var MyController = function($scope, greeter) {
  // ...
}
MyController.$inject = ['$scope', 'greeter'];
someModule.controller('MyController', MyController);
```

##### Implicit Annotation

함수의 파라미터는 dependency의 이름과 같다고 가정하고 사용

```js
someModule.controller('MyController', function($scope, greeter) {
  // ...
});
```

#### Using Strict Dependency Injection

`ng-strict-di` 디렉티브를 `ng-app`이 가진 같은 요소에 적용할 수 있다.

이를 통해서 서비스가 주석을 사용하려 할 때마다 strict 모드에서 오류가발생한다.

만약 수동적으로 bootstrapping을 하려면 `strictDi: true`를 사용하면 된다.

#### Why Dependency Injection

- - -

### Expression

- 주로 중괄호 안에 묶어서 사용

#### `$event`

$event 를 통해서 각 jquery에 해당하는 이벤트 발생에 대해서 사용가능하다.

#### One-time binding

`::`를 이용하면 해당 값이 다시 바뀌지 않게 된다.

- - -

### Filter

filter에는 다양한 기능 들이 있다. 이미 template에 존재하는 필터들로 통화나, 문자를 포함한 단어 찾기 등이 있다.

이러한 filter는 직접 만들어서 적용할 수도 있다.

- - -

### Directives

DOM의 marker이다. 이를 이용해 Angular의 HTML 컴파일러로 전달해 준다. DOM 요소들이 특정행동을 하거나 변경시키거나 할 수 있게 하는 것들이다.

AngularJS는 built-in directive의 모음이다. 컨트롤이나 서비스를 만드는 것처럼 directive도 만들 수 있다. 

보통 만드는 directive들은 dash `-` 를 이용하는 것이 정석 적이다. 

#### Directive types

`$compile`은 element name, attriubtes, class name, commnet를 기준으로 매칭시킨다. 

#### Template-expanding directive

해당 부분은 directive를 선언하고 이를 리턴하는 값을 template로 컨트롤러가 가지고있는 파라미터를 이용해 출력하는 방식이다.

이 것 외에도 templateUrl을 이용해서 html파일에다가 앞서 template로 설정했던 부분에 대해서 리턴 값을 html에 저장하고 그 html을 불러오도록한다.

이를 또 다시 templateUrl에 function에 두개의 파라미터를 넣을 수 있다. elem과 attr을 받고 attr 오브젝트는 엘리멘트와 관련되어 있다.

그리고 여기서 `restrict` 옵션을 지정할 수 있다.

- `'A'` attribute name
- `'E'` element name
- `'C'` class name
- `'E'` comment

를 이용해 지정할 수 있으며 이는 조합해서 사용할 수도 있다.

#### Isolating the Scope of a Directive

기본적으로 컨트롤러 자체를 두개로 나누어서 사용하는 경우도 있지만, 

이 방식이 아니라, isolate scope를 이용하는 방식을 택한다. 하나의 컨트롤러 안에 스코프를 선언해서 각각 사용한다.

#### Creating a Directive that Manipulates the DOM

directive를 통해 DOM을 조절할 때 link 옵션을 이용한다.
link는 다음과 같은 값들이 있다

- scope : Angular JS의 스코프
- element : 이 directive와 일치하는 jqLite-wrapped element
- attrs : key-value 페어인 hash object
- controller : directive가 필요로 하는 컨트롤러 인스턴스나 자기 자신의 컨트롤러를 의미한다. 정확한 value는 directive의 require 속성에 따라 다르다.
- transcludeFn : 사전 바인딩 된 연결 기능을 포함한 올바른 연결 범위

#### Creating a Directive that Wraps Other Elements

전체 템플릿 전체를 쓰는것이 아닌 문자열이나 오브젝트로 작성하는 경우도 존재한다.

이 때는, `transclude`를 이용해서 작성하면된다. 이 값은 directive값이 가지고 있으면 바깥에서 scope를 가져올 수 있다는 의미를 갖는다.

이때 link안에서 scope를 변경하는 경우에 발생하는 현상은 directive가 바깥에 존재하는 값을 먼저 확인한다는 것이다.

또한, directive가 자신의 scope를 만들지 않는다면 directive 내부에서 지정한 값을 외부에서 참조할 수 있다.

#### Creating a Directive that Adds Event Listeners

event에 대한 반응을 하는 것을 설정하는 것도 link를 통해서 가능하다.

#### Creating Directives that Communicate

템플릿을 이용해서 어떤 디렉티브도 작성할 수 있다. 이를 이용해서 여러개의 값들을 연결해서 다른 html내에 연결시켜줘서 사용도 가능하다.

- - -

### Understanding Components(<https://poiemaweb.com/angular-component-basics>)

Component는 directive의 하나의 종류고, component를 base로하는 app에 잘 맞는다.

Component의 역할은 애플리케이션의 화면을 구성하는 뷰(View)를 생성하고 관리하는 것이다.

#### 웹 컴포넌트

Web Component를 사용하며, 캡슐화된 HTML 커스텀 요소를 생성하는 웹 플랫폼 API의 집합이다. 제공해야 하는 기능은 다음과 같다.

1. 컴포넌트 뷰를 생성(HTML Template)
2. 외부로부터의 간섭을 제어하기 위해 스코프(Scope) 분리해 DOM을 캡슐화(Shaodw DOM)
3. 외부에서 컴포넌트를 호출(HTML import)
4. 컴포넌트를 명시적으로 호출하기 위한 명칭(alias) 선언해 이를 네이티브 HTML처럼 사용(Custom Element)

#### 컴포넌트 트리

대부분의 웹 애플리케이션은 불록구조를 가지면서

- header, section, article, aside, footer

로 구성되어 있다. 대부분 header, aside, footer는 모든 화면이 공통으로 사용되며 section만 바뀌는 경우가 많다.

이부분을 앵귤러에서는 각각을 다르게 지정해서 전달하려는 것으로 보인다.

- - -

### Module

모듈은 controller, service, filter, directive를 저장하는 컨테이너라고 생각하면 된다.

모듈은 각각의 피쳐에 대한 모듈, 다시 사용할 수 있는 component를 이용한 모듈, 그리고 상위 모듈에 의존하고, 초기화시키는 코드를 갖고 있는 어플리케이션 레벨의 모듈로 만들어준다.

- - -

## 이해안된 내용 재정리

### 디렉티브란?

HTML Compiler에 의해 해석된 특정한 행위의 기능을 가진 DOM 엘리먼트

앵귤러의 HTML Compiler 절차는 2단계로 축약된다

- compile 단계 : HTML의 DOM 엘리먼트를 돌며 디렉티브를 찾고, link function을 리턴한다.
- link 단계 : 디렉티브와 HTML이 상호작용 할 수 있도록 디렉티브에 event listner를 등록 scope와 DOM간의 2-way data binding을 위한 `$watch`를 설정한다.

ngBind, ngModel 같은 경우는 built-in 디렉티브이다.

### 사용자 정의 디렉티브

```js
var myModule = angular.module(...);  
myModule.directive('directiveName', function (injectables) {  
  return {
    restrict: 'A',
    template: '<div></div>',
    templateUrl: 'directive.html',
    replace: false,
    priority: 0,
    transclude: false,
    scope: false,
    terminal: false,
    require: false,
    controller: function($scope, $element, $attrs, $transclude, otherInjectables) { ... },
    compile: function compile(tElement, tAttrs, transclude) {
      return {
        pre: function preLink(scope, iElement, iAttrs, controller) { ... },
        post: function postLink(scope, iElement, iAttrs, controller) { ... }
      }
    },
    link: function postLink(scope, iElement, iAttrs) { ... }
  };
});
```

#### restrict

- 디렉티브를 사용하기 위한 DOM 엘리먼트의 속성을 설정
    1. E - element
    2. A - attribute
    3. C - class
    4. M - comment

#### template

In-Line value를 설정

#### templateUrl

template을 별도의 html로 관리하고, 이 위치에서 url에 해당하는 HTML을 로드

#### replace

디렉티브를 사용한 HTML 태그에 template 또는 templateUrl에 포함된 태그 내용을 추가할지 교체할지 설정

#### priority

디렉티브 별로 compile()과 link()의 호출 우선 순위 지정

#### transclude

template 또는 templateUrl에서 디렉티브내의 원본내용을 포함시킬지 설정

#### scope

- scope : false  -> 새로운 객체를 생성하지 않고, 부모가 가진 같은 scope 객체를 공유
- scope : true -> 새로운 객체를 생성하고 부모 scope 객체를 상속
- scope : {...} -> isolate/isolated scope를 새롭게 생성
- = : 부모 scope의 property와 디렉티브의 property를 바인딩하여 부모 scope에 접근
- @ : 디렉티브의 attribute value를 {{}} 방식을 이용해 부모 scope에 접근

#### controller()

다른 디렉티브와 통신하기 위한 역할을 하는 controller명칭을 정의

#### require

Angular의 다른 컨트롤러나 디렉티브의 controller()에 this로 정의된 function을 사용할 때 선언

#### compile

DOM 엘리먼트를 해석해 디렉티브로 변환해 link function을 리턴

- preLink(): child 엘리먼트가 링크되기 전에 호출
- postLink() : child 엘리먼트가 링크된 후 호출

#### link

2-way data binding을 위해 해당 디렉티브 DOM엘리먼트의 event listenr를 등록

### compile과 link 분리 목적

compile()은 단 한번 실행되서 template를 변환하는 용도로 사용

link()는 디렉티브 인스턴스 별로 여러번 호출되기 때문에 compile()과 link()를 분리
