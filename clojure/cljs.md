## clojurescript cookbook 

### clojure에서의 자료구조를 js 형태로 변환하기 

```clojure
(def list [1 2 3 4])
(clj->js list)
```

### reagent 

간단한 경우에는 상관이 없으나, `component-did-mount` 각종 lifecycle callback을 통해서 props, state에 접근하려는경우 destructuring 을 이용한 방법이 보통 권장된다. 

이걸 모르고있으니 let 로 lexical scope를 잡고 didmount callback에서 atom 값 변경하고 별짓을 다했었다 -_-;;; 

```clojure
(defn some-component []
  (r/create-class
   {:component-did-update (fn [comp [_ prev-props prev-more]] )
    :reagent-render (fn []
                      [:div "hello"])}))
```

