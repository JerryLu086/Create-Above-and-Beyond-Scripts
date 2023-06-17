# Crushed Cobalt Mod

A modification that allows cobalt to drop like other metals in the pack, 
since it's weird to see how it's not doing so.

## Server Scripts

### `recipes.js`

1. Replace the line `let native_metals = ['iron', 'zinc', 'lead', 'copper', 'nickel', 'gold']` at #37 with:

```javascript
let native_metals = ['iron', 'zinc', 'lead', 'copper', 'nickel', 'gold', 'cobalt']
```

2. Inject these lines after `event.get('forge:plates/zinc').add(KJ("zinc_sheet"))` at #155: 

```javascript
event.get('forge:dusts').add(KJ("zinc_dust")).add(KJ("cobalt_dust"))
event.get('forge:dusts/zinc').add(KJ("zinc_dust"))
event.get('forge:dusts/cobalt').add(KJ("cobalt_dust"))
event.get(CR('crushed_ores')).add(KJ("crushed_cobalt_ore"))
```

3. Remove these lines at #845:

```javascript
event.custom({
	"type": "tconstruct:ore_melting",
	"ingredient": {
		"tag": "forge:ores/cobalt"
	},
	"result": {
		"fluid": "tconstruct:molten_cobalt",
		"amount": 144
	},
	"temperature": 950,
	"time": 97,
	"byproducts": [
		{
			"fluid": "tconstruct:molten_iron",
			"amount": 48
		}
	]
})
```

4. Inject these lines after `event.recipes.createMilling(KJ("zinc_dust"), CR("zinc_ingot"))` at #1169:

```javascript
event.recipes.createMilling(KJ("cobalt_dust"), TC("cobalt_ingot"))
event.recipes.thermal.pulverizer(KJ("cobalt_dust"), TC('cobalt_ingot')).energy(2000)
```

5. Replace the line `let crushed = CR('crushed_' + name + '_ore')` at #1401 with:

```javascript
let crushed = (name.equals('cobalt') ? KJ('crushed_' + name + '_ore') : CR('crushed_' + name + '_ore'))
```

6. Replace the line `event.recipes.createMilling([Item.of(crushed, 1), stone], ore)` at $1407 with:

```javascript
event.recipes.createMilling([Item.of(crushed, 1), (name.equals('cobalt') ? netherrack : stone)], ore)
```

7. Add this line at #1475 after `dust_process('zinc', CR('zinc_ingot'), CR('zinc_nugget'), KJ('zinc_dust'), CR('zinc_ore'), TE('sulfur'), 'lead')`:

```javascript
dust_process('cobalt', TC('cobalt_ingot'), TC('cobalt_nugget'), KJ('cobalt_dust'), TC('cobalt_ore'), MC('iron_nugget'), 'iron')
```

### `loot.js`

1. Add this line at #356 after `event.addJson(TE('lead_ore'), metal_ores_drop_dust(TE('lead_ore'), CR('crushed_lead_ore')))`:
```javascript
event.addJson(TC('cobalt_ore'), metal_ores_drop_dust(TC('cobalt_ore'), KJ('crushed_cobalt_ore')))
```

## Startup Scripts

### `launch.js`

1. Inject these lines at #74:

```javascript
event.create('crushed_cobalt_ore').texture("kubejs:item/crushed_cobalt_ore").displayName('Crushed Cobalt Ore')
event.create('cobalt_dust').texture("kubejs:item/cobalt_dust").displayName('Cobalt Dust')
```

## Assets

### Textures

Simply download the `cobalt_dust.png` and `crushed_cobalt_ore.png`
from the repo path `kubejs/assets/kubejs/textures/item` 
and put them in the same folder of your installation, 
and you're good to go!