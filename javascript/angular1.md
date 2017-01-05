## angular 1 관련 삽질 노트입니다 -ㅅ-

### 00. IE에서 inline style element 에 평가식을 넣는경우 적용되지않는 문제 


AS-IS 

style element내부에 angular expression 을 직접 사용하였다. 
```html
    <div class="file-size" style="width:{{percent +'%'}}"></div>
```


TO-BE 

ng-style을 이용해서 좀더 구조적인 표현식으로 교체한다. 

```html
    <div class="file-size" ng-style="{width:percent +'%'}"></div>
```
비슷한 예로 attribute 을 동적으로 변경하려하는 경우에도 아래의 방법으로 적용이 가능하다. 

```html
<col width="{{width}}" />

-> 

<col ng-attr-width="{{width}}" /> 
```

### 01. IE8 이하버전에서 promise(q)를 사용하는경우 

보통 IE8 이하버전에서 `catch`, `finally` block은 예약어라서 함수호출을 직접하는 경우 스크립트 오류가 나게된다. 그래서 아래와 같은형식으로 사용하는것이 안전하다. 

```javascript
$http.get('/api/user/list').then(function(res) {
})['catch'](function(err) {
   // do something. 에러 다이얼로그를 연다던가.. 
})['finally'](function() {
  // do something. 프로그레스를 닫는다던가.. 
})
```

### 02. 검색엔진 최적화

검색엔진은 가차없다 -ㅅ-;; 아래 angular expression 으로 구성된 부분을 내용으로 인지하고 봇이 바로 긁어가버린다

AS-IS 

```html
<span> {{data.title}} </span>
```

TO-BE 

```html
<span ng-bind="data.title"></span>
```
앞단에 gateway 서버로 nginx / apache를 두고 phantomjs 서버를 하나 띄워둔뒤 특정 agent string(google bot) 인경우에는 phantomjs가 렌더링된 html을 리턴하는 형태로 구성하면 해결 가능하다
