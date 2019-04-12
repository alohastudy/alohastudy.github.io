# Table of Contents

* [About AlohaStudy](#about-alohastudy)
* [User Guide](#user-guide)
  * [Installation](#installation)
* [Project Goals](#project-goals)
* [Page Mockup](#page-mockup)
* [Application Design](#application-design)
  * [Page Mockup](#page-mockup)
  * [Beyond the Basics](#beyond-the-basics)
  * [Description of Application](#description-of-application)
* [Development History](#development-history)
* [Contact Us](#contact-us)

# About AlohaStudy

AlohaStudy is a Meteor application designed to provide students a listing of hidden study spots. In this site, you will be provided a listing with ratings, hours, capacity, accessibility, and capabilities of each study spot. It will also include a feature allowing students to give real-time feedback if it's either used or not used with a timestamp.

This application will give students the access and convenience of knowing where and when to go to hidden study spots.

# User Guide

includes screenshots of deployed app



## Installation
First, [install Meteor](https://www.meteor.com/install).

Second, [download a copy of AlohaStudy](https://github.com/alohastudy/alohastudy/archive/master.zip), or clone it using git.

Third, cd into the app/ directory and install libraries with:

```
$ meteor npm install
```

Fourth, run the system with:

```
$ meteor npm run start
```

If all goes well, the application will appear at [http://localhost:3000](http://localhost:3000). If you have an account on the UH test CAS server, you can login.  

#### Page Mockup

 - Landing Page -  http://alohastudy.meteorapp.com/#/
 <img src="mockups/landing.PNG">
 This is the first page a new user will see. It contains some information about the site as well as how to use it. There is also some information on the site's creators.
 
 - List Spots - http://alohastudy.meteorapp.com/#/listSpots
 <img src="mockups/listSpots.PNG">
 This page displays all verified spots. It is intended to allow general users to browse spots that have been posted. From here they can see if a spot is noisy, has outlets or is indoors. There are also ratings displayed for each spot. Clicking the title of a spot will redirect the user to that spot's info page. A banned user cannot visit this page. A sample banned user has the following credentials:
 
 email: banned@foo.com
 password: changeme
 
 - Admin List Spots - http://alohastudy.meteorapp.com/#/adminSpots
 <img src="mockups/adminListSpots.PNG">
 This page builds off the List Spots page but instead shows all spots, including unverified ones. This page is intended to allow admins to view all spots and moderate them. For example it allows admins to navigate to unverified posts and consider verifying them.
 
 - Spot Info Page - http://alohastudy.meteorapp.com/#/view/msAu33FngnCjMNcPv
 <img src="mockups/spotsInfo.PNG">
 This page shows public information on a certain spot. If not verified then there will be a large red tag displaying as such. This page will eventually have a comment system built into it.
 
 - Add Spot - http://alohastudy.meteorapp.com/#/add
 <img src="mockups/addSpots.PNG">
 This page allows a user to add a spot to the database. It currently doesn't work completely. It will have it so that in general posts will be initially unverified. A spot submitted by a verified user will be automatically verified.
 
 - Login/Logout/Sign up - http://alohastudy.meteorapp.com/#/signin
 <img src="mockups/login.png">
 This page is where users can login.


# Project Goals
 - People can create accounts as users
 - Users can submit spots but they must be verified by admins
 - Users can comment on posts
 - Accounts can get verified so they can freely post spots
 - Admins can delete comments and posts
 - Filter function available for viewing certain posts
 - Users have profiles that show spots they have made
 
# Application Design



#### Beyond the Basics

 - Allow users to create/edit/delete spots
 - Allow users to create/edit/delete reviews
 - Search for spots by keyword
 - Filter spots by attributes (noisiness, indoors, air-conditioned)
 - Banned users are restricted from viewing spot/leaving comments/uploading spots
    
# Description of Application
 
   We wish to have a landing page that would have tabs going to a different functions of our website. The Landing page would have tabs for signing up or logining. 
   
   For admins, they would have access to functionalities of the admin list of spots, adding spots, editing/deleting spots, and verifying users on a whitelist. For verified users, they would have access to the list of spots, add spots, edit spots, profile edit, and feedback. For unverified users, they must be approved by an admin. The unverified user would have access to the list of posts and an add spot which would be verified by an admin. 
   
   Both the admin and verified user would have access to a button that would indicate how filled a study spot is from empty, partially filled, and filled. Verified and Admin users can provided reviews and feedback.

# Development History

# Contact Us

 - Isaac Lee: ilee72@hawaii.edu

 - David Liang: david947@hawaii.edu

 - Andrew Millard: millarda@hawaii.edu


