# Introduction to Data Querying Using lodash

Kerry Ritter  
www.kerryritter.com    
ritter@kerryritter.com  
Twitter: @kerryritter   
Node.jSTL  				  							    
January 14, 2016		  								      

## What is lodash?
* “A modern JavaScript utility library delivering consistency, modularity, performance, & extras.”
* Methods for data querying and object transformations
* .NET developer? You’ll see similarities to LINQ
* The most depended-upon package in NPM 

## Why use lodash?
* Faster development and (typically) more legible code
* Safety, less bugs (== and === fun)
* Why not underscore? lodash seems to have a larger community, more methods, and underscore is merging into lodash in the future

## Examples

### Our Data
This data array becomes our employees variable in our examples.

    'use strict';

    class Person {
        constructor(firstName, lastName, gender, positions) {
            this.firstName = firstName;
            this.lastName = lastName;
            this.gender = gender;
            this.positions = positions;
        }
    }

    module.exports = {
        data: [
            new Person("Michael", "Scott", "Male", ["Regional Manager"]),
            new Person("Dwight", "Schrute", "Male", ["Sales Rep", "Assistant to the Regional Manager", "Regional Manager"]),
            new Person("Jim", "Halpert", "Male", ["Sales Rep", "Co-Regional Manager", "Assistant to the Regional Manager", "Regional Manager"]),
            new Person("Pam", "Beesly", "Female", ["Receptionist", "Sales Rep", "Office Administrator"]),
            new Person("Ryan", "Howard", "Male", ["Temporary Worker", "Sales Rep", "Vice President of Sales"]),
            new Person("Andy", "Bernard", "Male", ["Sales Rep", "Regional Manager"]),
            new Person("Robert", "California", "Male", ["Chief Executive Officer"]),
            new Person("Angela", "Martin", "Female", ["Accountant"]),
            new Person("Kelly", "Kapoor", "Female", ["Customer Service Rep"]),
            new Person("Oscar", "Martinez", "Male", ["Accountant"]),
            new Person("Darryl", "Philbin", "Male", ["Warehouse Foreman"]),
            new Person("Erin", "Hannon", "Female", ["Receptionist"])
        ]
    }

### Examples: Grouping by Property
**Code:**

    var employees = require("./employees").data;
    var _ = require("lodash");
    var genderGroups = _.groupBy(employees, "gender");
    console.log(genderGroups);

**Output:**

    {
       Male:
       [ 
         Person { firstName: 'Michael', lastName: 'Scott', gender: 'Male', positions: [Array] },
         Person { firstName: 'Dwight', lastName: 'Schrute', gender: 'Male', positions: [Array] },
         Person { firstName: 'Jim', lastName: 'Halpert', gender: 'Male', positions: [Array] },
         Person { firstName: 'Ryan', lastName: 'Howard', gender: 'Male', positions: [Array] },
         Person { firstName: 'Andy', lastName: 'Bernard', gender: 'Male', positions: [Array] },
         Person { firstName: 'Robert', lastName: 'California', gender: 'Male', positions: [Array] },
         Person { firstName: 'Oscar', lastName: 'Martinez', gender: 'Male', positions: [Array] },
         Person { firstName: 'Darryl', lastName: 'Philbin', gender: 'Male', positions: [Array] } 
       ],
       Female:
       [ 
         Person { firstName: 'Pam', lastName: 'Beesly', gender: 'Female', positions: [Array] },
         Person { firstName: 'Angela', lastName: 'Martin', gender: 'Female', positions: [Array] },
         Person { firstName: 'Kelly', lastName: 'Kapoor', gender: 'Female', positions: [Array] },
         Person { firstName: 'Erin', lastName: 'Hannon', gender: 'Female', positions: [Array] } 
       ] 
    }


### Examples: Grouping by Lambda

**Code:**

    var employees = require("./employees").data;
    var _ = require("lodash");
    var groupedByLastName = _.groupBy(employees, function(employee) {
        return employee.lastName[0];
    });
    console.log(groupedByLastName);

**Output:**

    { 
      S:
       [ Person { firstName: 'Michael', lastName: 'Scott', gender: 'Male', positions: [Array] },
         Person { firstName: 'Dwight', lastName: 'Schrute', gender: 'Male', positions: [Array] } ],
      H:
       [ Person { firstName: 'Jim', lastName: 'Halpert', gender: 'Male', positions: [Array] },
         Person { firstName: 'Ryan', lastName: 'Howard', gender: 'Male', positions: [Array] },
         Person { firstName: 'Erin', lastName: 'Hannon', gender: 'Female', positions: [Array] } ],
      B:
       [ Person { firstName: 'Pam', lastName: 'Beesly', gender: 'Female', positions: [Array] },
         Person { firstName: 'Andy', lastName: 'Bernard', gender: 'Male', positions: [Array] } ],
      C:
       [ Person { firstName: 'Robert', lastName: 'California', gender: 'Male', positions: [Array] } ],
       ...
    }

### Examples: Find()

**Code:**

    var employees = require("./employees").data;
    var _ = require("lodash");
    var michaelScott = _.find(employees, { firstName: "Michael", lastName: "Scott" });
    console.log(michaelScott);

**Output:**

    Person { firstName: 'Michael', lastName: 'Scott', gender: 'Male', positions: [Array] }

**Code:**

    var employees = require("./employees").data;
    var _ = require("lodash");
    var toby = _.find(employees, { firstName: "Toby", lastName: "Flenderson" });
    console.log(toby);

**Output:**

    undefined

### Examples: Filter()

**Code:**

    var employees = require("./employees").data;
    var _ = require("lodash");
    
    var salesReps = _.filter(employees, function (employee) {
        return _.includes(employee.positions, "Sales Rep");
    });

    console.log(salesReps);
    
**Output:**

    [ 
        Person { firstName: 'Dwight', lastName: 'Schrute', gender: 'Male', positions: [ 'Sales Rep' ... ] },
        Person { firstName: 'Jim', lastName: 'Halpert', gender: 'Male', positions: [ 'Sales Rep', ... ] },
        Person { firstName: 'Pam', lastName: 'Beesly', gender: 'Female', positions: [ 'Sales Rep', ... ] },
        Person { firstName: 'Ryan', lastName: 'Howard', gender: 'Male', positions: [ 'Sales Rep', ... ] },
        Person { firstName: 'Andy', lastName: 'Bernard', gender: 'Male', positions: [ 'Sales Rep', ... ] } 
    ]



### Examples: Reject()

**Code:**

    var employees = require("./employees").data;
    var _ = require("lodash");
    
    var notSalesReps = _.reject(employees, function (employee) {
        return _.includes(employee.positions, "Sales Rep");
    });
    
    console.log(notSalesReps);
    
**Output:**

    [ 
        Person { firstName: 'Michael', lastName: 'Scott', gender: 'Male', positions: [ 'Regional Manager' ] },
        Person { firstName: 'Robert', lastName: 'California', gender: 'Male', positions: [ ‘CEO' ] },
        Person { firstName: 'Angela', lastName: 'Martin', gender: 'Female', positions: [ 'Accountant' ] },
        Person { firstName: 'Kelly', lastName: 'Kapoor', gender: 'Female', positions: [ 'Cust Service Rep' ] },
        Person { firstName: 'Oscar', lastName: 'Martinez', gender: 'Male', positions: [ 'Accountant' ] },
        Person { firstName: 'Darryl', lastName: 'Philbin', gender: 'Male', positions: [ ‘Foreman' ] },
        Person { firstName: 'Erin', lastName: 'Hannon', gender: 'Female', positions: [ 'Receptionist' ] } 
    ]

### Examples: Map

**Code:**

    var employees = require("./employees").data;
    var _ = require("lodash");
    
    var viewModel = _.map(employees, function (employee) {
        return {
            name: employee.firstName + " " + employee.lastName,
            lastPosition: _.takeRight(employee.positions, 1)  
        }
    });
    
    console.log(viewModel);

**Output:**

    [ 
        { name: 'Michael Scott', lastPosition: [ 'Regional Manager' ] },
        { name: 'Dwight Schrute', lastPosition: [ 'Regional Manager' ] },
        { name: 'Jim Halpert', lastPosition: [ 'Regional Manager' ] },
        { name: 'Pam Beesly', lastPosition: [ 'Office Administrator' ] },
        { name: 'Ryan Howard', lastPosition: [ 'Vice President of Sales' ] },
        { name: 'Andy Bernard', lastPosition: [ 'Regional Manager' ] },
        { name: 'Robert California', lastPosition: [ 'Chief Executive Officer' ] },
        { name: 'Angela Martin', lastPosition: [ 'Accountant' ] },
        { name: 'Kelly Kapoor', lastPosition: [ 'Customer Service Rep' ] },
        { name: 'Oscar Martinez', lastPosition: [ 'Accountant' ] },
        { name: 'Darryl Philbin', lastPosition: [ 'Warehouse Foreman' ] },
        { name: 'Erin Hannon', lastPosition: [ 'Receptionist' ] } 
    ]

### Examples: Map #2

**Code:**

    var employees = require("./employees").data;
    var _ = require("lodash");
    
    var lastNames = _.map(employees, ‘lastName’);
    
    console.log(lastNames);
    
**Output:**

    [ 
      'Scott',
      'Schrute',
      'Halpert',
      'Beesly',
      'Howard',
      'Bernard',
      'California',
      'Martin',
      'Kapoor',
      'Martinez',
      'Philbin',
      'Hannon' 
    ]
    
### Examples: Chain

**Code:**

    var employees = require("./employees").data;
    var _ = require("lodash");
    
    var regionalManagerNames = _.chain(employees)
        .filter(function getOnlySalesReps(employee) {
            return _.includes(employee.positions, "Regional Manager");
        })
        .map(function returnOnlyNames(employee) {
            return {
                name: employee.firstName + " " + employee.lastName
            };
        }) 
        .orderBy("name")
        .value();
    
    console.log(regionalManagerNames);

**Output:**

    [ 
      { name: 'Andy Bernard' },
      { name: 'Dwight Schrute' },
      { name: 'Jim Halpert' },
      { name: 'Michael Scott' } 
    ]


## Other helpful functions
* _.defaults()
* _.union(), _.xor()
* _.every()
* Type checking: _.isUndefined(), _.isArray, _.isInteger(), ... 
* Math: _.max(), _.mean(), _.sum()
* Strings: _.capitalize(), _.kebabCase() [WordPress slugs], _.template()
* Template: _.template('hello <%= user %>!');
