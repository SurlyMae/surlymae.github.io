---
date: "2020-01-15"
title: Lightning Web Components (Salesforce) (notes)
---

_We've started implementing these over the last couple of months at work. Interesting!_

**Summary:**
- Salesforce (SF) implementation of lightweight frameworks built on web standards
- Built on code that runs natively in browsers, so lightweight and delivers exceptional performance
- Most of the code written is standard JS & HTML
- Provides a layer of specialized SF services on top of the core stack:
    - Base Lightning Components
        - a set of over 70 UI components all built as custom elements
    - Lightning Data Service
        - provides declarative access to SF data & metadata, data caching, and data synchronization
    - User Interface API
        - underlying service that makes Base Lightning Components and the Lightning Data Service 'metadata aware'
- Let LWCs manipulate the DOM instead of writing JS to do it.
- Leverages custom elements, templates, shadow DOM, decorators, modules

**Component Bundles**
- a UI component's folder can contain other JS files used to share code. 
    - these utility JS files must be ES6 modules and must have names that are unique within the component's folder.
- To share code between components, create an ES6 module and export the variables/functions that you want to expose. 
    - module folder and file name must follow same naming conventions
    - [more detail here](https://lwc.dev/guide/reference#modules)

**Composition:**
- Owner
    - The component that owns the template
    - Controls all the composed components it contains.
    - Can set public properties on, call public methods on, and listen for events dispatched by composed components
- Container
    - Contains other components but itself is contained within the owner component
    - less powerful than the owner
    - can:
        - read but not change public properties in contained components
        - call public methods on composed components
        - listen for some but not necessarily all events bubbled up by components that it contains
- Parent/child
    - a parent component contains a child component
    - a parent component can be the owner or a container
- To communicate down the containment hierarchy, use properties. To communicate up the containment hierarchy, fire an event in a child component and handle it in an owner or container component
- To communicate down the component hierarchy, an owner component can access a child component's public properties
    - An owner can set a property or attribute on a child component
    - An attribute in HTML turns into a property assignment in JS
    - examples [here](https://lwc.dev/guide/composition#set-a-property-on-a-child-component)
- To communicate down the component hierarchy, owner and parent components can call JS methods on child components


**Patterns:**
- Use `getters` to compute a value
- When a component renders, all the expressions used in the template are evaluated, which means that getters are invoked

**Decorators:**
- `@api`: makes the property public so other components can set it. 
    - public properties define the API for a component
    - If we remove `@api` the property still binds to the HTML template, but it's private
    - public props are reactive
    - also use `@api` for exposing public methods

**Directives:**
- <ins>Rendering Lists:</ins>
    - [`for:each`](https://lwc.dev/guide/html_templates#for%3Aeach)
        - used to render an array
        - add it to the nested `<template>` tag that encloses the HTML elements you want to repeat
        - to access current item, use `for:item="currentItem`
        - to access current item's index, use `for:index="index"`
    - [`iterator`](https://lwc.dev/guide/html_templates#iterator)
        - use to apply a special behavior to the first or last item in a list: `iterator:iteratorName={array}`
        - add `iterator` directive to a nested `<template>` tag that encloses the HTML elements you want to repeat
        - use `iteratorName` to access these properties:
            - value
                - the value of the item in the list
                - use this property to access the properties of the array, i.e. `iteratorName.value.propertyName`
            - index
                - the index of the item in the list
            - first
                - a boolean value indicating whether this item is the first item in the list
            - last
                - a boolean value indicating whether this item is the last item in the list
    - `key`
        - must use a `key` directive to assign a unique ID to each item. When a list changes, the framework uses the `key` to re-render only the item that changed.
            - key must be a string or number
            - can't use `index` as a value for `key`
            - assign unique keys to an incoming data set
            - to add new items to a data set, use a private property to track and generate keys
            - to assign a key to every element in the list, use the `key={uniqueId}` directive
- <ins>Rendering HTML conditionally:</ins>
    - [`if:true|false={property}`](https://lwc.dev/guide/html_templates#render-html-conditionally)
        - add this directive to a nested `<template>` tag that encloses the conditional content
        - this directive binds `{property}` to the template and removes/inserts DOM elements based on whether `{property}` is truthy or falsy


[LWC dev guide](https://lwc.dev/)