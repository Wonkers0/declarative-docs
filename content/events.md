---
title: Events List
---

This is a comprehensive list of all events called by the declarative library:

### Entity events

| DF Event        | Component Event | Deletes Component |
| --------------- | --------------- | ----------------- |
| EntityDmgEntity | onHurt          |                   |
| DamageEntity    | onHurt          |                   |
| EntityDeath     | onDeath         | Yes               |
| ClickEntity     | onClickEntity   |                   |
| KillMob         | onKilled        | Yes               |

### Block/Location events

| DF Event   | Component Event |
| ---------- | --------------- |
| RightClick | onRightClick    |
| LeftClick  | onLeftClick     |
| N/A        | onStep          |
