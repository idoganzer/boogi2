---
title: The player class
editable: false
---

<div align="center" style="background-color: #e5ecff">    <br/>    <div>DOC</div>    <h1>The player class</h1>    <br/>  </div>

### Files Used:
📄 src/player.js


<br/>

In Ancient Beast we support up to 4 players.
The player class represents each one of them, their avatar, score and more

![Dark Priest blue.png](https://firebasestorage.googleapis.com/v0/b/swimmio-content/o/repositories%2Fnq28y23sq0ibpxdn1JJT%2Fimg%2F7602c18c-6edd-475f-b75d-feaccbc9ddcd.png?alt=media&token=25744f0d-dccb-4f17-8b02-b51641b360dc)![Dark Priest gold.png](https://firebasestorage.googleapis.com/v0/b/swimmio-content/o/repositories%2Fnq28y23sq0ibpxdn1JJT%2Fimg%2Fc0d1d94b-3323-47fd-bf26-bd5161b2f92d.png?alt=media&token=3a434885-c9b3-4d1f-9937-906667d6df10)![Dark Priest green.png](https://firebasestorage.googleapis.com/v0/b/swimmio-content/o/repositories%2Fnq28y23sq0ibpxdn1JJT%2Fimg%2F4e9f5531-58a9-46ca-a2b0-7aacb7ef9e5b.png?alt=media&token=7a33d881-0467-4cbb-8804-957907966e1b)![Dark Priest red.png](https://firebasestorage.googleapis.com/v0/b/swimmio-content/o/repositories%2Fnq28y23sq0ibpxdn1JJT%2Fimg%2F28ce4e07-e5f1-4c91-a46d-b632cfa6744f.png?alt=media&token=1c7d5ff6-78bb-409a-80bd-434c84e1443c)

<br/>

Each player has it's own creatures

<div style="background: #e5ecff; padding: 10px 10px 10px 10px; border-bottom: 1px solid #c1c7d0; border-radius: 4px;">    📄 src/player.js  </div>

```js
⬜ 53     	}
⬜ 54     
⬜ 55     	// TODO: Is this even right? it should be off by 1 based on this code...
🟩 56     	getNbrOfCreatures() {
⬜ 57     		let nbr = -1,
⬜ 58     			creatures = this.creatures,
⬜ 59     			count = creatures.length,
```
<br/>

The player can summon creatures by creating a new Creature instance and adding it to it's creatures array

<div style="background: #e5ecff; padding: 10px 10px 10px 10px; border-bottom: 1px solid #c1c7d0; border-radius: 4px;">    📄 src/player.js  </div>

```js
⬜ 93     			}
⬜ 94     		}
⬜ 95     
🟩 96     		creature = new Creature(data, game);
🟩 97     		this.creatures.push(creature);
🟩 98     		creature.summon();
🟩 99     		game.onCreatureSummon(creature);
⬜ 100    	}
⬜ 101    
⬜ 102    	/* flee()
```
<br/>

It is interesting to see how the score is being calculated

<div style="background: #e5ecff; padding: 10px 10px 10px 10px; border-bottom: 1px solid #c1c7d0; border-radius: 4px;">    📄 src/player.js  </div>

```js
⬜ 116    	 *
⬜ 117    	 * Return the total of the score events.
⬜ 118    	 */
🟩 119    	getScore() {
🟩 120    		let total = this.score.length,
🟩 121    			s = {},
🟩 122    			points,
🟩 123    			totalScore = {
🟩 124    				firstKill: 0,
🟩 125    				kill: 0,
🟩 126    				deny: 0,
🟩 127    				humiliation: 0,
🟩 128    				annihilation: 0,
🟩 129    				timebonus: 0,
🟩 130    				nofleeing: 0,
🟩 131    				creaturebonus: 0,
🟩 132    				darkpriestbonus: 0,
🟩 133    				immortal: 0,
🟩 134    				total: 0,
🟩 135    				pickupDrop: 0,
🟩 136    				upgrade: 0,
🟩 137    			};
⬜ 138    
⬜ 139    		for (let i = 0; i < total; i++) {
⬜ 140    			s = this.score[i];
```
<br/>

The player class is an important class to understand and it has some important relations to other classes in the project

<br/>

<br/><br/>

This file was generated by Swimm. [Click here to view it in the app](https://swimm.io/link?l=c3dpbW0lM0ElMkYlMkZyZXBvcyUyRm5xMjh5MjNzcTBpYnB4ZG4xSkpUJTJGZG9jcyUyRnNQdm5LdEtCdlJKQVp4NmxxQUtm). Timestamp: 2021-04-22T07:38:25.476Z