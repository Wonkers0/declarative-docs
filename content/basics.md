---
title: Getting Started
---

# Installation / Set-up

You can grab the latest version of the library from plot 60983 (`/join 60983`). It will give you 2 shulker boxes, which you need to hold in one of your saved hotbars. Then, you can go onto your own plot and place the templates in the shulker boxes. I recommend placing them at the very top of your codespace (`/p codespace add tinted -c 40`) and then removing all of the codespaces below the declarative library by flying directly underneath the layer on which you placed the bottom-most library thread and running `/p codespace remove 40`.

Then, to initialize the library, simply call the `__init` function in your Join event.

> [!warning] Common Pitfall
>
> All internal functions are hidden in the call function menu and prefixed with `__`, so when calling `__init`, you have to right click the call function block sign with a
> string that has the function name. You won't be able to pick it from the menu by regularly right clicking the sign.

# Creating components

Components are _abstractions_ of things in your game. The library makes no assumption as to what these "things" are. It's up to you to give logic to an otherwise vacant component. Let's look at how they work.

To create a component simply call `createComponent`. It accepts the following arguments:
![](https://i.imgur.com/d3ksAvj.png)

- `name`: The name of the component. This is analogous to class names in OOP.
- `key`: This is used to fire component events, but does not serve as a unique identifier of the component instance; you can have 2 components with the same key. We'll look more into this soon.
- `props`: This is a plural argument that forms a list of values that can be accessed and modified from within the component. If omitted, the component will be instantiated with an empty list as its props.

# Component events

Events are functions that run in a vacuum (separate thread) for each component. For instance, all components will have an `init` event. This is called when the component is created, and allows you to run any initialization logic, such as spawning entities and the like. Let's try it with an example:

```js
PlayerEvent(Join) {
  CallFunction(__init)
  CallFunction(createComponent, "my_component", "") // If you don't wish to have a key,
                                                    // you can pass an empty string.
}

Function(my_component:init) {
  SendMessage("I have been created! My ID is: ", Variable(componentId, LOCAL))
}

// 💾 Output:
// I have been created! My ID is: i3upoxy2dw
```

> [!tip] Viewing Examples Live
>
> For a better experience, run `@e1` to view the example above on plot 60983

### Component info variables

In all component event threads, you have access to the following **local** variables:

- `componentId`: Unique identifier of this component instance; assigned by the library
- `loc/uuid`: The value of the component's key; `loc` if you passed a location, `uuid` if you passed a string.
- `props`: A list with the component's properties

## Keys and events

As previously mentioned, keys are used to fire component events related to things that happen in your game. For instance, passing a location as the key will give you access
to the `onStep` event, which will fire every tick that a player is standing on the block at that location:

```cs
PlayerEvent(Join) {
  CallFunction(__init)
  CallFunction(createComponent, "win_zone", new Location(12.5, 50.5, 42.5))
}

Function("win_zone:onStep") {
  SendTitle("", "<#8cb85a>You win!", 20, 0, 10)
  PlaySound("Pling", 1, 2)
}

// 🎬 Result:
// Standing on the gold block will continuously
// send a subtitle every tick.
```

> [!tip] Viewing Examples Live
>
> For a better experience, run `@e2` to view the example above on plot 60983

# Props

Props allow us to preserve **state** throughout the component's lifespan. In other words, props allow you to store data that is specific to each component instance.

In our previous example, we had a win zone that showed a title and played a sound in the `onStep` event. Since this event is triggered every tick when a player stands on
the location key of a component, we end up having a really annoying sound being played constantly. We can fix this by storing whether or not a player has stepped on this
`win_zone` component in the past. Let's take a look:

```cs
PlayerEvent(Join) {
  CallFunction(__init)
  CallFunction(createComponent, "win_zone", new Location(12.5, 50.5, 69.5))
  CallFunction(createComponent, "win_zone", new Location(5.5, 50.5, 69.5))
}

Function("win_zone:init") {
  CreateDict(Variable(playerDict, LINE))
  CreateList(Variable(props, LOCAL), Variable(playerDict, LINE))
  CallFunction(setComponentProps)
}

Function("win_zone:onStep") {
  // Get the dictionary from the props list; it is the first element:
  GetListValue(Variable(playerDict, LINE), Variable(props, LOCAL), 1)

  If(!DictHasKey(Variable(playerDict, LINE), "%default")) { // ! means NOT
    SendTitle("", "<#8cb85a>You win!", 20, 0, 10)
    PlaySound("Pling", 1, 2)
    SetDictValue(Variable(playerDict, LINE), "%default", 1) // Updating the dictionary will
                                                            // not change it in the list, so
                                                            // we need the code block below:
    SetListValue(Variable(props, LOCAL), 1, Variable(playerDict))
    CallFunction(setComponentProps)
  }
}

// 🎬 Result:
// The title and sound will be played only once per
// win zone for each player, since props are instance-specific.
```

> [!tip] Viewing Examples Live
>
> For a better experience, run `@e3` to view the example above on plot 60983

# Next Steps

You now understand the basics of the declarative library. For more advanced usage, check out the other pages via the explorer on the left! Maybe look into the [Key Change Pattern](key-change) next?
