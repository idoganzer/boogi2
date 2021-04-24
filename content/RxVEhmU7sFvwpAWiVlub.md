---
title: How the game is being served
editable: false
---

<div align="center" style="background-color: #e5ecff">    <br/>    <div>DOC</div>    <h1>How the game is being served</h1>    <br/>  </div>

### Files Used:
📄 package.json

📄 server.js

📄 webpack.config.js

📄 src/index.ejs

📄 src/script.js


<br/>

In order to serve the game we use the express server.
Here are some links if you want to know more about [node](https://nodejs.org/en/) or [express](https://expressjs.com).
Know that we only use it to serve the file so nothing fancy...



<br/>

When you run the dev env with `npm run start` we call the server.js script

<div style="background: #e5ecff; padding: 10px 10px 10px 10px; border-bottom: 1px solid #c1c7d0; border-radius: 4px;">    📄 package.json  </div>

```json
⬜ 67     	"scripts": {
⬜ 68     		"build": "npm run assetLister && webpack --mode=production",
⬜ 69     		"build:dev": "npm run assetLister && webpack --mode=development",
🟩 70     		"start": "cross-env NODE_ENV=production node server.js",
⬜ 71     		"start:dev": "cross-env NODE_ENV=development webpack-dev-server",
⬜ 72     		"assetLister": "node ./assetLister.js",
⬜ 73     		"lint": "eslint src/*.js src/**/*.js",
```
<br/>

The server uses only 2 folders for serving, the most important one is the `deploy` folder where we keep our initial html

<div style="background: #e5ecff; padding: 10px 10px 10px 10px; border-bottom: 1px solid #c1c7d0; border-radius: 4px;">    📄 server.js  </div>

```js
⬜ 10     // Enable gzip compression
⬜ 11     app.use(compression());
⬜ 12     
🟩 13     // Assets are hashed so they can be in the cache for longer
🟩 14     app.use(
🟩 15     	'/assets/',
🟩 16     	express.static(path.join(__dirname, 'deploy', 'assets'), {
🟩 17     		maxAge: '1d',
🟩 18     	}),
🟩 19     );
🟩 20     app.use(express.static(path.join(__dirname, 'deploy')));
⬜ 21     
⬜ 22     // Listen for server, and use static routing for deploy directory
⬜ 23     app.listen(port, ip, () => {
```
<br/>

The main file we build from is `script.js`

<div style="background: #e5ecff; padding: 10px 10px 10px 10px; border-bottom: 1px solid #c1c7d0; border-radius: 4px;">    📄 webpack.config.js  </div>

```js
⬜ 14     	const production = (argv && argv.mode === 'production') || process.env.NODE_ENV === 'production';
⬜ 15     
⬜ 16     	return {
🟩 17     		entry: ['babel-polyfill', path.resolve(__dirname, 'src', 'script.js')],
⬜ 18     		output: {
⬜ 19     			path: path.resolve(__dirname, 'deploy'),
⬜ 20     			filename: 'ancientbeast.js',
```
<br/>

The result of the build is our `ancientbeast.js` file which is being built by Webpack

<div style="background: #e5ecff; padding: 10px 10px 10px 10px; border-bottom: 1px solid #c1c7d0; border-radius: 4px;">    📄 webpack.config.js  </div>

```js
⬜ 15     
⬜ 16     	return {
⬜ 17     		entry: ['babel-polyfill', path.resolve(__dirname, 'src', 'script.js')],
🟩 18     		output: {
🟩 19     			path: path.resolve(__dirname, 'deploy'),
🟩 20     			filename: 'ancientbeast.js',
🟩 21     			// chunkFilename: '[id].chunk.js',
🟩 22     			// publicPath: process.env.PUBLIC_PATH ? process.env.PUBLIC_PATH : '/',
🟩 23     		},
⬜ 24     		devtool: production ? 'none' : 'source-map',
⬜ 25     		module: {
⬜ 26     			rules: [
```
<br/>

We later inject this js to `index.ejs` file

<div style="background: #e5ecff; padding: 10px 10px 10px 10px; border-bottom: 1px solid #c1c7d0; border-radius: 4px;">    📄 src/index.ejs  </div>

```ejs
⬜ 9      		<meta name="monetization" content="$pay.stronghold.co/1a15c4d9d46cdd64f9e9094a7c4f04240dc">
⬜ 10     		<meta name="theme-color" content="black">
⬜ 11     		<link rel="manifest" href="manifest.json">
🟩 12     		<title>Ancient Beast 0.4</title>
⬜ 13     		<style>
⬜ 14     			.scrim.loading {
⬜ 15     			opacity: 1;
```
<br/>

This is the webpack plugin we use for rendering the ejs file to HTML

<div style="background: #e5ecff; padding: 10px 10px 10px 10px; border-bottom: 1px solid #c1c7d0; border-radius: 4px;">    📄 webpack.config.js  </div>

```js
⬜ 76     				patterns: [{ from: 'static' }],
⬜ 77     			}),
⬜ 78     			new HtmlWebpackPlugin({
🟩 79     				template: path.resolve(__dirname, 'src', 'index.ejs'),
⬜ 80     				favicon: path.resolve(__dirname, 'assets', 'favicon.png'),
⬜ 81     				production,
⬜ 82     			}),
```
<br/>

And then the game loads the global variables the CSS and the HTML is mostly already rendered

<div style="background: #e5ecff; padding: 10px 10px 10px 10px; border-bottom: 1px solid #c1c7d0; border-radius: 4px;">    📄 src/script.js  </div>

```js
⬜ 12     // Load the stylesheet
⬜ 13     import './style/main.less';
⬜ 14     
🟩 15     // Generic object we can decorate with helper methods to simply dev and user experience.
🟩 16     // TODO: Expose this in a less hacky way.
🟩 17     let AB = {};
🟩 18     let session = {};
🟩 19     // Create the game
🟩 20     const G = new Game('0.4');
🟩 21     // Helper properties and methods for retrieving and playing back game logs.
🟩 22     // TODO: Expose these in a less hacky way too.
🟩 23     AB.currentGame = G;
🟩 24     AB.getLog = AB.currentGame.gamelog.get.bind(AB.currentGame.gamelog);
🟩 25     AB.restoreGame = AB.currentGame.gamelog.play.bind(AB.currentGame.gamelog);
🟩 26     window.AB = AB;
🟩 27     const connect = new Connect(G);
🟩 28     G.connect = connect;
🟩 29     
🟩 30     // const email = "junior@example.com";
🟩 31     // const password = "8484ndnso";
🟩 32     // const session = Cli.authenticateEmail({ email: email, password: password, create: true, username: "boo" })
🟩 33     // Load the abilities
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



<br/>

<br/><br/>

This file was generated by Swimm. [Click here to view it in the app](https://swimm.io/link?l=c3dpbW0lM0ElMkYlMkZyZXBvcyUyRm5xMjh5MjNzcTBpYnB4ZG4xSkpUJTJGZG9jcyUyRlJ4VkVobVU3c0Z2d3BBV2lWbHVi). Timestamp: 2021-04-22T07:38:25.403Z
