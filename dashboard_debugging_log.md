# Dashboard Debugging Log

## **Part 1 – GreetingWidget**

### **(1) Original Buggy Code**
```jsx
import React from 'react';

function GreetingWidget() {
  const currentHour = new Date().getHours;
  let greeting;

  if (currentHour < 12) {
    greeting = "Good morning!";
  } else if (currentHour > 12 && currentHour < 18) { 
    greeting = "Good afternoon!";
  } else {
    greeting = "Good evening!";
  }

  return (
    <div style={{ padding: '10px', border: '1px solid #ccc', borderRadius: '5px' }}>
      {greeting}
    </div>
  );
}

export default GreetingWidget;
```

---

### **(2) Debugging Prompt**
> I’m debugging a React component called `GreetingWidget`.  
> It should display “Good morning!”, “Good afternoon!”, or “Good evening!” based on the current time.  
> However, the greeting is not always correct.  
> Here is the code (below). Please identify what’s wrong and why it doesn’t display the correct greeting.  
> Suggest where the bug is and explain it briefly.  

---

### **(3) AI’s Bug Analysis**
> The issue is with how `getHours` is used.  
> `new Date().getHours` returns a **function reference**, not the actual hour.  
> It should be called as `new Date().getHours()`.  
>  
> Also, the condition `currentHour > 12 && currentHour < 18` skips exactly 12 PM.  
> It should be `currentHour >= 12 && currentHour < 18`.

---

### **(4) Fixing Prompt**
> Please provide the corrected `GreetingWidget` component using your identified fixes.  
> Ensure it correctly shows “Good morning!”, “Good afternoon!”, or “Good evening!” for all times.

---

### **(5) AI’s Corrected Code**
```jsx
import React from 'react';

function GreetingWidget() {
  const currentHour = new Date().getHours(); 
  let greeting;

  if (currentHour < 12) {
    greeting = "Good morning!";
  } else if (currentHour >= 12 && currentHour < 18) {
    greeting = "Good afternoon!";
  } else {
    greeting = "Good evening!";
  }

  return (
    <div style={{ padding: '10px', border: '1px solid #ccc', borderRadius: '5px' }}>
      {greeting}
    </div>
  );
}

export default GreetingWidget;
```

---

### **(6) Reflection**
> The AI quickly identified the missing parentheses in `getHours()` and the incorrect conditional range.  
> The prompt was effective because it clearly described the issue (“not always correct for current time”).  
> The fix worked perfectly after testing.  
> This shows how a well-phrased debugging prompt saves time.

---

## **Part 2 – Counter Component**

### **(1) Original Buggy Code**
```jsx
import React, { useState } from 'react';

function Counter() {
  const [count, setCount] = useState(0);

  const handleIncrement = () => {
    setCount(count + 1);
    setCount(count + 1);
    setCount(count + 1);
  };

  return (
    <div style={{ padding: '10px', border: '1px solid #ccc', borderRadius: '5px', marginTop: '10px' }}>
      <p>Count: {count}</p>
      <button onClick={handleIncrement}>Increment by 3</button>
    </div>
  );
}

export default Counter;
```

---

### **(2) Debugging Prompt**
> I have a React component called `Counter`.  
> It should increase the count by 3 every time the button is clicked.  
> However, when I click the button rapidly, it doesn’t always increment correctly.  
> Here’s the code below — can you identify what’s wrong and explain how React handles multiple state updates?

---

### **(3) AI’s Bug Analysis**
> React batches multiple `setState` calls made in the same event loop, and each call refers to the same **stale `count` value**.  
> Therefore, all three `setCount(count + 1)` calls effectively become one update.  
>  
> The correct approach is to use the **functional updater form**:  
> `setCount(prevCount => prevCount + 1)`  
> This ensures each increment uses the latest state.

---

### **(4) Fixing Prompt**
> Please rewrite the `Counter` component so it reliably increments by 3, even when clicked rapidly.

---

### **(5) AI’s Corrected Code**
```jsx
import React, { useState } from 'react';

function Counter() {
  const [count, setCount] = useState(0);

  const handleIncrement = () => {
    setCount(prev => prev + 1);
    setCount(prev => prev + 1);
    setCount(prev => prev + 1);
  };

  return (
    <div style={{ padding: '10px', border: '1px solid #ccc', borderRadius: '5px', marginTop: '10px' }}>
      <p>Count: {count}</p>
      <button onClick={handleIncrement}>Increment by 3</button>
    </div>
  );
}

export default Counter;
```

---

### **(6) Reflection**
> The AI clearly explained React’s batching behavior and why multiple `setCount(count + 1)` calls don’t work.  
> Using `prev => prev + 1` fixed the issue completely.  
> The prompt worked well because it combined the expected behavior (“increment by 3”) and the observed bug (“doesn’t always increment correctly”).  
> Overall, the AI was very helpful.
