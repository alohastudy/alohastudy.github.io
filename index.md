# Table of contents

* [About AlohaStudy](#about-alohastudy)
* [Installation](#installation)
* [Project Goals](#project-goals)
* [Page Mockup](#page-mockup)
* [Application Design](#application-design)
  * [Page Mockup](#page-mockup)
  * [Beyond the Basics](#beyond-the-basics)
* [Development History](#development-history)
* [Contact Us](#contact-us)

# About AlohaStudy

AlohaStudy is a Meteor application designed to provide students a listing of hidden study spots. In this site, you will be provided a listing with ratings, hours, capacity, accessibility, and capabilities of each study spot. It will also include a feature allowing students to give real-time feedback if it's either used or not used with a timestamp.

This application will give students the access and convenience of knowing where and when to go to hidden study spots.

# Installation
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

# Project Goals
W

# Application Design
 
   ### Page Mockup
    - Landing Page
    - List Spots
    - Admin List Spots
    - Spot Info Page
    - Profile Edit Page
    - Add Spot
    - Edit/Delete Spot
    - Login/Logout/Sign up
    - Button for saying you are at the spot along with how filled it is

   ### Beyond the Basics
    - Allow users to create/edit/delete spots
    - Allow users to create/edit/delete reviews
    - Search for spots by keyword
    - Filter spots by attributes (noisiness, indoors, air-conditioned)
    
 Description of Application 
 
   We wish to have a landing page that would have tabs going to a different functions of our website. The Landing page would have tabs for signing up or logining. 
   
   For admins, they would have access to functionalities of the admin list of spots, adding spots, editing/deleting spots, and verifying users on a whitelist. For verified users, they would have access to the list of spots, add spots, edit spots, profile edit, and feedback. For unverified users, they must be approved by an admin. The unverified user would have access to the list of posts and an add spot which would be verified by an admin. 
   
   Both the admin and verified user would have access to a button that would indicate how filled a study spot is from empty, partially filled, and filled. Verified and Admin users can provided reviews and feedback.

# Development History

# Contact Us

 - Isaac Lee: ilee72@hawaii.edu

 - David Liang: david947@hawaii.edu

 - Andrew Millard: millarda@hawaii.edu


