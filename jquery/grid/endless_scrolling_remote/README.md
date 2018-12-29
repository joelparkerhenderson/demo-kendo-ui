# Kendo UI → jQuery → Grid → endless scrolling via remote service

This page demonstrates:

* Progress Telerik Kendo UI component library
* The jQuery components
* The Grid component
* Endless scrolling via remote service
* Simple read-only view-only get-only data

Contents:

* [Introduction](#introduction)
* [Grid remote data binding](#grid-remote-data-binding)
* [DataSource overview](#datasource-overview)
* [DataSource binding to a remote service](#datasource-binding-to-a-remote-service)
* [Example of a remote service](#example-of-a-remote-service)
* [Mixed data operations mode](#mixed-data-operations-mode)
* [Create a DataSource for a remote service](#create-a-datasource-for-a-remote-service)
* [Remote service record filtering](#remote-service-record-filtering)
* [Remote service record grouping](#remote-service-record-grouping)


## Introduction

The Kendo UI Grid component is a widget which allows you to visualize and edit data via its table representation. 

See [README](../README.md) and https://demos.telerik.com/kendo-ui/grid

The Kendo UI DataSource component is an abstraction for using local data, such as arrays of JavaScript objects, or remote data, such as via remote web services that return JSON, JSONP, oData, or XML. 

See https://docs.telerik.com/kendo-ui/framework/datasource


## Grid remote data binding

See https://demos.telerik.com/kendo-ui/grid/remote-data-binding

To use the grid with remote data binding, you use the Kendo UI DataSource component (described below) to specify a remote service endpoint, such as web service that returns data formatted using JSON/JSONP, OData, or XML. 

The data source acts as a mediator between the grid and the underlying data. The data source needs information about the web service URL(s), the request type, the response data type, and the structure (schema) of the response, in case it is more complex than a plain array of objects.

For example, a grid can fetch its data from a remote service endpoint, assigned via the dataSource -> transport -> read attribute. The data source defines the schema model of the response from the web service, as well as the type of the service (odata) via the dataSource type property. 


## DataSource overview

See https://docs.telerik.com/kendo-ui/framework/datasource/overview

The Kendo UI DataSource component is an abstraction for using local data, such as arrays of JavaScript objects, or remote data, such as via remote web services that return JSON, JSONP, oData, or XML.

This demo uses remote data via a remote service.


## DataSource binding to a remote service

The DataSource needs information about the web service URLs, the request type, the response data type, and the structure (schema) of the response, if it is more complex than a plain array of objects. You are also able to provide custom parameters, which are going to be submitted during the data request.


## Example of a remote service

Here's an example that connects to [OpenWeatherMap](https://openweathermap.org) to get data.

Example:

```js
var dataSource = new kendo.data.DataSource({
    transport: {
        read: {
            // the remote service url
            url: "http://api.openweathermap.org/data/2.5/find",

            // the request type
            type: "get",

            // the data type of the returned result
            dataType: "json",

            // additional custom parameters sent to the remote service
            data: {
                lat: 42.42,
                lon: 23.20,
                cnt: 10
            }
        }
    },
    // describe the result format
    schema: {
        // the data which the data source will be bound to is in the "list" field of the response
        data: "list"
    }
});
```

## Mixed data operations mode

Make sure that all data operations (paging, sorting, filtering, grouping, and aggregates) are configured to take place either on the server, or on the client. While it is possible to use a mixed data operations mode, setting some of the data operations on the server and others on the client leads to undesired side-effects.

For example, if serverPaging is enabled and serverFiltering is disabled, the DataSource will filter only the data from the current page and the user will see less results than expected. In other scenarios, the DataSource might make more requests than necessary for the data operations to execute.


## Create a DataSource for a remote service

See https://docs.telerik.com/kendo-ui/framework/datasource/basic-usage

The example below demonstrates how to create a DataSource for data from a remote endpoint.

A transport must identify the protocols, URLs of endpoints, and serialization formats for any or all CRUD (Create, Read, Update, Destroy) data operations.

It is optionally required to use a parameterMap, which marshals request parameters to the format of a remote endpoint.

It is optionally configured to use server operations for calculating aggregates, defining filters, and supporting features like grouping, paging, and sorting.

Example:

```js
var dataSource = new kendo.data.DataSource({
    type: "odata",
    transport: {
        read: "http://odata.netflix.com/Catalog/Titles"
    }
});
```

The `remoteDataSource` variable in the example is a DataSource that is initialized to represent an in-memory cache of movie titles from the Netflix catalog service, which employs the oData protocol. It is only configured to act as a read-only source of data to any widgets to which it is bound.

The data provided by the Netflix catalog service is not loaded until the .read() method is called:

```js
remoteDataSource.read();
```

When the DataSource is bound to a Kendo UI widget or chart, the explicit invocation may not be necessary. The default configuration of the widgets is set to automatically bind to an associated DataSource. However, this may be overridden, i.e. `autoBind`.


## Remote service record filtering

Remote service record filtering is convenient for large datasets.

Be sure to set the `schema` and the `filter` properties as necessary.

```js
var dataSource = new kendo.data.DataSource({
    serverFiltering: true,
    transport: {
        ...
    },
    schema: {
        data: "result"
    },
    filter: {
        logic: 'or',
        filters: [
            { field: 'name', operator: 'contains', value: 'foo' },
            { field: 'price', operator: 'gte', value: 10 }
        ]
    }
});
```

## Remote service record grouping

Remote service record grouping is an excellent option when working with large datasets. Be sure to set the schema and group properties as necessary.

```js
var dataSource = new kendo.data.DataSource({
    serverGrouping: true,
    transport: {
        ...
    },
    schema: {
        groups: 'groups'
    },
    group: {
        field: 'length'
    },
});
```
