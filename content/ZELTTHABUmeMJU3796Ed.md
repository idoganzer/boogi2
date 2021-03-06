---
title: What are abilities
editable: false
---

<div align="center" style="background-color: #e5ecff">    <br/>    <div>DOC</div>    <h1>What are abilities</h1>    <br/>  </div>

### Files Used:
📄 src/ability.js

📄 src/creature.js


<br/>

Exploring the abilities folder

<br/>

Let's first explore the ability class

<div style="background: #e5ecff; padding: 10px 10px 10px 10px; border-bottom: 1px solid #c1c7d0; border-radius: 4px;">    📄 src/ability.js  </div>

```js
⬜ 5      import { isTeam, Team } from './utility/team';
⬜ 6      import * as arrayUtils from './utility/arrayUtils';
⬜ 7      
🟩 8      /**
🟩 9       * Ability Class
🟩 10      *
🟩 11      * Class parsing function from creature abilities
🟩 12      */
🟩 13     export class Ability {
🟩 14     	constructor(creature, abilityID, game) {
🟩 15     		this.creature = creature;
🟩 16     		this.game = game;
🟩 17     		this.used = false;
🟩 18     		this.id = abilityID;
🟩 19     		this.priority = 0; // Priority for same trigger
🟩 20     		this.timesUsed = 0;
🟩 21     		this.timesUsedThisTurn = 0;
🟩 22     		this.token = 0;
🟩 23     		this.upgraded = false;
🟩 24     
🟩 25     		let data = game.retrieveCreatureStats(creature.type);
🟩 26     		$j.extend(true, this, game.abilities[data.id][abilityID], data.ability_info[abilityID]);
🟩 27     		if (this.requirements === undefined && this.costs !== undefined) {
🟩 28     			this.requirements = this.costs;
🟩 29     		}
🟩 30     	}
⬜ 31     
⬜ 32     	hasUpgrade() {
⬜ 33     		return this.game.abilityUpgrades >= 0 && this.upgrade;
```
<br/>

So the creature class imports Ability

<div style="background: #e5ecff; padding: 10px 10px 10px 10px; border-bottom: 1px solid #c1c7d0; border-radius: 4px;">    📄 src/creature.js  </div>

```js
⬜ 1      import * as $j from 'jquery';
🟩 2      import { Ability } from './ability';
⬜ 3      import { search } from './utility/pathfinding';
⬜ 4      import { Hex } from './utility/hex';
⬜ 5      import * as arrayUtils from './utility/arrayUtils';
```
<br/>

and uses it to instanciate the creature's abilities
notice that we pass the game instance to the Ctor

<div style="background: #e5ecff; padding: 10px 10px 10px 10px; border-bottom: 1px solid #c1c7d0; border-radius: 4px;">    📄 src/creature.js  </div>

```js
⬜ 122    		this.remainingMove = 0; //Default value recovered each turn
⬜ 123    		this.dizzy = false;
⬜ 124    
🟩 125    		// Abilities
🟩 126    		this.abilities = [
🟩 127    			new Ability(this, 0, game),
🟩 128    			new Ability(this, 1, game),
🟩 129    			new Ability(this, 2, game),
🟩 130    			new Ability(this, 3, game),
🟩 131    		];
⬜ 132    
⬜ 133    		this.updateHex();
⬜ 134    
```
<br/>



<br/>

<br/><br/>

This file was generated by Swimm. [Click here to view it in the app](https://swimm.io/link?l=c3dpbW0lM0ElMkYlMkZyZXBvcyUyRm5xMjh5MjNzcTBpYnB4ZG4xSkpUJTJGZG9jcyUyRlpFTFRUSEFCVW1lTUpVMzc5NkVk). Timestamp: 2021-04-22T07:38:25.421Z
