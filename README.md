# Asynchronous JavaScript Patterns

### An ongoing quick reference guide for asynchronous code patterns

### ToC:
[XMLHttp Request](#XMLHttp-Request)<br>
[Fetch Promises and Async-Await](#Fetch-Promises-and-Async-Await)

## XMLHttp Request

This first section will cover the traditional way of pulling data from an API or JSON file.  For new devs including myself it is a handy reference when working on older code basis.  The below example is a complete single GET request pattern which includes error catching via a dynamic callback.  This example is sourced from a YouTube tutorial from Shaun AKA Net Ninja. <a href="https://www.youtube.com/playlist?list=PL4cUxeGkcC9jx2TTZk3IGWKSbtugYdrlu" target="_blank">Link to this playlist</a> 

![image](https://user-images.githubusercontent.com/73107656/107866472-54e75400-6e69-11eb-96f3-ea7bac285bdc.png)

The whole request is wrapped within an arrow function 'getTodos' that takes in a callback. The job of the callback is to run different code depending on the success or failure of the request.

Lets step through this code and break it down:

First up the XMLHttpRequest is aynchronous so the JavaScript thread adds the event listener but does not enter the callback.  Instead request.open is called with the 'Get' and endpoint and then request.send is invoked. At this point the request is live and the event listener is waiting. When the state changes the event listener callback is invoked:

The second argument on the event listener is a callback arrow function that checks for both state to be 4 and status to be 200.  If true then the JSON response has been recieved and the JSON text is changed to an object and saved to 'data' and the callback (getTodos argument) is invoked to run the succes code.

If the state is 4 but the status is not 200 then the callback function runs the code for a failed request.  

Below the getTodos function expression getTodos is called and in this case the callback function is defined at this point with an arrow function. 

Moving on, I have not included the practical pattern for promises ('then' and 'catch')because promises were built upon with Async/Await and also built into the Fetch API.  For this reason lets go straight to the current best practise as of writing.  

## Fetch Promises and Async-Await

The below example completely replaces the XMLHttpRequest above, it contains so little code but has so much going on.  First here is the core pattern, below this we will step through the code detailing how it all works. Again this example is sourced from the Net Ninja's Youtube tutorial, link to playlist at the top of the page.

![image](https://user-images.githubusercontent.com/73107656/107868885-1741f580-6e80-11eb-8033-f1abd41c3ab6.png)

1. On line 3 the getTodos asynchronous function expression is saved to memory.  
2. The JavaScript thread then moves down to line 17 and getTodos is invoked. 
3. The JavaScript thread sees that getTodos is an asynchronous function, passes it to the browser and then continiues on down the file.
4. Meanwhile within the getTodos function:
    - 'response' is declared and assigned the promise of the returned response. At this point the keyword await stops the function execution thread until the response comes back from the fetch call. 
    - Once returned the response is checked, if not status 200, throw an error to be caught (explained later)
    - Declare a variable 'data' and assign it a promise using the await key word. This is necessary as the .json() method returns a promise.  Once returned, 'data' is assigned the object.
5. Then finally the data object is returned and the execution thread goes to line 18 and executes the .then callback
6. If for any reason an error occurs during the process the .catch is triggered.

Lets dive into the error handling:

The if statement on line 7 catches any status errors, in this case the '.catch' on line 19 is triggered. This catches issues such as a broken or incorrect end point.  This is put in place so a more specific error message code can be run regarding response.

If there are any state issues for example a faulty JSON file, an error is traggered and the .catch handles this as normal.

