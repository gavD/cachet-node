# cachet-node

*cachet-node* is an implementation of the [Cachet](https://cachethq.io) API.  This API is automatically generated based
on a swagger documentation file on [cachet-swagger](https://github.com/mwillbanks/cachet-swagger) through the use of
[swagger-js-codegen](https://github.com/wcandillon/swagger-js-codegen).

Currently it supports the following operations from the [Cachet API Reference](https://docs.cachethq.io/reference):

* General
* Components
* Component Groups
* Incidents
* Incident Updates
* Metrics

## Usage

### Basics

In order to start leveraging the *cachet-node* library you will generally have an API key located under your Teams ->
User area.  This is going to be necessary to do anything that is not a read-only event.  You only need to set this
information once you have initialized the library.

```javascript
const Cachet = require('cachet-node').Cachet;

const cachet = new Cachet({
  domain: 'https://status.yourdomain.com/api/v1',
  token: {
    value: 'token value from user',
    headerOrQueryName: 'X-Cachet-Token'
  }
});
```

Now you can start to leverage one of the many functions such as fetching the current version:

```
cachet.version()
    .then(response => {
        console.log(response.body);
        // { meta: { on_latest: true, latest: { tag_name: 'v2.3.10', prelease: false, draft: false } }, data: '2.4.0-dev' }
    })
    .catch(err => {
        console.log(err);
    });
```

#### Parameters

Note that all parameters must be passed in via a JavaScript object.  Anytime you are creating, it requires a *body*
parameter.  You can find such parameters in the swagger file in cachet-swagger until this documentation reflects all of
the methods and functionality of the endpoint.

## Contributing

Contributions in this repository can be documentation and/or build changes.  Since the code is automatically generated
no changes to the index.js file will be taken.
