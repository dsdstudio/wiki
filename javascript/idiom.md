## 00. switch 구문으로 패턴매칭 구현 

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

### References 
- http://stackoverflow.com/questions/2896626/switch-statement-for-string-matching-in-javascript
