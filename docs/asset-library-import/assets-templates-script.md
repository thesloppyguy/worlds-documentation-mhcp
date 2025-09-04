🎥 Video Tutorial Script – Using Asset Templates in Horizon Worlds (Street Light Example)
[Opening Scene]

Narrator (voiceover):
“Welcome creators! In this video, we’re going to learn how to use Asset Templates in Horizon Worlds.
We’ll build a practical example: street lights with switches that you can toggle on and off. Let’s get started.”

[Section 1 – What are Asset Templates?]

On Screen: Show an existing world with grouped objects.
Narrator:
“Asset Templates are like Unity Prefabs—but more powerful. They let you create reusable objects that can be updated once and instantly refreshed across all your worlds.

Instead of manually updating every copy of an object, you just update the template, publish it, and all the instances get the changes. This saves hours of repetitive work.”

[Section 2 – When to Use Them]

On Screen: Show a scene with multiple street lights.
Narrator:
“Use Asset Templates when you need objects that repeat, like furniture, doors, or, in our case, street lights.

If you decide to make the lamp brighter later, you don’t want to update 100 lights one by one. With templates, you just update once, and it propagates everywhere.”

[Section 3 – Creating the Street Light Template]

On Screen Actions:

Add a cylinder (pole) and sphere (lamp head).

Select both, right-click → Create Asset → choose Template.

Narrator:
“Let’s create our street light. Add a cylinder for the pole, and a sphere for the lamp. Select them both, right-click, and choose Create Asset → Template.

Now we have our Street Light Template ready.”

[Section 4 – Adding a Switch and Script]

On Screen Actions:

Add a small cube → make it the switch.

Add it as a child of the street light.

Open the property panel → attach script.

Narrator:
“Next, let’s add a switch to control the light. We’ll use a cube for the switch, then attach a script that toggles the lamp on and off.”

On Screen: Show script editor with code.

-- StreetLightToggle Script
lightOn = true

function onInteract(player)
lightOn = not lightOn
self.parent.sphere.light.enabled = lightOn
end

Narrator:
“This script flips the light state each time a player interacts with the switch.”

[Section 5 – Publishing the Template]

On Screen Actions:

Edit Template Definition → Save → Publish.

Narrator:
“Once we’re happy, we publish the template. Now every instance of our Street Light—across all worlds—will have the switch and script included.”

[Section 6 – Using Overrides]

On Screen Actions:

Place several instances of the street light.

Change the lamp color on each one.

Narrator:
“What if you want variety? You can override properties like the light color per instance. Overrides stay even when you update the template later.”

[Section 7 – Summary & Call to Action]

On Screen: Show multiple lights, switches working, different colors.

Narrator:
“And that’s it! We built reusable street lights with switches using Asset Templates.

We covered:

What Asset Templates are and why they’re powerful.

How to create and publish a template.

How to attach and propagate scripts.

And how to customize instances with overrides.

Now it’s your turn—try making your own interactive objects with templates, and see how much time you save!”

Closing Screen Text:
“Thanks for watching! Don’t forget to like, share, and create something amazing in Horizon Worlds.”
