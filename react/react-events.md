# React Events

- React uses a single event listener per event type
  - then invokes all handlers within v-dom
- The normalize and deduplicate events to account for browser quirks
  - each event-type listener is ensured per single render cycle
  - consistent event arguments for all browsers
    - boolean `altKey`
    - number `button`
    - number `buttons`
    - number `clientX`
    - number `clientY`
    - boolean `ctrlKey`
    - boolean `getModifierState(key)`
    - boolean `metaKey`
    - number `pageX`
    - number `pageY`
    - DOMEventTarget `relatedTarget`
    - number `screenX`
    - number `screenY`
    - boolean `shiftKey`

*EventPluginHub*

- sends events into plugins
- example plugins
  - simpleeventplugin: same accross browsers (mouse, key)
  - changeventplugin: (onchange)
  - tepeventplugin:
  - enter/leaveplugin:

*Synthetic events*

- normalized events
- are generated by plugins
- they're being pooled
  - same instance is used in multiple handlers
  - reset with new properties before each invocation

*Annotations*

- PluginHub annotates each event with it's handlers and fibre nodes
- PluginHub goes through annotations and dispatches

*2 phases*

- capturing
- bubbling
-
