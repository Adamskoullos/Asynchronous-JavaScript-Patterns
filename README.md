# Asynchronous JavaScript Patterns

### An ongoing quick reference guide for asynchronous code patterns

### ToC:
- [XMLHttp Request](##XMLHTTP-Request)
- [Fetch & Async/Await](##Fetch-&-Async/Await)

## XMLHttp Request

This first section will cover the traditional way of pulling data from an API or JSON file.  For new devs including myself it is a handy reference when working on older code basis.  The below example is a complete single GET request pattern which includes error catching via a dynamic callback.  This example is sourced from a YouTube tutorial from Shaun AKA Net Ninja. <a href="https://www.youtube.com/playlist?list=PL4cUxeGkcC9jx2TTZk3IGWKSbtugYdrlu" target="_blank">Link to this playlist</a> 

![image](https://user-images.githubusercontent.com/73107656/107866472-54e75400-6e69-11eb-96f3-ea7bac285bdc.png)

The whole request is wrapped within an arrow function 'getTodos' that takes in a callback. The job of the callback is to run different code depending on the success or failure of the request.

Lets step through this code and break it down:

First up the XMLHttpRequest is aynchronous so the JavaScript thread adds the event listener but does not enter the callback.  Instead request.open is called with the 'Get' and endpoint and then request.send is invoked. At this point the request is live and the event listener is waiting. When the state changes the event listener callback is invoked:

The second argument on the event listener is a callback arrow function that checks for both state to be 4 and status to be 200.  If true then the JSON response has been recieved and the JSON text is changed to an object and saved to 'data' and the callback (getTodos argument) is invoked to run the succes code.

If the state is 4 but the status is not 200 then the callback function runs the code for a failed request.  

Below the getTodos function expression getTodos is called and in this case the callback function is defined at this point with an arrow function. 


## Fetch & Async/Await

The Fetch API returns a promise, this means by using 'then' and 'catch' we can chain promises together, essentially the process works in three stages:

1. Make the request and recieve the JSON response, turn it into data object and return the data object.
2. Undertake '.then's to execute any code on the returned data object each time returning the output as the input to the next '.then'
3. Catch any errors with '.catch' and run error code

The below example uses the ES6 Fetch API in conjunction with Async/Await. Lets have a look and then go through it:
