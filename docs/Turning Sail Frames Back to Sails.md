# Turning Sail Frames Back to Sails

Just found it annoying to get the Sail Frames from the ruins 
and being unable to turn them back, just made this to fix it.

## Server Scripts

### `recipe.js`

The only thing to do is inject the line below 
into the function `trickierWindmills` at around #1250 anywhere under the two recipe removal lines 
(`event.remove({ output: 'create:sail_frame' })` and `event.remove({ output: 'create:white_sail' })`), 
then you're good to go!

```javascript
event.shapeless('create:white_sail', ['create:sail_frame', '#appliedenergistics2:wool'])
```