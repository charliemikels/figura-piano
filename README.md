

# Figura Piano
A working piano that's a Figura player head!

## How to Use
You can spawn it in the world with this command. Simply copy and paste it and run it in game:

`/give @p minecraft:player_head{SkullOwner:{Id:[I;-1808656131,1539063829,-1082155612,-209998759]}}`

This avatar must be trusted for it to work. To do so, go to Figura > Permissions, click 'show disconnected avatars', and change 'Piano' to Max.
Once in the world, simply punch the notes, or right-click them with a shield to play!
Additionally, if you place a gold block 2 blocks under the piano, it will swap to a different texture ^^

## Piano Library
Instead of punching notes manually, you can manually trigger note plays though your script. If you ping this, everyone will be able to hear your note play. This can be used to automate playing songs, or use custom inputs like with your keyboard (or a midi keyboard??). You'll need to script this yourself though. To access the piano library, first create a variable based on the avatar variable.
```lua
piano_lib  =  world.avatarVars()["943218fd-5bbc-4015-bf7f-9da4f37bac59"]
```
(note, if you're using a 'ChloeSpacedIn' piano, instead use the UUID `b0e11a12-eada-4f28-bb70-eb8903219fe5`)
Once this is created, you'll be able to access the following functions:
### playNote()
```lua
piano_lib.playNote(pianoID, keyID, doesPlaySound, notePos, noteVolume)
```
The `playNote()` function just plays a note on the piano when run. It contains the following:
- `pianoID` is a string containing the ID of the selected piano. E.g. `"{1, 65, -102}"`. The ID is determined by the player head coordinates. To easily grab the ID, run `tostring(pos)` where `pos` is a vec3 of the selected piano head position.
- `keyID` is a string containing the ID of the note that should play. E.g. `"C2"`,`"F#3"`,`"A0"` This is just standard notation formatting of note as a letter, followed by octave as a number.
- `doesPlaySound` is a boolean which determines if a sound will play when the note is pressed. This exists to make the implementation for holding notes simple. Just keep this as `true`.
- `notePos` is a vec3 containing the world coordinates the note should play at. If left empty, it will just play at. You can simply ignore this and it will play at the player head coordinates. This is rarely useful, but if you want you can use the piano as a piano sample library (assuming you have it loaded), and play piano sounds anywhere in the world.
- `noteVolume` is a number that sets the volume of the sound. If left empty, it defaults to 2. Note that volumes higher than 1 don't increase the loudness of the sound, but instead multiplies the audable radious of the sound. (1 = 16 blocks, 2 = 32 blocks, etc.)

<details><summary>Click to see more functions included in the API.</summary>

### playSound()
```lua
piano_lib.playSound(keyID, notePos, noteVolume)
```
`playSound()` allows you to play the sounds of the piano from any position without requiring a real piano. 

Paramiters are the same as in `playNote()`, but without `pianoID` and `doesPlaySound`

### validPos()
```lua
local posIsValid = piano_lib.validPos(pianoID)
```
`validPos()` lets you test a pianoID to see if it is a valid piano before you interact with it. 
- Returns a boolean
- `pianoID` is a string containing the ID of the selected piano. See `playNote()` for more info. 

### getPlayingKeys()
```lua
local playingKeys = piano_lib.getPlayingKeys(pianoID)
```
`getPlayingKeys()` gives you a table of keys that are currently being played on the given piano. 
- Returns a table indexed by `keyID` and storing the world time of when the key was pressed. (See `world.getTime()` in the Figura docs.)
- `pianoID` is a string containing the ID of the selected piano. See `playNote()` for more info. 

### getPianoIDs()
```lua
local pianoIDs = piano_lib.getPianoIDs()
```
`getPianoIDs()` returns the IDs of all known pianos in a list. The list is indexed by integers starting at 1 so is sutable for `for _,_ in ipairs()` loops, but they are not in any particular order.

### getPianoPositions()
```lua
local pianoPositions = piano_lib.getPianoPositions()
```
`getPianoPositions()` returns the positions of all known pianos in a list. The list is indexed by integers starting at 1 so is sutable for `for _,_ in ipairs()` loops, but they are not in any particular order. 

You will still need to convert a position back to an ID using `tostring()` if you want to use the piano at that position.

### getNearestPianoID()
```lua
local nearestPianoID, nearestPianoPosition = piano_lib.getNearestPianoID(testPosition)
```
`getNearestPianoID()` returns the ID (and position) of the piano nearest to `testPosition`. 
- Returns a string and a vec3 representing the nearest piano's `pianoID` and position.
- `testPosition` is a vec3

This function allows for handy shorthands like 

```lua
local nearestPianoID = piano_lib.getNearestPianoID(player:getPos())
piano_lib.playNote(nearestPianoID, "C4", true)
```

to quickly control the nearest piano, without checking all the pianos or all of the nearby blocks for a valid piano. 

Note that this function simply loops through all the known pianos and compares their reletive distances. This can be an expensive opperation in worlds with lots of pianos. Furthermore, pianos only become "known" once they've been seen/rendered by the viewer. There may be cases where you are physicaly near a piano that isn't returned by `getNearestPianoID()`. EG: The piano is behind a wall.


</details>


## Planned Features
Remind me to make these things please >w>
- Height customisation using a `setHeight()` function
- Muting pianos (locally only) by closing the lid (or with `mutePiano()`)
- Avatar addon which will prevent accidentally breaking blocks when swinging with first
- Avatar addon which does note press calculations internally and pings to prevent desync issues

## Credits
- Model by TechnoCatza
- Default texture by PierraNova
- Fancy texture by Toast
