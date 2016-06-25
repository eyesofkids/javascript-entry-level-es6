
```
var callMe(fn, ...args) {
    return fn.apply(args);
}
 
callMe(Math.max, 4, 7, 6);
```
