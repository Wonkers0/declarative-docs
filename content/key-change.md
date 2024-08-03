---
title: Key Change Pattern
---

Oftentimes, a component will be comprised of a set of entities. For instance, you may want to create a component that represents a merchant villager in your world. To keep
everything declarative, you need to spawn the villager when the component is created, probably through the `init` event. Here arises a common problem: What should we pass
as the key to the component if the villager isn't spawned yet? We want to use events that are only available to components with a UUID key, but we don't have the UUID of
the villager until it has spawned.

The solution is to pass a location key as usual, and spawn the villager at that location. Then, you can change the key of the component from within the `init` event thread
using the `setComponentKey` function:

```cs
Function("merchant:init") {
  SpawnMob(SpawnEgg(Villager), Variable(loc, LOCAL))
  SetAI(None)
  CallFunction(setComponentKey, GameValue(UUID, LastEntity))
}
```

# Next Steps

Now that you understand this basic pattern, try looking into [Advanced Components](advanced-components) next!
