1. [Sous-Marin ASUQTR](index.html)
2. [Sous-Marin ASUQTR Knowledge Center](Sous-Marin-ASUQTR-Knowledge-Center_5144578.html)
3. [How-to articles](How-to-articles_13533186.html)
4. [How to: Software](42827871.html)

# Sous-Marin ASUQTR : How to work with Angular for the dashboard

Created by Maxime Verreault, last modified on Nov 22, 2020

The IT lord won't be with us forever and we might need to update the dashboard. The dashboard is written in the Angular framework version 10 and trying to write code will be pretty hard if you don't know how. This guide will give you a starting point to do so.

For more in-depth documentation, go to the [official Angular docs](https://angular.io/).

## Prerequisites

To use the Angular framework, you should be familiar with the following:

* [TypeScript](https://www.typescriptlang.org/)
* [HTML](https://developer.mozilla.org/docs/Learn/HTML/Introduction_to_HTML)
* [CSS](https://developer.mozilla.org/docs/Learn/CSS/First_steps)

To install Angular on your local system, you need the following:

* **Node.js**

  Angular requires a [current, active LTS, or maintenance LTS](https://nodejs.org/about/releases) version of Node.js. For more information on installing Node.js, see [nodejs.org](https://nodejs.org/ "Nodejs.org"). If you are unsure what version of Node.js runs on your system, run the following in a terminal window:

```
node -v
```

* **npm package manager**

  Angular, the Angular CLI, and Angular applications depend on[npm packages](https://docs.npmjs.com/getting-started/what-is-npm)for many features and functions. To download and install npm packages, you need an npm package manager. This guide uses the[npm client](https://docs.npmjs.com/cli/install)command-line interface, which is installed withNode.js by default. To check that you have the npm client installed, run the following in a terminal window:

```
npm -v
```

## Setup the environment

1. Clone the repository with

```
git clone https://bitbucket.asuqtr.com/scm/subuqtr/asuqtr_interface_web.git && cd ./asuqtr_interface_web
```

2. Install the dependencies

```
npm install
```

3. Run the application to see if everything is working properly

```
ng serve --open
```
4. It is recommended to use an IDE to work on an Angular project. You can either use Webstorm (recommended) or VS Code. Those IDEs will do most of the basic work for you.

## What is the purpose of this? Where is that?

An Angular project can be a little overwhelming if you don't know the structure. Everything is properly described in the [official Angular documentation](https://angular.io/guide/file-structure).

## UI layout

The UI is divided into 3 pages: main, secondary and debug. Each of these pages is using [CSS grid](https://css-tricks.com/snippets/css/complete-guide-grid/) for the layout. You will need to think about where to place your component and how to do so with CSS grid.

## Create your first component

1. Create a new Git branch for the new feature you want to add

```
git checkout -b feature/YOUR-NEW-FEATURE
```

2. Use the [Angular CLI](https://angular.io/cli) to configure your new component for you

```
ng generate component YourComponentName
```

3. Figure out where you want your new component (which page). For example, if you want to add the component to the main page, go into the main-page.component.html file and add your component

```
<app-your-component-name></app-your-component-name>
```
4. Modify the component to reflect your needs

5. Commit your changes and push them to the remote repository

```
git add . && git commit -m "Added NEW-FEATURE-NAME" && git push
```
6. Once everything is done, [create a pull request](https://www.atlassian.com/git/tutorials/making-a-pull-request) to merge your changes into the master branch

Document generated by Confluence on May 02, 2023 17:22

[Atlassian](https://www.atlassian.com/)