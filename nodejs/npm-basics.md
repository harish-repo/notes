# 'npm' the Node Package Manager
- NPM the package manager comes pre installed with node js
- Dependencies or packages that are used in the projects and come with a `package.json` file.
 
### package.json
 Tells
 - Who created
 - Version
 - Packages need to be installed

**Package**
Multiple files combined together that represents a specific function.

[npm offical website](https://www.npmjs.com/)
 
 ### Downloading and installing NPM
 
Go to [NodeJS GitHub repository](https://github.com/nodesource/distributions) to get latest version.

```
# Using Ubuntu
curl -sL https://deb.nodesource.com/setup_10.x | sudo -E bash -
sudo apt-get install -y nodejs
```

### Initializing a package.json file
```
npm init -y
```

### Adding node packages

#### Locally
Installed in the project directory, available only to the project in which it is installed
```
npm i express
```
Installing dev dependency: (Will not be included in production build)
```
npm i -D @babel/core @babel/node @babel/preset-env
```
#### Globally
Installed in the system available to all projects
```
npm i -g nodemon
```

## Global Directories

Linux:
`/usr/local/lib/node_modules`
 OR
`/usr/local/lib/node`

Windows:
`%AppData%\npm\node_modules`

## Installing specific version

```
npm i -g eslint@5.2.0
```
Check oudated dependencies

`Locally`

```
npm outdated
```

`Globally`
```
npm outdated -g
```

## Update a package
`Locally`
```
npm i eslint
```
`Globally`
```
npm i -g eslint
```
## Remove a package

`Locally`
```
npm un eslint
```
`Globally`
```
npm un -g eslint
```
## Semantic Versioning
"dependency":{
"express":"^4.16.3"
}

> 4 => Major Release: Full new vwrsion of a software
16 => Minor Release: Adding new functions to the major release
3 => Patch Release: Bug Fixes
^ => Any recent version 4.x.x
~ => Only update to Patch release 4.16.x

## Package-lock.json

Suposse we got a project from another developer without package-lock.json.
`package.json`
> "dependency": {
"express":"^4.16.3"
}

**Without package-lock.json:**

When we install the dependencies, because of '^' it will install the latest version due to the caret symbol i.e 4.25.3

The project may not work sometimes.

**With package-lock.json:** 

package-lock.json file will guarentee everytime we do an 'npm install' whoever we pass this project to, it will install 4.16.3


package.json is a great way to control the versioning inside the project.

## npm cache

npm keeps a cache of installed modules, so that it does not have to get them eveytime.  Sometime this doe not work properly.



Verify the cache:

```
npm cache verify
```

Force clear the cache
```
npm cache clean --force
```

## npm audit
Check the dependencies of the project and make sure they are safe to use.
Whenever you install a new package, the command npm audit runs automatically and tells if there are any issues with the package.

> Check more details about the vulnerabilities

```
npm audit
```

> Td fix vulnerabilities

```
npm audit fix
```

## Scripting in package.json

Default available scripts : [npm scripts](https://docs.npmjs.com/misc/scripts)

### Running default scripts:

> "scripts":{
"start": "nodemon ./index.js --exec babel-node -e js"
}

```
npm start
```

### Running custom scripts
> "scripts":{
"nodeThis": "nodemon ./index.js --exec babel-node -e js"
}

```
npm run nodeThis
```
> **Note**:
> Using 'run' with npm when running custom scripts.

## Introduction to npx:

Cli tool to create new project

Install temporarily the angular-cli with the package(-p)  and then use the command  from that package  to create a new app.
```
npx -p @angular/cli ng new myapp
```

The package will not be installed globally in the system directory.

## Alternative to npm
- yarn 
- ni
