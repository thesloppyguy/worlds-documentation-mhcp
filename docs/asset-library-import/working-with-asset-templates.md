# Using Asset Templates in Horizon Worlds: Street Lights with Switches

Sahil (thesloppyguy)

September 4, 2025

---

## Introduction

This tutorial will walk you through **using Asset Templates in Horizon Worlds**, focusing on a practical example: **creating reusable street lights that can are toggled when a user in within a certain area**.

By the end, you’ll understand:

- How Asset Templates differ from Unity Prefabs.
- When and why to use templates.
- How to create, edit, and publish templates.
- How to attach and propagate script updates across worlds.
- How to manage and mentain version control for the asset template.

<!-- This example shows how templates save time and keep your worlds consistent—perfect for interactive objects like lights, doors, or NPCs. -->

### Prerequisites and Expectations

You should be familiar with:

- Horizon Worlds’ basic editor (adding objects, shapes, and scripts).
- How to work with File-Backed Scripts (FBS).
- General knowledge of prefabs in Unity (optional, but helpful for comparison).

---

## Topic One: What Makes Asset Templates Special?

Asset Templates are like **prefabs on steroids**:

- They bundle objects, scripts, and behaviors into one reusable asset.
- You can spawn as many copies as you want.
- When you update the template, changes propagate to **every instance** in **every world**.

### How They Differ from Unity Prefabs

| Feature            | Unity Prefabs                        | Horizon Asset Templates                  |
| ------------------ | ------------------------------------ | ---------------------------------------- |
| Scope              | Single project                       | All your worlds                          |
| Change Propagation | Updates instances in current project | Updates instances across all worlds      |
| Versioning         | No built-in version history          | Built-in version control and rollback    |
| Property Overrides | Limited per instance                 | Explicit overrides with push/revert flow |

### When to Use Templates

Use Asset Templates when:

- You want **reusable objects** across multiple worlds (streetlights, furniture, NPCs).
- You expect to **update objects later** and want changes to sync everywhere.
- You want **per-instance customization** (e.g., different light colors per streetlight).

⚡ **Street Light Example:**
Imagine a city scene with 100 street lights. If you later decide to make the lights brighter, you don’t want to update each one by hand. With templates, you just update once and publish.

---

## Topic Two: Walkthrough – Street Lights with Switches

Now let’s build an interactive street light using Asset Templates.

### Step 1 – Create the Base Asset

1. Creating the basic Street light
   1. Add a **Hexagon Cylinder** for the base of the Pole
   2. Add a **Cylinder** (pole) and a **Bulb** (lamp head).
   3. (optional) Add a **Torus** around the Bulb to make it unique and stickout.
   4. Add a Light Gizmo to the Bulb. (we are using dynamic lights for spot lights but you should use static for better performance)
2. Add an Empty Block for Trigger Controller for managing Light state.
3. Add a Toggle Area for users to interact with the light.
4. Group them into an **Asset Template** (right-click → Create Asset → choose Template).
5. Rename it “Street Light”.

### Step 2 – Add the Script

Here’s a simple **File-Backed Script (FBS)** for toggling the light:

```typescript
// Trigger Broadcast Script
```

```typescript
// Trigger Handler Script
```

### Step 3 - Attach Script and Test

1. Attach Trigger Broadcast Script to the Trigger and provide the Controller block as an argument.
2. Attach Handler Script to the Controller Block and provide the Light Gizmo as an argument.
3. Press P and the Test out the interaction and performance.

### Step 4 – Publish the Template and Spawn a Clone

1. Edit the template definition and description.
2. Save → Publish.
3. Go to your Templates and Find the asset.
4. Drag and Place the Asset and test the same.

### Step 5 – Property Overrides

Want different colored lights?

1. Place several instances of your Street Light.
2. Override the **light color property** on each one (blue dot means override).
3. Overrides stay even if you later update the script.

### Step 6 - Version Control

1. For every change you are prompted with notes asking if you want to publish the same.
2. It is up to you what changes you would like to push across all your instance and Worlds
3. Once you have selected the changes you can Confirm and Add Discription for the same.
4. Push and all the instance will be updated.
5. On another world if the assets are not upto date you will see a button on the top right to update the see the changes.
6. You can Also Edit the Template directly from the Edit Template Option.

---

## Topic Three: Future Steps and Exercise.

Now that you have the basics down Here is a sample exercise for you.

1. Create a world with A lot of Rock (you can use environment generator)
2. Create a Template for the Rocks and Try the following
   1. Change Color and size of the rocks from the main template.
   2. Override some of the rocks to get variation.
   3. Add Script to the Rock to play Sound in proximity of player (cickets or rocks falling with a low percentage chance)
   4. Publish some Changes and test if instances are updated.

---

## References

- [Meta Horizon Worlds Documentation – Asset Templates](https://www.oculus.com/horizon-worlds/)
- [Unity Prefabs Overview](https://docs.unity3d.com/Manual/Prefabs.html)
- [Technical Writing Guide](https://medium.com/shecodeafrica/a-guide-to-technical-writing-7efcd0e70166)

---
