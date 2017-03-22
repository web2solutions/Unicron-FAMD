Unicron FAMDS  --- DRAFT
================

Unicron-FAMDS is a data definition schema which attempts to standardize data schema to be used in Front and Back end sofwate development.

FAMDS stands for Framework Agnostic Medatadata standard for Data Schemas

### Scenario

During the process to create web-like softwares from the scratch, it is very commom the fact of `repeating yourself` when implementing models, data validation, REST and CRUD interfaces both on client and server side.

Once modern software development approaches encourage decoupled and interoperable software layers/services, it is trivial to find entire teams performing repetitive tasks on different layers which defines things like data access, data manipulation, data input/output, form interfaces, data validation, etc.


### Problems

#### Repetitive writing of very similar layer code, for the same application, which performs the same/similar task, for different environments (browser and server).

- ***Data Models***

Example:

1 - writing Backbone code for models on client side
2 - writing Mongoose code for models on server side

- ***Data validation***

Example:

1 - Writing data validation logic for a REST end point which receives data submmited through a web form

2 - writing the logic which will be used on client side, to validate the data input when submmiting the form.

- ***User Interfaces***

Define similar component settings for different data components.

For example:

Lets suppose you are developing a CRUD interface for the `Customers` collection in your application.
At least in one moment, you will be repeating yourself in terms of writing similar codes when:

1 - writing the fields properties and rules of a form, `to insert and update` data into the collection

2 - define the columns properties and rules of a grid, `to list` data from the collection

_Why I'm repeating myself if I'm writing settings code for different components?_

*Because both components settings are based on the same data schema. Even if you don't have any kind of document or standard explicity defining that data schema.*

#### Significant lost time and effort repeating tasks while starting every new application from the scratch

As you know, all above steps, but not least, are part of several softwares development life cycle. There is no way, you need to put your hands on and write from the scratch very similar code when building new applications.

If you build a `To Do` application, you will need to define models, data validation, form interfaces, REST interfaces, etc. 

And, if you build a `CRM` application, you will also need to define the same things as in the first case. The difference here, which is not relevant, is the complex/size of each application.

### Solutions

Our main problem consist in `repeat yourself` in the software development life cycle. And the main solution, in resume would be something which could implement `DRY` on the side of the software development life cycle.

Certainly there are several software development methodologies and paradigms that can be used to achieve this.

But we would like to introduce the `metaprogramming` as base for our proposed solution.

#### The FAMDS

The Framework Agnostic Medatadata standard for Data Schemas is not more than JSON structures which would define the entire application data model.

A FAMDS metadata structure shall to be/contain:

- Framework agnostic. It is a generic standard and shall to be useful to be used in any application development life cycle. It does not matter if you use any third party framework.
- It define Data Collections
- It defines Data models
 - It define model properties
  - It defines property type
  - It defines property validation
- Provide model versioning


Let's check the FAMD for a Petstore:

```javascript

{
    "title": "FAMD Sample App",
    "description": "This is a sample Petstore.",
    "termsOfService": "http://fake.io/terms",
    "contact": {
        "name": "Support Channel",
        "url": "http://www.fake.io/contact",
        "email": "me@fake.io"
    },
    "license": {
        "name": "Apache 2.0",
        "url": "http://www.apache.org/licenses/LICENSE-2.0.html"
    },
    "version": "0.0.1",
    "collections": {
        "pets": {
            "model": {
                "title": "Pet",
                "description": "Pet from the system", // (GFM syntax can be used for rich text representation)
                "properties": [{
                    "name": "Name",
                    "description": "The pet name", // (GFM syntax can be used for rich text representation)
                    "type": "string", // http://swagger.io/specification/#dataTypeFormat
                    "default": null,
                    "unique": false,
                    "maxLength": 255,
                    "minLength": 4,
                    "maxItems": 0,
                    "maxItems": 0,
                    "validate": {
                        "required": true,
                        "rules": "NotEmpty",
                    },
                    "ui": {
                        "noteShow": false, // show text information
                        "noteText": "the pet name", // plain text information
                        "mask": "",
                        "form": {
                            "inputLabel": "Name",
                            "inputType": "input",
                        },
                        "grid": {
                            "columnHeader": "Name",
                            "columnAlign": "left",
                            "columnType": "readonly",
                            "columnWidth": "120px"
                        }
                    }
                }, {
                    "name": "Status",
                    "description": "The pet status in the system", // (GFM syntax can be used for rich text representation)
                    "type": "boolean", // http://swagger.io/specification/#dataTypeFormat
                    "default": true,
                    "unique": false,
                    "maxLength": 0,
                    "minLength": 0,
                    "maxItems": 0,
                    "minItems": 0,
                    "validate": {
                        "required": true,
                        "rules": "",
                    },
                    "ui": {
                        "noteShow": false, // show text information
                        "noteText": "The pet status in the system", // plain text information
                        "mask": "",
                        "form": {
                            "inputLabel": "Status",
                            "inputType": "input",
                        },
                        "grid": {
                            "columnHeader": "Status",
                            "columnAlign": "center",
                            "columnType": "checkbox",
                            "columnWidth": "120px"
                        }
                    }
                }]
            }
        }
    }
}


```

##### Data types

| Common Name | type    | format    | Comments                                         |
|-------------|---------|-----------|--------------------------------------------------|
| integer     | integer | int32     | signed 32 bits                                   |
| long        | integer | int64     | signed 64 bits                                   |
| float       | number  | float     |                                                  |
| double      | number  | double    |                                                  |
| string      | string  |           |                                                  |
| byte        | string  | byte      | base64 encoded characters                        |
| binary      | string  | binary    | any sequence of octets                           |
| boolean     | boolean |           |                                                  |
| date        | string  | date      | As defined by full-date - RFC3339                |
| dateTime    | string  | date-time | As defined by date-time - RFC3339                |
| password    | string  | password  | Used to hint UIs the input needs to be obscured. |


##### Commom Validation rules

| Name              | Comments                                                 |
|-------------------|----------------------------------------------------------|
| NotEmpty          | must contain at least one symbol;                        |
| Empty             | must be empty;                                           |
| ValidAplhaNumeric |                                                          |
| ValidBoolean      | can contain one of 4 values: 'true', 'false', 0 or 1;    |
| ValidCurrency     |                                                          |
| ValidDate         | (value from 0000-00-00 to 9999-12-31);                   |
| ValidDatetime     | (value from 0000-00-00 00:00:00 to 9999:12:31 59:59:59); |
| ValidEmail        |                                                          |
| ValidInteger      |                                                          |
| ValidIPv4         | (value from 0.0.0.0 to 255.255.255.255);                 |
| ValidNumeric      |                                                          |
| ValidSIN          | (Social Insurance Number);                               |
| ValidSSN          | (Social Security Number);                                |
| ValidTime         | (value from 00:00:00 to 59:59:59);                       |


#### Automated software development tools

##### Visual FAMD editor

##### Generate instead write

- Data models logic - client side and server side
- Validation logic - client and server side
- Data access rules - client and server side
- Component settings
- CRUD interfaces
- REST end points
- Swagger

![Automated software development diagram](http://i.imgur.com/HINi6PD.jpg)

