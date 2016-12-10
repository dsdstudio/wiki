## event processor 

```javascript
var fn;
var EventBus = function EventBus() {
    this.listenersMap = {};
    this.getOrCreateListeners = function(k) {
        if ( !this.listenersMap[k] ) return (this.listenersMap[k] = []);
        else return this.listenersMap[k]; 
    };
};
(fn = EventBus.prototype).addLast = function(k, fn) {
    var listeners = this.getOrCreateListeners(k);
    listeners.push(fn);
},
fn.addFirst = function(k, fn) {
    if ( !k ) throw new Error('인자가 잘못되었습니다[' + k + ']');
    var listeners = this.getOrCreateListeners(k), self = this;
    this.listenersMap[k] = [fn].concat(listeners);
    return (function() {
        console.log(self);
        self.unsubscribe(k, fn);
    });
},
fn.addLast = function(k, fn) {
    var listeners = this.getOrCreateListeners(k), self = this;
    listeners.push(fn);
    return (function() {
        self.unsubscribe(k, fn);
    });
},
fn.fireEvent = function(k, v) {
    var listeners, i, n;
    listeners = this.getOrCreateListeners(k);
    if ( !listeners ) return;
    for (i=0,n=listeners.length;i<n;i++) listeners[i](this, v);
},
fn.unsubscribe = function(k, fn) {
    var listeners = this.getOrCreateListeners(k);
    var i = listeners.length;
    while(i--) {
        if ( listeners[i] === fn ) {
            listeners.splice(i, 1);
            break;
        }
    }
},
fn.unsubscribeByKey = function(k) {
    if ( !k ) throw new Error('잘못된 인자입니다.[k:' + k  +']');
    delete this.listeners[k];
};
        
  

var eq = new EventBus();
 
eq.addLast('ADDED', function(v) {
  console.log('ADDED LAST', v); 
});
  

var fn = function(v) {
  console.log('ADDED FIRST', v); 
};
eq.addFirst('ADDED', fn);
eq.fireEvent('ADDED', 'a');

eq.remove('ADDED', fn);

eq.fireEvent('ADDED', 'a')
```
