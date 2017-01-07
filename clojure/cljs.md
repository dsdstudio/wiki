## clojurescript cookbook 

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

