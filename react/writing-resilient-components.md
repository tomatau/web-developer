# Writing resilient components

https://overreacted.io/writing-resilient-components/

## Style guides

- maybe arrows or declarations
- maybe sort props alphabetically
- style guides can create friction
- see the linter as an overzealous noisy gatekeeper
  - drowns useful errors in sea of nitpicking
- extra difficulty for designers to work with
- don't learn what is valid / invalid
- enforcer mindset, false sense of accomplishment
- remove rules that don't prevent bugs

## Guides

- ensure every rule has prevented a bug
- don't assume they're "best practice", question it
- use auto formatting to fix things rather than a linter to shout at you
- use a linter only to find bugs, not enforce aesthetics
- there're inconsistencies a linter can't catch
  - so build a team relationship and trust, use wiki pages, etc...
- don't need to automate everything
  - insights from reading a rationale used in styleguides
  - rather than simply adopting the style guides

## Component design principles

1. Don't stop the data flow
2. Always be ready to render
3. No component is a singleton
4. Keep the local state isolated

## 1. Don't stop the data flow

- accept props over time and let them change things
- e.g. don't copy props into state
  - if intentional, use name like `initialX` or `defaultX`

### expensive calculations

- do expensive calcs in render instead, not constructor
- purecomponent to avoid the calc
- use state and componentDidUpdate to re-calc based on props/state
  - now second re-render after change
  - getDerivedStateFromProps is clunky
- use memoization `useMemo`
  - can use `"memoize-one"` in class components
  - hook has more advantages as built in

### Side effects

- if your componentDidMount performs a fetch based on props
  - prop changes mean fetch results aren't appropriate anymore
  - need to re-fetch in componentDidUpdate on props change
  - easy to forget to update both (cdm, cdu) life-cycle methods
  - easy to forget to handle both state and prop changes
- useEffect hook, is statically analyzed
  - dependencies are explicit
  - `exhaustive-deps` lint rule warns when deps aren't declared

### Optimizations

- when manually optimizing components
  - `PureComponent` and `Reace.memo` are safe
  - when writing own comparison on SCU, forget to compare function props
  - won't see changes to callback props
    - e.g. if the function is rebound to a new value, or replaced with `null`
- avoid manually implementing SCU and avoid custom comparison in `React.memo`
  - The default comparison respects changing function identity

#### Optimizations with hooks

- functions are different on every render...
- `useContext` can avoid prop drilling functions
  - therefore, can optimize rendering without worrying about functions

## 2. Always be ready to render

- components are declarative
  - describe how the UI looks at any moment in time
- e.g. setState in componentDidReceiveProps
  - On^2 updates from parent animation
  - the prop setting state will "blow away" state set from event listener in child
    - PureComponent can still have the same issue if multiple props
  - causes problems with derived state
- renders caused by props or state should behave the same
  - components should be resilient to time of rendering
- use controlled components or uncontrolled with a key to reset
  - `<TextInput key={formId} />` will reset state

stress test:

```js
componentDidMount() {
  // Don't forget to remove this immediately!
  setInterval(() => this.forceUpdate(), 100);
}
```

- never use `PureComponent`, `sCU` or `React.memo` for **behaviour**

## 3. No component is a singleton

- sometimes assume component is displayed only once
  - e.g. NavBar
- can lead to design problems that surface later
  - e.g. add an animation between two pages
- to test, render `App` twice
- example problem: `componentWillUnmount` to cleanup redux state
  - or `componentDidMount`
  - one component can wipe state for another

## 4. Keep local state isolated

- if this component needed rendering twice, would this state need reflecting in the other copy
- avoid making the local state global, helps with resilience
