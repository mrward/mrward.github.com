---
layout: post
title: "Creating a JavaScript Parser with ANTLR that can be used from CSharp"
date: 2011-08-21 19:30
comments: true
categories: ANTLR JavaScript
---

How do you create a JavaScript parser that you can use from C#? One way is to use [ANTLR](http://www.antlr.org/about.html). ANother Tool for Language Recognition (ANTLR) is a parser generated created and maintained by Terence Parr.

The ANTLR website has a JavaScript grammar already defined so we will take that grammar, get ANTLR to create a parser, and then use that parser to parse some JavaScript code. The following guide is based on using [ANTLRWorks 1.4.2](http://www.antlr.org/works/index.html), which includes ANTLR 3.3, and the CSharp3 code generator version 3.3.1.

##Installing ANTLR

1. Install the [Java JDK 1.5](http://www.oracle.com/technetwork/java/javase/downloads/index-jdk5-jsp-142662.html). Newer versions than 1.5 may work with ANTLR.
2. Download [ANTLRWorks 1.4.2](http://www.antlr.org/download/antlrworks-1.4.2.jar) (which includes Antlr 3.3).

Check ANTLRWorks can be started by either double clicking the antlrworks-1.4.2.jar file or opening a command prompt and running the following command.

    java -jar antlrworks-1.4.2.jar

##Generating a JavaScript Parser

Download a JavaScript grammar from http://www.antlr.org/grammar/list](http://www.antlr.org/grammar/list).

I used [Chris Lambrou's JavaScript grammar](http://www.antlr.org/grammar/1206736738015/JavaScript.g) since this was the most recent.

Open the JavaScript.g file in ANTLRWorks by selecting Open from the File menu.

Edit the JavaScript.g file and add "language=CSharp3" to the options section.

    options 
    { 
        output=AST; 
        backtrack=true; 
        memoize=true; 
        language=CSharp3; 
    }

The language determines what language the parser code will be generated in. To generate C# there are 3 choices for the language.

1. [CSharp](http://www.antlr.org/wiki/display/ANTLR3/Antlr+3+CSharp+Target)  (.NET 1.1 code generated)
2. CSharp2 (.NET 2.0 code generated)
3. [CSharp3](http://www.antlr.org/wiki/display/ANTLR3/Antlr3CSharpReleases) (.NET 3 code generated)

To specify a namespace for the generated C# classes add another line to the JavaScript.g file.

    @namespace { ICSharpCode.JavaScriptBinding }
        
 In ANTLRWorks select Generate Code from the Generate menu. Three files should be generated in an output directory below where  JavaScript.g file is located

* JavaScript.tokens
* JavaScriptLexer.cs
* JavaScriptParser.cs

The two C# files are the ones we are interested in and we will now take a look at how we can use them in a C# application.

##Using the JavaScript Parser

The C# files that were generated depend on another set of assemblies.

If you generated code using the CSharp3 target then download the [CSharp3 assemblies](http://www.tunnelvisionlabs.com/downloads/antlr/antlr-dotnet-tool-3.3.1.7705.7z).

For code generated using the CSharp2 and CSharp then download [CSharp2 assemblies](http://www.antlr.org/download/CSharp).

In both cases the assembly you need is Antlr3.Runtime.dll so extract that file from the downloaded zip file.

Now we need to use our parser and lexer.

Create a C# console project and add the JavaScriptLexer.cs and JavaScriptParser.cs files into the project. Add a reference to the Antlr3.Runtime.dll.

Compile the project.

If you receive an error about HIDDEN being undefined then rename HIDDEN to Hidden in the JavaScriptLexer.cs file.

Remove the "[System.CLSCompliant(false)]" from the lexer and parser files to fix the warning about not requiring CLSCompliant to be set.

In order to get access to the results the parser generates you will need to make a modification to the JavaScriptParser.cs. Make the program() method public, as shown below.

    public JavaScriptParser.program_return program()

If you generate the parser and lexer as java source code files this method is public but for it is private when using any of the CSharp targets.

The correct way to fix the problem with the program method being private is to modify the JavaScript grammar file. If you add the public keyword to JavaScript.g grammar file as shown below then after regenerating the lexer and parser files using ANTLRWorks the  program method will be public.

     public program 
          : LT!* sourceElements LT!* EOF! 
          ; 

Now how do we use the parser? The following code shows you  how.

    using System; 
    using Antlr.Runtime; 
    using Antlr.Runtime.Tree; 
    using ICSharpCode.JavaScriptBinding; 
     
    namespace JavaScriptParserConsole 
    { 
        class Program 
        { 
            public static void Main(string[] args) 
            { 
                try { 
                    string text = "var a = 1;"; 
                     
                    var stream = new ANTLRStringStream(text); 
                    var lexer = new JavaScriptLexer(stream); 
                    var tokenStream = new CommonTokenStream(lexer); 
                    var parser = new JavaScriptParser(tokenStream); 
                    JavaScriptParser.program_return programReturn = parser.program(); 
                     
                } catch (Exception ex) { 
                    Console.WriteLine(ex.ToString()); 
                } 
                 
                Console.Write("Press any key to continue..."); 
                Console.ReadKey(true); 
            } 
        } 
    } 

First we have some JavaScript code to parse. In this case we have a simple variable assignment. This JavaScript code is then passed to a ANTLRStringStream class. The lexer takes this stream and produces a set of tokens. We turn the lexer into a CommonTokenStream and then this is passed to the parser. The program_return class returned from the parser is an [Abstract Syntax Tree](http://en.wikipedia.org/wiki/Abstract_syntax_tree) (AST). Getting access to the AST will depend on how the grammar has been defined. In the JavaScript grammar the top part of the AST is called program.

      public program 
          : LT!* sourceElements LT!* EOF! 
          ;
      
This maps to a method called program in our parser class.

Does a grammar always create an AST? It depends on the grammar options. For the JavaScript grammar we are using:

    options 
    { 
        output=AST; 
        backtrack=true; 
        memoize=true; 
        language=CSharp3; 
    }

The output is defined to be AST so ANTLR generates classes to produce an AST after parsing the source code. There are two [options](http://www.antlr.org/wiki/display/ANTLR3/Grammar+options) for output: AST and template. If this option is not specified the default is AST.

Now we still have not done anything useful with the parse results. So let us display the AST.

    using System; 
    using Antlr.Runtime; 
    using Antlr.Runtime.Tree; 
    using ICSharpCode.JavaScriptBinding; 
     
    namespace JavaScriptParserConsole 
    { 
        class Program 
        { 
            public static void Main(string[] args) 
            { 
                try { 
                    string text = "var a = 1;"; 
                     
                    var stream = new ANTLRStringStream(text); 
                    var lexer = new JavaScriptLexer(stream); 
                    var tokenStream = new CommonTokenStream(lexer); 
                    var parser = new JavaScriptParser(tokenStream); 
                    JavaScriptParser.program_return programReturn = parser.program(); 
                     
                    var tree = programReturn.Tree as CommonTree; 
                    WriteTree(tree); 
                     
                } catch (Exception ex) { 
                    Console.WriteLine(ex.ToString()); 
                } 
                 
                Console.Write("Press any key to continue..."); 
                Console.ReadKey(true); 
            } 
             
            static void WriteTree(CommonTree tree) 
            { 
                WriteLine("Tree.Text: " + tree.Text); 
                WriteLine("Tree.Type: " + tree.Type); 
                WriteLine("Tree.Line: " + tree.Line); 
                WriteLine("Tree.CharPositionInLine: " + tree.CharPositionInLine); 
                WriteLine("Tree.ChildCount: " + tree.ChildCount); 
                WriteLine(""); 
                 
                if (tree.Children != null) { 
                    IncreaseIndent(); 
                    foreach (CommonTree child in tree.Children) { 
                        WriteTree(child); 
                    } 
                    DecreaseIndent(); 
                } 
            } 
             
            static int indent = 0; 
             
            static void WriteLine(string text) 
            { 
                string indentText = String.Empty.PadRight(indent * 2); 
                Console.WriteLine(indentText + text); 
            } 
             
            static void IncreaseIndent() 
            { 
                indent++; 
            } 
             
            static void DecreaseIndent() 
            { 
                indent--; 
            } 
        } 
    } 

We have taken the program_return class that is returned from the parser's program method and displayed its tree.

    public class program_return 
        : ParserRuleReturnScope<IToken>, IAstRuleReturnScope<object> 
    { 
        private object _tree; 
        public object Tree { 
            get { return _tree; } 
            set { _tree = value; } 
        } 
    }

The Tree property is a CommonTree but by default ANTLR returns this as an object. We then pass this tree to a method that displays a few properties from the tree and then looks at the tree's children and displays them. Each child is also a tree so we call the WriteTree method recursively. With the simple javascript code of "var a = 1;" we get the following output when running this code:

    Tree.Text: 
    Tree.Type: 0 
    Tree.Line: 1 
    Tree.CharPositionInLine: 0 
    Tree.ChildCount: 4 
     
      Tree.Text: var 
      Tree.Type: 37 
      Tree.Line: 1 
      Tree.CharPositionInLine: 0 
      Tree.ChildCount: 0 
     
      Tree.Text: a 
      Tree.Type: 5 
      Tree.Line: 1 
      Tree.CharPositionInLine: 4 
      Tree.ChildCount: 0 
     
      Tree.Text: = 
      Tree.Type: 39 
      Tree.Line: 1 
      Tree.CharPositionInLine: 6 
      Tree.ChildCount: 0 
     
      Tree.Text: 1 
      Tree.Type: 7 
      Tree.Line: 1 
      Tree.CharPositionInLine: 8 
      Tree.ChildCount: 0 

Can we specify the type of the Tree? The answer is yes. In the  JavaScript grammar we can specify the type to be used for the Tree property by using the ASTLabelType.

    options 
    { 
        output=AST; 
        backtrack=true; 
        memoize=true; 
        ASTLabelType=CommonTree; 
        language=CSharp3; 
    } 

So we can set the tree's type to be CommonTree and we would not need to convert the Tree property from an Object to a CommonTree each time. With this change made the lexer and parser class files need to be regenerated with AntlrWorks. We also have to make the fixes described above (HIDDEN and ClsCompliant) to the generated code.

Now the program_return class has been changed:

    public class program_return 
        : ParserRuleReturnScope<IToken>, IAstRuleReturnScope<CommonTree> 
    { 
        private CommonTree _tree; 
        public CommonTree Tree {
            get { return _tree; } 
            set { _tree = value; }
        } 
    } 

Unfortunately this code does not compile and we get the error shown below.

Error CS0738:  'ICSharpCode.JavaScriptBinding.JavaScriptParser.program_return' does not implement interface member 'Antlr.Runtime.IAstRuleReturnScope.Tree'. ICSharpCode.JavaScriptBinding.JavaScriptParser.program_return.Tree' cannot implement 'Antlr.Runtime.IAstRuleReturnScope.Tree' because it does not have the matching return type of 'object'.

Looking at the interface definitions it looks like there is a problem with the code generation since the new program_return class does not implement the IAstRuleReturnScope interface completely:

    public interface IAstRuleReturnScope<TAstLabel> 
        : IAstRuleReturnScope, IRuleReturnScope 
    { 
        TAstLabel Tree 
        { 
            get; 
        } 
    } 
     
    public interface IAstRuleReturnScope : IRuleReturnScope 
    { 
        object Tree 
        { 
            get; 
        } 
    }

So we will have to switch back to not defining the tree's type by not using the ASTLabelType option.

##Summary

In summary we have taken an existing JavaScript grammar, generated a parser using ANTLR and then used this parser to parse some JavaScript code and display the results held in the AST.

