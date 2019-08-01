# Rendering large lists with React-window

https://addyosmani.com/blog/react-window/

- display large lists of data
- **react-virtualized**
  - windowing library by Brian Vaughn
  - only render items in viewport
  - less cost of rendering non visible items, potentially thousands

- "virtualizing" is having a virtual window around your list
  - small container DOM element, e.g. a `ul` with relative positioning
  - big DOM element for scrolling
  - absolute positioned children, style: top,left,width,height
- fetch/display items as user scrolls

- **react-window** is a rewrite of react-virtualized
  - to be smaller, faster and "tree-shakable"
  - tree-shakable
    - size is a function of the API surfaces you use
  - uses grid internally
