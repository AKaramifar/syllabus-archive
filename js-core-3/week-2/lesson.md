# JavaScript Core III - Week 2

---

**Teaching this lesson?**

Read the Mentors Notes [here](./mentors.md)

---

## Learning Objectives

- The learner should understand what the acronym API means
- The learner can define what an API's purpose is and why it is useful
- The learner should be able to edit the structure of a API URL and to change the data retrieved from the server
- The learner should be able to define what a `Promise` is
- The learner should understand what `fetch` is and what it is used for
- The learner should be able to use `fetch` to retrieve `JSON` from an API
- The learner should be able to parse the `JSON` and extract data from it
- The learner should be able to use `DOM` manipulation to add content to the `DOM`
- The learner should understand `window.onload` and `document.onload` and should be able to assign functions to run at specific life cycle events

## Agenda

The purpose of this class is to introduce to the student:

1. Debugging Quiz
2. How the web works
3. What are `APIs` and how to interact with them
4. How to use the `fetch` API to do AJAX calls

## 1. Debugging Quiz

Let's see what you remember from last week!

_Answer in a thread on Slack_

1. What are the four questions we ask ourselves in the Debugging Framework?
2. What are three of the tools we could use to debug our programs?
3. What is a `syntax` error?
4. What is a `reference` error?
5. What is a `type` error?

## 2. How the web works - quick recap

In this session we will look at how computer talk to each other using the web.

At the core of the web is the URL, which stands for Uniform Resource Locator. We use the term resource to mean anything that a server might return such as webpage, CSS, JavaScript, image, data etc. A good way to think of a URL is as an address. It allows us to refer to webpages, images, data etc. that is stored on servers elsewhere.

### Methods

The main methods used to send requests on the web are `GET` and `POST`. However, later in the course when we look at building APIs using Node we will also look at other methods such as `PUT`, `PATCH` and `DELETE`.

A `GET` method is a way of asking a server for a webpage, resource or a piece of data. For example, when we type a URL into a browser and submit it. The browser will send a `GET` request.

A `POST` method is used to send data to a server.

The main difference between `GET` and `POST` is that a `POST` method has a body, that is it can contain some data that we are sending. Whereas a `GET` does not have a body since we use it to request data.

### Headers

Each HTTP request and response sent has metadata, information about the data, at the beginning called a `header`. The `header` contains information such as a:

- status code indicating whether a request was successful
- content type which indicates what the request or response contains

as well as lots of other things we won't cover here

### Status codes

Each response returned needs to contain a `status` code which tells the client whether the request was successful. If the request succeeded the response code will be `200`. If the resource you tried to access was not found the response code used is `404`.

Some status codes you may have come across before are:

- `200` ok. Request was successful
- `301` moved permanently. Used to redirect request when moved permanently
- `401` Unauthorised. User credentials were not supplied
- `404` Not found
- `500` Internal server error

The response codes can be grouped into categories

- 1xx: Informational
- 2xx: Success
- 3xx: Redirection
- 4xx: Client Error
- 5xx: Server Error

If you want a fun look at HTTP codes, take a look at [https://httpstatusdogs.com/](https://httpstatusdogs.com/) or [https://http.cat/](https://http.cat/) if you are cat person. For a technical perspective take a look at [https://en.wikipedia.org/wiki/List_of_HTTP_status_codes](https://en.wikipedia.org/wiki/List_of_HTTP_status_codes).

### Content type

When sending data across the web, we need to specify in the header what the request or response contains. To do that, the `content-type` header is used. That way the receiver knows what to do with the data received.

Common content types include

- `text/html` - HTML web page
- `text/css` - CSS
- `image/jpeg` - JPEG image
- `application/javascript` - JavaScript code
- `application/json` - JSON data

### Exercise

```
In Slack post answers to the following

- What can HTTP headers contain?
- What is the purpose of status codes?
- What can an HTTP message contain?
```

## 3. What are APIs and how to interact with them

### Explanation

- API stands for `Application Programming Interface`
- APIs are created by providers and used by consumers
- It is a specific part of a larger system that can be contacted by other systems, for example from the internet.
- When we connect to an `API` we say that we are connecting to an `Endpoint`
- Some well-known APIs are [Facebook APIs](https://developers.facebook.com/), [Twitter APIs](https://developer.twitter.com/en/docs), [Maps APIs](https://developers.google.com/maps/documentation) and many many more
- In particular, an API doesn't care what language or technology is used in the consumer or the provider

An API is a set of rules that allow programs to talk to each other. The developer creates the API on the server and allows the client to talk to it. An example of a server is the application on a computer hosting a website and an example of a client is the browser on the phone trying to access the website.

#### Why do we need APIs?

Imagine that I am a big social network and I want to give developers all over the world access to the data on the people on my website.

What are some problems that I would have with sharing my data with everyone?

1. Some of the information that I have is public (for example, peoples names) whilst other information I have is private (for example, email addresses). I want to make sure that I only ever give developers access to peoples names but never to their email addresses - otherwise they could send them spam email.
2. I want to make sure that when developers ask for my data I can control who has access to it. I like that my users data is being used to make their lives better but I don't like it when companies try to sell them new stuff they don't need.
3. Some developers might want to change some of the users details on my social network and this would get very messy quickly if people where allowed to change whatever they wanted

An API is a special type of program what acts as a **gatekeeper** to all of this information. Having an API means that I can control which information is shared about my users and who it is shared with. Perfect!

#### Types of APIs:

- Internal: for other development teams within an organisation to consume (e.g. microservices).
- Semi-private: for clients who paid for the API.
- Public: for everyone on the web (but may or may not need an account to use).

##### Examples

Here is the API endpoint for Transport For London

https://api.tfl.gov.uk

The data from this endpoint will be used by many apps that you use every day - Google Maps and Citymapper to name two.

This endpoint will get location of all of the Bikepoints in London.

https://api.tfl.gov.uk/BikePoint

That's a lot of bikes! It would be better if we could search for a location. Luckily this API let's us search for places.

https://api.tfl.gov.uk/BikePoint/Search?query=Clerkenwell

This API also has lots of other endpoints that we can use to get other data. For example, lets find the Air Quality of London.

https://api.tfl.gov.uk/AirQuality

As you can see the URL changes the data that we get from the API. This can be broken down like this

![](./assets/api-breakdown.png)

### Exercise

**Task (5 mins):**

```
Let's use the TFL API to get data about the London Underground

https://api.tfl.gov.uk

1.  Which endpoint would you use to find out if there is a disruption on the 'central' line? 2. Does the 'central' line have a disruption right now? Name another line that has a disruption on it.

Hint: Use your browser to access the endpoints
```

#### Recap

**Question:**

    Which of the following statements below about APIs is false?

    A) Public APIs can be accessed by anyone on the Internet.
    B) You must use JavaScript to access an API.
    C) APIs can control access to data or features of an application.
    D) You can change data via an API.

**Question:**

    Give an example of a company that uses an API to allow access to their data.

**Question:**

    What is the `myapi/` part of a url called in this url?
    http://www.google.com/**myapi**/

## 4. How to use `fetch` to do network requests

### Explanation

- Fetch is a function to request resources from other places on the web (using the [HTTP](https://developer.mozilla.org/en-US/docs/Web/HTTP/Overview) protocol)
- Fetch is defined in the browser which means it can be used without using an external library (`window.fetch`)
- Fetch is available in nearly all browser but it's good to check which ones it won't work in
  - We can use this website to help us - [caniuse.com](https://caniuse.com/#feat=fetch))
- Fetch API documentations by Mozilla [link](https://developer.mozilla.org/en-US/docs/Web/API/Fetch_API/Using_Fetch)

### How does fetch work?

Fetch uses a JavaScript pattern called "Promises" which has a very specific structure.

You can think of a Promise as you would think of a promise you make to another person - you make a promise that something will happen in the future. For example - I promise to call you later, I promise to go to the shops and buy milk later.

Using Promises allows us to schedule functions to be called after some asynchronous code finishes running. We can specify different functions depending on whether the asynchronous code was successful or ran into an error.

Promises can make it easier to split our code into small functions and make code easier to read. They also make it easier to handle errors.

In this example we

1. Get the `Promise` that we will get the milk from the shops (this could take a long time so it's good that it's a `Promise`!)
2. When the milk has arrived from the shop `then` I should drink it and `return` the bottle so I can do something else with it
3. When I've drank the milk `then` I should recycle the bottle
4. If anything goes wrong with those steps I should `catch` the error and `warn` everyone what happened

### Example

```javascript
getMilkFromShops
  .then((milk) => {
    console.log(`I've got the milk`);
    milk.drink();
    return milk.bottle;
  })
  .then((bottle) => {
    console.log(`I'm going to recycle the bottle`);
    bottle.recycle();
  })
  .catch((error) => console.warn("Oh no, I dropped the milk"));
```

#### Example 1

_Live Coding Exercise_

Let's step through how the Fetch function is used and what it is comprised of

```javascript
//Retrieve the JSON
fetch("https://cat-fact.herokuapp.com/facts")
  // Get the response and extract the JSON
  .then(function (response) {
    return response.json();
  })
  // Do something with the JSON
  .then((headlines) => {
    console.log(headlines);
  })
  // If something goes wrong
  .catch((error) => console.log(error));
```

#### Example 2

_Live Coding Exercise_

Wouldn't it be cool to make a new friend with just the click of a button?

Write a function that makes an API call using `fetch` to `https://www.randomuser.me/api`

- The function should make an API call to the given endpoint: `https://www.randomuser.me/api`
- Log the received data to the console
- Incorporate error handling
- Show how you can build a profile page for the user using the DOM
  - Add a name
  - Add a profile picture
  - Add some styling using CSS

### Error handling

We saw earlier that each HTTP response contains an status code which indicates if our request was successful or not. If the our request failed we usually want to handle it appropriately.

We can handle these errors gracefully in your code by checking the `status` and `statusText` value of the response:

```javascript
fetch("https://httpstat.us/500")
  .then((response) => {
    if (response.status >= 200 && response.status <= 299) {
      return response.json();
    } else {
      throw new Error(
        `Encountered something unexpected: ${response.status} ${response.statusText}`
      );
    }
  })
  .then((jsonResponse) => {
    // do whatever you want with the JSON response
  })
  .catch((error) => {
    // Handle the error
    console.log(error);
  });
```

### Exercises

#### Exercise 1

In groups the students should create a page of details about the United Kingdom.

The API endpoint can be found [here](https://restcountries.eu/rest/v2/name/Great%20Britain?fullText=true)

The website should include

- The name of the country
- The country's capital city
- An unordered list of the country's name in other all of the other returned languages

##### Steps

Example `html` and `javascript` files can be found in the section below

1. Create a `HTML`, `CSS` and `JavaScript` file to hold different types of code
2. In your `HTML` file, write a simple basis for your website (e.g. [this](https://www.sitepoint.com/a-basic-html5-template/))
   - Make sure all of your `HTML`, `CSS` and `JavaScript` files are linked together!
3. Write a function using fetch that retrieves the `JSON` from the _Country API_
   - To make sure it's working print the JSON to the console using `console.log()`
4. Create a `h1` tag on the website using DOM manipulation and add the country's name inside it
   - Go back to [Week 5](../../js-core-2/week-2/lesson.html) if you need a reminder
5. Create a `h2` tag on the website using DOM manipulation and add the capital city's name inside it
6. Create a `ul` tag on the website using DOM manipulation
   - For each of the translated names in the JSON, add a `li` tag
7. Uncomment the lines inside `onLoad()` to load other countries details!

**Extra**

- Load the country's flag into an `img` tag
- Add CSS to make your website look really nice
- Add other information from the JSON to your Country Details

##### Framework Files

**index.html**

```html
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="utf-8" />

    <title>Country Information</title>
    <meta name="description" content="Country Information" />
    <meta name="author" content="Code Your Future" />

    <link rel="stylesheet" href="styles.css?v=1.0" />
  </head>

  <body>
    <div id="content"></div>
    <script src="scripts.js"></script>
  </body>
</html>
```

**scripts.js**

```javascript
// This function should retrieve the JSON from the `countryURL` and then call onCountryDataReceived() with the JSON
function getData(countryURL) {}

function onCountryDataReceived(country) {
  addCountryName(country);
  addCountryCapital(country);
  addNameInOtherLanguages(country);
}

// This function should take the JSON for the country and put a H1 tag on the screen containing its name
function addCountryName(countryData) {}

// This function should take the JSON for the country and put a H2 tag on the screen containing its capital city
function addCountryCapital(countryData) {}

// This function should take the JSON for the country and put UL and LI tags on the screen with the countries name translated into other languages
function addNameInOtherLanguages(countryData) {}

function getContentDiv() {
  return document.querySelector("#content");
}

function onLoad() {
  getData(
    "https://restcountries.eu/rest/v2/name/Great%20Britain?fullText=true"
  );

  /** Remove this line when you have completed the task

    getData("https://restcountries.eu/rest/v2/name/France?fullText=true");

    getData("https://restcountries.eu/rest/v2/name/Germany?fullText=true");

    getData("https://restcountries.eu/rest/v2/name/Spain?fullText=true");

    getData("https://restcountries.eu/rest/v2/name/Portugal?fullText=true");

    getData("https://restcountries.eu/rest/v2/name/Hungary?fullText=true");

    getData("https://restcountries.eu/rest/v2/name/Russia?fullText=true");

    */ // Remove this line when you have completed the task
}

window.onload = onLoad;
```

##### Finished Example

You can find the finished example of this website [here](js-core-3\week-1\completed_country_website).

##### Recap

**Question (5 mins):**

```
Complete the following sentence:

Fetch is a web API that allows you to **\_** from **\_**.
```

**Task (5 mins):**

```
Complete the rest of this code to connect to the following API: `https://dog.ceo/api/breeds/image/random`

    fetch(_____)
    .then(_____)
    .then(body => console.log(body))
    .catch(error => console.log(error));

1. Post your code on Slack
2. Post the image you retrieved on Slack
```


{% include './homework.md' %}

## Quiz (10 - 15 minutes)

1. What does API stand for?
2. Give an example of a semi-private API and a public API
3. What is the difference between a GET and a POST request?
4. How might you add a new `h1` tag containing a string obtained from an API onto your webpage?
   Assume that the `content` variable defines the obtained string, and the `container` variable holds the DOM node that is to be inserted into.
5. What is the difference between `window.onload` and `document.onload`?
6. What does `fetch` return when called?
7. When are the functions in the `then` and `catch` methods of a promise executed?
8. What are two useful bits of information the header of an HTTP response contain?
9. What do the following error codes mean?
   1. 200
   2. 201
   3. 404
10. What should you do if you receive a 429 status code from an API during development? How might you resolve this?
