Note: This document is a work in progress

## Table of Contents
- [Motivation](#motivation)
- [Architecture Overview](#architecture-overview)
- [Front-end Architecture](#front-end-architecture)
  - [Core Technologies](#core-technologies)
  - [Pages](#pages)
    - [Login/Registration Page](#loginregistration-page)
    - [Home Page](#home-page)
    - [Folder Detail Page](#folder-detail-page)
    - [Browse Tweets Page](#browse-tweets-page)
  - [State Management](#state-management)
  - [Testing](#testing)
  - [Accessibility](#accessibility)
  - [Performance](#performance)
  - [Routing](#routing)
- [REST API](#rest-api-architecture)
  - [Routes](#routes)
  - [Authentication](#authentication)
  - [Error Handling](#error-handling)
- [Code Snippets](#code-snippets)


## Motivation 
This application can be thought of as an enhancement to Twitter's bookmarks feature. It allows users to easily import tweets from twitter, save them to their private account, and organize them into searchable sub-folders. Additionally, the tweets are saved forever, even if the tweet on the app is deleted or the tweet-author's account is deactivated.

The in-app Twitter bookmarks feature is very lacking and the goal is to build something that makes it much easier to archive your favourite tweets.

## Architecture Overview

![image](https://user-images.githubusercontent.com/20101874/222025549-8460ea77-a045-494a-9fa3-a5318d6fef7e.png)

This is a MERN stack application. 

I chose React for the frontend because of my familiarity with it and its many inherent advantages, such as the relative ease of handling state management, its reusable component based architecture, its performance benefits, and its excellent libraries. I used TypeScript because I find it saves me time by avoiding bugs as I write my code, and is self-documenting, making the code more readable and maintainable. I also like that it allows you to skip having to use PropTypes, as you can just define an interface for your component props.

JWT is used for authentication, and the REST API is built using Node.js and Express. This API interacts with the Twitter API, enabling simple fetching of tweet data.

I chose to use MongoDB to store user data because of my familiarity with it and its simplicity, flexibility, and scalability. It also has the benefit of a free and easy to set up cloud platform, MongoDB Atlas.


## Front-end Architecture

### Core Technologies

      React

      TypeScript

      CSS

      Material UI

      React Router

      React Context API

      Axios

      Jest and React Testing Library

### Pages

#### Login/Registration Page
Create account and authenticate.

![image](https://user-images.githubusercontent.com/20101874/222032917-3aa1e161-b643-4a4a-a049-7774efb6fa91.png)


#### Home Page
Form to import tweet (via url or tweet id) and save to folder. Another form to create a new folder. All folders, which are links to folder detail page, displayed below. Can rename and delete folders from here too.

![image](https://user-images.githubusercontent.com/20101874/222032229-0fb5f3e1-efd9-45f9-bcc1-b1eccf22d109.png)

![image](https://user-images.githubusercontent.com/20101874/222034398-ca5a84b1-89e4-4044-badf-e313ad6e23e6.png)


![image](https://user-images.githubusercontent.com/20101874/222034063-cdabd606-c448-4f93-b6db-5b74f9beac67.png) ![image](https://user-images.githubusercontent.com/20101874/222034139-6d6273de-7dc6-4ba7-80e0-7bbd596b379e.png)



#### Folder Detail Page
View all tweets saved to specific folder. Delete and manage tweets from here. Filter and search for tweets within folder.

![image](https://user-images.githubusercontent.com/20101874/222032609-93031bc7-2be1-4845-acd2-4cae81685ec2.png)

![image](https://user-images.githubusercontent.com/20101874/222034338-356abdca-55da-4158-a20e-bbc3e339f8a3.png)


#### Browse Tweets Page
A form to input a twitter user's account name. When submitted, displays a table of all their recent tweets, which can then easily be saved to your folders without the need of copying and pasting urls or ids.

![image](https://user-images.githubusercontent.com/20101874/222032820-378f95f8-4ce6-4330-955b-68dde5b9ce19.png)

![image](https://user-images.githubusercontent.com/20101874/222034485-85cba6dc-7255-469a-bad7-3512c104de5d.png)

### State Management

I didn't find the need to use Redux for this app, so all state is handled with the useState and useReducer hooks. I also use the Context API for authentication and folder data to avoid too much prop drilling.

### Testing
In addition to manual testing, component tests are written in Jest and React Testing Library to improve robustness and prevent bugs getting into to production. TypeScript and eslint are also used in the dev process to catch bugs early.

### Accessibility
The app was built with accessibility in mind. The Material UI component library which is used throughout the app handles a lot of this out of the box. The app is keyboard-navigable and employs a high-contrast colour scheme and proper font-usage to make sure text is always legible. The UX is designed with the goal of being as intuitive as possible. It is responsive so can be used on phones, tablets, laptops, and large monitors, and also works on all major browsers. 

### Performance
Expensive queries are avoided whenever possible (for instance in the browse tweets page a detail view is used to avoid too many fetches all at once). User data is fetched once from the DB on login and then stored in Context for instant lookup afterwards. React's Virtual DOM ensures that UI updates are seamless.

### Routing

React router is used for easy routing. Invalid routes show a page with a button allowing you to return home. If a user tries to access a protected route without authenticated first, they are denied.

![image](https://user-images.githubusercontent.com/20101874/222038023-d91acda8-577f-46a3-a89b-17448a5f2a5f.png)



## REST API Architecture
### Introduction

This REST API provides access to Twitter data and allows users to manage folders of saved tweets. The API is built using Express.js and MongoDB and interacts with the Twitter API using the twitter-api-v2 library.

### Routes
#### POST /folders

This route creates a new folder and saves it to the database.
Request

    name (required): the name of the new folder
    userId (required): the ID of the user creating the folder

Response

Returns the newly created folder object.
#### GET /folders/:userId

This route retrieves all folders associated with a given user ID.
Request

    userId (required): the ID of the user whose folders to retrieve

Response

Returns an array of all folders associated with the given user ID.
#### DELETE /folders/:folderId

This route deletes a folder with the specified ID.
Request

    folderId (required): the ID of the folder to delete

Response

Returns a success message if the folder was deleted successfully.
#### PUT /folders/:folderId

This route renames a folder with the specified ID.
Request

    folderId (required): the ID of the folder to rename
    newName (required): the new name for the folder

Response

Returns the new name of the folder if the rename was successful.
#### GET /tweets/:username/tweets

This route retrieves the 100 most recent tweets by a given Twitter username.
Request

    username (required): the Twitter username to retrieve tweets for

Response

Returns an array of objects, each representing a tweet with the id and text fields.
#### GET /tweets/:username/likedTweets

This route retrieves the 100 most recent liked tweets by a given Twitter username.
Request

    username (required): the Twitter username to retrieve liked tweets for

Response

Returns an array of objects, each representing a liked tweet with the id and text fields.
#### GET /tweets/details/:tweetId

This route retrieves details for a single tweet with the specified ID.
Request

    tweetId (required): the ID of the tweet to retrieve details for

Response

Returns an object containing details for the tweet, including the text, url, username, displayName, and datePosted fields.
#### POST /tweets

This route saves a tweet to a specified folder.
Request

    idOrUrl (required): the ID or URL of the tweet to save
    folder (required): the ID of the folder to save the tweet to
    isUrl (required): a boolean indicating whether the idOrUrl parameter is a URL or ID
    userId (required): the ID of the user saving the tweet

Response

Returns the newly created tweet object.

### Authentication

Dependencies:

      Express.js
      bcryptjs
      jsonwebtoken
      config
      express-validator

This module provides authentication and authorization functionality to the API. It has two routes: one for authenticating a user and returning a JSON web token (JWT) and another for getting information about the currently logged-in user using the JWT. The authentication route accepts a user's email and password, checks whether the email exists in the database, and then compares the password with the hashed password in the database. If the password is correct, it generates a JWT with a payload containing the user's ID and a secret key, and returns the JWT to the client. The get user route requires the client to send the JWT in the authorization header, and then uses the token to extract the user ID from the payload and retrieve the corresponding user object from the database. If the token is valid and the user exists, it returns the user object without the password field.

### Error Handling

If any error occurs while processing a request, the API will respond with an error message and an HTTP status code indicating the type of error. In general, the API will respond with a 404 status code if the requested resource was not found, a 400 status code if the request was malformed or missing required parameters, and a 500 status code if an unexpected error occurred on the server.



## Code Snippets

Folder Page UI code:

![image](https://user-images.githubusercontent.com/20101874/222036533-e056b6ed-d8e2-4b40-9870-8611405829e5.png)

Tweet table context code:

![image](https://user-images.githubusercontent.com/20101874/222035645-765dc8be-15b4-487c-9438-257939abd2bc.png)

An example route, for fetching tweets by username, on the browse tweets page:

![image](https://user-images.githubusercontent.com/20101874/222035811-10cec0e2-3bb6-418a-875f-21a74bc123e6.png)

DB folder schema

![image](https://user-images.githubusercontent.com/20101874/222036653-68374bbe-1a8b-49f0-970b-7d7a11f1314a.png)




