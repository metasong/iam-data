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

```js
function foo(a) {
	console.log( a ); // 2
}

foo( 2 );
```

> Engine: Hey Scope, I have an RHS reference for foo. Ever heard of it?

> Scope: Why yes, I have. Compiler declared it just a second ago. He's a function. Here you go.

> Engine: Great, thanks! OK, I'm executing foo.

> Engine: Hey, Scope, I've got an LHS reference for a, ever heard of it?

> Scope: Why yes, I have. Compiler declared it as a formal parameter to foo just recently. Here you go.

>Engine: Helpful as always, Scope. Thanks again. Now, time to assign 2 to a.

> Engine: Hey, Scope, sorry to bother you again. I need an RHS look-up for console. Ever heard of it?

> Scope: No problem, Engine, this is what I do all day. Yes, I've got console. He's built-in. Here ya go.

> Engine: Perfect. Looking up log(..). OK, great, it's a function.

> Engine: Yo, Scope. Can you help me out with an RHS reference to a. I think I remember it, but just want to double-check.

> Scope: You're right, Engine. Same guy, hasn't changed. Here ya go.

> Engine: Cool. Passing the value of a, which is 2, into log(..).

> ...

## Nested Scope

Scope is the set of rules that determines where and how a variable (identifier) can be looked-up. This look-up may be for the purposes of assigning to the variable, which is an LHS (left-hand-side) reference, or it may be for the purposes of retrieving its value, which is an RHS (right-hand-side) reference.

LHS references result from assignment operations. Scope-related assignments can occur either with the = operator or by passing arguments to (assign to) function parameters. Just as a block or function is nested inside another block or function, scopes are nested inside other scopes. Engine starts at the currently executing Scope, looks for the variable there, then if not found, keeps going up one level, and so on. If the outermost global scope is reached, the search stops, whether it finds the variable or not.Unfulfilled RHS references result in ReferenceErrors being thrown. Unfulfilled LHS references result in an automatic, implicitly-created global of that name (if not in "Strict Mode" [^note-strictmode]), or a ReferenceError (if in "Strict Mode" [^note-strictmode]).
[^note-strictmode]: MDN: Strict Mode


https://github.com/getify/You-Dont-Know-JS/blob/master/scope%20%26%20closures/ch2.md

lexical scope is scope that is defined at lexing time. In other words, lexical scope is based on where variables and blocks of scope are authored, by you, at write time, and thus is (mostly) set in stone by the time the lexer processes your code.

> **Note**: We will see in a little bit there are some ways to cheat lexical scope, thereby modifying it after the lexer has passed by, but these are frowned upon. It is considered best practice to treat lexical scope as, in fact, lexical-only, and thus entirely author-time in nature.

```js
function foo(a) {

	var b = a * 2;

	function bar(c) {
		console.log( a, b, c );
	}

	bar(b * 3);
}

foo( 2 ); // 2 4 12
```


