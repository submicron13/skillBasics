# **SKILL Basics**

## Resources
1. Type <code>startFinder()</code> in the CIW.  
    * This opens the Skill finder which is comprehensive searchable database of ALL available skill functions.
2.  support.cadence.com  
    * This is the Cadence support site. An amazing resource for examples and manuals.
## Variables
---
* Skill isn't a strongly typed language
* This means you can reuse the same variable with 
different data.  
* **By default variables are global**
---

    someNumber = 0  
    someText = "This is a string"  
    someList = list( "Aspen" "Cam" "Shelly" "Riley" )  
    bool1 = t  ; skill equivalent to true
    bool2 = nil  ; skill equivalent to false
    bool1 = "THIS IS REUSED"

## Getting the value of a variable
***
You can get the value of a variable 2 different way

1. In the CIW type the name of the variable and hit enter

    * The value will be printed to the CIW.  

2. Use the printf function to print the variable  
    * <code>printf("Some Variable: %L" someNumber)</code>

## Operators
***
    \+ ;addition  
    \- ;subtraction  
    \* ;multiplication
    \/ ;division
    \> < >= <= ; greater than, less than, etc
    ~> ;arrow operator
    -> ;arrow operator
    % ;modulo
    = ;assignment 
    == ;equality
    || ;or
    && ;and
    != ; not equal
    ? ;query 
    ?? ;query 

## Important Functions
***

Most of skill relys on database id (dbId from now on) which is just the unique identifier for a particular object

2 of the most used functions in skill are:


    ;Get the cellviews database id
    cv = dbGetEditCellView()

    ;get the dbid's of the selected objects
    selSet = geGetSelSet()


2 more functions to know that will come in handy later are car and cadr.

These functions only work with lists.

***car( someList ) gets the 1st item from a list***  

***cadr( someList ) gets the 2nd item from a list***


    ;Store a list of coordinates to a variable  
    coordPair = list( 10 10 )  
    coordx = car( coordPair ) ;gets the 1st item from a list
    coordy = cadr( coordPair ) ;gets the 2nd item from a list




> # OK, I've got dbId's now what the heck can I do?
> 
> 
## Database object interogation
***

The dbId's give us access to lots of amazing information

The dbId's contain a property list which is just a list of key/value pairs.

We ask for information from the property list using the
<code>~></code> and the <code>-></code>.

To get a list of the key from a propety list we use the <code>?</code> operator after the arrow operator.


<code>cv~>?</code>
To get all the key/value pairs from a propety list we use the <code>??</code> operator after the arrow operator.


<code>cv~>??</code>

OUTPUT HERE

You can ask for a specific value from the property list by doing:

<code>cellname = cv~>cellName</code>

This returns the name of the current cellview as a string.

***
## **Basic Flow Control**
***
Flow control is how we make the real magic of Skill, and programming in general, happen.

### Types of Flow Control
***
- **Conditional**  
- **Branching**  
- **Loops**
***
### **Conditional**  
Skill has <code>when, unless</code> statements.   

These either stop or allow a section of code to execute depending on the input.

> **When example:**  

    ;Set our variable
    test = t

    when( test == t  
        ;do some stuff here
        printf("Test was true\n")
    )

> **Unless example:**  

    ;Set our variable
    test = nil

    unless( test == t  
        ;do some stuff here
        printf("Test was nil\n")
    )


**I hate <code>unless</code> statements they confuse me.**
***
### **Branching**
Branching tells Skill to do different things depending on the input.

The <code>if</code> or <code>case</code> statements are the means that we do that.

In an <code>if</code> statement if the input criteria evaluates to true ( <code>t</code>  )the first branch executes. If it evaluates to false ( <code>nil</code> ) it leaves the if or continues to the <code>else</code> statement.

> ## **If statement example**  

This can be literally read as:  
>**IF** var is equal to Rob **THEN** print this.
    ;Set our variable
    var = "Rob"  

    if(var == "Rob" then  
         printf("My name is Rob\n")
    );if



> ## **If Else statement example**  

This can be literally read as:  
>**IF** var is greater than 25 **THEN** print this, **ELSE** print that.  
    ;Set our Variable
    var = 10  

    if(var > 25 then  
        printf("Var is greater than 25\n")
    else  
       printf("Var is less than 10\n")
    );if


Case example can be found in the Cadence documents.

***
### **Loops**

Loops are a means to repeat a section of code more then once.

Skill has <code>for</code>, <code>foreach</code>, <code>while</code>, <code>until</code>

The <code>foreach</code> is my **goto** loop in Skill. The reason for that is when I need to iterate on something in Skill it is usually a list. A <code>foreach</code> breaks the list down into each of it's individual list cells and provides that cell to the loop as a local variable.

    ;get a list of the selected items  
    cvInstances = geGetSelSet()   

    foreach( inst cvInstance  
        ;check inst obj to make sure its an instance 
        if( inst~>objType == "inst" then  
            printf("CellName %L Coordinates %L\n" inst~>cellName inst~>xy)   
        );if
    );foreach

***
### **Functions**
Functions are blocks of codes; similar to an instance in layout; that can be recalled later multiple times.

**Best practices for functions**  
1. They should have a descriptive name. <code>RDcreateFile</code>  

    * Make sure the name is unique
2. Should only perform a single task  
3. Should be short. Long code is hard to follow.
4. **ALWAYS** use a <code>let</code> statement  
    *  <code>let</code> keeps variables scoped to the function. Keeps them from becoming global.

    ;;Define out function
    defun( RDcheckWire ( input )  
    let( (isInputAWire)   
        isInputAWire = nil  

        if( input~>objType == "pathSeg" then  
            isInputAWire = t  
        );if

        isInputAWire ;return  
    );let  
    );defun  

    ;;;;END OF FUNCTION

    ;;Call our function  
    if( RDcheckWire( obj )  
        printf("The object is a wire\n")  
    );if

