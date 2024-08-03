---
title: Advanced Components
---

For more complex component relationships, the declarative offers a more extensive function, `createAdvancedComponent`. This allows you to specify 2 more parameters about the
behavior of the component. Let's take a look:

![](https://i.imgur.com/aqrJqGr.png)

This allows us to create a component that inherits the event implementations of another (or pass an empty string if you do not wish to inherit anything), as well as enable the `tick` event for specific component instances, which will fire every tick.

For instance, imagine that we had an `npc` component, which would spawn a villager with no AI and update the component key using the [Key Change Pattern](key-change). We could then create a `merchant` component that inherits the `npc` component and adds some inventory menu functionality:

```cs
PlayerEvent(Join) {
  CallFunction(__init)
  CallFunction(createComponent, "npc", new Location(25.5, 50.5, 69.5))
  // Create an unticked "merchant" component that inherits the "npc" component events:
  CallFunction(createComponent, "merchant", "npc", 0, new Location(22.5, 50.5, 69.5))
}

Function("npc:init") {
  SpawnMob(SpawnEgg(Villager), Variable(loc, LOCAL), "<#bf7e45>NPC")
  SetAI(None)
  CallFunction(setComponentKey, GameValue(UUID, LastEntity))
}

Function("merchant:init") {
  // Select the villager spawned in the npc:init event now that the key has changed
  SelectEntity(Variable(uuid, LOCAL))
  SetName("<#bf7e45>MERCHANT")
  SetVillagerBiome("Taiga")
}

Function("merchant:onClickEntity") {
  ShowInv(...)
  PlaySound("Wandering Trader Disappear", 1, 2)
}
```

> [!tip] Viewing Examples Live
>
> To view the results of the snippet above, run `@e4` to view the example above on plot 60983
