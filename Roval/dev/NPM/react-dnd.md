Created Date：2024-02-18 10:58:12  
Last Modified：2024-02-18 10:58:12

# Tags

# Content

## Overview

### Items and Types

What is an item? An *item* is a plain JavaScript object *describing* what's being dragged. For example, in a Kanban board application, when you drag a card, an item might look like `{ cardId: 42 }`. In a Chess game, when you pick up a piece, the item might look like `{ fromCell: 'C5', piece: 'queen' }`. **Describing the dragged data as a plain object helps you keep the components decoupled and unaware of each other.**

What is a type, then? A *type* is a string (or a [symbol](https://developer.mozilla.org/en/docs/Web/JavaScript/Reference/Global_Objects/Symbol)) uniquely identifying *a whole class of items* in your application. In a Kanban board app, you might have a `'card'`type representing the draggable cards and a `'list'`type for the draggable lists of those cards. In Chess, you might only have a single `'piece'`type.

Types are useful because, as your app grows, you might want to make more things draggable, but you don't necessarily want all the existing drop targets to suddenly start reacting to the new items. **The types let you specify which drag sources and drop targets are compatible.** You're probably going to have an enumeration of the type constants in your application, similar to how you may have an enumeration of the Redux action types.

### Monitors

Drag and drop is inherently stateful.**The monitors let you update the props of your components in response to the drag and drop state changes.** For each component that needs to track the drag and drop state, you can define a **collecting function** that retrieves the relevant bits of it **from the monitors**. React DnD then takes care of timely calling your collecting function and merging its return value into your components' props.

### Connectors

**The connectors let you assign one of the predefined roles (a drag source, a drag preview, or a drop target) to the DOM nodes** in your `render`function.

### Drag Sources and Drop Targets

Whenever you want to make a component or some part of it draggable, you need to wrap that component into a *drag source* declaration. Every drag source is registered for a certain *type*, and has to implement a method producing an *item* from the component's props. It can also optionally specify a few other methods for handling the drag and drop events. The drag source declaration also lets you specify the *collecting function* for the given component.

The *drop targets* are very similar to the drag sources. The only difference is that a single drop target may register for several item types at once, and instead of producing an item, it may handle its hover or drop.

## Hooks

### useDrag

[React DnD](https://react-dnd.github.io/react-dnd/docs/api/use-drag)

#### Parameters

- `spec`: A specification object or a function that creates a specification object.
- `deps`: A dependency array used for memoization. This behaves like the built-in `useMemo`React hook. The default value is an empty array for function spec, and an array containing the spec for an object spec.

##### Specification Object Members

**`end(item, monitor)`**: Optional. When the dragging stops, `end`is called. For every `begin`call, a corresponding `end`call is guaranteed.

#### Return Value Array

### useDrop

When the dragging stops, `end`is called.

# Reference

[React DnD](https://react-dnd.github.io/react-dnd/docs/overview)
