## DOM(Document ObjectModel cookbook) 

### 00. 셀렉트박스나, 달력등을 구현할때 해당 엘리먼트의 외부클릭 판정에 유용한 `node.contains(el)`

보통 `closet()`을 이용해서많이들 구현하는데 오늘 문서를 찾아보니 `contains`함수를 찾게되었다. 이 함수의 장점은 무려 IE5+ 부터 지원된다는것..

```
var selectWrap = document.getElementsByClassName('select-wrap')[0];
var selectList = document.getElementsByClassName('select-list')[0];
var input = document.getElementsByTagName('input');

input.onfocus = function() {
};

input.onfocusout = function() {
  console.log('focusout');
}
document.onclick = function(e) {
  if ( selectWrap.contains(e.target ) ) {
    selectList.style.display = 'inline-block';
  } else { 
    selectList.style.display = 'none';
  }
  console.log(e.target, selectWrap.contains(e.target));
};
```

References 

- https://developer.mozilla.org/en-US/docs/Web/API/Node/contains
