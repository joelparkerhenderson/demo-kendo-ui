# Kendo UI → jQuery → Grid → endless scrolling via remote service

Contents:

* [Introduction](#introduction)
* [Grid overview](#grid-overview)
* [Grid instantiation](#grid-instantiation)
* [Grid remote data binding](#grid-remote-data-binding)
* [Grid Excel export](#grid-excel-export)
* [Grid PDF export](#grid-pdf-export)


## Introduction

The Kendo UI Grid component is a widget which allows you to visualize and edit data via its table representation. 

See https://demos.telerik.com/kendo-ui/grid

The Kendo UI DataSource component is an abstraction for using local data, such as arrays of JavaScript objects, or remote data, such as via remote web services that return JSON, JSONP, oData, or XML. 

See https://docs.telerik.com/kendo-ui/framework/datasource


## Overview

See https://demos.telerik.com/kendo-ui/grid

The Kendo UI Grid component is a widget which allows you to visualize and edit data via its table representation. 

To fill the grid with data, you can supply either local data or remote data via the Kendo UI DataSource component as described below.

The grid provides a variety of options about how to present and perform operations over the underlying data, such as paging, sorting, filtering, grouping, editing, etc. The grid provides online capabilties, offline capabilities, and sync capabilities.


## Instantiation

There are two possible ways to instantiate a Kendo UI grid:

* From an empty div element. In this case all the Grid settings are provided in the initialization script statement.

* From an existing HTML table element. In this case some of the Grid settings can be inferred from the table structure and elements HTML attributes.

In both cases the grid is registered as a jQuery plugin.

Example:

```js
$("#grid").kendoGrid({
    sortable: true,
    pageable: true,
    groupable: true,
    filterable: true,
    columnMenu: true,
    reorderable: true,
    resizable: true,
    ...
```


## Remote data binding

See https://demos.telerik.com/kendo-ui/grid/remote-data-binding

To use the grid with remote data binding, you use the Kendo UI DataSource component (described below) to specify a remote service endpoint, such as web service that returns data formatted using JSON/JSONP, OData, or XML.

The data source acts as a mediator between the grid and the underlying data. The data source needs information about the web service URL(s), the request type, the response data type, and the structure (schema) of the response, in case it is more complex than a plain array of objects.

For example, a grid can fetch its data from a remote service endpoint, assigned via the dataSource -> transport -> read attribute. The data source defines the schema model of the response from the web service, as well as the type of the service (odata) via the dataSource type property. 


## Toolbar template

The grid exposes the option to define a template for the content of its toolbar, which can vary based on your requirements or preferences.

For example, a toolbar template can show a a Kendo UI dropdown that list that helps the user choose how to view the record. For example, a dropdown can populated with a list of item categories, and then the filter is applied on its change event by invoking the `grid.dataSource.filter(params)` method.

Note that the toolbar template is instantiated by calling the `kendo.template` method and assigning the returned value to the `toolbar` attribute of the grid instance. 

Example template that shows a "refresh" icon and a "category" dropdown:

```html
<div id="grid"></div>
<script type="text/x-kendo-template" id="template">
    <div class="refreshBtnContainer">
        <a href="\\#" class="k-pager-refresh k-link k-button k-button-icon" title="Refresh">
            <span class="k-icon k-i-reload"></span>
        </a>
    </div>
    <div class="toolbar">
        <label class="category-label" for="category">Show products by category:</label>
        <input type="search" id="category" style="width: 150px"/>
    </div>
</script>
```

Example:

```js
var grid = $("#grid").kendoGrid({
    toolbar: kendo.template($("#template").html()),
    ...

var dropDown = grid.find("#category").kendoDropDownList({
    dataTextField: "CategoryName",
    dataValueField: "CategoryID",
    autoBind: false,
    optionLabel: "All",
    ...
```


## Filter row

The grid can show a filter row in the grid header. To enable it, set the grid `filterable -> mode` property to `row`. The grid will render filter components based on the data type of the underlying columns' data. For example, the grid can render textboxes for string values, numeric inputs for numbers, date pickers for dates, etc. 

You can specify default filter operator to be applied when a user enters a value in the filter textbox, then presses Enter or Tab. This can be done using the `filterable -> cell -> operator` attribute of the corresponding column.

Example:

```js
$("#grid").kendoGrid({
    dataSource: {
        type: "odata",
        transport: {
            read: "https://demos.telerik.com/kendo-ui/service/Northwind.svc/Orders"
        },
        schema: {
            model: {
                fields: {
                    Name: { type: "string" },
                    Value: { type: "number" },
                    Date: { type: "date" },
                }
            }
        },
        pageSize: 20,
        serverPaging: true,
        serverFiltering: true,
    },
    height: 550,
    filterable: {
        mode: "row"
    },
    pageable: true,
    columns:
    [
        {
            field: "Name",
            width: 100,
            filterable: {
                cell: {
                    operator: "contains",
                    suggestionOperator: "contains"
                }
            }
        },{
            field: "Value",
            width: 50,
            filterable: {
                cell: {
                    operator: "gte"
                }
            }
        },{
            field: "Date",
            format: "{0:yyyy-MM-dd}"
            filterable: {
                cell: {
                    operator: "eq"
                }
            }
        }
    ]
});
```


## Excel export

See https://demos.telerik.com/kendo-ui/grid/excel-export

The grid provides Excel export functionality. To enable it, include the corresponding command to the grid toolbar, and configure the Excel export settings accordingly. Alternatively, you can trigger Excel export by invoking the method `saveAsExcel` from the client API of the grid. 

Additionally, you have the option to customize the rows/columns and cells of the exported file by intercepting the event `excelExport`. 

Example:

```js
$("#grid").kendoGrid({
    toolbar: ["excel"],
    excel: {
        fileName: "Kendo UI Grid Export.xlsx",
        proxyURL: "https://demos.telerik.com/kendo-ui/service/export",
        filterable: true
    },
    ...
```

## PDF export

See https://demos.telerik.com/kendo-ui/grid/pdf-export

The grid provides PDF export functionality, which is powered by the underlying Drawing API engine of the Kendo UI framework. To enable it, include the corresponding command to the grid toolbar and configure the PDF export settings accordingly. For instance, you can specify to export all pages, margins, paper size, font, etc. To initiate export programmatically, you can call the saveAsPdf method from the client API. 

You can customize the look and feel of the exported grid table by wiring the pdfExport event of the grid.

The grid can load the pako zlib library (pako_deflate.min.js) to enable compression in the PDF. This is highly recommended as it improves performance and rises the limit on the size of the content that can be exported.

The Standard PDF fonts do not include Unicode support. In order for the output to match what you see in the browser you must provide source files for TrueType fonts for embedding. Please read the documentation about custom fonts and drawing.

Example:

```js
$("#grid").kendoGrid({
    toolbar: ["pdf"],
    pdf: {
        allPages: true,
        avoidLinks: true,
        paperSize: "A4",
        margin: { top: "2cm", left: "1cm", right: "1cm", bottom: "1cm" },
        landscape: true,
        repeatHeaders: true,
        template: $("#page-template").html(),
        scale: 0.8
    },
    ...
```
