# Throttle

This implementation executes a function immediately on the first call, then enforces a "cool-down" period where subsequent calls are ignored.

```javascript
/**
 * @callback func
 * @param {number} wait
 * @return {Function}
 */
export default function throttle(func, wait) {
  let shouldThrottle = false;

  return function(...args){ //return一个function!
    // shouldThrottle -> in cool-down冷却 period
    if(shouldThrottle) return;

    //should not throttle
    func.apply(this, args);

    shouldThrottle = true;
    setTimeout(() => {
      shouldThrottle = false; // 注意:setTimeout给shouldThrottle设置
    }, wait);
  }
}
