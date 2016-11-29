## angular 1 관련 삽질 노트입니다 -ㅅ-

### 00. IE에서 inline style element 에 평가식을 넣는경우 적용되지않는 문제 


AS-IS 

style element내부에 angular expression 을 직접 사용하였다. 
```
    <div class="file-size" style="width:{{percent +'%'}}"></div>
```


TO-BE 

ng-style을 이용해서 좀더 구조적인 표현식으로 교체한다. 

```
    <div class="file-size" ng-style="{width:percent +'%'}"></div>
```
비슷한 예로 attribute 을 동적으로 변경하려하는 경우에도 아래의 방법으로 적용이 가능하다. 

```
<col width="{{width}}" />

-> 

<col ng-attr-width="width" /> 
```
