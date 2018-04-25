# Intro to REST with Express

### Objectives
1. Understand why RESTful architecture is the industry standard
1. Understand what we mean by a *resource*
1. Create a small, basic RESTful API in Express using hard-coded data

# Understanding REST

#### REST stands for Representational State Transfer. 
- The "State" here refers to the data we are operating on.  
- Transfer is talking about GETing, POSTing, PUTing, or PATCHing.

#### REST is a Style of Architecture
- In computer science architecture is how we design our applications. API design has a lot to do with how our routes are set up. REST is mostly about **routes**

#### There is more to REST than just routes, but that's all we need to know for now. 

![](https://www.geekstone.org/media/1084/batman.png)


## Before REST

**Billy Backend:** "I wrote a backend to serve up the baseball scores you needed on your front end!"

**Franny Frontend:** "But I'm still getting a 404!"

**Billy Backend:** "Let me see your request!"

**Franny Frontend:**
```js
$.get( "https://www.baseball-api/getScores/cleveland-indians-scores-for-yesterday", function( data ) {
  $( ".result" ).html( data );
  alert( "Load was performed." );
});
```

**Billy Backend:** "Ah, well I wrote the route differently..."


```js
app.get('/get-the-dang-scores/clevelandIndiansScores/jan-3-2018', function(req, res){
  ...
});
```

![](https://media.giphy.com/media/y65VoOlimZaus/giphy.gif)

## After REST

**Billy Backend:** "I wrote a backend to serve up the baseball scores you needed on your front end!"

**Franny Frontend:** "I know."

```js
$.get( "https://www.baseball-api/teams/1/scores/?date=2018-01-02", function( data ) {
  $( ".result" ).html( data );
  alert( "Load was performed." );
});
```

![](https://media.giphy.com/media/s3tpyHuSSr98A/giphy.gif)


## Resources 

In communicating with APIs, every piece of information is a *__resource__*. These are the _nouns_ of your program.

There are only a few _verbs_ that you can perform on these resources:

- GET
- POST
- PUT
- PATCH

Let's just look at GET for the time being.

### Let's look at a concrete example:

Say we want to create our own RESTful API that for comedy video objects.

We want urls that can return the following resources:

1. all videos in our database
1. an individual video based on the video ID
1. info on all comedians
1. info on a specific comedian based on comedian ID
1. all videos by a specific comedian
1. specific video by a specific comedian

In Express this will be done using JavaScript, but we will be PSEUDOCODING to remove the element of syntax from the equation so we can focus on REST itself. Here is pseudocode of routes for the above API:

// My pseudo-code routes:

GET ALL VIDEOS "/videos"

GET VIDEO BY ID "/vidoes/:id"

GET ALL COMEDIANS "/comedians"

GET COMEDIAN: "/comedians/:id"

GET ALL VIDEOS BY COMEDIAN "/comedians/:id/videos"

GET VIDEO BY COMEDIAN "/comedians/:id/videos/:id"

__Note__: In the above example any code preceded by a colon is a __variable__ which would be filled in by specific information in the real requests. This is typical syntax in talking about variables in routes.

That's all there is to it!

### Your turn!

Say we want to create an API similar to the one above, that returns data about icecream. We need to be able to get information about specific flavors, as well as all of the flavors we have in our database. We also want to get some flavors by category, like "fruity", "gluten free", or "chunky". Finally we want to add one POST route that allows users of our API to add new flavors!

&#x1F535; **Activity**
```
* Write Pseudocode like the above. Be creative, and follow RESTful conventions and best practices.
* 7 minutes
```

### JSON API best practices

A REST API can serve up JSON or web pages. If you are building it to serve JSON, you might want to make your data available to others! There are a couple easy conventions that should be followed when doing so, so you can safely update your API without breaking people's code!

1. scope your api in an 'api' namespace.
2. version your api

```
https://www.comedyvideos.com/api/v2/comedians/2/videos
```

Developers will thank you forever. 

![](https://media.giphy.com/media/3oz8xIsloV7zOmt81G/giphy.gif)


### RESTful Routes in Express

You have already build some basic routes that look like this:

```javascript
const express = require('express'); 
const app = express();

app.get('/somedata', (request, response) => {
    response.send('here is your information');
});

app.listen(3000, () => {
    console.log("I am listening");
});
```

Now let's take your pseudocode and turn it into real routes! Don't worry about what gets returned yet, just have each route do: 
```js
res.send("some data")
```

&#x1F535; **Activity**
```
* create a file called server.js
* add the express boilerplate from above
* modify your basic get route to return videos instead of somedata
* add routes for 
  - video by id
  - comedians
  - videos by comedian
  - video by comedian
* 10 minutes

```

Note: We won't be covering the videos by comedian today, because for that we need to learn how to create *relationships* in our data. That comes next week!

### Returning Real (hardcoded) Data

Soon we will learn to connect to a database so we can persist data, even if our app crashes! But for now let's work with some hardcoded arrays. We will start simple, by making our `/videos` and `/comedians` routes work.

We will create an array of video objects and res.send that array!

```javascript
const express = require('express'); 
const app = express();
const videos = [
    {id: 1, title: "America is the Greatest Country in the United States", url: "https://www.netflix.com/watch/80208273?trackId=13752289&tctx=0%2C0%2C"},
    {id: 2, title:, "Micheal Che Matters", url: "https://www.netflix.com/watch/80049871?trackId=13752289&tctx=0%2C0%2C"},
    {id: 3, title:, "Baby Cobra", url: "https://www.netflix.com/watch/80101493?trackId=13752290&tctx=1%2C1%2C"}
  ];

app.get('/videos', (request, response) => {
    let videosJSON = JSON.stringify(videos)
    response.send(videosJSON);
});

app.listen(3000, () => {
    console.log("I am listening");
});
```

Let's go to `localhost:3000/videos` and see our JSON API in action! 

![](https://media.giphy.com/media/oMpJql97PCGHe/giphy.gif)

Awesome! Now you guys go ahead and add create a comedians array that returns comedian objects, with names and ids!

&#x1F535; **Activity**
```
* modify your basic get route to return and array of comedian objects instead of 'somedata'
* 3 minutes

```

### Getting by ID

OK! Now we want to return individual videos by ID. For that we need to write some JavaScript. 

```
app.get('/videos/:id', (request, response) => {
    let video = // WHAT GOES HERE??
    let videoJSON = JSON.stringify(video);
    response.send(videoJSON);
});
```

&#x1F535; **Activity**
```
* Pair with a partner!
* How can we pull an element out of the array by id? write the code so our video by ID route works!
* You should be able to go to localhost:3000/videos/1 and see the first video object
* You can assume that the ID's are all unique
* BONUS: return a friendly message if there is no video by the id entered in the URL
* 12 minutes

```

### Conclusion

- REST is a style of web architecture that creates a standard for all APIs no matter what resource they provide
- We can write RESTful routes in Express that can serve collections, and items from a collection by ID
- In the future we will be able to serve up resources that have relationships with other relationships
