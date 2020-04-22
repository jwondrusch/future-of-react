---?include=sections/cover.md
---?include=sections/intro.md

---

## The Last Few Years

- Fiber
- Functional Components
- Hooks

Note:
- Fiber: New algorithm, completely internal, reduced roadblocks in developing react
- Functional Components: Removed lifecycle methods, BUT made components easier to test, to reason about, and have better performance.
- Hooks: Re-connecting React features w/o classes. useEffect, useState, useContext, useReducer, useMemo, useRef

---

## The Real Reason You're Here

### Looking Forward

---

## The Real Reason You're Here

### Will I need to rewrite my app... again?

---

# The Goal

> "Apps should start fast and stay fast, regardless of device or network speed"

Note:
- True for FB scale or mom and pop scale
- 

---

![IMAGE](assets/img/rt-performance.png)

Note:
- These are real issues

---

## Two Major Features

---

## Two Major Features

### 1. Concurrent Mode

Note:
- Current lifecycle is data driven.
- Async is a pain.
- "In Concurrent Mode, React can work on several state updates concurrently."

---

## Two Major Features

### 2. Suspense

Note:
- NOT a data fetching library
- Way to let React know that something is pending

---

## Who These Are For

- Early Adopters
- Tinkerers
- Library Authors
- Worriers

---

## Opt-In Only

Experimental builds only

![IMAGE](assets/img/concurrent-mode.png)

---

## Opt-In Only

**Before:**
```
ReactDOM.render(<App />, document.getElementById('root'));
```

<small>Strict Mode **strongy** encouraged</small>

---

## Opt-In Only

**After:**
```
ReactDOM.createRoot(document.getElementById('root')).render(<App />);
```

<small>Strict Mode **strongy** encouraged</small>

---

## Good News for Chicken Little

- Taking it Slow. Introduced in 2018. React v15 = 3 yrs
- Collaborative Effort (FB, Google, Next, others)

---

## The Problem

An analogy

Notes:
- Jump out of the web and react world and into the graphics world
- Let's say you want to render a happy face on the screen. 
- Your display is constantly reading from your framebuffer - the list of all the data shown on your screen. Individual pixels.
- When you decide that you want to render an image, like an emoji, your computer updates the buffer.
- But before you're done with the data update, the display has outpaced you, meaning you get half an image, briefly.
- This is called "tearing", and it's no fun.

---

## The Problem

To the web

- Rendering a UI
- Need data from multiple places

---

The Goal: Apps should start fast and stay fast, regardless of device or network speed

---

![IMAGE](assets/img/concurrent-mode.png)

---



Notes:
- Like a long running feature branch

---

- Interrupt Rendering based on Urgency
- REnder before data is received, reducing intermediate states

---

## Better Experiences

- Fewer intermediate states = _feels_ faster

---

## What Suspense Is not

- Suspense is not a data fetching library. It’s a mechanism for data fetching libraries to communicate to React that the data a component is reading is not ready yet.
- It's not a Client
- 

---
## What Is Suspense

- Integrate deeply w/ React
- Controls the loading sequence

---

A way to tell React that something is pending.

---

## Terminology

**Suspending** = the ability for a component let React know that it's waiting. 

---

## Example

If a component suspends, React looks up the tree, finds the first suspense

```jsx
<Suspense fallback="Loading...">
  <YourComponent />
</Suspense>
```

---

```js
import React, {lazy} from 'react';
import { fetchData } from './api';

const resource = api.fetchPosts(); // Fetch data
const Posts = lazy(() => import('./Posts')); // Fetch code

// Render data
const App = () => (
    <>
        <Suspense fallback="Loading...">
            <Posts resource={resource} />
        </Suspense>
    </>
);
```

---

## How Suspense Works

```js
let isResolved = false;

const AsyncFetcher = ({ result, ms }) => {
  if (isResolved === true) {
    return result;
  }

  throw new Promise(resolve => 
    setTimeout(() => {
      promiseResolved = true;
      resolve();
    }, ms)
  );
}
```
Note:
- Instead of throwing an error, throw a promise.
- Allows it to keep coming back until it's ready.
---

```js
function ProfilePage() {
  const [startTransition, isPending] = useTransition({ timeoutMs: 10000 });
  const [resource, setResource] = useState(initialResource);

  function handleRefreshClick() {
    startTransition(() => {
      setResource(fetchProfileData());
    });
  }

  return (
    <Suspense fallback={<h1>Loading profile...</h1>}>
      <ProfileDetails resource={resource} />
      <button onClick={handleRefreshClick} disabled={isPending}>
        {isPending ? "Refreshing..." : "Refresh"}
      </button>

      <Suspense fallback={<h1>Loading posts...</h1>}>
        <ProfileTimeline resource={resource} />
      </Suspense>
    </Suspense>
  );
}
```
@[2,3]
@[4-8]
@[11,12]
@[13-15]
@[17-19]

---

## Old School

```js
var $theRighAncestor = $('.great-great-grandchild').parents('.the-first-ones');
```

---

---

## Old Fetching Paradigms

- Fetch on Render
- Fetch then Render

---

## Old Fetching Paradigms

- Start Fetching
- Finish Fetching
- Start Rendering

---

## Suspense's Paradigm

- Render as you Fetch

---

## Suspense's Paradigm

- Start Fetching
- Start Rendering
- Finish Fetching
- Finish Rendering

## 

---

## How?

- Don't wait for component to load to start rendering
- Fetch code and data in parallel

--- 

## Data vs Components

- Understand the React Lifecycle
- Render when Prop Change
- Render on State Change

---

## Right State, Right Time

- Before suspense, you had to set data at the right time
- With Suspense, you set state immediately, bypassing the issue

---

## Removing Dependencies

- Find the middleground
- Design your loading states

Notes:
- Don't wait for everything
- Don't render everything as soon as it's ready

---

## Concurrent Mode & Suspense In Practice

- Think of it like concurrent development on a team
- useTransition returns `startTransition` (callback, tells React what to defer) and `isPending` (bool, if we're fetching)
- startTransition tracks the changes under the hood/in the background
    - Like double buffering
- isPending lets your app know when to handle the results

---

## Ecosystems

- GraphQL: relay, react-apollo-hooks
- Lazy Loading: react.lazy
- SSR: Experimental libraries
