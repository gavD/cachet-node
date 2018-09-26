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
    * [Installation](#installation)
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

### Installation

```
npm install cachet-node
```

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

##### Request

```javascript
cachet.ping()
```

##### Response

```json
{
  "data": "Pong!"
}
```

#### version

Fetch the current version of the system.

##### Request

```javascript
cachet.version()
```

##### Response

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

##### Parameters

  * sort
  * order
  * perPage
  * page
  * id
  * name
  * status
  * groupId
  * enabled

##### Request

```javascript
cachet.getComponents({ ... parameters ... })
```

##### Response

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

##### Parameters

  * component

##### Request

```javascript
cachet.getComponentById({
  component: 4
})
```

##### Response

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

##### Parameters

  * body
    * name
    * description
    * status
    * link
    * order
    * group_id
    * enabled

##### Request

```javascript
cachet.createComponent({
  body: {
    name: 'Foo'
  }
})
```

##### Response

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

##### Parameters

  * component
  * body
    * name
    * status
    * link
    * order
    * group_id
    * enabled

##### Request

```javascript
cachet.updateComponentById({
  component: 4,
  body: {
    enabled: 0
  }
})
```

##### Response

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

##### Parameters

  * component

##### Request

```javascript
cachet.deleteComponentById({
  component: 4
})
```

##### Response

There is no response for this other than the status code.

### Component Groups

#### getComponentGroups

##### Parameters

  * id
  * name
  * collapsed
  * sort
  * order
  * perPage
  * page

##### Request

```javascript
cachet.getComponentGroups({ ... parameters ... })
```

##### Response

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
      "name": "Example Group",
      "created_at": "2017-01-05 16:52:50",
      "updated_at": "2017-01-05 16:53:23",
      "order": 0,
      "collapsed": 0,
      "visible": 1,
      "enabled_components": [
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
      ],
      "enabled_components_lowest": [
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
      ],
      "lowest_human_status": "Operational"
    }
  ]
}
```

#### createComponentGroup

##### Parameters

  * name
  * order
  * collapsed

##### Request

```javascript
cachet.createComponentGroup({
  body: {
    name: 'Foo Group'
  }
})
```

##### Response

```json
{
  "data": {
    "order": 0,
    "collapsed": 0,
    "visible": 0,
    "name": "Foo Group",
    "updated_at": "2017-01-06 20:06:49",
    "created_at": "2017-01-06 20:06:49",
    "id": 2,
    "lowest_human_status": null
  }
}
```

#### getComponentGroupById

##### Parameters

  * group

##### Request

```javascript
cachet.getComponentGroupById({
  group: 2
})
```

##### Response

```json
{
  "data": {
    "id": 2,
    "name": "Foo Group",
    "created_at": "2017-01-06 20:06:49",
    "updated_at": "2017-01-06 20:06:49",
    "order": 0,
    "collapsed": 0,
    "visible": 0,
    "enabled_components": [],
    "enabled_components_lowest": [],
    "lowest_human_status": null
  }
}
```

#### updateComponentGroupById

##### Parameters

  * group
  * body
    * name
    * order
    * collapsed

##### Request

```javascript
cachet.updateComponentGroupById({
  group: 2,
  body: {
    order: 10
  }
})
```

##### Response

```json
{
  "data": {
    "id": 2,
    "name": "Foo Group",
    "created_at": "2017-01-06 20:06:49",
    "updated_at": "2017-01-06 20:09:43",
    "order": 10,
    "collapsed": 0,
    "visible": 0,
    "enabled_components": [],
    "enabled_components_lowest": [],
    "lowest_human_status": null
  }
}
```

#### deleteComponentGroupById

##### Parameters

  * group

##### Request

```javascript
cachet.deleteComponentGroupById({
  group: 2
})
```

##### Response

There is no response for this other than the status code.

### Incidents

#### getIncidents

##### Parameters

  * id
  * componentId
  * name
  * status
  * visible
  * sort
  * order
  * perPage
  * page

##### Request

```javascript
cachet.getIncidents({ ... parameters ... })
```

##### Response

```json
{
  "meta": {
    "pagination": {
      "total": 1,
      "count": 1,
      "per_page": "20",
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
      "component_id": 0,
      "name": "Incident Name",
      "status": 4,
      "visible": 1,
      "message": "Incident Message",
      "scheduled_at": "2015-08-01 12:00:00",
      "created_at": "2015-08-01 12:00:00",
      "updated_at": "2015-08-01 12:00:00",
      "deleted_at": null,
      "human_status": "Fixed"
    }
  ]
}
```

#### createIncident

##### Parameters

  * body
    * name
    * message
    * status
    * visible
    * component_id
    * component_status
    * notify
    * created_at
    * template
    * vars

##### Request

```javascript
cachet.createIncident({
  body: {
    name: 'Incident Name',
    message: 'Incident Message',
    status: 4,
    visible: 1
  }
})
```

##### Response

```json
{
  "data": {
    "id": 1,
    "component_id": 0,
    "name": "Incident Name",
    "status": 4,
    "visible": 1,
    "message": "Incident Message",
    "scheduled_at": "2015-08-01 12:00:00",
    "created_at": "2015-08-01 12:00:00",
    "updated_at": "2015-08-01 12:00:00",
    "deleted_at": null,
    "human_status": "Fixed"
  }
}
```

#### getIncidentById

##### Parameters

  * incident

##### Request

```javascript
cachet.getIncidentById({
  incident: 1
})
```

##### Response

```json
{
  "data": {
    "id": 1,
    "component_id": 0,
    "name": "Incident Name",
    "status": 4,
    "visible": 1,
    "message": "Incident Message",
    "scheduled_at": "2015-08-01 12:00:00",
    "created_at": "2015-08-01 12:00:00",
    "updated_at": "2015-08-01 12:00:00",
    "deleted_at": null,
    "human_status": "Fixed"
  }
}
```

#### updateIncidentById

##### Parameters

  * incident
  * body
    * name
    * message
    * status
    * visible
    * component_id
    * component_status
    * notify

##### Request

```javascript
cachet.updateIncidentById({
  incident: 1,
  body: {
    name: 'Foo'
  }
})
```

##### Response

```json
{
  "data": {
    "id": 1,
    "component_id": 0,
    "name": "Foo",
    "status": 4,
    "visible": 1,
    "message": "Incident Message",
    "scheduled_at": "2015-08-01 12:00:00",
    "created_at": "2015-08-01 12:00:00",
    "updated_at": "2015-08-01 12:00:01",
    "deleted_at": null,
    "human_status": "Fixed"
  }
}
```

#### deleteIncidentById

##### Parameters

  * incident

##### Request

```javascript
cachet.deleteIncidentById({
  incident: 1
})
```

##### Response

There is no response for this other than the status code.

#### getIncidentUpdatesById

##### Parameters

  * incident
  * sort
  * order
  * perPage
  * page

##### Request

```javascript
cachet.getIncidentUpdatesById({
  incident: 1
})
```

##### Response

```json
{
  "meta":{
    "pagination":{
      "total":4,
      "count":4,
      "per_page":20,
      "current_page":1,
      "total_pages":1,
      "links":{
        "next_page":null,
        "previous_page":null
      }
    }
  },
  "data":[
    {
      "id":1,
      "incident_id":1,
      "status":4,
      "message":"The monkeys are back and rested!",
      "user_id":1,
      "created_at":"2016-12-05 19:37:20",
      "updated_at":"2016-12-05 19:37:20",
      "human_status":"Fixed",
      "permalink":"http://cachet.app/incidents/1#update-1"
    },
    {
      "id":2,
      "incident_id":1,
      "status":3,
      "message":"Our monkeys need a break from performing. They'll be back after a good rest.",
      "user_id":1,
      "created_at":"2016-12-05 19:37:20",
      "updated_at":"2016-12-05 19:37:20",
      "human_status":"Watching",
      "permalink":"http://cachet.app/incidents/1#update-2"
    },
    {
      "id":3,
      "incident_id":1,
      "status":2,
      "message":"We have identified the issue with our lovely performing monkeys.",
      "user_id":1,
      "created_at":"2016-12-05 19:37:20",
      "updated_at":"2016-12-05 19:37:20",
      "human_status":"Identified",
      "permalink":"http://cachet.app/incidents/1#update-3"
    },
    {
      "id":4,
      "incident_id":2,
      "status":3,
      "message":"We're actively watching this issue, so it remains unresolved.",
      "user_id":1,
      "created_at":"2016-12-05 19:37:20",
      "updated_at":"2016-12-05 19:37:20",
      "human_status":"Watching",
      "permalink":"http://cachet.app/incidents/2#update-4"
    }
  ]
}
```

#### getIncidentUpdateById

##### Parameters

  * incident
  * update

##### Request

```javascript
cachet.getIncidentUpdateById({
  incident: 1,
  update: 1
})
```

##### Response

```json
{
  "data": {
    "id":1,
    "incident_id":1,
    "status":4,
    "message":"The monkeys are back and rested!",
    "user_id":1,
    "created_at":"2016-12-05 19:37:20",
    "updated_at":"2016-12-05 19:37:20",
    "human_status":"Fixed",
    "permalink":"http://cachet.app/incidents/1#update-1"
  }
}
```

### Metrics

#### getMetrics

##### Parameters

  * sort
  * order
  * perPage
  * page

##### Request

```javascript
cachet.getMetric({ ... parameters ... })
```

##### Response

```json
{
  "meta": {
    "pagination": {
      "total": 1,
      "count": 1,
      "per_page": "20",
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
      "name": "Coffee",
      "suffix": "Cups",
      "description": "Cups of coffee consumed.",
      "default_value": "0.000",
      "calc_type": 1,
      "display_chart": 1,
      "created_at": "2015-08-01 12:00:00",
      "updated_at": "2015-08-01 12:00:00",
      "default_view_name": "Last 12 Hours"
    }
  ]
}
```

#### createMetric

##### Parameters

  * body
    * name
    * suffix
    * description
    * default_value
    * display_chart

##### Request

```javascript
cachet.createMetric({
  body: {
    name: "Visitors",
    description: "How many visitors",
    suffix: "Visitors per hour",
    default_value: 0,
    display_chart: 1
  }
}
```

##### Response

```json
{
  "name":"Visitors",
  "description":"How many visitors",
  "suffix":"Visitors per hour",
  "default_value":"0",
  "display_chart":"1"
}
```

#### getMetricById

##### Parameters

  * metric

##### Request

```javascript
cachet.getMetricById({
  metric: 1
})
```

##### Response

```json
{
  "data": {
    "id": 1,
    "name": "Coffee",
    "suffix": "Cups",
    "description": "Cups of coffee consumed.",
    "default_value": "0.000",
    "calc_type": 1,
    "display_chart": 1,
    "created_at": "2015-08-01 12:00:00",
    "updated_at": "2015-08-01 12:00:00",
    "default_view_name": "Last 12 Hours"
  }
}
```

#### deleteMetricById

##### Parameters

  * metric

##### Request

```javascript
cachet.deleteMetricById({
  metric: 1
})
```

##### Response

There is no response for this other than the status code.

#### getMetricPointsById

##### Parameters

  * metric
  * sort
  * order
  * perPage
  * page

##### Request

```javascript
cachet.getMetricPointsById({
  metric: 1
})
```

##### Response

```json
{
  "data": [
    {
      "id": 1,
      "metric_id": 1,
      "value": 1,
      "created_at": "2015-03-11 14:21:44",
      "updated_at": "2015-03-11 14:21:44"
    },
    {
      "id": 2,
      "metric_id": 1,
      "value": 3,
      "created_at": "2015-03-11 14:22:11",
      "updated_at": "2015-03-11 14:22:11"
    },
    {
      "id": 3,
      "metric_id": 1,
      "value": 3,
      "created_at": "2015-03-11 14:34:55",
      "updated_at": "2015-03-11 14:34:55"
    }
  ]
}
```

#### createMetricPointById

##### Parameters

  * metric
  * body
    * value
    * timestamp

##### Request

```javascript
cachet.createMetricPointById({
  metric: 1,
  body: {
    value: 3
    timestamp: '2015-03-11 14:34:55'
  }
}
```

##### Response

```json
{
  "data": [
    {
      "id": 1,
      "metric_id": 1,
      "value": 1,
      "created_at": "2015-03-11 14:21:44",
      "updated_at": "2015-03-11 14:21:44"
    },
    {
      "id": 2,
      "metric_id": 1,
      "value": 3,
      "created_at": "2015-03-11 14:22:11",
      "updated_at": "2015-03-11 14:22:11"
    },
    {
      "id": 3,
      "metric_id": 1,
      "value": 3,
      "created_at": "2015-03-11 14:34:55",
      "updated_at": "2015-03-11 14:34:55"
    }
  ]
}
```

#### deleteMetricPointById

##### Parameters

  * metric
  * point

##### Request

```javascript
cachet.deleteMetricPointById({
  metric: 1,
  point: 3
})
```

##### Response

There is no response for this other than the status code.

## Contributing

Contributions in this repository can be documentation and/or build changes.  Since the code is automatically generated
no changes to the index.js file will be taken.
