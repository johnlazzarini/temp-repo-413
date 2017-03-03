# ﻿John Lazzarini's submission for CSC 413 Assignment 2
#### https://github.com/SFSU-CSC-413/assignment-2-johnlazzarini
## Project introduction and overview
This program demonstrates the functionality of a simple lexical analyzer.  It receives a source program as
input and breaks it down into tokens -- where a token, such as "program", "int", etc) is defined
by the grammar of the language being analyzed.

Much of the program was written for me, but I made some changes to add or improve functionality as required by the program spec.

#### Changes made to meet requirement 1
- Replaced the file argument in Lexer.java's main() method with a String assigned to args[0].

#### Changes made to meet requirement 2

- Added two lines ("VOid void" and "FLoat float") to the end of the tokens file.

<br>Running TokensSetup regenerates the Tokens and TokenTypes classes to add the functionality necessarry to support void and float tokens.

#### Changes made to meet requirement 3

- Added a private data member, private int lineNumber, to the Token class.
- Adjusted the Token class constructor to accommodate the new private member
- Adjusted all Token object constructor calls in Lexer.java to support the new constructor
definition -- of particular importance was the change to Token's newNumberToken() method, which now
returns the following: 
````
        return new Token(
          startPosition, endPosition,
          Symbol.symbol(number, Tokens.INTeger), source.getLineno()
        );
````
Note that the last argument, source.getLineno(), will now instantiate Token objects with
a private lineNumber datum assigned to the corresponding token's line number.

#### Changes made to meet requirement 4

- Removed the System.out.println() methods in SourceReader.java which output file information.
- Removed the output source code from Lexer.java's main() method and replaced it with
the following:

````
                System.out.printf("%-20s %-20s %-20s %s",
                  tok.toString(),
                  "Left: " + tok.getLeftPosition(),
                  "Right: " + tok.getRightPosition(),
                  "Line: " + tok.getLineNumber() + "\n");
                      
````
This "one-line" statement combines the output and formatting operations by itself.

#### Changes made to meet requirement 5
- Added a private StringBuilder sourceAsString data member to SourceReader.java -- a private String
could also have been used, but this method saves more memory.
- Modified the body of SourceReader.java's read() method to look like the following: 

````
if( nextLine != null ) {
     System.out.println( "READLINE:   " + nextLine);
     sourceAsString.append(lineno + ".  " + nextLine + "\n");

      } 
````

- read() now adds the line and linenumber of the current line to sourceAsString.
- Redefined SourceReader's toString method to return sourceAsString.toString().
- Redefined Lexer.java's toString method to return source's toString().
- Added a print statement to the end of the try-catch block in Lexer.java's main() method -- this calls its own toString method, which I redefined in the bulletpoint above in order to get around main's unwillingness to work with instance methods as a static method itself.
- Removed a null assignment that otherwise breaks the program.

## Instructions for compiling and executing as Jar

```
echo Main-Class: lexer/Lexer.java > manifest.txt
javac lexer/*.java
jar cvfm Lexer.jar manifest.txt lexer/*.class
java -jar Lexer.jar factorial.x
```
Replace "factorial.x" with whatever file that you want to run as an argument to Lexer.java's main() method (simple.x, etc).

## Assumptions

This program assumes that the files that it analyzes will be "x files" -- it's specifically written to accommodate x filetype grammar and assumes that the user wouldn't try to input, say, a .java file.  I'm also, of course, assuming that all of the programming that was provided for me does what it's supposed to do.

## Implementation discussion

![alt text](http://i.imgur.com/YlZFoT0.png "Lexer, SourceReader, Token")

 The above image describes the architecture of the Lexer, SourceReader, and Token classes.  Most of the variable names are self describing to somebody who is familiar with file input/output, but might be a little confusing otherwise -- Lexer's EOF, for example, indicates whether the "End of File" has been reached or not.
 
#### toString()
You can see that I've defined toString() methods for both SourceReader.java and Lexer.java.  Lexer's toString() places a function call to SourceReader's toString(), which itself calls the sourceAsString's own toString() method.

#### Why are we making three calls to three differen't toString() methods?
It might seem like a bad design decision, and in retrospect perhaps I should have picked slightly more descriptive output names.  But this chain of toString() calls was the only way that I could display the contents of sourceAsString from Lexer without using static members or methods (which you told us was wrong in class). 
 
 ![alt text](http://i.imgur.com/dDy2Zda.png "Lexer, SourceReader, Token")
 
 The above image describes the architecture of the Tokens, Symbol, and TokenType classes.  Sorry for the very long Tokens UML, but I felt it would be helpful to see the list of all its members -- particularly the GREAT, FLOAT, and VOID tokens that were added by editing the tokens file.  Note that the symbol class contains a HashMap that is very similar to the one that was used in Assignment One.
 
 ![alt text](http://i.imgur.com/qgIS5nR.png "Complex UML graphic")
 
The above image is a complex UML diagram that displays both relationships and content for classes -- I threw this in just to provide a more complete picture of what the program looks like.

## Results and Conclusions

This program meets the requirements of the spec and works as expected.  It successfully breaks programs down into tokens and displays them alongside their line numbers.  Illegal tokens are identified alongside their line numbers, and a String representation of the source code is outputted at the end of the token line items.

There were a couple of things that I had a hard time with understanding -- one was the while loop in the main() method within in Lexer.java.  It was difficult for me to see how it could be possible for the program to end while involving an infinite loop with no obvious break case -- this was before I really understood that there was an exception being thrown within it.

Another issue that I had involved the fact that main() didn't want me to access the source member, since main is a static method. I had to use a hacky fix in order to get around it, described in the toString() explanation in the implementation discussion.

This was a good assignment though, I ended up learning a little bit more about how a program can be broken down into portions that a parser is able to interpret and understand.
