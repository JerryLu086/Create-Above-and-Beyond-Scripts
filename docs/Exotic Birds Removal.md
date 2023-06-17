# Exotic Birds Removal

Many people don't like this mod in their pack mainly because it's too laggy.

If you feel so, feel free to do this.

## Mods folder

Simply remove the Exotic Birds' jar file from it.

## Server Scripts

### `recipes.js`

1. Remove this line at #112:

```javascript
event.get("forge:raw_chicken").add("exoticbirds:raw_birdmeat")
```

## FTB Quests

1. Find the `market.snbt` in `config/ftbquests/quests/chapters` and remove these lines at #754:

```
{
	title: "Adopt a Duck"
	icon: "minecraft:egg"
	x: -11.0d
	y: -2.0d
	subtitle: "5 Silver"
	description: ["Quack"]
	id: "6C00F2935F27C2AD"
	tasks: [
		{
			id: "491FF95EFE8DCB67"
			type: "item"
			icon: { id: "thermal:silver_coin", Count: 5b }
			item: "thermal:silver_coin"
			count: 5L
		}
		{
			id: "5F58BA2473FD6DDA"
			type: "item"
			item: "rubber_duck:rubber_duck_item"
		}
	]
	rewards: [
		{
			id: "500098FD4762A38F"
			type: "item"
			auto: "enabled"
			item: "exoticbirds:duck_spawn_egg"
		}
		{
			id: "79C8307ED8450347"
			type: "custom"
			title: "Repeatable"
			icon: "thermal:machine_cycle_augment"
			tags: ["reset"]
			auto: "no_toast"
		}
	]
}
```

So sad you have to go, duckie, but this is what happens when you weird birds make the performance drop.
(Added this line because I actually felt bad for them lol)