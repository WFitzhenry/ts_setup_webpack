# Setting up basic typescript project

### Basic project setup

- add .gitignore
- add src/index.ts

### Saves ts a dev dependency as it's needed for development

- npm i typescript --save-dev

### Initialise the project

- npx tsc -init

The command above uses npx (a tool that allows you to execute any JavaScript package available on the NPM registry without even installing it) to run the command that initializes a TypeScript project.
Running this command creates a new tsconfig.json file, which defines how TypeScript and the tsc compiler should interact (what JavaScript version to produce, what files to compile, what directory to compile files to, amongst others)

### tsconfig.json

See https://www.totaltypescript.com/tsconfig-cheat-sheet for details.

```
{
  "compilerOptions": {
    /* Base Options: */
    "esModuleInterop": true,
    "skipLibCheck": true,
    "target": "es2022",
    "allowJs": true,
    "resolveJsonModule": true,
    "moduleDetection": "force",
    "isolatedModules": true,
    /* Strictness */
    "strict": true,
    "noUncheckedIndexedAccess": true,
    "noImplicitOverride": true,
    /* If transpiling with TypeScript: */
    "module": "NodeNext",
    "outDir": "dist",
    /* If your code runs in the DOM: */
    "lib": ["es2022", "dom"],
  },
  "include": ["src"]
}
```

### Run the compiler

- npx tsc

This compiles the typescript files into javascript and saves them in the `dist` folder

- npx tsc -w

This runs the compiler in watch mode to we don't have to run it all the time; it will compile when there are changes.

### Some basic scripts

In package.json:

```
{
  "scripts": {
    "build": "tsc",  // runs the typescript compiler
    "dev": "tsc --w" // runs the compiler in watch mode
  },
  "devDependencies": {
    "typescript": "^5.7.2"
  }
}
```

### VSCode and IntelliSense

[IntelliSense](https://code.visualstudio.com/docs/languages/typescript#_intellisense) from VSCode shows you intelligent code completion,
hover information, and signature help so that you can write code more quickly and correctly.

### Setting up ESlint with TypeScript

- npm install --save-dev eslint @eslint/js typescript-eslint

And add a `eslint.config.mjs` file in the top level of the directory. Populate it with:

```
import eslint from "@eslint/js";
import tseslint from "typescript-eslint";

export default tseslint.config({
    files: ['**/*.ts'],
    ignores: [".node_modules/*"],
    extends: [
      eslint.configs.recommended,
      tseslint.configs.recommended,
    ],
    rules: {
      '@typescript-eslint/array-type': 'error',
      '@typescript-eslint/consistent-type-imports': 'error',
    },
  });
```

We can add a script for this in package.json

```
"lint": "eslint --ext .ts ."
```

and run with: `npm run lint`

### Prettier

- npm install prettier --save-dev

The primary function of a linter is to improve your code by analyzing it and alerting you to any potential issues based on customizable or pre-defined rulesets. These rulesets, or rules, allow development teams to maintain a consistent coding style and identify potential bugs caused by inconsistent coding styles.

On the other hand, a code formatter like Prettier ensures a consistent style by parsing your code and re-printing it according to its rules. For example, you can specify a style that all JavaScript statements must end with a semicolon; the code formatter will automatically add the semicolon to all statements without one.

In essence, you can use ESLint to specify rulesets that must be adhered to and then use Prettier to fix cases in your code where these rulesets are broken.

Compared to ESLint, Prettier doesn’t need a config file, meaning you can run and use it straight away. However, if you want to set a config, you will need to create a file called .prettierrc.json in the project’s root directory, where you can define your format options.

You don't need a config file with prettier, but you can add extra format options in a `.prettierrc.json` file.

```
{
  "singleQuote": true,
  "trailingComma": "all",
  "printWidth": 100,
  "arrowParens": "avoid"
}
```

And you can also set a `.prettierignore` file to exlude files.

We can use:

- npx prettier --write src/index.ts

to apply prettier to files. But a much easier way to work is to use it whenever saving a file. To do this with VSCode. In `settings.json` (Command + Shift + P) set:

```
{
  "editor.defaultFormatter": "esbenp.prettier-vscode",
  "editor.formatOnSave": true,
}
```
