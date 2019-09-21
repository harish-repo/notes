# Node JS Essentials
NodeJS is a powerful tool for controlling servers, building web applications, and creating event-driven programs.

- Node JS is `single threaded`.
- Non blocking and event driven I/O.
- Events are raised and recorded in an event queue.
- Single thread that will respond to the events in the order that they are raised.
- The thread behaves asynchronously i.e it can do more than one thing at a time.
- The ability to multitask is what makes NodeJS fast.

## Node Globals

### The global object
Everything on the global object is available globally.
We can use values and methods available in global object without importing.

Some of the global objects are:
- __dirname
- __filename
- exports
- module
- require()
- console
- process

### Get full path of the directory currently using

```javascript
console.log(__dirname);
/*
output:
C:\Users\harish\Documents\node-ws\nodeJS-essentials
*/
```

### Get name of the current file

```javascript
console.log(__filename);
/*
output:
C:\Users\harish\Documents\node-ws\nodeJS-essentials\global.js
*/
```
### Common js module pattern
#### importing other modules(code) into our file
We can load
- Modules which are shipped with nodeJS installation
- Modules we installed with npm
- Modules we created


### 'path' module (Shipped with nodeJS)
- Help us to work with path strings

Get file name from the full path:

```javascript
const path = require("path");
console.log(`Filename: ${path.basename(__filename)}`);
/*
output:
Filename: global.js
*/
```
### Module
- Every nodeJS file is refered to as a `module`. 
- It contain its own code, and when we want to load other modules we must use **require()** function.

### 'process' object
It contains information about the current process and tools to allow us to interact with that process.
We can 
- Get environment information.
- Read environment variables.
- Communicate with the terminal or parent process through standard input and standard output.
- Exit the current process.
- Collect information from the terminal when we load the application.

Get the process ID:
```javascript
console.log(process.pid);
/*
output:
16368
*/
```
Get current version of node js used to run the process:
```javascript
console.log(process.versions.node);
/*
output:
10.15.3
*/
```

### Collect information from the terminal
Argument variables that are sent to the process when we run it.

**process.argv** is an array.

### Send arguments to nodeJS module
**Terminal**
>C:\nodeJS-essentials>node global-process.js Harish S
```javascript
console.log(process.argv)
/*
output:
[ 'C:\\Program Files\\nodejs\\node.exe',
  'C:\\Users\\haris\\Documents\\node-ws\\nodeJS-essentials\\global-process.js',
  'Harish',
  'S' ]
  */
```
**Terminal**
>C:\nodeJS-essentials>node global-process.js Harish S
```javascript
const [,,firstName,lastName] = process.argv;

console.log(`Your name is ${firstName} ${lastName}`);
/*
output:
Your name is Harish S
*/
```
### Provide flags to know what variables we are passing
>C:\nodeJS-essentials>node global-process.js --user Harish S --greeting "Good Night"

### Grab the values using the flags.

```javascript
const grab = flag => {
    let indexAfterFlag = process.argv.indexOf(flag) + 1;
    return process.argv[indexAfterFlag];
}

const greeting = grab("--greeting");
const user = grab("--user");

console.log(`${greeting} ${user}`);
/*
output:
Good Night Harish
*/
```
### Communicate with the process while its running.
#### Read and write data from the terminal
#### `Standard Output`
- process.stdout is a writable stream
- It implements a write method
- We can use write method to send data out of our program

`console.log() Vs. process.stdout`
- console.log() add new line.
- process.stdout() will not add new line.

#### Write a string to the terminal
```javascript
process.stdout.write("Hello ");
process.stdout.write("World!");
```
Output: 
> Hello World!

```javascript
const questions = [
"What is your name?",
"What is your age?",
"Where are you from?"
];

const ask = (i=0) => {
    process.stdout.write(`\n ${questions[i]}`);
    process.stdout.write(`\n > `);
};

ask();

const answers = [];
process.stdin.on('data',data=>{
    answers.push(data.toString().trim());
    if(answers.length < questions.length){
        ask(answers.length);
    }else{
        process.exit();
    } 
});

process.on('exit',()=>{
    const [name,age,city] = answers;
    console.log(`\n Name: ${name} Age: ${age} City: ${city}`);
});
```
Output: 
> What is your name?
 > harish

 What is your age?
 > 28

 Where are you from?
 > KA

> Name: harish Age: 28 City: KA


### Timing functions
- setTimeout
- clearTimeout
- setInterval
- clearInterval

#### setTimeout()
```javascript
const waitTime = 3000;

console.log(`Setting a ${waitTime/1000} seconds delay`);

const TimerFinished = () => console.log('Done.');

setTimeout(TimerFinished,waitTime);
```
Output:
>Setting a 3 seconds delay
Done.

### setInterval()
```javascript
const waitTime = 3000;
const waitInterval = 500;
let currentTime = 0;

console.log(`Setting a ${waitTime/1000} seconds delay`);

const incTime = () =>{
    currentTime += waitInterval;
    console.log(`Waiting ${currentTime/1000} seconds`);
}
const TimerFinished = () => {
    clearInterval(interval);
    console.log('Done.');
} 

setTimeout(TimerFinished,waitTime);
const interval = setInterval(incTime,waitInterval);
```
Output:
> Setting a 3 seconds delay
Waiting 0.5 seconds
Waiting 1 seconds
Waiting 1.5 seconds
Waiting 2 seconds
Waiting 2.5 seconds
Done.

### Reporting progress with setInterval

```javascript
const waitTime = 5000;
const waitInterval = 500;
let currentTime = 0;

console.log(`Setting a ${waitTime/1000} seconds delay`);

const incTime = () =>{
    currentTime += waitInterval;
    const p = Math.floor((currentTime/waitTime) * 100);
    process.stdout.clearLine(); //Clear the last line wrote to the terminal
    process.stdout.cursorTo(0); //Move the cursor to the start position
    process.stdout.write(`waiting...${p}%`);
}
const TimerFinished = () => {
    clearInterval(interval);
    process.stdout.clearLine();
    process.stdout.cursorTo(0);
    console.log('Done.');
} 

setTimeout(TimerFinished,waitTime);
const interval = setInterval(incTime,waitInterval);
```
Output: 
> Setting a 5 seconds delay
Waiting...60%

### Node Modules (Core Modules)
#### 'path' module
Path module helps to create path to diferent directories

```javascript
const path = require('path');

const dirUploads = path.join(__dirname,'www','files','uploads');

console.log(dirUploads);
```
> C:\Users\haris\Documents\nodeJS-essentials\www\files\uploads

__dirname will give full path to current directory, to reference other directories pass it as arguments to path.join function.

#### 'util' module

utility module logs the strings to console like console.log(), but with utilities module, we also get date. So every log has a date and time stamp associated with it.

We have to require(load) the utility module before we use it.
```javascript
const path = require('path');

const util = require('util');

util.log(path.basename(__filename));    //name of the current file

util.log("^ The name of the current file");
```
> 17 Sep 06:55:59 - core.js
17 Sep 06:55:59 - ^ The name of the current file

#### 'v8' module
GetHeapStatistic function available on v8 module, which displays memory usage.
```javascript
const v8 = require('v8');

console.log(v8.getHeapStatistics());
```
>{ total_heap_size: 6537216,
  total_heap_size_executable: 1048576,
  total_physical_size: 6537216,
  total_available_size: 1520724088,
  used_heap_size: 4044344,
  heap_size_limit: 1526909922,
  malloced_memory: 8192,
  peak_malloced_memory: 406336,
  does_zap_garbage: 0 }
  
  ### Readline
  Help us to build an application that would ask questions of a terminal user.  
  it's an interface around readable and writable streams, that would prompt the user and collect the answer.
```javascript
const readline = require('readline');

const rl = readline.createInterface({
    input: process.stdin,
    output: process.stdout
});

rl.question(`What is your name?`, (answer)=>{
    console.log(`Your name is ${answer}`);
});
```
  > What is your name?harish
Your name is harish

### Q & A in cmd line

```javascript
const readline = require('readline');

const rl = readline.createInterface({
    input: process.stdin,
    output: process.stdout
});

const questions = [
'What is your name?',
'What is your age?',
'Where are you from?'
];

const collectAnswers = (questions, done) => {
    const answers = [];
    const [firstQuestion] = questions;

    const questionAnswered = answer => {
        answers.push(answer);
        if(answers.length < questions.length){
            rl.question(questions[answers.length],questionAnswered);
        }else{
            done(answers);
        }
    };

    rl.question(firstQuestion,questionAnswered);
}

collectAnswers(questions, answers => {
    console.log("Thank you for your answers.");
    console.log(answers);
    process.exit();
});
```
> What is your name?harish
What is your age?28
Where are you from?Tumkur
Thank you for your answers.
[ 'harish', '28', 'Tumkur' ]

### Export custom module

Export data and functionality from a module.
> myModule.js
```javascript
let count = 0;

const inc = () => ++count;
const dec = () => --count;

const getCount = () => count;
module.exports = { inc, dec, getCount};
```
> app.js

```javascript
const {inc, dec, getCount} = require ('./myModule.js');

inc();
inc();
inc()
dec();
console.log(getCount());
```
Output: 
> 2

### Creating custom module
> collectAnswer.js


```javascript
const readline = require('readline');

const rl = readline.createInterface({
    input: process.stdin,
    output: process.stdout
});

module.exports = (questions, done = f => f) => {
    const answers = [];
    const [firstQuestion] = questions;

    const questionAnswered = answer => {
        answers.push(answer);
        if(answers.length < questions.length){
            rl.question(questions[answers.length],questionAnswered);
        }else{
            done(answers);
        }
    };

    rl.question(firstQuestion,questionAnswered);
}
```
> question.js
```javascript
const collectAnswers = require('./lib/collectAnswers');

const questions = [
    'What is your name?',
    'What is your age?',
    'Where are you from?'
];

collectAnswers(questions, answers => {
    console.log("Thank you for your answers.");
    console.log(answers);
    process.exit();
});
```
Output: node question.js
> What is your name?harish
What is your age?29
Where are you from?BAN
Thank you for your answers.
[ 'harish', '29', 'BAN' ]

### Event emitter

Node JS implementation of publish-subscribe design pattern.
Used to emitt custom events, and wiring uplisterns and handlers for those events.

Event emitter gives us  a way to raise and handle custom events.
```javascript
const events = require('events');

const emitter = new events.EventEmitter();

emitter.on('customEvent',(message, user)=>{
    console.log(`${user}: ${message}`)
});

emitter.emit('customEvent',"Hello world","Computer");
emitter.emit('customEvent',"Hello node","Harish");
```
Output: 
> Computer: Hello world
Harish: Hello node

> `Note:`
> Event emitter is asynchronous. The events are raised when they happen.
> 

### Listening for user input from the terminal
Whenever the user type we will collect it

> collectAnswer.js
```javascript
const readline = require('readline');
const {EventEmitter} = require('events');

const rl = readline.createInterface({
    input: process.stdin,
    output: process.stdout
});

module.exports = (questions, done = f => f) => {
    const answers = [];
    const [firstQuestion] = questions;
    const emitter = new EventEmitter();

    const questionAnswered = answer => {
        emitter.emit("answer", answer);
        answers.push(answer);
        if(answers.length < questions.length){
            rl.question(questions[answers.length],questionAnswered);
        }else{
            emitter.emit("complete",answers);
            done(answers);
        }
    };

    rl.question(firstQuestion,questionAnswered);

    return emitter;
}

```
> questions.js
```javascript
const collectAnswers = require('./lib/collectAnswers');

const questions = [
    'What is your name?',
    'What is your age?',
    'Where are you from?'
];

const answerEvents = collectAnswers(questions);

answerEvents.on("answer",answer => console.log(`Question answered: ${answer}`));

answerEvents.on("complete", answers => {
    console.log("Thank you for your answers.");
    console.log(answers)
});

answerEvents.on("complete", () => process.exit());
```
Output:
> What is your name?harish
Question answered: harish
What is your age?29
Question answered: 29
Where are you from?Tmk
Question answered: Tmk
Thank you for your answers.
[ 'harish', '29', 'Tmk' ]

### File system basics
Working with files and directories:
Interacting with the file system
`fs module` : 
- List files in the directory
- Create new files in the directory
- Stream files
- Watch files
- Modify file permissions

### Listing the contents of a directory
```javascript
const fs = require('fs');

const files = fs.readdirSync("../assets");

console.log(files);
```
Output:
> ['ReadeMe.md','banner.jpg','config.json']

`readdirSync` this function is executing synchronously. that means program will stop untill the files within the directory are read.

### Asynchronous function
```javascript
const fs = require('fs');

fs.readdir("../assets", (err, files)=>{
    if(err){
        throw err;
    }
    console.log(files);
});
```

### Reading the contents of a file
  `Synchronous` function:
  
  ```javascript
const fs = require('fs');

const text = fs.readFileSync("./assets/ReadMe.md","UTF-8");

console.log(text);
```

### Asynchronous function
```javascript
const fs = require('fs');

fs.readFile("./assets/ReadMe.md","UTF-8", (err, text)=>{
    if(err){
        console.log(`An error has occoured: $(err.message)$`);
        process.exit();
    }
    console.log(text);
});
```

## Write and append file
`Asynchronously`
```javascript
const fs = require('fs');

const text = `
# This is a new file.

We can write text to file with fs.writeFile.

* fs.readdir
* fs.readFile
* fs.writeFile
`
fs.writeFile("ReadMe.md",text.trim(),err => {
    if(err){
        throw err;
    }
    console.log("File saved.");
});
```
For `Synchronous` function use: fs.writeFileSync()

### Creating directories
```javascript
const fs = require('fs');

// check for directory existance
if(fs.existsSync('storage-files')){
    console.log('Already there');
}else{
    fs.mkdir('storage-files', err => {
        if(err){
            throw err;
        };
        console.log('Directory created');
    });
}
```
### Append files

If the file does not exists , it will create the file and append
```javascript
const fs = require('fs');
const colorList = [
    {"color":"red","hex":"#FF0000"},
    {"color":"green","hex":"#00FF00"},
    {"color":"blue","hex":"#0000FF"}
];

colorList.forEach( c => {
    fs.appendFile("./storage-files/color.md",`${c.color}:${c.hex}\n`, err => {
        if(err){
            throw err;
        }
    });
});
```
### Rename and remove files
```javascript
const fs = require('fs');

//Renaming file ReadMe.md to colors.md
fs.renameSync("ReadMe.md","colors.md");

//Moving file colors.md into storage-file directory
fs.rename("colors.md","./storage-file/myColors.md",(err)=>{
    if(err){
        throw err;
    }
});

//Delete file color.md
fs.unlink("./assets/color.md",(err)=>{
    if(err){
        throw err;
    }
});
```

### Renaming a directory
```javascript
const fs = require('fs');

//Renaming the directory
fs.renameSync("storage-file","storage");
```

### Removing a directory
Before removing a directoy make sure that the directory is empty.

#### Read all the files in the directory and remove them all before deleting the directory
```javascript
const fs = require('fs');

//Remove all files in the directory before deleteing the directory
fs.readdirSync('./storage').forEach(fileName => {
    fs.unlinkSync(`./storage/${fileName}`);
});

//Removing a directory
fs.rmdir("storage",(err)=>{
    if(err){
        throw err;
    }
    console.log(`./storage directory removed.`)
});
```

### Files and Streams
The stream interface provides us the technique to read and write data.
We can use it to
- Read and write data to files.
- Communicate with the internet.
- Communicate with other processes.

#### Readable stream
`process.stdin` is a readable stream.

filesystem comes wit a way to create readble streams.
 File can be read bit by bit or chunk by chunk.
 Reading files using streams helps nodejs application to use less memory becxuse instead of reading everything  all at once and loading it into buffer,reading file bit by bit or chunk by chunk
 They also give control by raising events.
 ```javascript
const fs = require('fs');

const readStream = fs.createReadStream('lorum-ipsum.txt','UTF-8');

readStream.on("data",data=>{
    console.log(`I read ${data.length-1} characters of text`);
});

readStream.once("data",data=>{
    console.log(data);
});

//To know read stream is completed
readStream.once("end",() =>{
    console.log("Finished reading file");
});
 ```
 Output:
 > I read 65535 characters of text
I read 65535 characters of text
I read 65535 characters of text
I read 65535 characters of text
I read 65535 characters of text
I read 65535 characters of text
I read 65535 characters of text
I read 65535 characters of text
I read 18710 characters of text
Finished reading file

 #### Readable stream
 `process.stdout` is a writable stream.
 ##### Write data to writable stream
```javascript
  const fs = require('fs');

const writeStream = fs.createWriteStream('myFile.txt','UTF-8');

writeStream.write("hello ");
wrteStream.write("World!\n");
```
 Readable stream are made to work with writable streams.
 
 #### Write data typed on terminal to a file
```javascript
 const fs = require('fs');

const writeStream = fs.createWriteStream('myFile.txt','UTF-8');

process.stdin.on("data", data =>{
    writeStream.write(data)
});
  ```
#### Read from one file and write to another file
```javascript
 const fs = require('fs');

const writeStream = fs.createWriteStream('myFile.txt','UTF-8');
const readStream = fs.createReadStream('lorum-ipsum.txt','UTF-8');

readStream.on("data", data =>{
    writeStream.write(data)
});
```
   ### Another way
```javascript
const fs = require('fs');

const writeStream = fs.createWriteStream('myFile.txt','UTF-8');
const readStream = fs.createReadStream('lorum-ipsum.txt','UTF-8');

readStream.pipe(writeStream);
```

### Create child process with execute and spawn

Execute function is for synchronous commands.
 
