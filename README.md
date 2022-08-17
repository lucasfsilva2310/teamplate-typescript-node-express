# Simple Typescript Node/Express template

# Steps to create this template

## Initial Config

### Create a folder for your application

` mkdir <application_folder_name>`

### Basic Node.js setup

`npm init`

You also have the option to run with `npm init -y` to approve all default config.

### Installing Express and its typo

Just run `npm i express` and `npm i --save-dev @types/express`

### Adding Typescript as a dev dependency

`npm install typescript --save-dev`

By installing this dependency now we have access to the `tsc` command to compile all typescript files to javascript.

### Adding node types to help us with auto-completion

`npm install @types/node --save-dev`

By installing this type we're enabling auto-completion for file, path and process as well as other ones.

### Creating a tsconfig.json file

This is where we're going to config our typescript compiler options.
We're gonna run with some pre set options but remember that you can reconfigure all of that later in the tsconfig.json

`npx tsc --init --rootDir src --outDir build \ --esModuleInterop --resolveJsonModule --lib es6 \ --module commonjs --allowJs true --noImplicitAny true`

### Create the first and main typescript file

You can just create a folder called `src` and a file called `index.ts` for starters.

Before you finish your initial config, put a random code inside of this file for example `console.log('Hello World')` so your CLI or you next configs won't bother you for now.

### Adding nodemon to watch for changes and restart our app Automatically and ts-node to compile our typescript files to javascript

`npm install --save-dev ts-node nodemon`

Nodemon takes care of that for us so we don't need to always end and start again our app manually every time that we change something.

We can also create a basic nodemon config to show nodemon where he needs to look to restart.

Create a `nodemon.json` config file and add this inside of it:

`{ "watch": ["src"], "ext": ".ts,.js", "ignore": [], "exec": "ts-node ./src/index.ts" }`
Then, inside of your `package.json` just create a new script to start your application "dev-side" -> `"start:dev": "nodemon"`.

By running that new script we're essencially running `ts-node ./src/index.ts` in the background because of our nodemon.json config.

At the same time, by putting `ts-node` inside of the same command we're already compiling our typescript file to javascript inside of a `dist` folder.

### Cleaning and compilling our project for production

We can create a build script to do so, by installing rimraf.

`npm install --save-dev rimraf`

and then add this new script to your list:

`"build": "rimraf ./build && tsc",`

Now by running this script `rimraf` will remove the old build and then compile the new one to dist folder.

Now that we have the build command, we need the start command to start our app for production.

### Creating a script to start our app for production

First we need to compile the definitive version of our app, and then we need to show to the script which file it needs to look at and run. So we end up with this new script command:

`"start": "npm run build && node build/index.js"`

[Referral Link to basic config](https://khalilstemmler.com/blogs/typescript/node-starter-project/)

## Adding ESLint with typescript

### Installing basic dev dependencies for it

In here we're installing eslint itself, a parser and basic plugin

`npm install --save-dev eslint @typescript-eslint/parser @typescript-eslint/eslint-plugin`

### Creating a basic config file

Create a new falled in the root your project called `.eslintrc`.

Inside of that file, add this basic config with our newly libraries:

```
{
     "root": true,
     "parser": "@typescript-eslint/parser",
     "plugins": [
         "@typescript-eslint"
                 ],
     "extends": [
         "eslint:recommended",
         "plugin:@typescript-eslint/eslint-recommended",
         "plugin:@typescript-eslint/recommended"
                 ]
 }
```

### Ignoring files that we don't want eslint to check

Just create a file called `.eslintignore` and add them to it.
For now we're just going to ignore node_modules and the dist folder, because the dist is the our final compilled code already.

Then, inside of our package.json file we're going to create a new script to run this lint:

```
{
    "scripts": {
         ...
         "lint": "eslint . --ext .ts",
          }
}
```

So, by simply running `npm run lint`, we're going to check if our whole app is following our new rules.

To create new rules just read the eslint docs and add a new key to the `.eslintrc` file:

```
"rules": {
   // Your rules here
         }
```

### Adding new plugins/extensions (features)

To add new pluging just check eslint plugins/extensions doc. Have fun configuring any way you like :)

### Creating a new script to fix our code according to our eslint

Sometimes when you run your list script you're going to have all warning and errors showed to you. You can also create another script to run the lint and fix it at the same time:

```
{
    "scripts": {
         ...
         "lint-and-fix": "eslint . --ext .ts --fix"
         },
}
```

[Referral Link Eslint](https://khalilstemmler.com/blogs/typescript/eslint-for-typescript/)

## Config Prettier to help our code constantly

Eslint is amazing and it can help us alot, but its so annoying needing to run eslint everytime that we need to correct our code.

For that we can add Prettier to check all the time for when we're coding, and this fixed would instantly.

To install Prettier just run the command:

`npm install --save-dev prettier`

### Creating basic config file and basic configs

Just create a `.prettierrc`

Then add this basic configs:

```
{
    "semi": true,
    "trailingComma": "none",
    "singleQuote": true,
    "printWidth": 80
    }
```

To set new configs and rules just read the prettier docs.

To test if the prettier is running just type anything in the index.ts file and check if the basic configs will be applied.

### Using Prettier CLI

You can also create a new script command to format your code if you're not using the prettier extension in your VSCode. If you're using it your code you automatically be formatted when saved. If you don't have you can create a new script command.

```
{ "scripts":
        {
            ...
             "prettier-format": "prettier --config .prettierrc 'src/**/*.ts' --write"
             }
 }
```

### Config Prettier to work with ESLint

Install these two packages:

`npm install --save-dev eslint-config-prettier eslint-plugin-prettier`

Then add this new plugins and the prettier itself to eslint:

```
{
    "root": true,
     ...
     "plugins":
     [ ...
      "prettier"
      ],
      "extends":
       [
        ...
        "prettier"
        ],
        "rules": {
            ...
            "prettier/prettier": 2 // can be 0, 1 or 2
            }
}
```

[Referral Prettier Link](https://khalilstemmler.com/blogs/tooling/prettier/)

## Creating a basic pre commit / pre push with Husky

First of all, install husky dependency:

`npm install husky --save-dev`

Then, at you package.json file, add a new key called Husky with this config:

```
"husky": {
  "hooks": {
    "pre-commit": "",       // Command goes here
    "pre-push": "",         // Command goes here
    "...": "..."
  }
}
```

So now when we use a git commit or a git push command, it will execute these two before actually running the git command.

As an example you could run prettier and eslint before committing to make sure you updates are following all rules:

```
{
  "scripts": {
    "prettier-format": "prettier --config .prettierrc 'src/**/*.ts' --write",
    "lint": "eslint . --ext .ts",
    ...
  },
  "husky": {
    "hooks": {
      "pre-commit": "npm run prettier-format && npm run lint"
    }
  }
}
```

You can do another specific thing with pre-push as well. Feel free to use it anyway you like.

[Referral Husky Link](https://khalilstemmler.com/blogs/tooling/enforcing-husky-precommit-hooks/)
