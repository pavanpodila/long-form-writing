# MobX Quick Start Guide

**State Management** is an aspect of your application that controls what is ultimately rendered. It's the pumping heart that spreads data across your app and brings various components to life. The _state_ of a Web Application, like the ones built with `React.js`, rely on the central Store as the _single source of truth_. Actions originating in the UI are fired on this store to modify the state. This results in a new version of the state, which is notified back to the UI by the Store. This cycle of _Actions --> Store --> UI_ is always uni-directional and makes it easy to understand the data-flow in the application.

MobX embraces this idea and provides a model of Transparent Functional Reactive Programming (TFRP) that eliminates much of the boilerplate in setting up this data-flow. MobX is a state-management framework that comes with a simple, yet powerful set of building blocks like Observables, Actions and Reactions. These three concepts acts as the junctions of the data-flow triad.

![Observables-Actions-Reactions triad](mobx-triad.png)

Observables carry the central mutable-state of your application. As this state changes, it fires notifications that can be observed by other participants (aka observers) of the data-flow triad. Reactions are the primary observers in the MobX framework. The UI (React Components) is the most glorious observer in the MobX system. These components observe the observable-state and render the visual representations of data.

> Reacting to a change in observable-state is also known as a **_side-effect_**. The UI is the most visible _side-effect_ in your application.
