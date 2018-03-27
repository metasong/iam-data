# You Don't Know JS: Scope & Closures 
*summery*
* Engine: responsible for start-to-finish compilation and execution of our JavaScript program.

* Compiler: one of Engine's friends; handles all the dirty work of parsing and code-generation.

* Scope: another friend of Engine; collects and maintains a look-up list of all the declared identifiers (variables), and enforces a strict set of rules as to how these are accessible to currently executing code.

`var a = 2;`

The first thing Compiler will do with this program is perform lexing to break it down into tokens, which it will then parse into a tree.

Encountering `var a`, Compiler asks Scope to see if a variable a already exists for that particular scope collection. If so, Compiler ignores this declaration and moves on. Otherwise, Compiler asks Scope to declare a new variable called a for that scope collection.

Compiler then produces code for Engine to later execute, to handle the a = 2 assignment. The code Engine runs will first ask Scope if there is a variable called a accessible in the current scope collection. If so, Engine uses that variable. If not, Engine looks elsewhere.If Engine eventually finds a variable, it assigns the value 2 to it. If not, Engine will raise its hand and yell out an error!

> **Note:** LHS and RHS meaning "left/right-hand side of an assignment" doesn't necessarily literally mean "left/right side of the = assignment operator". There are several other ways that assignments happen, and so it's better to conceptually think about it as: "who's the target of the assignment (LHS)" and "who's the source of the assignment (RHS)".
