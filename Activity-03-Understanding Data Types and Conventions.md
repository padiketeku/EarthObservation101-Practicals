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
| *Fig. 1. Commenting a multiple line of code using //* |


Alternatively, to add a detailed comment or if you want to make a long line of code inactive you may use a forward slash and * to start the comment and at the end of the comment use  * plus a forward slash (Fig. 2) .

![image](https://github.com/user-attachments/assets/a7907842-08f6-41b8-a17c-7bbd10b0a834) |
|:--:|
| *Fig. 2. Commenting a multiple line of code using /*… */ * |

