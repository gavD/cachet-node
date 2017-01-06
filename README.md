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

## Table of Contents

  * [Usage](#usage)
    * [Basics](#basics)
      * [Parameters](#parameters)
    * [General](#general)
      * [ping](#ping)
      * [version](#version)
    * [Components](#components)
      * [getComponents](#getcomponents)
      * [createComponent](#createcomponent)
      * [updateComponentById](#updatecomponentbyid)
      * [deleteComponentById](#deletecomponentbyid)
    * [Component Groups](#component-groups)
      * [getComponentGroups](#getcomponentgroups)
      * [createComponentGroup](#createcomponentgroup)
      * [getComponentGroupById](#getcomponentgroupbyid)
      * [updateComponentGroupById](#updatecomponentgroupbyid)
      * [deleteComponentGroupById](#deletecomponentgroupbyid)
    * [Incidents](#incidents)
      * [getIncidents](#getincidents)
      * [createIncident](#createincident)
      * [getIncidentById](#getincidentbyid)
      * [updateIncidentById](#updateincidentbyid)
      * [deleteIncidentById](#deleteincidentbyid)
      * [getIncidentUpdatesById](#getincidentupdatesbyid)
      * [getIncidentUpdateById](#getincidentupdatebyid)
    * [Metrics](#metrics)
      * [getMetrics](#getmetrics)
      * [createMetric](#createmetric)
      * [getMetricById](#getmetricbyid)
      * [deleteMetricById](#deletemetricbyid)
      * [getMetricPointsById](#getmetricpointsbyid)
      * [createMetricPointById](#createmetricpointbyid)
      * [deleteMetricPointById](#deletemetricpointbyid)
  * [Contributing](#contributing)

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

Note that all of the methods will provide back a promise with the exception of the setToken method (not documented as
you can use the constructor).

This library also makes heavy use of a request object, what you are receiving back is the what the request provides raw
and that is where you will need to utilize the "body" parameter.

#### Parameters

Note that all parameters must be passed in via a JavaScript object.  Anytime you are creating, it requires a *body*
parameter.  You can find such parameters in the swagger file in cachet-swagger until this documentation reflects all of
the methods and functionality of the endpoint.

### General

#### ping

Ping the status system to ensure it is available.

**Request**

```javascript
cachet.ping()
```

**Response**

```json
{
  "data": "Pong!"
}
```

#### version

Fetch the current version of the system.

**Request**

```javascript
cachet.version()
```

**Response**

```json
{
  "meta": {
    "on_latest": true,
    "latest": {
      "tag_name": "v2.3.10",
      "prelease": false,
      "draft": false
    }
  },
  "data": "2.4.0-dev"
}
```

### Components

#### getComponents

Fetch all of the components.

**Request Parameters**

  * sort
  * order
  * perPage
  * page
  * id
  * name
  * status
  * groupId
  * enabled

**Request**

```javascript
cachet.getComponents({ ... parameters ... })
  .then(response => {
    console.log(response.body);
    //
  });
```

**Response**

```json
{
  "meta": {
    "pagination": {
      "total": 1,
      "count": 1,
      "per_page": 20,
      "current_page": 1,
      "total_pages": 1,
      "links": {
        "next_page": null,
        "previous_page": null
      }
    }
  },
  "data": [
    {
      "id": 1,
      "name": "Example",
      "description": "",
      "status": 1,
      "order": 0,
      "group_id": 1,
      "created_at": "2017-01-05 16:52:00",
      "updated_at": "2017-01-06 02:27:32",
      "deleted_at": null,
      "enabled": true,
      "meta": null,
      "link": "https://status.example.com",
      "status_name": "Operational",
      "tags": {
        "": ""
      }
    }
  ]
}
```

#### getComponentById

**Request Parameters**

  * component

**Request**

```javascript
cachet.getComponentById({
  component: 4
});
```

**Response**

```json
{
  "data": {
    "id": 4,
    "name": "Foo",
    "description": "",
    "status": 0,
    "order": 0,
    "group_id": 0,
    "created_at": "2017-01-06 19:51:29",
    "updated_at": "2017-01-06 19:52:59",
    "deleted_at": null,
    "enabled": false,
    "meta": null,
    "link": "",
    "status_name": "Unknown",
    "tags": []
  }
}
```

#### createComponent

**Parameters**

  * body
    * name
    * description
    * status
    * link
    * order
    * group_id
    * enabled

**Request**

```javascript
cachet.createComponent({
  body: {
    name: 'Foo'
  }
});
```

**Response**

```json
{
  "data": {
    "order": 0,
    "group_id": 0,
    "description": "",
    "link": "",
    "enabled": true,
    "meta": null,
    "name": "Foo",
    "status": 0,
    "updated_at": "2017-01-06 19:51:29",
    "created_at": "2017-01-06 19:51:29",
    "id": 4,
    "status_name": "Unknown",
    "tags": []
  }
}
```

#### updateComponentById

**Parameters**

  * component
  * body
    * name
    * status
    * link
    * order
    * group_id
    * enabled

**Request**

```javascript
cachet.createComponent({
  component: 4,
  body: {
    enabled: 0
  }
});
```

**Response**

```json
{
  "data": {
    "id": 4,
    "name": "Foo",
    "description": "",
    "status": 0,
    "order": 0,
    "group_id": 0,
    "created_at": "2017-01-06 19:51:29",
    "updated_at": "2017-01-06 19:52:59",
    "deleted_at": null,
    "enabled": false,
    "meta": null,
    "link": "",
    "status_name": "Unknown",
    "tags": []
  }
}
```

#### deleteComponentById

**Parameters**

  * component

**Request**

```javascript
cachet.deleteComponentById({
  component: 4
});
```

**Response**

There is no response for this other than the status code.

### Component Groups

**Parameters**

**Request**

**Response**

#### getComponentGroups

**Parameters**

**Request**

**Response**

#### createComponentGroup

**Parameters**

**Request**

**Response**

#### getComponentGroupById

**Parameters**

**Request**

**Response**

#### updateComponentGroupById

**Parameters**

**Request**

**Response**

#### deleteComponentGroupById

**Parameters**

**Request**

**Response**

### Incidents

**Parameters**

**Request**

**Response**

#### getIncidents

**Parameters**

**Request**

**Response**

#### createIncident

**Parameters**

**Request**

**Response**

#### getIncidentById

**Parameters**

**Request**

**Response**

#### updateIncidentById

**Parameters**

**Request**

**Response**

#### deleteIncidentById

**Parameters**

**Request**

**Response**

#### getIncidentUpdatesById

**Parameters**

**Request**

**Response**

#### getIncidentUpdateById

**Parameters**

**Request**

**Response**

### Metrics

**Parameters**

**Request**

**Response**

#### getMetrics

**Parameters**

**Request**

**Response**

#### createMetric

**Parameters**

**Request**

**Response**

#### getMetricById

**Parameters**

**Request**

**Response**

#### deleteMetricById

**Parameters**

**Request**

**Response**

#### getMetricPointsById

**Parameters**

**Request**

**Response**

#### createMetricPointById

**Parameters**

**Request**

**Response**

#### deleteMetricPointById

**Parameters**

**Request**

**Response**

## Contributing

Contributions in this repository can be documentation and/or build changes.  Since the code is automatically generated
no changes to the index.js file will be taken.
