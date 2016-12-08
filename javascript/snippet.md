## event processor 

```javascript
var fn;
var EventQueue = function EventQueue() {
    this.listenersMap = {};
    this.getOrCreateListeners = function(k) {
      if ( !this.listenersMap[k] ) return (this.listenersMap[k] = []);
      else return this.listenersMap[k]; 
    }
};
(fn = EventQueue.prototype).addLast = function(k, fn) {
    var listeners = this.getOrCreateListeners(k);
    listeners.push(fn);
};
fn.addFirst = function(k, fn) {
    var listeners = this.getOrCreateListeners(k);
    this.listenersMap[k] = [fn].concat(listeners);
};

fn.fireEvent = function(k, v) {
  var listeners, i, n;
  listeners = this.getOrCreateListeners(k);
  if ( !listeners ) return;
  for (i=0,n=listeners.length;i<n;i++) listeners[i](v);
}

fn.remove = function(k, fn) {
  var listeners = this.getOrCreateListeners(k);
  var i = listeners.length;
  while(i--) {
    if ( listeners[i] === fn ) {
      listeners.splice(i, 1);
      break;
    }
  }
}
  

var eq = new EventQueue();
 
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
