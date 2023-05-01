# Putting it All Together: Client-Server Communication

## Learning Goals

- Understand how to communicate between client and server using fetch, and how
  the server will process the request based on the URL, HTTP verb, and request
  body
- Debug common problems that occur as part of the request-response cycle

## Introduction

Just like the last lesson, we've got code for a React frontend and Rails API
backend set up. This time though, it's up to you to use your debugging skills to
find and fix the errors!

To get the backend set up, run:

```console
$ bundle install
$ rails db:migrate db:seed
$ rails s
```

Then, in a new terminal, run the frontend:

```console
$ npm install --prefix client
$ npm start --prefix client
```

Confirm both applications are up and running by visiting
[`localhost:4000`](http://localhost:4000) and viewing the list of toys in your
React application.

## Deliverables

In this application, we have the following features:

- Display a list of all the toys
- Add a new toy when the toy form is submitted
- Update the number of likes for a toy
- Donate a toy to Goodwill (and delete it from our database)

The code is in place for all these features on our frontend, but there are some
problems with our API! We're able to display all the toys, but the other three
features are broken.

Use your debugging tools to find and fix these issues.

There are no tests for this lesson, so you'll need to do your debugging in the
browser and using the Rails server logs and `byebug`.

**Note**: You shouldn't need to modify any of the React code to get the
application working. You should only need to change the code for the Rails API.

As you work on debugging these issues, use the space in this README file to take
notes about your debugging process. Being a strong debugger is all about
developing a process, and it's helpful to document your steps as part of
developing your own process.

## Your Notes Here
    For all of the debuggings, I mainly referred to the errors that my rails server and browser console spat back at me.  I tried deleting a toy and saw the following errors:

- Add a new toy when the toy form is submitted:
    browser: 500 (Internal Server Error)
    server console: NameError (uninitialized constant ToysController::Toys):

    While the browser told me nothing, as 500 is a blanket error message that doesn't directly discern the issue, the puma console told all.  The #create method in the toys_controller was trying to call #create on a Toys class, rather than a Toy class.  Class names are always singular where as their tables are plural.


- Update the number of likes for a toy:
    browser: caught (in promise) SyntaxError: Unexpected end of JSON input
    server console: 204 No Content

    This told me something was wrong with toys_controller.rb file.  Upon inspection, the #update method did not render the updated json. Easy fix.


- Donate a toy to Goodwill (and delete it from our database):

    browser:  http://localhost:4000/toys/9 404 (Not Found)
    server console: ActionController::RoutingError (No route matches [DELETE] "/toys/9"):

    The first thing I checked was the ./config/routes.rb file.  I very quickly realized the issue, which was that the :destroy resource was not included in the permitted methods.  Pretty easy fix here.
