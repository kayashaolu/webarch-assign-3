# Assignment 3
Build and consume your own API

In this assignment you will be tasked with creating your very own api, which you will then modify your web server in assignment 2 to use. You will also learn how to connect to a database to insert and retrieve data from a persistent store of information.

What you will be doing is moving the title and content of your blog from your web server to a database. You will be running two servers: a web server that will be hosting your webpage as in assignment 2, and a server that will be hosting your API that will be delivering the content of your blog to your web server.

There are a series of steps that you'll need to complete:

## Copy your assignment 2 web server code to it's own repository
While we are grading your assignment 2 code, you will need to modify your webserver to call your API to get the title and content of your blog. So your first step is to craete your webarch-assign-3-webserver repo from the customary link and then copy your assignment 2 code to this repo. Be sure that you are able to install all relevant node packages using the command ```npm install``` (ensure that your package.json file has all of the dependencies that you need)

## Set up your API in your webarch-assign-3-api repo

Next you will need to set up your API in a separate repo called webarch-assign-3-api. You will need at least the following npm modules installed (be sure to install with the ```--save``` option to modify the package.json).

 - [sqlite](https://www.npmjs.com/package/sqlite3) A full file based database
 - [express](http://expressjs.com) The Express framework we used for our webserver
 - [request](https://www.npmjs.com/package/request) Enables you to easily send HTTP request from the server
 
**Note:** Be sure to run your server on a different port (other than 3000). If you run two servers on the same port you will get an error during the startup of the second server

## Create two endpoints to the API in your webarch-assign-3-api repo

### POST /blog
You will first create an endpoint with the path /blog that would do the following:
 - Create a table using sqllite (**only** if it doesn't exist) in a static file with the following fields
  - **slug**: the unique slug of the blog post. For example, the blog post: A Mindful Shift of Focus should have the slug: a-mindful-shift-of-focus.md
  - **title**: the title of the blog post. For example for the "A Mindful Shift of Focus.md" post, the title should be: "A Mindful Shift of Focus"
  - **content**: the content of the blog post. This should contain all of the HTML content that is after the title of your pages. The goal is to grab the content of the blog post from this api instead of hardcoding it in your web server.
 - Accept the following body parameters with content type "application/json":
  - slug
  - title
  - content
 - Insert the new slug, title, and content into your database
 
### GET /blog

You will then create another post that will send the data of the blog as json

 - It will accept the following query parameters:
  - slug (i.e. "a-mindful-shift-of-focus")
 - It will return a json response that will look like the following:
```json
  {
    slug: "a-mindful-shift-of-focus",
    title: "A Mindful Shift of Focus",
    content: "<h2>By Leo Laparte</h2>..."
  }
```

## Populate your database

You will now need to populate your database by sending POST /blog requests to your API to populate your database. I would suggest creating a json file for each blog post (i.e. "a-mindful-shift-of-focus.json") and writing the contents of a single blog post in the file.

For example, in the file "a-mindful-shift-of-focus.json", the contents could be something like this:
```json
  {
    slug: "a-mindful-shift-of-focus",
    title: "A Mindful Shift of Focus",
    content: "<h2>By Leo Laparte</h2>..."
  }
```
You can now use curl to send a request to your api, sending a-mindful-shift-of-focus.json as the data:

```bash
curl -vX POST https://localhost:3001/blog -d @a-mindful-shift-of-focus.json --header "Content-Type: application/json"
```

## Modify your webarch-assign-3-webserver repo to call your webarch-assign-3-api to retrieve the blog information

You will now need to delete all of the hardcoded blog content and send a request from your web server to your api to retrieve the blog title and content. You will need to use the request npm package in order to accomplish this. Consider the GET requests that you would need to make in order to retrieve the title and the content data of each blog post.

# Extra Credit

Create another webpage on your web server with the path "/admin" and create a form where you can dynamically add new blog posts to your blog without editing any more code.

The form should have the following fields:
 - Slug
 - Title
 - Content
 
This form should be submitted to the web server, and the web server will need to use the POST /blog endpoint in order to modify the database. After this change the web server should be able to view the new blog post.

For example, if a user submitted the form with the content:
 - Slug: test-blog-post
 - Title: Test blog post
 - Content: ```<h1>This is my first dynamic blog post</h1>```
 
Then if the same user went to http://localhost:3000/test-blog-post, they should see a blog post with Title "Test blog post" and with the content ```<h1>This is my first dynamic blog post</h1>```
