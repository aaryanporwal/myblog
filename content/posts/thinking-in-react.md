---
title: "React Re-rendering: Stop the Madness 🎯"
date: 2025-12-01T17:30:40+05:30
draft: false
tags:
  - "react"
  - "frontend"
  - "javascript"
---

Ever wonder why your React app feels sluggish? Let's fix those unnecessary re-renders.

## When Does React Re-render?

Three triggers:

1. Props change
2. State changes
3. Parent re-renders

**The catch?** React does shallow comparisons. New object/function references = re-render, even with identical values. That's where `useMemo`, `useCallback`, and `React.memo` save the day.

## The Problem

```jsx
// ❌ ExpensiveChild re-renders on EVERY keystroke
function Parent() {
  const [count, setCount] = useState(0);
  const [text, setText] = useState("");

  return (
    <>
      <input value={text} onChange={(e) => setText(e.target.value)} />
      <button onClick={() => setCount(count + 1)}>Count: {count}</button>
      <ExpensiveChild count={count} />
    </>
  );
}
```

## The Fix

```jsx
// ✅ Only re-renders when count actually changes
const ExpensiveChild = memo(function ExpensiveChild({ count }) {
  return <div>Count is: {count}</div>;
});
```

## Callbacks Are Trickier

```jsx
// ❌ New function reference every render
const handleClick = () => console.log("Clicked!");

// ✅ Stable reference with useCallback
const handleClick = useCallback(() => {
  console.log("Clicked!");
}, []);
```

> **Pro Tip:** Use named functions in `memo()` instead of arrow functions. React DevTools will thank you with better debugging instead of "Anonymous" components everywhere.

## Quick Reference

- **Function?** → `useCallback`
- **Value?** → `useMemo`
- **Component?** → `React.memo`

Master these three, and your app will fly.

## When useCallback and useMemo Become Too Much?

React’s memo hooks are great, until they’re not.
Every useCallback and useMemo has its own cost. React must track dependencies, store values, and compare them every render.

### The “Over-Optimized” Trap:

```jsx
const handleChange = useCallback((e) => setFilter(e.target.value), []);
const filtered = useMemo(
  () => items.filter((i) => i.match(filter)),
  [items, filter]
);
```

Looks fast? Not really.
If items is small and no child is memoized, this “optimization” actually adds overhead and hurts readability.

### The Rule of Thumb:

| Situation                              | Memoization? | Why                        |
| -------------------------------------- | ------------ | -------------------------- |
| Simple handlers, small lists           | ❌           | Overhead > benefit         |
| Expensive computations                 | ✅           | Worth caching              |
| Passing props to `React.memo` children | ✅           | Prevents re-renders        |
| No real performance issue              | ❌           | Don’t optimize prematurely |

> 💡 Tip: Measure first, memoize later. Unnecessary `useMemo` and `useCallback` often make code slower and harder to reason about.

Alright, that's it for this blog post.

More performance tricks coming soon.

Stay tuned and happy coding!
