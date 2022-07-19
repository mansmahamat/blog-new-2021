---
template: blog-post
title: Git Hooks, robust commit with Husky, Prettier and ESLint
slug: /git-hooks-husky
date: 2022-07-19
description: Let your Git hooks run your tests and lint your code automatically
featuredImage: ../posts/img/husky.jpg
---

Have you ever had a moment when your code turned to be hard to read because of the style inconsistency like semicolon, string declaration with a mix of single-quote and double-quote, or bad indentation? 

Creating code is easy, but creating great code is not.
We can stop poor code from being pushed in our repository through linting and formatting.

Git hooks are scripts that Git executes before or after events such as commits, pushes... It’s a really cool and quick way to validate your code.

<div className="Image__Small">
  <img
    src="https://images.unsplash.com/photo-1532185825713-1e3ecd119b8f?ixlib=rb-1.2.1&ixid=MnwxMjA3fDB8MHxwaG90by1wYWdlfHx8fGVufDB8fHx8&auto=format&fit=crop&w=1470&q=80"
    alt="row"
  />
</div>

You can find the github repo  <a target="_blank" href="https://github.com/mansmahamat/commit-hoosks-husky">Here</a>

### What is a hook?
 A hook is simply a script that runs automatically when a particular event occurs in a Git repository. Here we will use `pre-commit`.

 - pre-commit: This hook is triggered first even before entering the commit message;

 There are many others I let you look at the documentation of Husky 

 ### Why use ESLint and Prettier

 When building apps, it's important to have a good setup of automated and manual tools that ensures the best standards and code quality. Each project must have a linting tool to fulfill these needs


### Tools

- 🦮 Husky is a library to facilitate the creation and sharing of hooks within a project.
- 🎨 Prettier : Keeps code formatting consistent based on our own preferences.
- ✅ ESLint is a tool for identifying and reporting on patterns found in JavaScript code, with the goal of making code more consistent and avoiding bugs
- 🚧 Lint-Staged : Lints code before a commit occurs to keep production code clean.

### Getting Started

Let’s start with the React app but you can use another technologies like Vue JS, Angular... 

We will create a React application by the simpliest way with a single command:

 ```javascript
npx create-react-app test-husky
```

Now you should have the React application, you can run `npm run start` from your terminal and navigate to http://localhost:3000.


### Adding ESLint and Prettier

ESLint is already installed in 'create-react-app` by default, but we will create custom configuration files for both ESLint and Prettier.

Let's install Prettier and eslint-config-prettier and make our config files in root project directory. 

 ```javascript
npm install --save-dev --save-exact prettier eslint-config-prettier
```

Create an ESLint config, select JSON format

 ```javascript
npm init @eslint/config 
```

Add this config in your `.eslintrc.json ` file : 

 ```javascript
.eslintrc.json
{
    "env": {
        "browser": true,
        "es2021": true,
        "jest": true
    },
    "extends": ["eslint:recommended", "plugin:react/recommended"],
    "parserOptions": {
        "ecmaFeatures": {
            "jsx": true
        },
        "ecmaVersion": "latest",
        "sourceType": "module"
    },
    "plugins": ["react"],
    "rules": {
        "indent": ["warn", "tab"],
        "quotes": ["error", "single"],
        "semi": ["error", "always"]
    }
}
```


A lot more errors pop in the code. 
That is ESLint enforcing our selected code style based in the config file. 

Before we fix these errors, lets create the Prettier config in root project.

 ```javascript
touch .prettierrc.json
```

Add this config in your `.prettierrc.json ` file : 

 ```javascript
.prettierrc.json
{
    "tabWidth": 2,
    "useTabs": true,
    "printWidth": 80,
    "semi": true,
    "trailingComma": "es5",
    "jsxSingleQuote": true,
    "singleQuote": true
}
```

Update `eslintrc.json` to include `prettier`:

 ```javascript
.eslintrc.json
...,
"extends": [
    "extends": ["eslint:recommended", "plugin:react/recommended", "prettier"],
],
...,
```

### Setting Up Husky

So, now if there are some problems with the code, we know how to check them. However, we are sometimes too busy to fix it or we just miss it.

In order to force people to fix the code before commit it, we can use Husky. We need Husky to run a command before git commit runs. In this case, we use Husky to run ESLint and Prettier. 

If a problem is found, Husky will stop the process and commit will fail. If no problem is found, git commit will run.

Install `Husky`:

 ```javascript
npm install --save —dev husky
```

Initialize our pre-commit hooks run:

 ```javascript
npx husky-init
```

This command will freshly add Husky to our project in the .husky folder. 

Inside this folder, we can create files that match the git hooks we want to use.

Let's install lint-staged:

 ```javascript
npm i --save-dev lint-staged
```

Now go to package.json and write the following script pre-commit which runs the lint-staged in our project.

 ```javascript
package.json
  "scripts": {
    ...
    "pre-commit": "lint-staged",
    "prepare": "husky install"
  },
```

Now create file named `.lintstagedrc` on our root directory and let us write configuration what we want the lint-staged to do before commits.

 ```javascript
.lintstagedrc
 {
  "src/**/*.+(js|json|ts|tsx)": ["eslint"],
  "src/**/*.{js,jsx,ts,tsx,json,css,scss,md}": ["prettier --write"]
}
```

Inside .husky/pre-commit add the following script :

 ```javascript
.husky/pre-commit
#!/bin/sh
. "$(dirname "$0")/_/husky.sh"

npm run pre-commit
```

### Testing Our Setup

I have this React file with a few errors inside :
<div className="Image__Small">
  <img
    src="../posts/img/error-husky.png"
    alt="School"
  />
   
</div>


I'll try to commit it, with the errors, let's see what happens.

 ```javascript
git add.
git commit -m 'test commit with husky'
```

<div className="Image__Small">
  <img
    src="../posts/img/error-console-husky.png"
    alt="School"
  />
   
</div>

I can't commit it, I have to fix each error before, so let's fix that.


<div className="Image__Small">
  <img
    src="../posts/img/final-husky.png"
    alt="School"
  />
   
</div>

No more errors now, let's try to commit our code again.

 ```javascript
git add.
git commit -m 'test commit with husky'
```

<div className="Image__Small">
  <img
    src="../posts/img/success-commit.png"
    alt="School"
  />
   
</div>

Success! 🥳 We have just committed clean code to our repository.



Your program may not bug-less, but if it is consistent and pretty, it would be easier to debug and maintain it. Those tools are only meant to reduce chance for potential problems to arise. At the end of the day, you and/or your team are the one in charge to make sure your code is easy to read.



You can find the github repo  <a target="_blank" href="https://github.com/mansmahamat/commit-hoosks-husky">Here</a>