---
title: Characters configuration 
editable: false
---

<div align="center" style="background-color: #e5ecff">    <br/>    <div>DOC</div>    <h1>Characters configuration </h1>    <br/>  </div>

### Files Used:
📄 src/script.js

📄 assets/units/data.json


<br/>

All of our characters configuration is done in the `data.json` file.

<br/>

First we load the json from the assets library

<div style="background: #e5ecff; padding: 10px 10px 10px 10px; border-bottom: 1px solid #c1c7d0; border-radius: 4px;">    📄 src/script.js  </div>

```js
⬜ 1      // Import jQuery related stuff
⬜ 2      import * as $j from 'jquery';
⬜ 3      import 'jquery.transit';
🟩 4      import dataJson from '../assets/units/data.json';
⬜ 5      import Game from './game';
⬜ 6      import { Fullscreen } from './ui/fullscreen';
⬜ 7      
```
<br/>

Then we load each of the creatures 

<div style="background: #e5ecff; padding: 10px 10px 10px 10px; border-bottom: 1px solid #c1c7d0; border-radius: 4px;">    📄 src/script.js  </div>

```js
⬜ 31     // const password = "8484ndnso";
⬜ 32     // const session = Cli.authenticateEmail({ email: email, password: password, create: true, username: "boo" })
⬜ 33     // Load the abilities
🟩 34     dataJson.forEach(async (creature) => {
🟩 35     	if (!creature.playable) {
🟩 36     		return;
🟩 37     	}
🟩 38     
🟩 39     	import(`./abilities/${creature.name.split(' ').join('-')}`).then((generator) =>
🟩 40     		generator.default(G),
🟩 41     	);
🟩 42     });
⬜ 43     
```
<br/>

This is a json entry for one character it include everything we need to render and play a character in the game
It is highly recommended to play with these values and see how they affect the character 

<div style="background: #e5ecff; padding: 10px 10px 10px 10px; border-bottom: 1px solid #c1c7d0; border-radius: 4px;">    📄 assets/units/data.json  </div>

```json
⬜ 1      [
🟩 2      	{
🟩 3      		"id": 0,
🟩 4      		"name": "Dark Priest",
🟩 5      		"playable": true,
🟩 6      		"level": "-",
🟩 7      		"realm": "-",
🟩 8      		"size": 1,
🟩 9      		"stats": {
🟩 10     			"health": 100,
🟩 11     			"regrowth": 1,
🟩 12     			"endurance": 60,
🟩 13     			"energy": 100,
🟩 14     			"meditation": 25,
🟩 15     			"initiative": 50,
🟩 16     			"offense": 3,
🟩 17     			"defense": 3,
🟩 18     			"movement": 2,
🟩 19     			"pierce": 2,
🟩 20     			"slash": 2,
🟩 21     			"crush": 2,
🟩 22     			"shock": 2,
🟩 23     			"burn": 2,
🟩 24     			"frost": 2,
🟩 25     			"poison": 2,
🟩 26     			"sonic": 2,
🟩 27     			"mental": 30
🟩 28     		},
🟩 29     		"animation": {
🟩 30     			"walk_speed": 500
🟩 31     		},
🟩 32     		"display": {
🟩 33     			"width": 108,
🟩 34     			"height": 200,
🟩 35     			"offset-x": 0,
🟩 36     			"offset-y": -164
🟩 37     		},
🟩 38     		"ability_info": [
🟩 39     			{
🟩 40     				"title": "Plasma Field",
🟩 41     				"desc": "Protects from most harmful abilities when unit not being currently active.",
🟩 42     				"info": "-1 plasma for each countered attack.",
🟩 43     				"upgrade": "9 pure damage counter hit.",
🟩 44     				"costs": {
🟩 45     					"plasma": 1,
🟩 46     					"special": "per countered attack"
🟩 47     				},
🟩 48     				"requirements": {
🟩 49     					"plasma": 1
🟩 50     				}
🟩 51     			},
🟩 52     			{
🟩 53     				"title": "Electro Shocker",
🟩 54     				"desc": "Does shock damage to a nearby unit. More effective versus larger enemies.",
🟩 55     				"info": "12 shock damage × unit hexagon size.",
🟩 56     				"upgrade": "The range is increased to 4.",
🟩 57     				"damages": {
🟩 58     					"shock": "12 × creature size"
🟩 59     				},
🟩 60     				"costs": {
🟩 61     					"energy": 20
🟩 62     				}
🟩 63     			},
🟩 64     			{
🟩 65     				"title": "Disruptor Beam",
🟩 66     				"desc": "Does pure damage to a nearby unit, based on its missing health points.",
🟩 67     				"info": "Plasma cost is equal to the unit size.",
🟩 68     				"upgrade": "Minimum 40 pure damage.",
🟩 69     				"requirements": {
🟩 70     					"plasma": 2,
🟩 71     					"energy": 20
🟩 72     				},
🟩 73     				"damages": {
🟩 74     					"pure": "25 + missing health"
🟩 75     				},
🟩 76     				"costs": {
🟩 77     					"energy": 20,
🟩 78     					"plasma": "target unit size"
🟩 79     				}
🟩 80     			},
🟩 81     			{
🟩 82     				"title": "Godlet Printer",
🟩 83     				"desc": "Materializes a unit within 4 hexagons that will serve and obey given orders.",
🟩 84     				"info": "Plasma cost equals unit's size + level.",
🟩 85     				"upgrade": "Range becomes 6 hexagons.",
🟩 86     				"costs": {
🟩 87     					"energy": 20,
🟩 88     					"plasma": "unit size + level"
🟩 89     				}
🟩 90     			}
🟩 91     		]
🟩 92     	},
⬜ 93     	{
⬜ 94     		"id": 1,
⬜ 95     		"name": "Bounty Hunter",
```
<br/>



<br/>

<br/><br/>

This file was generated by Swimm. [Click here to view it in the app](https://swimm.io/link?l=c3dpbW0lM0ElMkYlMkZyZXBvcyUyRm5xMjh5MjNzcTBpYnB4ZG4xSkpUJTJGZG9jcyUyRm9NdUM1T05OVTd3b1o3WGlkQTJk). Timestamp: 2021-04-22T07:38:25.441Z
