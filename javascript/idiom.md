## 00. switch 구문으로 패턴매칭 구현 

yona.io 소스까보다가 특이한 패턴이 나와서 메모해둔다.

```javascript
function test(val) {
  switch(true) {
    case /xyz/.test(val): 
      console.log('matches..');
      break;
    case /abc/.test(val): 
      console.log('matches.. abc ' + val);
      break;
    default:
      console.log('default');
  }
}
```

이게 가능한 이유는 `case` 구문 뒤에 `expression` (식) 이 올수 있기때문이다. java의 경우는 value만 올수 있기때문에 용도가 많이 제한된다 -ㅅ-ㅋ 


### References 
- [http://es5.github.io/#x12.11](http://es5.github.io/#x12.11)
- http://stackoverflow.com/questions/2896626/switch-statement-for-string-matching-in-javascript
