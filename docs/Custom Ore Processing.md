# Custom Ore Processing

If you want to add more ores to your pack 
while still following the ore processing scheme of the pack, 
you might want to make the original scripts easier to deal with. 
This script will show how to do that without breaking the existing ones!

Note that this is only acting as a template, 
you need to know what you want and what to do with it, 
so it won't be like a regular modification that just adds some specific things.

## Server Scripts

### `recipes.js`

1. Replace the whole function `dust_process` at #1401 with:

```javascript
    let generic_ore_process = (name, ingot_path, nugget_path, dust_path, ore_path, crushed_path, crushed_rock, byproduct, fluid_path, fluid_byproduct) => {
	    let ingot = ingot_path(name + '_ingot')
	    let nugget = nugget_path(name + '_nugget')
		let dust = dust_path(name + '_dust')
		let ore = ore_path(name + '_ore')
		let crushed = crushed_path('crushed_' + name + '_ore')
		let fluid = fluid_path('molten_' + name)

		event.smelting(Item.of(nugget, 3), crushed)
		event.smelting(Item.of(nugget, 1), dust).cookingTime(40)
		event.recipes.createMilling([Item.of(crushed, 1), crushed_rock], ore)
		event.recipes.createMilling([Item.of(dust, 3)], crushed)
		event.recipes.createCrushing([Item.of(dust, 3), Item.of(dust, 3).withChance(0.5)], crushed)
		event.recipes.thermal.pulverizer([Item.of(dust, 6)], crushed).energy(15000)
		event.recipes.thermal.pulverizer([crushed], ore).energy(3000)
		event.recipes.thermal.crucible(Fluid.of(fluid, 144), ingot).energy(2000)

		event.recipes.thermal.crucible(Fluid.of(fluid, 48), dust).energy(3000)
		event.recipes.createSplashing([Item.of(nugget, 2)], dust)
		event.recipes.createMixing([Fluid.of(fluid, 288)], [Item.of(dust, 3), AE2('matter_ball')]).superheated()

		event.remove({ input: "#forge:ores/" + name, type: TE("smelter") })
		event.remove({ input: "#forge:ores/" + name, type: TE("pulverizer") })
		event.remove({ input: "#forge:ores/" + name, type: MC("blasting") })
		event.remove({ input: "#forge:ores/" + name, type: MC("smelting") })
		event.remove({ input: "#forge:ores/" + name, type: CR("crushing") })
		event.remove({ input: "#forge:ores/" + name, type: CR("milling") })

		event.custom({
			"type": "thermal:smelter",
			"ingredient": {
				"item": crushed
			},
			"result": [
				{
					"item": nugget,
					"chance": 9.0
				},
				{
					"item": byproduct,
					"chance": (byproduct.endsWith('nugget') ? 1.8 : 0.2)
				},
				{
					"item": "thermal:rich_slag",
					"chance": 0.2
				}
			],
			"experience": 0.2,
			"energy": 20000
		})

		event.custom({
			"type": "tconstruct:melting",
			"ingredient": {
				"item": dust
			},
			"result": {
				"fluid": fluid,
				"amount": 48
			},
			"temperature": 500,
			"time": 30,
			"byproducts": [
				{
					"fluid": fluid_byproduct,
					"amount": 16
				}
			]
		});

	}
```

This can let you add the path of the objects needed for unifying the ore processing, 
such as ingot, nugget, dust, ore, etc. Yes, you can pass arrow functions or just regular ones 
as args (which I just found out yesterday xD).

2. Replace the invocations of the functions below it with:

```javascript
    generic_ore_process('nickel', TE, TE, TE, TE, CR, stone, CR('copper_nugget'), TC, TC('molten_copper'))
	generic_ore_process('lead', TE, TE, TE, TE, CR, stone, MC('iron_nugget'), TC, TC('molten_iron'))
	generic_ore_process('iron', MC, MC, TE, MC, CR, stone, TE('nickel_nugget'), TC, TC('molten_nickel'))
	generic_ore_process('gold', MC, MC, TE, MC, CR, stone, TE('cinnabar'), TC, TC('molten_zinc'))
	generic_ore_process('copper', CR, CR, TE, CR, CR, stone, MC('gold_nugget'), TC, TC('molten_gold'))
	generic_ore_process('zinc', CR, CR, KJ, CR, CR, stone, TE('sulfur'), TC, TC('molten_lead'))
```

If you have the cobalt modification enabled then just add the additional line: 

```javascript
	generic_ore_process('cobalt', TC, TC, KJ, TC, KJ, netherrack, MC('iron_nugget'), TC, TC('molten_iron'))
```

3. If your ore comes with a dust variant, 
you might need to add it into the `native_metals` list at #37.

## Adding a New Ore Type

When you get a new ore type, you might want to do this mod for it.
What you need to do is make sure it has all the things the processing requires, namely:

- Ingot
- Nugget
- Dust
- Ore
- Crushed Ore
- Crushed Byproduct 
(`crushed_rock`, the crushed item of the thing your ore is embedded into, 
like Cobblestone [Stone] for Iron, Netherrack for Cobalt)
- Pulverizer Byproduct (`byproduct`)
- Molten Fluid
- Byproduct of The Fluid

If some of them are absent, then you'll need to manually create them with KJS, 
which I won't show you here, 
since there should already be a ton of instructions if you google for them.

When all these things are all present, then you're good to go. 
The next thing is just pass in the name of your ore (e.g., `'iron'`, `'gold'`, `'copper'`), 
the paths of the other things, and all the other things in the parameters. 

And for the Pulverizer Byproduct, there're already some constants you can use, 
like `stone`, `netherrack` and some others.