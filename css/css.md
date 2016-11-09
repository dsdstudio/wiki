## CSS를 사용하면서 생긴 일들 정리 


## IE에서 float를 초기화할때

아래의 형태로 사용하다 IE에서 동작하지않는것을 보고 좀 찾아보았다. 

initial이 아니라 none을 사용하는것이 맞는듯. display속성에따라 computedValue가 바뀌기도하는데 이부분은 mdn쪽의 문서를 참고해보자. 

```
 float:initial; (x)
 float:none; (o)
```

References

- https://www.w3.org/TR/CSS2/visuren.html#float-position
- https://developer.mozilla.org/ko/docs/Web/CSS/float

