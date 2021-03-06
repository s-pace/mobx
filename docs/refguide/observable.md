---
title: observable
sidebar_label: observable
hide_title: true
---

# observable

Usage:

-   `observable(value)`
-   `makeObservable(this, {someProperty: observable})`

`observable` is a programmatic API to declare objects as observable directly.
Often you do not invoke it directly but instead use it to declare properties
observable using [`makeObservable` / `makeAutoObservable`](make-observable.md).

## the rules of `observable`

Observable values can be JS primitives, references, plain objects, class instances, arrays and maps.
The following conversion rules are applied, but can be fine-tuned by using _modifiers_. See below.

1. If _value_ is an array, a new [Observable Array](array.md) will be returned.
1. If _value_ is an ES6 `Map`: a new [Observable Map](map.md) will be returned. Observable maps are very useful if you don't want to react just to the change of a specific entry, but also to the addition or removal of entries.
1. If _value_ is an ES6 `Set`: a new [Observable Set](set.md) will be returned.
1. If _value_ is an object _without_ prototype, all its current properties will be made observable. See [Observable Object](object.md)
1. If _value_ is an object _with_ a prototype, a JavaScript primitive or function, `observable` will throw. Use [Boxed Observable](boxed.md) observables instead if you want to create a stand-alone observable reference to such a value. MobX will not make objects with a prototype automatically observable; as that is considered the responsibility of its constructor function. Use `makeObservable` in the constructor instead.

These rules might seem complicated at first sight, but you will notice that in practice they are very intuitive to work with.

By default making a data structure observable is _infective_; that means that `observable` is applied automatically to any value that is contained by the data structure, or will be contained by the data structure in the future. This behavior can be changed by using [modifiers](modifiers.md).

For examples of the declarative use of `observable` with `makeObservable`, see [Concepts & Principles](../intro/concepts.md) and [`makeObservable` / `makeAutoObservable`](make-observable.md).

## Using `observable` programmatically

You can also use `observable` programmatically. `observable(value)` is a convenience API that succeeds only if the value can be made into an observable data structure (_Array_, _Map_, _Set_, or _observable-object_). For all other values, no conversion will be performed.

You can't use `observable` to make objects with a prototype observable; as that is the responsibility of its constructor function. Use `makeObservable` or `makeAutoObservable` in its constructor.

## Inspecting whether something is observable

### `isObservable`

Usage:

-   `isObservable(value)`

Returns `true` if `value` is observable.

Works for all observables, computed values and disposer functions of reactions.

### `isObservableProp`

Usage:

-   `isObservableProp(obj, propertyName)`

Returns `true` if property named `propertyName` of `obj` is
observable.

Works for all observables, computed values and disposer functions of reactions.

### Type-specific APIs

MobX also exposes `isObservable*`, like `isObservableArray` and so on to expect for specific observables.
