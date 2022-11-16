---
layout: activity
permalink: /Activities/ParserInterpreter3
title: "CS374: Programming Language Principles - Parsers and Interpreters"
excerpt: "CS374: Programming Language Principles - Parsers and Interpreters"

info: 
  prev: ./ParserInterpreter2
  next: ./ParserInterpreter4
  goals: 
    - To describe the components of an LL(k) parser
    - To implement a simple parser given a grammar, from code or using tools such as lex and yacc
  models:
    - model: |
        <h2>calc.y</h2>
        <script type="syntaxhighlighter" class="brush: cpp"><![CDATA[
        %{

        #include <stdio.h>
        #include <stdlib.h>

        extern int yylex();
        extern int yyparse();
        extern FILE* yyin;

        void yyerror(const char* s);
        %}

        %union {
            int ival;
        }

        %token<ival> T_INT
        %token T_PLUS T_MINUS T_MULTIPLY T_DIVIDE T_LEFT T_RIGHT T_NEWLINE

        %type<ival> expr term factor
        %start calc

        %%

        calc:                               { printf("EOF\n"); exit(0); }
            | expr T_NEWLINE                { printf("%d\n", $1); exit(0); } // no return value needed, no type

        expr: term T_PLUS expr              { $$ = $1 + $3; }
            | term T_MINUS expr             { $$ = $1 - $3; }
            | term                          { $$ = $1; }
            
        term: factor T_MULTIPLY term        { $$ = $1 * $3; }
            | factor T_DIVIDE term          { $$ = $1 / $3; }
            | factor                        { $$ = $1; }
            
        factor: T_INT                       { $$ = $1; }            // just resolve the value!
              | T_LEFT expr T_RIGHT         { $$ = $2; }            // expr will resolve to a value!
              
        %%

        int main() {
            yyin = stdin;
            
            do {
                yyparse();
            } while(!feof(yyin));
        }

        void yyerror(const char* s) {
            fprintf(stderr, "Parse error: %s\n", s);
            exit(1);
        }

        // https://github.com/meyerd/flex-bison-example
        // the token types are as defined in the %union above
        ]]></script> 
        <br>
        <h2>calc.h</h2>
        <script type="syntaxhighlighter" class="brush: cpp"><![CDATA[
        %option noyywrap       

        %{
        #include <stdio.h>

        #define YY_DECL int yylex()

        #include "calc.tab.h"

        %}

        %%

        [ \t]	                ; // ignore all whitespace
        [0-9]+		            {yylval.ival = atoi(yytext); return T_INT;}
        "+"		                {return T_PLUS;}
        "-"		                {return T_MINUS;}
        "*"		                {return T_MULTIPLY;}
        "/"		                {return T_DIVIDE;}
        "("		                {return T_LEFT;}
        ")"		                {return T_RIGHT;}
        \n                      {return T_NEWLINE;}

        %%

        // https://github.com/meyerd/flex-bison-example
        // noyywrap assumes that there are no additional files to be parsed
        ]]></script>         
      title: "Scanning with lex and Parsing with yacc"
      questions:
        - "How would you revise this to support the original corrected LL(1) grammar?"

  additional_reading:
    - title: "A BASIC Language Interpreter with lex and yacc"
      link: "https://www.viscerallogic.com/programming/blog/basic-interpreter-part-i/"
    - title: "A simple language interpreter with lex and yacc"
      link: "https://github.com/MJ10/Yet-Another-Programming-Language"
      
tags:
  - parser
  - interpreter
  
---

