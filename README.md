# Instant-Clip-Chef
## On Mac
### 1. Prepare CLI CyberChef on NodeJS
```
$ brew install node
$ npm install cyberchef-node
```
e.g. Save as /Users/test/Desktop/MyCyberChef/run-cyberchef.js  
```js
const fs = require("fs");
const chef = require("cyberchef-node");

const input = fs.readFileSync(0, "utf-8"); // stdin
const recipe = JSON.parse(fs.readFileSync(process.argv[2], "utf-8"));

const result = chef.bake(input, recipe);

// NodeDish → 文字列化して出力
process.stdout.write(result.toString());
```

### 2. Prepare CyberChef recipes on Local
e.g. /Users/test/Desktop/MyCyberChef/recipes/(recipe name).json

### 3. Set QuickAction with Automator
Woorkflow receives : "no input" in "any application"  
Input is : (gray out)  
Output replaces selected text : off  
Image : Action  
Color : (Anything is fine)  

Run Apple Script
```
-- Recipe List
set recipes to {"ToUpperCase.json", "UrlDecode.json", "Base64Encode.json"}

-- Selection Dialog
set chosenRecipe to choose from list recipes with prompt "Choose a CyberChef recipe:"
if chosenRecipe is false then return -- If canceled

-- Path of Selected Recipe
set recipeFile to "/Users/test/Desktop/" & item 1 of chosenRecipe

-- pbpaste -> CyberChef -> pbcopy
do shell script "pbpaste | /usr/local/bin/node /Users/test/Desktop/run-cyberchef.js " & quoted form of recipeFile & " | pbcopy"
```

Save: Cmd + s

### 4. Set Keyboard Shortcut on System Settings
System Settings > Keyboard > Keyboard Shortcuts ... > Services > General > (your quick action name)  

e.g. ctl + shift + h  

When invoking via the shortcut key, the app will request accessibility access, so please allow it each time.　　
