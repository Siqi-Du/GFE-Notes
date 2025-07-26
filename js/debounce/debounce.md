# Debounce Function

```javascript
/**
 * @param {Function} func
 * @param {number} wait
 * @return {Function}
 */
export default function debounce(func, wait) {
  let timeoutId = null;
  return function(...args) {
    clearTimeout(timeoutId); //cancel previous timer if called again!
    
    // keep a reference to this so func.apply() can access it
    const context = this;
    timeoutId = setTimeout(() => {
      func.apply(context, args);
    }, wait);
  }
}
```

---

### Notes

1.  **语法:** `func.apply(thisArg, args)` or `func.call(thisArg, ...args)`

2.  **pitfall: why .apply(this)** -> The goal is ensure the original func runs with this context of whatever obj called the debounce function.

3.  **arrow functions:**
    * **the outer function (debounce returns : `return function(...args) {}`)**
        -> must be regular function: `this` should be determined by whatever object calls the debounced. (For example, if you attach it to a button click, you want `this` to be the button.)
    * **the inner function (the callback function inside `setTimeout`)**
        -> can be arrow function:
        The goal here is call the original `func` with `this` from the outer function. An arrow function is perfect because it doesn't have its own `this`; it "borrows" it from its surrounding context, which is the outer regular function. This is a modern, clean way to solve a classic JavaScript problem where `this` inside a `setTimeout` callback would normally be lost.
