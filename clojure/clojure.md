# clojure cookbook 


## hashmap 순회하기 

```clojure
(def m {:a 1 :b 2})

;; 계속된 함수호출로 코드를 만들기엔 지저분하니 threading last 매크로로 만들었다. 
(->> m ;; 여기에서는 걍 맵이 다음 함수의 인자로 넘어감 
     seq ;; 맵을 시퀀스로 변경 ([:a 1] [:b 2]) 이런형태로 변경됨 
     (map (fn [[k v]] ;; 인수분해를 이용하여 배열 안의 key, value를 바인딩.. 함수내에서 무언가 하고싶은것을 하면 된다.
             (dosomething))))
```
