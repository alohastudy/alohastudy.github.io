## About AlohaStudy

AlohaStudy is a Meteor Application designed to provide students a listing of hidden study spots. In this site, you will be provided a listing with ratings, hours, capacity, accessibility, and capabilities of each study spot. It will also include a feature allowing students to give real-time feedback if it's either used or not used with a timestamp. 

## Installation

## Application Design

## Development History

## Contact Us

# Table of contents

* [About Bowfolios](#about-bowfolios)
* [Installation](#installation)
* [Application design](#application-design)
  * [Directory structure](#directory-structure)
  * [Import conventions](#import-conventions)
  * [Naming conventions](#naming-conventions)
  * [Data model](#data-model)
  * [CSS](#css)
  * [Routing](#routing)
  * [Authentication](#authentication)
  * [Authorization](#authorization)
  * [Configuration](#configuration)
  * [Quality Assurance](#quality-assurance)
    * [ESLint](#eslint)
    * [Data model unit tests](#data-model-unit-tests)
    * [JSDoc](#JSDoc)
* [Development history](#development-history)
  * [Milestone 1: Mockup development](#milestone-1-mockup-development)
  * [Milestone 2: Data model development](#milestone-2-data-model-development)
  * [Milestone 3: Connect UI to data model](#milestone-3-connect-ui-to-data-model)
  * [Milestone 4: Authentication](#milestone-4-authentication)
  * [Milestone 5: Administration](#milestone-5-administration)
* [Walkthrough videos](#walkthrough-videos)
* [JSDocs](/jsdocs)

# About BowFolios 

BowFolios is a Meteor application providing portfolios for the University of Hawaii community. When you come to the site, you are greeted by the following landing page:

![](images/landing.png)

Anyone with a UH account can login to BowFolios by clicking on the login button. The UH CAS authentication screen then appears and requests your UH account and password:
 
![](images/bowfolios-cas.png)
 
Once authenticated, you can create a profile that provides a biographical statement and list of interests, plus links to selected social media sites (GitHub, FaceBook, Instagram):

![](images/profile.png)
  
After creating a profile, you will be listed on the public directory page:

![](images/directory.png)

BowFolios also provides a filter page, available to those who can login to the system with their UH account. The filter page allows you to display all portfolios with a given interest:

![](images/filter.png)

# Installation

First, [install Meteor](https://www.meteor.com/install).

Second, [download a copy of BowFolios](https://github.com/bowfolios/bowfolios/archive/master.zip), or clone it using git.
  
Third, cd into the app/ directory and install libraries with:

```
$ meteor npm install
```

Fourth, run the system with:

```
$ meteor npm run start
```

If all goes well, the application will appear at [http://localhost:3000](http://localhost:3000). If you have an account on the UH test CAS server, you can login.  

# Application Design

## Directory structure

The top-level directory structure contains:

```
app/        # holds the Meteor application sources
config/     # holds configuration files, such as settings.development.json
.gitignore  # don't commit IntelliJ project files, node_modules, and settings.production.json
```

This structure separates configuration files (such as the settings files) in the config/ directory from the actual Meteor application in the app/ directory.

The app/ directory has this top-level structure:

```
client/
  lib/           # holds Semantic UI files.
  head.html      # the <head>
  main.js        # import all the client-side html and js files. 

imports/
  api/           # Define collection processing code (client + server side)
    base/
    interest/
    profile/
  startup/       # Define code to run when system starts up (client-only, server-only)
    client/        
    server/        
  ui/
    components/  # templates that appear inside a page template.
    layouts/     # Layouts contain common elements to all pages (i.e. menubar and footer)
    pages/       # Pages are navigated to by FlowRouter routes.
    stylesheets/ # CSS customizations, if any.

node_modules/    # managed by Meteor

private/
  database/      # holds the JSON file used to initialize the database on startup.

public/          
  images/        # holds static images for landing page and predefined sample users.
  
server/
   main.js       # import all the server-side js files.
```

## Import conventions

This system adheres to the Meteor 1.4 guideline of putting all application code in the imports/ directory, and using client/main.js and server/main.js to import the code appropriate for the client and server in an appropriate order.

This system accomplishes client and server-side importing in a different manner than most Meteor sample applications. In this system, every imports/ subdirectory containing any Javascript or HTML files has a top-level index.js file that is responsible for importing all files in its associated directory.   

Then, client/main.js and server/main.js are responsible for importing all the directories containing code they need. For example, here is the contents of client/main.js:

```
import '/imports/startup/client';
import '/imports/ui/components/form-controls';
import '/imports/ui/components/directory';
import '/imports/ui/components/user';
import '/imports/ui/components/landing';
import '/imports/ui/layouts/directory';
import '/imports/ui/layouts/landing';
import '/imports/ui/layouts/shared';
import '/imports/ui/layouts/user';
import '/imports/ui/pages/directory';
import '/imports/ui/pages/filter';
import '/imports/ui/pages/landing';
import '/imports/ui/pages/user';
import '/imports/api/base';
import '/imports/api/profile';
import '/imports/api/interest';
import '/imports/ui/stylesheets/style.css';
```

Apart from the last line that imports style.css directly, the other lines all invoke the index.js file in the specified directory.

We use this approach to make it more simple to understand what code is loaded and in what order, and to simplify debugging when some code or templates do not appear to be loaded.  In our approach, there are only two places to look for top-level imports: the main.js files in client/ and server/, and the index.js files in import subdirectories. 

Note that this two-level import structure ensures that all code and templates are loaded, but does not ensure that the symbols needed in a given file are accessible.  So, for example, a symbol bound to a collection still needs to be imported into any file that references it. 
 
## Naming conventions

This system adopts the following naming conventions:

  * Files and directories are named in all lowercase, with words separated by hyphens. Example: accounts-config.js
  * "Global" Javascript variables (such as collections) are capitalized. Example: Profiles.
  * Other Javascript variables are camel-case. Example: collectionList.
  * Templates representing pages are capitalized, with words separated by underscores. Example: Directory_Page. The files for this template are lower case, with hyphens rather than underscore. Example: directory-page.html, directory-page.js.
  * Routes to pages are named the same as their corresponding page. Example: Directory_Page.


## Data model

The BowFolios data model is implemented by two Javascript classes: [ProfileCollection](https://github.com/bowfolios/bowfolios/blob/master/app/imports/api/profile/ProfileCollection.js) and [InterestCollection](https://github.com/bowfolios/bowfolios/blob/master/app/imports/api/interest/InterestCollection.js). Both of these classes encapsulate a MongoDB collection with the same name and export a single variable (Profiles and Interests)that provides access to that collection. 

Any part of the system that manipulates the BowFolios data model imports the Profiles or Interests variable, and invokes methods of that class to get or set data.

There are many common operations on MongoDB collections. To simplify the implementation, the ProfileCollection and InterestCollection classes inherit from the [BaseCollection](https://github.com/bowfolios/bowfolios/blob/master/app/imports/api/base/BaseCollection.js) class.

The [BaseUtilities](https://github.com/bowfolios/bowfolios/blob/master/app/imports/api/base/BaseUtilities.js) file contains functions that operate across both classes. 

Both ProfileCollection and InterestCollection have Mocha unit tests in [ProfileCollection.test.js](https://github.com/bowfolios/bowfolios/blob/master/app/imports/api/profile/ProfileCollection.test.js) and [InterestCollection.test.js](https://github.com/bowfolios/bowfolios/blob/master/app/imports/api/interest/InterestCollection.test.js). See the section below on testing for more details.

## CSS

The application uses the [Semantic UI](http://semantic-ui.com/) CSS framework. To learn more about the Semantic UI theme integration with Meteor, see [Semantic-UI-Meteor](https://github.com/Semantic-Org/Semantic-UI-Meteor).

The Semantic UI theme files are located in [app/client/lib/semantic-ui](https://github.com/ics-software-engineering/meteor-application-template/tree/master/app/client/lib/semantic-ui) directory. Because they are located in the client/ directory and not the imports/ directory, they do not need to be explicitly imported to be loaded. (Meteor automatically loads all files into the client that are located in the client/ directory). 

Note that the user pages contain a menu fixed to the top of the page, and thus the body element needs to have padding attached to it.  However, the landing page does not have a menu, and thus no padding should be attached to the body element on that page. To accomplish this, the [router](https://github.com/bowfolios/bowfolios/blob/master/app/imports/startup/client/router.js) uses "triggers" to add an remove the appropriate classes from the body element when a page is visited and then left by the user. 

## Routing

For display and navigation among its four pages, the application uses [Flow Router](https://github.com/kadirahq/flow-router).

Routing is defined in [imports/startup/client/router.js](https://github.com/ics-software-engineering/meteor-application-template/blob/master/app/imports/startup/client/router.js).

BowFolios defines the following routes:

  * The `/` route goes to the public landing page.
  * The `/directory` route goes to the public directory page.
  * The `/<user>/profile` route goes to the profile page associated with `<user>`, which is the UH account name.
  * The `/<user>/filter` route goes to the filter page associated with `<user>`, which is the UH account name.


## Authentication

For authentication, the application uses the University of Hawaii CAS test server, and follows the approach shown in [meteor-example-uh-cas](http://ics-software-engineering.github.io/meteor-example-uh-cas/).

When the application is run, the CAS configuration information must be present in a configuration file such as  [config/settings.development.json](https://github.com/ics-software-engineering/meteor-application-template/blob/master/config/settings.development.json). 

Anyone with a UH account can login and use BowFolio to create a portfolio.  A profile document is created for them if none already exists for that username.

## Authorization

The landing and directory pages are public; anyone can access those pages.

The profile and filter pages require authorization: you must be logged in (i.e. authenticated) through the UH test CAS server, and the authenticated username returned by CAS must match the username specified in the URL.  So, for example, only the authenticated user `johnson` can access the pages `http://localhost:3000/johnson/profile` and  `http://localhost:3000/johnson/filter`.

To prevent people from accessing pages they are not authorized to visit, template-based authorization is used following the recommendations in [Implementing Auth Logic and Permissions](https://kadira.io/academy/meteor-routing-guide/content/implementing-auth-logic-and-permissions). 

The application implements template-based authorization using an If_Authorized template, defined in [If_Authorized.html](https://github.com/bowfolios/bowfolios/blob/master/app/imports/ui/layouts/user/if-authorized.html) and [If_Authorized.js](https://github.com/bowfolios/bowfolios/blob/master/app/imports/ui/layouts/user/if-authorized.js).

## Configuration

The [config](https://github.com/bowfolios/bowfolios/tree/master/config) directory is intended to hold settings files.  The repository contains one file: [config/settings.development.json](https://github.com/bowfolios/bowfolios/blob/master/config/settings.development.json).

The [.gitignore](https://github.com/bowfolios/bowfolios/blob/master/.gitignore) file prevents a file named settings.production.json from being committed to the repository. So, if you are deploying the application, you can put settings in a file named settings.production.json and it will not be committed.

BowFolios checks on startup to see if it has an empty database in [initialize-database.js](https://github.com/bowfolios/bowfolios/blob/master/app/imports/startup/server/initialize-database.js), and if so, loads the file specified in the configuration file, such as [settings.development.json](https://github.com/bowfolios/bowfolios/blob/master/config/settings.development.json).  For development purposes, a sample initialization for this database is in [initial-collection-data.json](https://github.com/bowfolios/bowfolios/blob/master/app/private/database/initial-collection-data.json).

## Quality Assurance

### ESLint

BowFolios includes a [.eslintrc](https://github.com/bowfolios/bowfolios/blob/master/app/.eslintrc) file to define the coding style adhered to in this application. You can invoke ESLint from the command line as follows:

```
meteor npm run lint
```

ESLint should run without generating any errors.  

It's significantly easier to do development with ESLint integrated directly into your IDE (such as IntelliJ).

### Data model unit tests

To run the unit tests on the data model, invoke the script named 'test', which is defined in the package.json file:

```
meteor npm run test
```

This outputs the results to the console. Here is an example of a successful run, with timestamps removed:

```
[~/github/bowfolios/bowfolios/app]-> meteor npm run test

> bowfolios@ test /Users/philipjohnson/github/bowfolios/bowfolios/app
> TEST_WATCH=1 meteor test --driver-package meteortesting:mocha

[[[[[ Tests ]]]]]                             

=> Started proxy.                             
=> Started MongoDB.  
                         
(STDERR) Note: you are using a pure-JavaScript implementation of bcrypt.
(STDERR) While this implementation will work correctly, it is known to be
(STDERR) approximately three times slower than the native implementation.
(STDERR) In order to use the native implementation instead, run
(STDERR) 
(STDERR)   meteor npm install --save bcrypt
(STDERR) 
(STDERR) in the root directory of your application.

 --------------------------------
 ----- RUNNING SERVER TESTS -----
 --------------------------------
   
=> Started your app.

=> App running at: http://localhost:3000/
    InterestCollection
    ✓ #define, #isDefined, #removeIt, #dumpOne, #restoreOne (69ms)
    ✓ #findID, #findIDs  
    ProfileCollection
    ✓ #define, #isDefined, #removeIt, #dumpOne, #restoreOne (66ms)
    ✓ #define (illegal interest)
    ✓ #define (duplicate interests)

   5 passing (178ms)

Load the app in a browser to run client tests, or set the TEST_BROWSER_DRIVER environment variable. See https://github.com/DispatchMe/meteor-mocha/blob/master/README.md#run-app-tests
```


### JSDoc

BowFolios supports documentation generation with [JSDoc](http://usejsdoc.org/). The package.json file defines a script called jsdoc that runs JSDoc over the source files and outputs html to the ../../bowfolio.github.io/jsdoc directory.  When committed, the index.html file providing an overview of all the documentation generate at that point in time is available at [http://bowfolios.github.io/jsdocs](https://bowfolios.github.io/jsdocs/). 

# Development History

The development process for BowFolios conformed to [Issue Driven Project Management](http://courses.ics.hawaii.edu/ics314f16/modules/project-management/) practices. In a nutshell, development consists of a sequence of Milestones. Milestones consist of issues corresponding to 2-3 day tasks. GitHub projects are used to manage the processing of tasks during a milestone.  

The following sections document the development history of BowFolios.

## Milestone 1: Mockup development

This milestone started on December 6, 2016 and ended on January 31, 2017.

The goal of Milestone 1 was to create a set of HTML pages providing a mockup of the pages in the system. To simplify things, the mockup was developed as a Meteor app. This meant that each page was a template and that FlowRouter was used to implement routing to the pages. 

Mockups for the following four pages were implemented during M1:

<img width="200px" src="images/landing.png"/>
<img width="200px" src="images/profile.png"/>
<img width="200px" src="images/directory.png"/>
<img width="200px" src="images/filter.png"/>

Milestone 1 was implemented as [BowFolio GitHub Milestone M1](https://github.com/bowfolios/bowfolios/milestone/1)::

![](images/m1-milestone.png)


Milestone 1 consisted of five issues, and progress was managed via the [BowFolio GitHub Project M1](https://github.com/bowfolios/bowfolios/projects/1):

![](images/m1-project.png)

Each issue was implemented in its own branch, and merged into master when completed:

![](images/m1-branch-graph.png)

## Milestone 2: Data model development 

This milestone started on Jan 31, 2017 and ended on Feb 2, 2017.

The goal of Milestone 2 was to implement the data model: the underlying set of Mongo Collections and the operations upon them that would support the BowFolio application.  We implemented the data model as a set of Javascript classes. The BaseCollection class provides common fields and operations. The ProfileCollection and InterestCollection classes inherit from BaseCollection and provide the persistent data structures useful for BowFolios. 
 
Also in this milestone, we implemented a set of mocha tests for the data model classes. These tests make sure we can create, manipulate, and delete the data model documents successfully.  These tests are documented above.

Milestone 2 was implemented as [BowFolio GitHub Milestone M2](https://github.com/bowfolios/bowfolios/milestone/2)::

![](images/m2-milestone.png)


Milestone 2 consisted of two issues, and progress was managed via the [BowFolio GitHub Project M2](https://github.com/bowfolios/bowfolios/projects/2):

![](images/m2-project.png)

Each issue was implemented in its own branch, and merged into master when completed:

![](images/m2-branch-graph.png)

## Milestone 3: Connect UI to data model

This milestone started on Feb 2, 2017 and ended on Feb 10, 2017.

The goal of Milestone 3 was to connect the user interface to the underlying data model. This meant that we updated the templates for each page with calls to helper functions, and we created Javascript files for the templates with helper functions. We used the form control templates from [meteor-example-form](https://ics-software-engineering.github.io/meteor-example-form/) to simplify implementation of form processing.

Milestone 3 was implemented as [BowFolio GitHub Milestone M3](https://github.com/bowfolios/bowfolios/milestone/3)::

![](images/m3-milestone.png)


Milestone 3 consisted of four issues, and progress was managed via the [BowFolio GitHub Project M3](https://github.com/bowfolios/bowfolios/projects/3):

![](images/m3-project.png)

Each issue was implemented in its own branch, and merged into master when completed:

![](images/m3-branch-graph.png)

## Milestone 4: Authentication

This milestone started on Feb 10, 2017 and ended on Feb 14, 2017.

The goal of Milestone 4 was to set up authentication using the University of Hawaii test CAS system. We used the templates from [meteor-example-uh-cas](http://ics-software-engineering.github.io/meteor-example-uh-cas/) to guide the implementation. Although the example restricts logins to those in a list in the configuration file, BowFolios allows anyone with a UH account to access the system. 

Authentication also implies that users cannot access the profile or filter page associated with another user.

Milestone 4 was implemented as [BowFolio GitHub Milestone M4](https://github.com/bowfolios/bowfolios/milestone/4)::

![](images/m4-milestone.png)


Milestone 4 consisted of two issues, and progress was managed via the [BowFolio GitHub Project M4](https://github.com/bowfolios/bowfolios/projects/4):

![](images/m4-project.png)

Each issue was implemented in its own branch, and merged into master when completed:

![](images/m4-branch-graph.png)

## Milestone 5: Administration

This milestone started on Feb 14, 2017 and is ongoing.

[BowFolio GitHub Milestone 5](https://github.com/bowfolios/bowfolios/milestone/5) involves the creation of an administrator role in the system. The administrator can manage the set of defined interests. (Currently, interests are defined in the database file loaded at system startup time.)

This milestone will also include the implementation of Meteor methods and removal of the insecure package. 

We will manage progress on this milestone using [BowFolio GitHub Project M5](https://github.com/bowfolios/bowfolios/projects/5).

# Walkthrough videos

BowFolios is intended as a model of how an ICS 314 project could be organized and executed. Here are some videos to walk through various aspects of the system and development process:

* [BowFolios: User Interface](https://www.youtube.com/watch?v=aZvxRQfQdkE)
* [BowFolios: Development Process](https://www.youtube.com/watch?v=8pTgFtbcjTc)
* [BowFolios: Application Structure](https://www.youtube.com/watch?v=_5g5CzZ0Toc)
* [BowFolios: Authentication and Authorization](https://www.youtube.com/watch?v=AaXShN8cYNY)
* [BowFolios: Initialization](https://www.youtube.com/watch?v=P3Kigb1gtVo)
* [BowFolios: Unit Testing](https://www.youtube.com/watch?v=EexZfw1yMJs)
* [BowFolios: Design Patterns](https://www.youtube.com/watch?v=yP-t44HBCPQ). Maybe watch [this](https://www.youtube.com/watch?v=Z2yjimK_MJU) first.
