

A Compiler for the IBM XPL language
===================================
<br>
This is a compiler for the IBM XPL language (see here: https://en.wikipedia.org/wiki/XPL). For a while IBM made a compiler available for this system programming language and then they stopped. 
<br>
This compiler compiles the IBM XPL syntax into S/390 code. This compiler is written in IBM PL/I which is an industry semi-standard. It has compiled on my z/OS 2.1 system with the IBM Enterprise PL/I compiler 4.3.1 just fine. 
<br>
It is supplied as is, without any gurantee. 
<br>

More Information about the XPL language (incl syntax and sample code)
---------------------------------------------------------------------

http://teampli.net/XPL/
<br>Sample Code:

<pre>
*  This program reads n cards (n= 100), sorts them in
    alphabetical (collating) order and prints them. */

declare n literally '100';
declare cards (n) character, (i,j,k) fixed, temp character;

output = 'Input cards:';
do i = 1 to n;
   output, cards(i) = input; /* read and list    */
end;

k,l = n;
do while k <= l;             /* bubble sort loop */
    l = - n;
    do i = 1 to k;
       l = i - 1;
       if cards(l) > cards(i) then
           do;
               temp = cards(l);
               cards(l) = cards(i);
               cards(i) = temp;
               k = l;
           end;
    end;
end;                         /* of sort loop */

output = 'Sorted cards:';
do i = 1 to n;
    output = cards(i);
end;

</pre>



<br>
<br>

thanks

moshix
<p>August 2020</P>
<br>
<br>
<br><br>

Backus-Naur Form of XPL
-----------------------
<pre>
BNF Definition of the XPL Language


<program>		::= <statement list> EOF

<statement list>	::= <statement>
			  | <statement list> <statement>

<statement>  		::= <basic statement>
			  | <if statement>

<basic statement>  	::= <assignment> ;
			  | <group> ;
			  | <procedure definition> ;
			  | <return statement> ;
			  | <call statement> ;
			  | <go to statement> ;
			  | <declaration statement> ;
                          | ;
			  | <label definition> <basic statement>

<if statement>       	::= <if clause> <statement> 
			  | <if clause> <true part> <statement>
			  | <label definition> <if statement>

<if clause>		::= IF <expression> THEN 

<true part>	        ::= <basic statement> ELSE

<group>                 ::= <group head> <ending>

<group head>            ::= DO ;
                          | DO <step definition> ;
                          | DO <while clause> ;
                          | DO <case selector> ;
                          | <group head> <statement>

<step definition>       ::= <variable> <replace> <expression> <iteration control>

<iteration control>     ::= TO <expression>
                          | TO <expression> BY <expression>

<while clause>	        ::= WHILE <expression>

<case selector>         ::= CASE <expression>

<procedure definition>  ::= <procedure head> <statement list> <ending>

<procedure head>        ::= <procedure name> ;
                          | <procedure name> <type> ;
                          | <procedure name> <parameter list> ;
                          | <procedure name> <parameter list> <type> ;

<procedure name>        ::=  <label definition> PROCEDURE

<parameter list>        ::= <parameter head> <identifier> )

<parameter head>        ::=  (
                          | <parameter head> <identifier> ,

<ending>                ::= END
                          | END <identifier>
                          | <label definition> <ending>

<label definition>      ::= <identifier> :

<return statement>      ::= RETURN
                          | RETURN <expression>

<call statement>        ::= CALL <variable>

<go to statement>       ::= <go to> <identifier>

<go to>                 ::= GO TO
                          | GOTO

<declaration statement> ::= DECLARE <declaration element>
		          | <declaration statement> , <declaration element>

<declaration element>   ::= <type declaration>
                          | <identifier> LITERALLY <string>

<type declaration>      ::= <identifier specification> <type>
                          | <bound head> <number> ) <type>
                          | <type declaration> <initial list>

<type>                  ::= FIXED
                          | CHARACTER
                          | LABEL
                          | <bit head> <number> )

<bit head>              ::= BIT (

<bound head>            ::= <identifier specification> (

<identifier specification> ::= <identifier>
                             | <identifier list> <identifier> )

<identifier list>          ::= (
                             | <identifier list> <identifier> ,
 
<initial list>             ::= <initial head> <constant> )

<initial head>             ::= INITIAL (
                             | <initial head> <constant> ,

<assignment>               ::= <variable> <replace> <expression>
                             | <left part> <assignment>

<replace>                  ::= =

<left part>                ::= <variable> ,

<expression>               ::= <logical factor>
                             | <expression> | <logical factor>

<logical factor>           ::= <logical secondary>
                             | <logical factor> & <logical secondary>

<logical secondary>        ::= <logical primary>
                             | ¬ <logical primary>

<logical primary>          ::= <string expression>
                             | <string expression> <relation> <string expression>

<relation>                 ::= =
                             | <
                             | >
                             | ¬ =
                             | ¬ <
                             | ¬ >
                             | < =
          		     | > =

<string expression>        ::= <arithmetic expression>
			     | <string expression> ||  <arithmetic expression>

<arithmetic expression>    ::= <term>
                             | <arithmetic expression> + <term>
                             | <arithmetic expression> - <term>
                             | + <term>
                             | - <term>

<term>                     ::= <primary>
                             | <term> * <primary>
                             | <term> / <primary>
                             | <term> MOD <primary>

<primary>         ::= <constant>
                    | <variable>
                    | ( <expression> )

<variable>        ::= <identifier>
                    | <subscript head> <expression> )

<subscript head>  ::= <identifier> (
                    | <subscript head> <expression> ,

<constant>        ::= <string>
                    | <number>


The following definitions are hardcoded into the lexical scanner:


<number>	  ::= <integer>
                    | <bit string>

<integer>         ::= <decimal digit>
                    | <integer> <decimal digit>

<decimal digit>   ::= 0|1|2|3|4|5|6|7|8|9

<bit string>      ::= "<bit list>"

<bit list>        ::= <hex integer>
		    | <bit group>
                    | <bit list> <bit group>

<bit group>       ::= (1)<binary integer>
                    | (2)<quartal integer>
                    | (3)<octal integer>
                    | (4)<hex integer>

<binary integer>  ::= <binary digit>
                    | <binary integer> <binary digit>

<binary digit>    ::= 0|1

<quartal integer> ::= <quartal digit>
                    | <quartal integer> <quartal digit>

<quartal digit>   ::= 0|1|2|3

<octal integer>   ::= <octal digit>
                    | <octal integer> <octal digit>

<octal digit>     ::= 0|1|2|3|4|5|6|7

<hex integer>     ::= <hex digit>
                    | <hex integer> <hex digit>

<hex digit>       ::= 0|1|2||3|4|5|6|7|8|9|A|B|C|D|E|F

<string>          ::= '<characters>'
                    | ''

<characters>      ::= <character>
                    | <characters> <character>

<character>       ::= ''
                    | {any EBCDIC character other than ' }

<identifier>      ::= <id character>
                    | <identifier> <id character>
                    | <identifier> <decimal digit>

<id character>    ::= <letter> | <break character>

<letter>          ::= A|B|C ... |Z|a|b|c ... |z

<decimal digit>   ::= 0|1| ... |9

<break character> ::= _ | @ | # | $

No identifier can exceed 256 characters.  The following reserved words cannot be
used as identifiers:
BIT	DECLARE 	GOTO	PROCEDURE
BY	ELSE	IF	RETURN
CALL	END	INITIAL	THEN
CASE	EOF	LABEL	TO
CHARACTER 	FIXED	LITERALLY 	WHILE
DO	GO	MOD


<comment>           ::= <opening bracket> <almost anything> <closing bracket>

<opening bracket>   ::= /*

<closing bracket>   ::= */

<almost anything>   ::= {any string of valid characters which does not contain a <closing bracket>}

A $ within a comment specifies that the next <character> is a control character.
There are 256 control toggles corresponding to the 256 valid character codes.  
Each toggle can have a value true or false.  
When $<character> is encountered, the value of the corresponding toggle is complemented.  
Currently defined toggles are:
toggle	Description	initial
B	Interlist emitted byte codes in hex.	off
D	Print statistics and symbol table.	on
E	Interlist emitted assembly code.	off
L	List compiled program.	on
M	List program without auxiliary information.	off
N	Warn if procedure args¬=parameters.	off
S	Dump symbol table at end of each procedure.	off
T	Begin tracing.	 
U	Terminate tracing.	 
Z	Allow program to execute in spite of errors.	off
|	Set margin. Input scan will be terminated at column containing the |	 

</pre>

