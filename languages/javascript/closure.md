# (Stale) Closure

## Example

Let's say we have this React code:
```js
function WatchCount() {
  const [count, setCount] = useState(0);

  useEffect(function() {
    setInterval(function log() {
      console.log(`Count is: ${count}`);
    }, 2000);
  }, []);

  return (
    <div>
      {count}
      <button onClick={() => setCount(count + 1) }>
        Increase
      </button>
    </div>
  );
}
```
This gives us stale closure because the `setInterval`'s callback captures the initial count as 0. Even though the function saved a reference to the variable in its lexical environment, this variable is never updated - `setCount(count + 1)` doesn't update the variable count that the log function captured. In React we need to re-render the components (call `WatchCount` again in this case), which, in this case, would refresh the values and the log function's free variable[^1] count.

[^1]: Free variable - a variable that is taken to a function's scope due to it being in its surrounding closure.

Meanwhile, this works:  
```js
const outer = () => {
  let count = 0;

  const inner = () => {
    count++;
    console.log(count)
  }

  return inner;
}

const counter = outer();
counter();
counter();
counter();
```
Because the inner function directly updates the free variable.

The aforementioned React code could be fixed by changing the state into a simple variable:
```js
function WatchCount() {
  // const [count, setCount] = useState(0);
  let count = 0;

  useEffect(function() {
    setInterval(function log() {
      console.log(`Count is: ${count}`);
    }, 2000);
  }, []);

  return (
    <div>
      {count}
      <button onClick={() => count += 1 }>
        Increase
      </button>
    </div>
  );
}
```
But then the newest `count` wouldn't get re-rendered.
