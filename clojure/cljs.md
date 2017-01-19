## clojurescript cookbook 

### map에 접근하기에는 string이 키인 맵보다 keyword인 맵이 좀더 깔끔하다. 

왜 그런지는 아래 예제를 보자 

```clojure
(def user {"name" "borong" "age" 20})

(get user "name") ; borong 
```

clojure 에서의 keyword는 그 자체로 함수가 될 수 있다. map 의 value에 접근할때 더 우아한 해법을 제공함.. 
```clojure
(def user {:name "borong" :age 20})

(:name user) ; borong 
```

이걸 보다보니 한가지 의문점이 생겼다. string을 키로쓰는 맵 자체를 keyword 키를 사용하는 맵으로 변환해서 쓰면 좀더 유용하지않을까 ?

http://stackoverflow.com/questions/9406156/clojure-convert-hash-maps-key-strings-to-keywords

```clojure
(defn to-keyword-map [m]
  (into {}
    (for [[k v] m]
      [(keyword k) v])))
```


### 짧은 함수는 리더 매크로를 이용하자 

일반적으로 이벤트 리스너의 경우 아래와 같은형태로 많이들 작성하게되는데.. 

```clojure
[:button {:on-click (fn [e] (js/console.log (cljs->js e)))}]
```

함수내용이 복잡하지않고 1줄내로 끝난다면 리더 매크로 사용하는게 더 나아보인다.  내가 생각하기에

```clojure
[:button {:on-click #(js/console.log (cljs->js %1))}]
```

리더매크로는 `#()` 의 형태로 생겼는데.. 괄호 내부에는 S-Expression이 올수 있다. 함수의 인자는 `%1` `%2` ... 와 같은형태로 받을수 있음. 퍼센트 문자 뒤에 숫자는 인자의 인덱스를 의미한다.


### clojure에서의 자료구조를 js 형태로 변환하기 

```clojure
(def list [1 2 3 4])
(clj->js list)
```

### native js객체 setter 와 함께 묶기 

일반적으로 date 객체를 js로 선언하고 사용한다면 아래와 같은 형태가 된다. 

```javascript 

var d = new Date();
d.setDate(1);
d.setHours(0);
d.setMinutes(0);

console.log(d); 
```

cljs 로 위 코드를 전환해보면

```cljs
(doto (js/Date.)
  (.setDate 1)
  (.setHours 0)
  (.setMinutes 0))
```
`doto`를 이용하면 clojure의 threading macro를 쓰는것처럼 하나로 묶을수 있다. 상당히 자주 쓰게되는 패턴 

### reagent lifecycle callback에서 props 값 접근하기

간단한 경우에는 상관이 없으나, `component-did-mount` 각종 lifecycle callback을 통해서 props, state에 접근하려는경우 destructuring 을 이용한 방법이 보통 권장된다. 

이걸 모르고있으니 let 로 lexical scope를 잡고 didmount callback에서 atom 값 변경하고 별짓을 다했었다 -_-;;; 

```clojure
(defn some-component []
  (r/create-class
   {:component-did-update (fn [comp [_ prev-props prev-more]] )
    :reagent-render (fn []
                      [:div "hello"])}))
```

