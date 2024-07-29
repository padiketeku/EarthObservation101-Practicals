# Activity 3: Understanding Data Types and Conventions

## Introduction
JavaScript is the main programming language underlying the Earth Engine. JavaScript is implemented in Earth Engine through client-side and server-side objects. The server-side processing is strongly recommended for your analysis. If you are new to JavaScript, knowing the fundamentals of JavaScript, including variables, comments, data types, and writing user-defined functions or understanding functions, is invaluable. This activity aims to explain the basics of JavaScript while writing simple codes in the Code Editor. 

## Learning Outcomes
- Familiarity with the Earth Engine Code Editor
- Familiarity with the JavaScript syntax
- Ability to use the Earth Engine API functions from the Code Editor

### Variables
In computer programming, a variable is used to store data values. In JavaScript, you define a variable declaring the `var` keyword and follow it up with the name of the variable, as shown below.
```JavaScript
var age = 55;
```
The above code is a variable (because of the **var**), the name of the variable is age, and 55 is assigned to the variable. The semicolon marks the end of the statement (think of the semicolon as a full stop for every English sentence you write). Although it is a good practice to end every variable statement with a semicolon, the code editor runs well even if this is missing. 

### Comments
In computer programming, it is a good practice to add comments to the code you write. Comments are texts given to help the developer and user of the code understand the code. Commenting on a code can be easily ignored by developers as the code can still run without the comments. However, adding useful comments to a code is strongly recommended. The comment would help you explain every line of the code and can serve as a reminder if you come back to the code in the future. In JavaScript, you can start a comment using two forward slashes (//), as shown below. 

```JavaScript
// This is a JavaScript comment
````
If // precedes a code, that code is treated as a comment and will not be executed. Thus, a comment can be used to deactivate a code. Apply // to multiple lines of code if you do not want to run the code block as shown in Fig. 1.

![image](https://github.com/user-attachments/assets/3278e5e1-2797-4499-91fd-74f09a872432) |
|:--:|
| *Fig. 1. Commenting a multiple line of code using //*|


Alternatively, to add a detailed comment or if you want to make a long line of code inactive you may use a forward slash and * to start the comment and at the end of the comment use  * plus a forward slash (Fig. 2) .

![image](https://github.com/user-attachments/assets/a7907842-08f6-41b8-a17c-7bbd10b0a834) |
|:--:|
| *Fig. 2. Commenting a multiple line of code using /*...*/*|

## Basic JavaScript Data Types

### Numbers
When you assign a number to a variable, as seen in the variable **age**, a number variable is created. If you print the variable age, the value stored in the variable will be printed to the Console.
```JavaScript
// create a variable called 'age'and assign 55 to this
var age = 55;

// print the variable to the Console
print (age);
``` 
### Strings
A string variable is created once a text value is assigned. The text is locked up either in single ' or double " quotes. Never mix them up; just select one (preferably a single quote) and be consistent. A native JavaScript string variable for an African country will be declared, as shown below.

```var country = 'Ghana';
print (country);
```

### Lists
Lists are defined using square brackets [ ] and are useful for holding multiple values which can be of dissimilar data types. A list variable of 5 countries in Europe will be declared as:

```JavaScript
var europe = ['Sweden', 'Latvia', 'Portugal', 'France', 'Slovakia'];
print (europe);
```
If you run the print command, the result appears in the **`Console`**, (Fig. 3). The expander arrow is highlighted with a red polygon. 

![image](https://github.com/user-attachments/assets/0eeecef3-260c-4c80-ba1f-57250470cb50) |
|:--:|
| *Fig. 3. List. An example of JavaScript list.*|

Click the expander to reveal further information about the list. The list has five elements and each element is indexed; Sweden is 0, Latvia is 1, Portugal is 2, France is 3 and Slovakia is 4. The index shows the position of the element in the list. Note that the index of the first item is 0, not 1.

![image](https://github.com/user-attachments/assets/e694112d-4e2c-4b1f-a95b-09900dae9f78)


### Objects
Objects in JavaScript are dictionaries of `key:value` pairs. An object (or dictionary) is declared using curly brackets { }. A value can be referenced using its key. The code below creates an object called darwinCity with some information about Darwin City in Australia. 

Note a few important things about the JavaScript syntax here. First, we can use multiple lines to define the object. Only when we put in the semicolon (;) is the command considered complete. This helps format the code to make it more readable. Also note the choice of the variable name darwinCity. The variable contains two words. The first word is in lowercase, and the first letter of the second word is capitalized. This type of naming scheme of joining multiple words into a single variable name is called “camel case.” While it is not mandatory to name your variables using this scheme, it is considered a good practice. 

```JavaScript
var darwinCity ={
'city': 'Darwin',
'size': 3164,
'coordinates': [130.841782, -12.462827],
'population': 139902
};

print(darwinCity);
```
The object will be printed in the Console. Make sure you expand the output. The object has four properties, and you can see that instead of a numeric index, each item has a label. This is known as the key and can be used to retrieve the value of an item

![image](https://github.com/user-attachments/assets/9e8284fe-9527-4199-902b-863203f4d47d)
|:--:|
| *Fig. 4. A JavaScript Object. This is also known as a dictionary variable.*|

The result above will be replicated if the container object is used to declare the object, as shown below.

```JavaScript
var darwinCity =ee.Dictionary({
'city': 'Darwin',
'size': 3164,
'coordinates': [130.841782, -12.462827],
'population': 139902
});

print(darwinCity);
```


#### Container object
A container object, always defined using **`ee`** (meaning Earth Engine), is used to wrap a client-side JavaScript object for the efficient functioning of Google servers. Earth Engine users are encouraged to use the **`ee`** wrapper to minimise computation issues. The data types discussed above can be more Earth Engine-specific if the **`ee`** precedes the data type. Examples for number, string, and list objects shown below. 

```JavaScript
var age = ee.Number(55);
print(age);
```

```JavaScript
var country = ee.String('Ghana');
print(country);
```

```JavaScript
var europe = ee.List(['Sweden', 'Latvia', 'Portugal', 'France', 'Slovakia']);
print (europe);
```

Through the **`ee`** objects, which is also the application programming interface (API), you can use Google servers for remote sensing analysis. The Earth Engine API provides many objects and methods for different levels of image analysis. In Earth Engine, click the **`Docs`** tab to view the API functions (Figure 1.14).

![image](https://github.com/user-attachments/assets/e438de85-e0de-49ef-add0-507e02597abb)

