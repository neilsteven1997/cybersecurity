# Python Basics 

---

In this introductory room on Python scripting, the focus rests on building foundational skills that support tool creation and 
automation in security work, even though programming remains optional for success in the field. Related content like the Scripting for
Pentesters module shows how such abilities help generate quick scripts for hacking, defense, and analysis tasks. The covered topics 
include variables, loops, functions, data structures, if statements, and files, with all exercises executed directly through the code
editor positioned on the right-hand side of the interface. For a local setup, Python downloads from the official website supply a full
integrated development environment. Syntax governs every valid program by enforcing strict rules on symbols, punctuation, and keywords
so the interpreter processes instructions without error. 

A basic program simply prints output through the print function after a comment line marked by #, with strings placed inside quotation 
marks, and all examples here target Python 3. Mathematical operators mirror calculator functions and support addition with +,
subtraction using -, multiplication by *, division via /, modulus with %, and exponentiation through **, allowing scripts to perform
calculations on given inputs. Comparison operators evaluate program states at any point and include greater than with >, less than 
with <, equal to using ==, not equal to via !=, greater than or equal to with >=, and less than or equal to with <=. These become 
central once loops and conditionals enter play. 

Variables store and update data under user-defined names, such as assigning a string to one identifier and an integer to another, 
then modifying the value later by reassigning the same name to an expression involving its current content. Data types classify stored 
information precisely: strings handle character sequences, integers cover whole numbers, floats manage decimals or fractions, booleans
restrict values to true or false, and lists collect mixed items in a single variable. Logical operators enable conditional tests inside 
structures like if statements and consist of equivalence with ==, less than with <, less than or equal to with <=, greater than with >, and greater than or equal to with >=. Boolean operators link multiple conditions: AND demands both true, OR accepts any one true,
>and NOT reverses the operand’s truth value. 

Code examples illustrate these through simple if checks using OR on numeric conditions, or multi-branch if-elif-else blocks that 
evaluate string equality alongside boolean flags to select output paths. If statements drive decision logic by testing a condition 
after the if keyword, executing indented code only when true and defaulting to an else block otherwise, always ending the header with
a colon. Loops repeat actions efficiently, split into while variants that continue while a condition holds and for variants that 
traverse sequences. A while loop initializes a counter, tests the limit each iteration, prints the value, increments the counter, and
restarts until the condition fails. For loops step through list elements one by one until exhausted or cycle through numbers generated
by the range function, which counts from zero by default. 

The Python for Pentesters room later applies for loops to enumerate targets, build keyloggers, scan networks, and similar operations. 
Functions eliminate repetition in growing programs by wrapping reusable code under a def keyword, followed by a chosen name, 
parentheses for parameters, a colon, and an indented body that can return computed results. File handling opens content with the 
built-in open function in read mode to access or display data via read or readlines, or switches to append mode for existing files 
and write mode for new ones, always closing afterward to prevent further changes. Imports bring in external libraries such as datetime
for time functions accessed through the library name and method calls. Popular options for pentesting include Request for simple HTTP
work, Scapy for crafting and dissecting network packets, and Pwntools for capture-the-flag and exploit development; any not 
pre-installed load through the pip package manager.

---

| Description | Code/Command |
|-------------|--------------|
| Hello World program with comment | # This is an example comment<br>print("Hello World") |
| Variable assignment for string and integer | food = "ice cream"<br>money = 2000 |
| Updating variable value and printing result | age = 30<br>age = age + 1<br>print(age) |
| If statement using logical OR | a = 1<br>if a == 1 or a > 10:<br>    print("a is either 1 or above 10") |
| Complex if-elif-else with string and boolean | name = "bob" <br>hungry = True<br>if name == "bob" and hungry == True:<br>    print("bob is hungry")<br>elif name == "bob" and not hungry:<br>    print("Bob is not hungry")<br>else: # If all other if conditions are not met<br>    print("Not sure who this is or if they are hungry") |
| Basic if-else condition on age | if age < 17:<br>    print('You are NOT old enough to drive')<br>else:<br>    print('You are old enough to drive') |
| While loop counting from 1 to 10 | i = 1<br>while i <= 10:<br>     print(i)<br>     i = i + 1 |
| For loop iterating over list of websites | websites = ["facebook.com", "google.com", "amazon.com"]<br>for site in websites:<br>     print(site) |
| For loop using range to print 0-4 | for i in range(5):<br>     print(i) |
| Function definition with parameter and call | def sayHello(name):<br>     print("Hello " + name + "! Nice to meet you.")<br><br>sayHello("ben") # Output is: Hello ben! Nice to meet you |
| Function with if-elif return values and usage | def calcCost(item):<br>     if(item == "sweets"):<br>          return 3.99<br>     elif (item == "oranges"):<br>          return 1.99<br>     else:<br>          return 0.99<br><br>spent = 10<br>spent = spent + calcCost("sweets")<br>print("You have spent:" + str(spent)) |
| Opening file in read mode and printing content | f = open("file_name", "r")<br>print(f.read()) |
| Appending text to existing file | f = open("demofile1.txt", "a") # Append to an existing file<br>f.write("The file will include more text..")<br>f.close() |
| Creating and writing to new file | f = open("demofile2.txt", "w") # Creating and writing to a new file<br>f.write("demofile2 file created, with this content in!")<br>f.close() |
| Importing datetime and printing current time | import datetime<br>current_time = datetime.datetime.now()<br>print(current_time) |
| Command to install Scapy library | pip install scapy |

---

Extracted Tables

**Mathematical Operators**

| Operator | Syntax | Example |
|----------|--------|---------|
| Addition | + | 1 + 1 = 2 |
| Subtraction | - | 5 - 1 = 4 |
| Multiplication | * | 10 * 10 = 100 |
| Division | / | 10 / 2 = 5 |
| Modulus | % | 10 % 2 = 0 |
| Exponent | ** | 5**2 = 25 (52) |

**Comparison Operators**

| Symbol | Syntax |
|--------|--------|
| Greater than | > |
| Less than | < |
| Equal to | == |
| Not Equal to | != |
| Greater than or equal to | >= |
| Less than or equal | <= |

**Logical Operators**

| Logical Operation | Operator | Example |
|-------------------|----------|---------|
| Equivalence | == | if x == 5 |
| Less than | < | if x < 5 |
| Less than or equal to | <= | if x <= 5 |
| Greater than | > | if x > 5 |
| Greater than or equal to | >= | if x >= 5 |

**Boolean Operators**

| Boolean Operation | Operator | Example |
|-------------------|----------|---------|
| Both conditions must be true for the statement to be true | AND | if x >= 5 AND x <= 100Returns TRUE if x isa number between 5 and 100 |
| Only one condition of the statement needs to be true  | OR | if x == 1 OR x == 10Returns TRUE if X is 1 or 10 |
| If a condition is the opposite of an argument | NOT | if NOT yReturns TRUE if the y value is False |

---

### Key Takeaways
- The room teaches core Python elements for security scripting: variables, loops, functions, data structures, if statements,
  files
- Data types stored in variables: string for character combinations, integer for whole numbers, float for decimals or fractions,
   boolean for true/false only, list for series of mixed items in one collection
- Key components of an if statement: if keyword starts the block, followed by condition that must evaluate true to run indented code,
   terminated by colon, with else handling unmet conditions
- While loop execution sequence: set initial variable, test condition at start of each iteration, perform action such as printing,
  update variable, repeat until condition becomes false
- For loop execution sequence: create list or range, iterate through every element automatically, execute indented code on each, stop
  after final item
- Key components of a function: def keyword begins definition, followed by programmer-chosen name and parentheses for parameters,
  colon ends header, indented body contains logic and optional return
- File operations steps: use open with filename and mode (r for read, a for append to existing, w for new/write), apply read/write
  methods, always call close to finalize
- Useful pentesting libraries: Request for simple HTTP handling, Scapy to send sniff dissect and forge packets, Pwntools for CTF and
   exploit development (install non-built-in ones via pip)

---





