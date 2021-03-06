﻿Freshdesk-nodejs
================

**WARNING**

[![npm version](https://img.shields.io/badge/NPM-deprecated-red.svg)](https://github.com/maxkoryukov/node-freshdesk-api/issues/1)

This repository and corresponding NPM package are deprecated in favor of [**freshdesk-api**](https://www.npmjs.com/package/freshdesk-api).

Useful links:

1. [NPM package](https://www.npmjs.com/package/freshdesk-api):
    * v2 is **preferred**: `npm install freshdesk-api`
    * v1: `npm install freshdesk-api@APIv1`
2. GitHub, `node-freshdesk-api`:
    * [v2 on master branch](https://github.com/arjunkomath/node-freshdesk-api)
    * [v1 in additional branch](https://github.com/arjunkomath/node-freshdesk-api/tree/API-v1)

This module is a simple module to integrate Freshdesk with NodeJS, using the latest API from Freshdesk.

Install
=======

```
$ npm install freshdesk-nodejs
```

Usage
=====

You will need your API Key and the URL of your support website to configure the module. You can refer to the [Freshdesk Documentation](https://support.freshdesk.com/support/solutions/articles/215517) for this

```
var fd = require('freshdesk-nodejs');
var Freshdesk = new fd('https://mydomain.freshdesk.com', 'APIKEY');

Freshdesk.listTickets(function(err, res, body){
    console.log("Here are the tickets", body);
})
```

Configuration Options
=====================

The constructor of the class takes three parameters :
* url : The base URL of your support portal
* apikey : The API key to authenticate on your support portal
* headers : Optional parameters. Those are the headers that will be added to each HTTP request

Functions
=========

Not all functionalities are yet implemented. However you can still access most of them with the get/post/put/del method.
[Refer to Freshdesk's documentation for more information.](https://developer.freshdesk.com/api/)
#### get
Do a GET request against the `path` parameter. Used by all other methods requiring a GET action, but you can also use it with your own `path` to access functions not yet implemented.
```js
Freshdesk.get('/api/v2/contacts/2', function(err, res, body){
    if(err){
        console.log(err);
    };
    if(res.statusCode === 200){
        console.log("Here is Contact 2 : " + body);
    };
});
```

#### post
Do a POST request against the `path` parameter while sending `data`. Used by all other methods requiring a POST action, but you can also use it with your own `path` and `data` to access API functions not yet implemented.
```js
const myNewContact = {
    "name" : "Customer 1",
    "email" : "customer@example.com",
    "other_emails" : ["assistant@example.com", "assistant2@example.com"]
};
Freshdesk.post('/api/v2/contacts', myNewContact, function(err, res, body){
    if(err){
        console.log(err);
    };
    if(res.statusCode === 200){
        console.log("Contact created : " + body);
    };
});
```

#### put
Do a PUT request against the `path` parameter while updating `data`. Used by all other methods requiring a PUT action, but you can also use it with your own `path` and `data` to access API functions not yet implemented.
```js
const newInfoOnContact = {
    "name" : "Customer ACME",
    "job_title" : "IT Guy"
};
Freshdesk.put('/api/v2/contacts/1', newInfoOnContact, function(err, res, body){
    if(err){
        console.log(err);
    };
    if(res.statusCode === 200){
        console.log("Contact updated : " + body);
    };
});
```

#### del
Do a DELETE request against the `path` parameter. We used `del` instead of DELETE because `delete` is a reserved word. Used by all other methods requiring a DELETE action, but you can also use it with your own `path` to access API functions not yet implemented.
```js
Freshdesk.del('/api/v2/contacts/1', function(err, res, body){
    if(err){
        console.log(err);
    };
    if(res.statusCode === 200){
        console.log("Contact deleted : " + body);
    };
});
```

#### createTicket
Create a ticket
```js
const newTicket = {
    "description" : "My printer stopped working",
    "subject" : "Printers are Satan's children",
    "email" : "contact@example.com",
    "priority" : 1,
    "status": 2
};
Freshdesk.createTicket(newTicket, function(err, res, body){
    if(err){
        console.log(err);
    };
    if(res.statusCode === 200){
        console.log("Ticket created : " + body);
    };
});
```

#### listTickets
List current tickets
```js
Freshdesk.listTickets(function(err, res, body){
   if(err){
        console.log(err);
    };
    if(res.statusCode === 200){
        console.log("Tickets : " + body);
    }; 
});
```

#### allTickets
List all tickets. This is a hack as Freshdesk does not support listing all tickets. Therefore we are using a filter to list all tickets updated since 01/01/1970 at 00:00.
```js
Freshdesk.allTickets(function(err, res, body){
    if(err){
        console.log(err);
    };
    if(res.statusCode === 200){
        console.log("Tickets : " + body);
    };
}); 
```

#### listTicketsSince
This method lists all tickets since the provided `date` parameter. It is also used by the `allTickets` method.
```js
Freshdesk.listTicketsSince('2016-08-01T00:00:00Z', function(err, res, body){ // Date is a string using ISO8601
    if(err){
        console.log(err);
    };
    if(res.statusCode === 200){
        console.log("Tickets since August 1st : " + body);
    };
});
```

#### getTicket
Returns all information about a ticket
```js
Freshdesk.getTicket(5, function(err, res, body){
    if(err){
        console.log(err);
    };
    if(res.statusCode === 200){
        console.log("Ticket with ID 5 : " + body);
    };
});
```

#### updateTicket
Returns all information about a ticket
```js
const newTicketInfo = {
    "priority" : 2,
    "status": 2
};
Freshdesk.putTicket(5, newTicketInfo, function(err, res, body){
    if(err){
        console.log(err);
    };
    if(res.statusCode === 200){
        console.log("Ticket with ID 5 has been updated : " + body);
    };
});
```

#### deleteTicket
Returns all information about a ticket
```js
Freshdesk.deleteTicket(5, function(err, res, body){
    if(err){
        console.log(err);
    };
    if(res.statusCode === 200){
        console.log("Ticket with ID 5 has been deleted: " + body);
    };
});
```

#### restoreTicket
Restores a delete ticket
```js
Freshdesk.restoreTicket(5, function(err, res, body){
    if(err){
        console.log(err);
    };
    if(res.statusCode === 200){
        console.log("Ticket with ID 5 has been restored : " + body);
    };
});
```

#### ticketFields
Returns the expected ticket fields
```js
Freshdesk.ticketFields(function(err, res, body){
    if(err){
        console.log(err);
    };
    if(res.statusCode === 200){
        console.log("Those are the required ticket fields :\n" + body);
    };
});
```

#### addNoteToTicket
Add a note to the ticket bearing `id`. 
```js
const note = "That's some weird black magic";
Freshdesk.addNoteToTicket(5, note, true, function(err, res, body){ // The third parameter is a boolean, which indicates if the note is private or not.
    if(err){
        console.log(err);
    };
    if(res.statusCode === 200){
        console.log("Note successfully added to ticket 5 :\n" + body);
    };
});
```

#### timeSpentOnTicket
Returns the time spent on a ticket
```js
Freshdesk.timeSpentOnTicket(5, function(err,res, body){
    if(err){
        console.log(err);
    };
    if(res.statusCode === 200){
        console.log("This is the time that has been spent on ticket 5 :\n" + body);
    };
});
```

#### createUser
Created a user/contact
```js
const myNewContact = {
    "name" : "Customer 1",
    "email" : "customer@example.com",
    "other_emails" : ["assistant@example.com", "assistant2@example.com"]
};
Freshdesk.createUser(myNewContact, function(err, res, body){
    if(err){
        console.log(err);
    };
    if(res.statusCode === 200){
        console.log("Contact created : " + body);
    };
});
```

#### listUsers
List all contacts
```js
Freshdesk.listUsers(function(err, res, body){
    if(err){
        console.log(err);
    };
    if(res.statusCode === 200){
        console.log("Here are the users :\n" + body);
    };
});
```

#### updateUser
Update user bearing `id` with `data`
```js
const newInfoOnContact = {
    "name" : "Customer ACME",
    "job_title" : "IT Guy"
};
Freshdesk.updateUser(newInfoOnContact, function(err, res, body){
    if(err){
        console.log(err);
    };
    if(res.statusCode === 200){
        console.log("Contact updated : " + body);
    };
});
```

#### getUserByEmail
Returns the user using the `email`
```js
const email = 'john@example.com'
Freshdesk.getUserByEmail(email, function(err,res,body){
    if(err){
        console.log(err);
    };
    if(res.statusCode === 200){
        console.log("Here is user using the email : " + body);
    };
})
```

Contributing
============
If you want to add features, fix bugs or generally improve freshdesk-nodejs, please open a pull-request.

License
=======
MIT
