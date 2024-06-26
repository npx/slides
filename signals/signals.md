---
title: Angular Signals: Gamechanger für reaktive Entwicklung
author: Yannick Baron
theme: thinktecture
description: "Das Angular-Team stellt sein reaktives Modell auf den Prüfstand und schlägt als neuen
  Ansatz die von anderen Frameworks bekannten Signals vor. Während Altanwendungen zunächst weiter
  funktionieren wie gehabt, bieten Signals entscheidende Vorteile gegenüber dem bestehenden Modell.
  Brauchen Anwendungen in Zukunft überhaupt noch RxJS? Welche Vorteile haben Signals auf die
  Performance einer Anwendung? Und wie funktioniert das Reaktivitätsmodell im Detail? Yannick Baron
  von Thinktecture und Google Developer Expert für Angular beantwortet alle Fragen rund um die neuen
  Angular-Signals." 
---

<!-- _class: title -->

# Angular Signals

## Gamechanger für reaktive Entwicklung

- Yannick Baron
- @yannick_baron

---

# Angular application

- Components are the main building blocks
- Consist of a TypeScript class, an HTML template, and CSS styles
- Essentially define your custom DOM nodes
- Component templates can use more components

1. **An application is essentially a tree of components**

---

# A simple Angular component...

```typescript
@Component()
export class CounterComponent {
  counter = 0;

  increment() {
    this.counter++;
  }

  reset() {
    this.counter = 0;
  }
}
```

```html
<p>{{ counter }}</p>

<button (click)="increment()">Increase Counter</button>
<button (click)="reset()">Reset</button>
```

1. _note:_ setting a variable on the component updates the template

---

# The Magic of Angular: Zone.js

- Zones are execution contexts that track asynchronous operations
- Achieved via monkey patching
- Angular creates a root zone for the application ("NgZone")
- When an event or asynchronous task occurs, Zone.js notifies Angular
- Angular triggers change detection and updates the views

---

# Angular Change detection

- Start from root component
- Walk through the component tree
- Check template bindings
- If change detected, update DOM node
- Changes propagated to children

[![w:800pt](https://mermaid.ink/svg/pako:eNp1kL0KwzAMhF8laE5ewEOhP3Rql5ZuXkSsNKaxHWSZUkLevW4CgYZWgxB33w26AepgCBQ0XXjWLbJoX-TZ9v0-uD548lJU1aa4ReK4SDM0729ngU82yor_Qx5tJ8Q_2JUxBS5obNglkeAXA0pwxA6tyY8Mn7wGacmRBpVPg_zQoP2YOUwSri9fgxJOVELqDQodLN4ZHagGu5hVMlYCn-dmpoLGN2s-ZnM)](https://mermaid.live/edit#pako:eNp1kL0KwzAMhF8laE5ewEOhP3Rql5ZuXkSsNKaxHWSZUkLevW4CgYZWgxB33w26AepgCBQ0XXjWLbJoX-TZ9v0-uD548lJU1aa4ReK4SDM0729ngU82yor_Qx5tJ8Q_2JUxBS5obNglkeAXA0pwxA6tyY8Mn7wGacmRBpVPg_zQoP2YOUwSri9fgxJOVELqDQodLN4ZHagGu5hVMlYCn-dmpoLGN2s-ZnM)

---

# Downsides of Zone.js approach

- Reliant on Zone.js monkey patching
- Runs more often than necessary
- Sometimes leads to unpredictable behavior
- Recheck all template bindings

1. Advanced concepts to learn to improve performance:
   _OnPush, Immutability, runOutsideAngular, ..._

---

# Reactive Angular with RxJS

- Use OnPush strategy
- Reactive state management
- Subscribe via Async pipe
- Async pipe notifies the change detector

1. Only check relevant components

[![w:800pt](https://mermaid.ink/svg/pako:eNp1kE0LwjAMhv_KyHnzB_Qg-IEnvSjeeglrpsX1gzZFZOy_Wy1MHJpDCO_7JJB3gNYpAgFd7-7tFQNLW-Vaeb9xxjtLlqtm0Syrc6QQJ61QpX87H3qvI88W_qE73TOFH_DMKBtHVNqtE7OzkwM1GAoGtcrPDK8DEvhKhiSIPCoMNwnSjpnDxO70sC0IDolqSF4h01bjJaAB0WEfs0pKswuHks47pPEJhudnKw)](https://mermaid.live/edit#pako:eNp1kE0LwjAMhv_KyHnzB_Qg-IEnvSjeeglrpsX1gzZFZOy_Wy1MHJpDCO_7JJB3gNYpAgFd7-7tFQNLW-Vaeb9xxjtLlqtm0Syrc6QQJ61QpX87H3qvI88W_qE73TOFH_DMKBtHVNqtE7OzkwM1GAoGtcrPDK8DEvhKhiSIPCoMNwnSjpnDxO70sC0IDolqSF4h01bjJaAB0WEfs0pKswuHks47pPEJhudnKw)

---

# Downsides of RxJS (for template rendering)

- Observables asynchronous by nature
- AsyncPipe default null value
- Subscription management
- Recheck all template bindings
- No reactive inputs

---

# Enter Signals

- Reactive primitive
- Hold value, notify when value changes
- Integrated with Angular

1. Results in knowing **exactly** which **nodes** need updating

---

# Same component as before

```typescript
@Component()
export class CounterComponent {
  counter = 0;

  increment() {
    this.counter++;
  }

  reset() {
    this.counter = 0;
  }
}
```

```html
<p>{{ counter }}</p>

<button (click)="increment()">Increase Counter</button>
<button (click)="reset()">Reset</button>
```

---

# Same component as before with signals

```typescript
@Component()
export class CounterComponent {
  counter = signal(0);

  increment() {
    this.counter.update((count) => count + 1);
  }

  reset() {
    this.counter.set(0);
  }
}
```

```html
<p>{{ counter() }}</p>

<button (click)="increment()">Increase Counter</button>
<button (click)="reset()">Reset</button>
```

---

# Signals API

- **signal()**
  Create a writable Signal
- **set()**
  Set a new value to the Signal
- **update()**
  Update the Signal's value based on its current one
- **computed()**
  Create a _read only_ Signal deriving its value from other Signals

---

<!-- _class: section-slide -->

# Code with me?

https://github.com/thinktecture/ngrx-demo

Branch: signals/code-with-me

<!--
Code With Me:

* start on signals/code-with-me
* explain what the counter does
  - changes color of count
  - count
  - reset
  - persistence
* show color function and CD runs -> OnPush
* migrate to signals
* introduce effect()
-->

---

# Signals API - effect()

- React to signal value changes
- Used for side effects
  - _logging, persisting to browser storage, ..._
- Executed once initially
- Can not write to Signals by default
- Reliant on the injection context

<!--
Code With Me:

* migrate persisting to effect
* introduce signal based components
-->

---

# Signal based Components

- **input()**
  Create a writable Signal bound to a component input
- **model()**
  Creates a writable Signal that reflects changes (two way binding)
- **viewChild() / viewChildren()**
  Access references to components, directives and elements in your view

<!--
Code With Me:

* read storage key from input
* make storage key required
* toggle key in host
* introduce untracked() to prevent persisting same value
* refactor to model() two way binding (counter value from host)
* move persisting to host

* show stopping and starting an effect (start/stop persisting)

* viewChildren: find counters
* add public api to change colors?

* advanced demos
-->

---

# RxJS has its place

- RxJS is used to handle asynchronicity
- Plenty of useful operators
  - error handling, retrying, scheduling, buffering, ...
- RxJS interop provided by angular
  - toObservable / toSignal / takeUntilDestroyed
- Used for asynchronous side effects
  - @ngrx/effects, ComponentStore effects, rxMethod

---

# What the future holds

- Zone-less applications
- Signal based components
- Signal based, synchronous APIs
- Function based life cycle hooks

1. Less concepts to learn
2. Better performance out of the box
3. Less wrestling with the CD

---

# Read more about upcoming changes in Angular

1. https://github.com/angular/angular/discussions
2. https://angular.io/guide/signals

---

<!-- _class: section-slide -->

# Thank You!

https://www.thinktecture.com/thinktects/yannick-baron

<!--

---

# Workshop Ideen: Advanced

- signal comparator function
  - show mutable data type and default equal function

- rxjs interop
  - toObservable

Flow:
- equal function
  - show via reset that signal does not fire with same value
  - build save signal<void> "trigger"
- untracked
  - show untracked in effect as function
  - show untracked around signal
  - show untracked in computed
- toSignal
  - use interval to periodically save
  - show that unsubscribed when component is destroyed
  - explain destroyRef and takeUntilDestroyed
- effect write
  - cleanup
-->
