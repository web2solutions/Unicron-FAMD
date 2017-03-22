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


Let's check the `pet` `model metadata`:

```javascript

var pet = {
    title: "Pet",
    description: "Pet from the system", // (GFM syntax can be used for rich text representation)
    properties: [
    	{
	        name: "Name",
	        description: "The pet name", // (GFM syntax can be used for rich text representation)
	        type: 'string', // http://swagger.io/specification/#dataTypeFormat
	        default: null,
	        unique: false,
	        maxLength: 255,
	        minLength: 4,
	        maxItems: 0,
			minItems: 0,
	        validate: {
	            required: true,
	            rules: 'NotEmpty',
	        },
	        ui: {
	            show_note: false, // show text information
	            text_note: 'the pet name', // plain text information
	            mask: '',
	            form: {
	                input_label: 'Name',
	                input_type: 'input',
	            },
	            grid: {
	                column_header: 'Name',
	                column_align: 'left',
	                column_type: 'readonly',
	                column_width: '120px'
	            }
	        }
	    },
	    {
	        name: "Status",
	        description: "The pet status in the system", // (GFM syntax can be used for rich text representation)
	        type: 'boolean', // http://swagger.io/specification/#dataTypeFormat
	        default: true,
	        unique: false,
	        maxLength: 0,
	        minLength: 0,
	        maxItems: 0,
			minItems: 0,
	        validate: {
	            required: true,
	            rules: '',
	        },
	        ui: {
	            show_note: false, // show text information
	            text_note: 'The pet status in the system', // text information
	            mask: '',
	            form: {
	                input_label: 'Status',
	                input_type: 'checkbox',
	            },
	            grid: {
	                column_header: 'Status',
	                column_align: 'center',
	                column_type: 'checkbox',
	                column_width: '120px'
	            }
	        }
	    }
    ]
};

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





#### Automated software development tools

##### Visual FAMD editor

##### Generate instead write






### License

The MIT License (MIT)    
Copyright (c) 2016 Dinamenta LLC

Permission is hereby granted, free of charge, to any person obtaining a copy of this software and associated documentation files (the "Software"), to deal in the Software without restriction, including without limitation the rights to use, copy, modify, merge, publish, distribute, sublicense, and/or sell copies of the Software, and to permit persons to whom the Software is furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in all copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY, FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM, OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE SOFTWARE.
