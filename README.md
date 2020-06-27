# CCA
Counter Project

Skip to end of metadata
Created by Julia Stepiko, last modified about 2 hours agoGo to start of metadata
1. Prerequisites
------------
1.1. Node.js:
Install the latest Node.js Recommended For Most Users from https://nodejs.org/en/

1.2. Install Git:
https://git-scm.com/

1.3. Install Java SE Development Kit:
https://www.oracle.com/technetwork/java/javase/downloads/jdk12-downloads-5295953.html

---------------

Additional MacOS prerequisites:
1.4. Install Xcode:
https://developer.apple.com/xcode/

----------------

Additional Windows prerequisites:
1.5. Install all the required tools and configurations using Microsoft's windows-build-tools:
Open Command Prompt as administrator and run the following script:

npm install --global --production windows-build-tools
1.6. Install Python 2.7.
https://www.python.org/downloads/release/python-2716/

1.7. Use Git Bash instead of using Windows Command Prompt (for WebStorm).
Open WebStorm
Go to File => Settings
Go to Tools => Terminal
Specify Shell Path to Git Bash e.g.C:\Program Files\Git\bin\sh.exe
-------------------

2. Creating project
2.1. Create project folder:
Open terminal and navigate to desired folder where you're going to store all of your projects. For example projects folder:

cd projects
Create a folder for automation project. For example, CCA:

mkdir CCA
And navigate to the created folder:

cd CCA
2.2. Initialize your project:
npm init -y
This action creates package.json file.



3. Modules installation and configuration
3.1. Install webDriver I/O:
npm i webdriverio
3.2. Install CLI:
npm i @wdio/cli
3.3. Create WebDriver I/O configuration by running:
./node_modules/.bin/wdio config
This script runs WDIO Configuration Helper which will help you to create configuration file.



=========================
WDIO Configuration Helper
=========================

? Where is your automation backend located? (Use arrow keys)
> On my local machine
   In the cloud using Experitest
   In the cloud using Sauce Labs
   In the cloud using Browserstack or Testingbot or LambdaTest or a different service
   I have my own Selenium cloud

? Which framework do you want to use? (Use arrow keys)
> mocha
   jasmine
   cucumber



? Do you want to run WebdriverIO commands synchronous or asynchronous? (Use arrow keys)
> sync
   async


? Where are your test specs located? (./test/specs/**/*.js) ← change to (./test/*.js)

? Are you using a compiler? (Use arrow keys)
   Babel (https://babeljs.io/)
   TypeScript (https://www.typescriptlang.org/)
> No!

? Which reporter do you want to use?
   (*) spec
   (*) dot
   ( ) junit
>(*) allure
   ( ) sumologic
   ( ) concise
   ( ) reportportal
(Move up and down to reveal more choices)

? Do you want to add a service to your test setup?
   ( ) chromedriver
   ( ) sauce
   ( ) testingbot
>(*) selenium-standalone
   ( ) devtools
   ( ) applitools
   ( ) browserstack
(Move up and down to reveal more choices)

? What is the base url? https://likejean.github.io/homework-5/

---

Wait till the end of the installation process.

---

Configuration file was created successfully!
To run your tests, execute:
$ wdio run wdio.conf.js

---


4. Update WebDriver I/O configuration:
Open configuration file wdio.conf.js and configure browser: 

maxInstances: 1,
browserName: 'chrome'


5. Modify test script:
Open package.json and modify test script to get the following:


"test": "wdio wdio.conf.js"


6. Creating the first test
6.1. Create test folder and open it:
mkdir test
cd test
6.2. Create test.js file and open it:
touch test.js
open test.js
cd ..
6.3. Add the first test:

import assert from 'assert';

describe('Complex Counter App', function () { //define suite title by passing a string

    describe('Default counter', function () { //define sub-suite title by passing a string

        it('TC-001 Header is present on the page  ', function () { //define test title by passing a string
            browser.url('https://likejean.github.io/homework-5/'); //open baseUrl
            let title = browser.getTitle(); //get page title and assign it to the "title" variable
            browser.pause(2000); //just pause to visually see that something is happening on the page
            assert.equal(title, 'Complex Counter App'); //compare {title} (actual) and "Complex Counter App" (expected)
        })

    });

});



7. Instal Babel to use JavaScript ES6 syntax
7.1. Go back to the root folder and install necessary modules:
npm install @babel/core @babel/cli @babel/preset-env @babel/register
7.2. Create and open Babel Configuration file:
touch babel.config.js
open babel.config.js
7.3. Paste the following code:
module.exports = {
    presets: [
        ['@babel/preset-env', {
            targets: {
                node: 8
            }
        }]
    ]
}
7.5 Update WebDriver I/O configuration adding Babel configuration:
Open configuration file:

open wdio.conf.js
And add the following line to the mochaOpts objest:

compilers: ['js:@babel/register']
It should look like this at the end:

mochaOpts: {
        ui: 'bdd',
        timeout: 60000,
        compilers: ['js:@babel/register']
    },


8. Running the first test
8.1. Run the first test:
npm test
Wait until test is finished. You should see the message with one suite, one sub-suite, and one test passed:

[chrome 83.0.4103.116 Windows NT #0-0] Complex Counter App
[chrome 83.0.4103.116 Windows NT #0-0] Default counter
[chrome 83.0.4103.116 Windows NT #0-0] ✓ TC-001 Header is present on the page
[chrome 83.0.4103.116 Windows NT #0-0]
[chrome 83.0.4103.116 Windows NT #0-0] 1 passing (3.2s)



9. Adding more tests:
9.1. Rename test.js to make it more clear:
cd test
mv test.js elements.js
open elements.js
Replace the old code by the following one in elements.js:


describe('Complex Counter App', function () { //define suite title by passing a string

    describe('Getting to the page', function () { //define sub-suite title by passing a string

        it('TC-001 Page title is Complex Counter App', function () { //define test title by passing a string
            browser.url('https://likejean.github.io/homework-5/'); //open baseUrl
            const title = browser.getTitle(); //get page title and assign it to the "title" variable
            browser.pause(2000); //just pause to visually see that something is happening on the page
            expect(title).toEqual('Complex Counter App'); //compare {title} (actual) and "Complex Counter App" (expected)
        })

    });

    describe('Elements exist', function () {

        it('TC-002 Header', function () {
            const header = $('h1').isDisplayed();
            expect(header).toEqual(true);
        })

        it('TC-003 Total Result', function () {
            const header = $('h3.total-count').isDisplayed();
            expect(header).toEqual(true);
        })

        it('TC-004 Counter Name', function () {
            const header = $$('h3')[1].isDisplayed();
            expect(header).toEqual(true);
        })

    });

});

9.2. Create one more test file defaultFunctionality.js:
touch defaultFunctionality.js
open defaultFunctionality.js
cd ..
9.3. Copy and paste the following code to defaultFunctionality.js:

describe('Default counter functionality', function () {

    describe('Calculation works', function () {

        it('TC-021 Subtract 1 gives -1', function () {
            browser.url('https://likejean.github.io/homework-5/');
            browser.pause(2000);
            const button = $$('.btn-black')[0];
            button.click();
            const countValue = $('.badge').getText();
            expect(countValue).toEqual('-1');
        })

        it('TC-022 Add 3 gives 2', function () {
            browser.pause(2000);
            const button = $$('.btn-black')[5];
            button.click();
            const countValue = $('.badge').getText();
            expect(countValue).toEqual('2');
        })

    });

});

9.4. Edit the configuration including elements.js and updating functionality.js:
Open wdio.conf.js and update the specs array:

    specs: [
        './test/elements.js',
        './test/defaultfunctionality.js'
    ],
Now your code testing Elements and Functionality suites. More info regarding Webdriver I/O commands you can find here: https://webdriver.io/docs/api.html



10. Configuring Allure reporter:
10.1. Configure reporter in wdio.conf.js:
Add the following code under reporters: ['dot','spec','allure'],:

    reporterOptions: {
        allure: {
            outputDir: 'allure-results'
        }
    },
10.2. Install Allure Commandline globally:
npm i allure-commandline -g
10.3. Create a script to generate and open a report:
Add the following script to package.json:

"report": "allure generate --clean && allure open"
10.4. Create a script to clean allure-results folder before running a test:
Add the following script to package.json:

"clean": "rm -rf allure-results"
10.5. Modify test script to include clean and report scripts:
Replace the test script in package.json by the following code:

"test": "npm run clean && wdio wdio.conf.js && npm run report"
Now your scripts should look like this:

"test": "npm run clean && wdio wdio.conf.js && npm run report",
"report": "allure generate --clean && allure open",
"clean": "rm -rf allure-results"
10.6. Run npm test to try.
allure-results folder should be removed automatically before the test starts. Allure Report should appear once the testing finished. To kill the process press Ctrl+C.



11. Adding chai assertion library:
11.1. Install chai module:
npm i chai
11.2. Import new module into your files:
Replace the old assert import (if you used it):

import assert from 'assert';
by new assert from chai:

import { assert } from 'chai';
Please make sure you replaced it in all the test files. Now you can use all the methods described here: https://www.npmjs.com/package/chai



12. Creating smoke and regression

13. Working with Git
13.1. Create GitHub account:
https://github.com/join

13.2. Once logged in create new repository:
https://github.com/new Provide Repository name and click Create repository. On the next page you have the instruction how to connect your local repo with the remote one.

13.3. Initialize local git repo:
Get back to Terminal and type:

git init
13.4. Create and configure .gitignore file:
This file is used to list all folders and files which should be ignored by Git.

touch .gitignore
open .gitignore
Add the following lines to the file:

node_modules
allure-results
allure-report
.git
.idea
13.5. Add all files to Git and create first commit:
git add .
git commit -m "first commit"
13.6. Add all files to Git and create first commit:
git add .
git commit -m "first commit"
13.7. Connect your local git with remote GitHub repo:
Get the link from the page you get on GitHub:

git remote add origin {link}
13.8. Push your local code to the remote repo:
git push -u origin master
13.9. Some git commands to use:
List of branches:
git branch
Create branch:
git branch {name}
Delete branch:
git branch -D {name}
Switch to another branch:
git checkout {name}
Commiting changes:
git commit -m "commit message"
Pushing changes:
git push
