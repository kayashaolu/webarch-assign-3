# Assignment 3
Build and consume your own API

In this assignment you will be tasked with creating your very own api, which you will then modify your webserver in assignment 2 to use. You will also learn how to connect to a datatabse to insert and retrieve data from a persistent store of information.

What you will be doing is moving the title and content of your blog from your webserver to a database. You will be running two servers: a web server that will be hosting your webpage as in assignment 2, and a server that will be hosting your API that will be delivering the content of your blog to your webserver.

There are a series of steps that you'll need to complete:

## Copy your assign 2 webserver code to it's own repository

While we are grading your assignment 2 code, you will need to modify your webserver to call your API to get the title and content of your blog. So your first step is to craete your webarch-assign-3-webserver repo from the customary link and then copy your assignment 2 code to this repo. Be sure that you are able to install all relevant node packages using the command ```npm install``` (ensure that your package.json file has all of the dependencies that you need)

## Set up your API in your webarch-assign-3-api repo

Next you will need to set up your API in a separate repo called webarch-assign-3-api. You will need at least the following npm modules installed (be sure to install with the ```--save``` option to modify the package.json).

 - [sqlite](https://www.npmjs.com/package/sqlite3) A full file based database
 - [express](http://expressjs.com) The Express framework we used for our webserver
 - [request](https://www.npmjs.com/package/request) Enables you to easily send HTTP request from the server
 
**Note:** Be sure to run your server on a different port (other than 3000). If you run two servers on the same port you will get an error on starting up the second server

## Create two endpoints to the API in your webarch-assign-3-api repo

### POST /blog

You will first create an endpoint with the path /blog that would do the following:
 - Create a table using sqllite (**only** if it doesn't exist) in a static file with the following fields
  - **slug**: the unique slug of the blog post. For example, the blog post: A Mindful Shift of Focus should have the slug: a-mindful-shift-of-focus.md
  - **title**: the title of the blog post. For example for the A Mindful Shift of Focus.md post, the title should be: "A Mindful Shift of Focus"
  - **content**: the content of the blog post. This should contain all of the HTML content that is after the title of your pages. The goal is to grab the content of the blog post from this api instead of hardcoding it in your webserver.
 - Accept the following body parameters with content type "application/json":
  - slug
  - title
  - content
 - Insert the new slug, title, and content into your database
 
### GET /blog

You will then create another post that will send the data of the blog as json

 - It will accept the following query parameters:
  - slug
 - It will return a json response that will look like the following:
```json
  {
    slug: "a-mindful-shift-of-focus",
    title: "A Mindful Shift of Focus",
    content: "<h2>By Leo Laparte</h2>..."
  }
```

## Populate your database

You will now need to populate your database by sending POST /blog requests to your API to populate your database. You can use curl for this, and add the content of your five blog posts to your database table through making one time curl commands:

```bash
curl --data "slug=a-mindful-shift-of-focus&title=A Mindful Shift of Focus&content=<h2>By Leo Laparte</h2>..." https://localhost:3001/blog
```

## Modify your webarch-assign-3-webserver repo to call your webarch-assign-3-api to retrieve the blog information

You will now need to delete all of the hardcoded blog content and send a request from your webserver to your api to retrieve the blog title and content. You will need to use the request npm package in order to accomplish this.
 
 
