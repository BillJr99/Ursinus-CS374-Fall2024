---
layout: activity
permalink: /Activities/ParserInterpreter4
title: "CS374: Programming Language Principles - Parsers and Interpreters"
excerpt: "CS374: Programming Language Principles - Parsers and Interpreters"

info: 
  prev: ./ParserInterpreter3
  goals: 
    - To describe the components of an LR(k) parser
    - To differentiate an LR(k) parser from an LL(1) parser
    - To generate a parse table for a given grammar
  models:
    - model: |
        <div align="left">
        Begin with a grammar definition:
        <script type="syntaxhighlighter" class="brush: cpp"><![CDATA[
         /* 
            S -> Xyx
            X -> xX | y
         */
        ]]></script>   
        <br>
        Augment with a clean starting state:
        <br>
        <script type="syntaxhighlighter" class="brush: cpp"><![CDATA[
         /* 
            S' -> S
            S -> Xyx
            X -> xX | y
         */
        ]]></script>
        <br>
        State <code>0</code> is the <code>S' -> .S</code> initial state plus the closure, <code>.</code> indicates the <code>S</code> hasn't been read yet.  Closure includes whatever can be produced by the production (plus any productions that can be executed from those - as indicated by a dot to the left of the nonterminal).
        <br>
        <script type="syntaxhighlighter" class="brush: cpp"><![CDATA[
         /* 
            State 0:
                S' -> .S
                S -> .Xyx
                X -> .xX | .y
         */
        ]]></script>
        <br>
        Now, transition from State 0 on each of the possible productions in the closure:
        <ul>
        <li>State 0 has 4 outputs upon reading S, X, x, and y.  State 1: State 0 -> State 1 on reading S<br>
        Move the dot to the right of the production that results from reading this transition, and perform the closure on productions that follow the dot (here, there are none).  This state contains <code>S' -> S.</code></li>
        <li>State 2: State 0 -> State 2 on reading X<br><code>S -> X.yx</code></li>
        <li>State 3: State 0 -> State 3 on reading x<br><code>X -> x.X; X -> .xX | .y</code></li>
        <li>State 4: State 0 -> State 4 on reading y<br><code>X -> y.</code></li>
        <li>States 2 and 3 have additional outputs, so we proceed until the dot is all the way to the right.  State 5: State 2 -> State 5 on reading y<br><code>S -> Xy.x</code></li>
        <li>State 6: State 3 -> State 6 on reading X<br><code>X -> xX.</code></li>
        <li>State 3 -> State 3 on reading x.  Notice the productions in the closure are the same as in state 3.<br><code>X -> x.X; X -> .xX | .y</code></li>
        <li>Similarly, State 3 -> State 4 on reading y</li>
        <li>Now, state 5 can continue - State 7: State 5 -> State 7 on reading x<br><code>S -> Xyx.</code></li>
        </ul>
        <br>
        Now the parse table: one state row per state above (0 through 7)
        ACTION is the set of terminals plus <code>$</code> meaning the end of the string, and GOTO is the set of nonterminals besides the augmented first state <code>S' -> S</code>

        Fill in what state you go to if you read that terminal or nonterminal from that state.  For example, state 0 goes to 1 on S, and it goes to 2 on X, per the rules at the top.  Go to state 3 on x, and state 4 on y, and we call these shifts because the dot is still to the left of the end of the production, indicating a shift, as opposed to a reduce after the dot reaches the end of the production.  The augmented state (state 1) accepts on end of string.

        The remaining states (4, 6, 7) have dots on the right.  These are our reduce states.  States 4 and 6 produce X, while state 7 produces S.  State 4 is <code>X -> y.</code>, State 6 is <code>X -> xX.</code>, and State 7 is <code>S -> Xyx.</code>.  We reduce these state rows to their corresponding production.  X was initiated by state 2, due to its X. in the production, so we reduce on all inputs to r2 in state 4 and state 6.  State 1 contains S. via <code>S' -> S.</code>, so this is an r1 from state 7.
        <br>
        <style type="text/css">
        .tg  {border-collapse:collapse;border-spacing:0;}
        .tg td{border-color:black;border-style:solid;border-width:1px;font-family:Arial, sans-serif;font-size:14px;
          overflow:hidden;padding:10px 5px;word-break:normal;}
        .tg th{border-color:black;border-style:solid;border-width:1px;font-family:Arial, sans-serif;font-size:14px;
          font-weight:normal;overflow:hidden;padding:10px 5px;word-break:normal;}
        .tg .tg-7btt{border-color:inherit;font-weight:bold;text-align:center;vertical-align:top}
        .tg .tg-fymr{border-color:inherit;font-weight:bold;text-align:left;vertical-align:top}
        .tg .tg-0pky{border-color:inherit;text-align:left;vertical-align:top}
        </style>
        <table class="tg">
        <thead>
          <tr>
            <th class="tg-7btt">STATE</th>
            <th class="tg-7btt">ACTION</th>
            <th class="tg-7btt">x</th>
            <th class="tg-fymr">y</th>
            <th class="tg-fymr">$</th>
            <th class="tg-fymr">GOTO</th>
            <th class="tg-fymr">S</th>
            <th class="tg-fymr">X</th>
          </tr>
        </thead>
        <tbody>
          <tr>
            <td class="tg-fymr">0</td>
            <td class="tg-0pky"></td>
            <td class="tg-0pky">s3</td>
            <td class="tg-0pky">s4</td>
            <td class="tg-0pky"></td>
            <td class="tg-0pky"></td>
            <td class="tg-0pky">1</td>
            <td class="tg-0pky">2</td>
          </tr>
          <tr>
            <td class="tg-fymr">1</td>
            <td class="tg-0pky"></td>
            <td class="tg-0pky"></td>
            <td class="tg-0pky"></td>
            <td class="tg-0pky">acc</td>
            <td class="tg-0pky"></td>
            <td class="tg-0pky"></td>
            <td class="tg-0pky"></td>
          </tr>
          <tr>
            <td class="tg-fymr">2</td>
            <td class="tg-0pky"></td>
            <td class="tg-0pky"></td>
            <td class="tg-0pky">s5</td>
            <td class="tg-0pky"></td>
            <td class="tg-0pky"></td>
            <td class="tg-0pky"></td>
            <td class="tg-0pky"></td>
          </tr>
          <tr>
            <td class="tg-fymr">3</td>
            <td class="tg-0pky"></td>
            <td class="tg-0pky">s3</td>
            <td class="tg-0pky">s4</td>
            <td class="tg-0pky"></td>
            <td class="tg-0pky"></td>
            <td class="tg-0pky"></td>
            <td class="tg-0pky">6</td>
          </tr>
          <tr>
            <td class="tg-fymr">4</td>
            <td class="tg-0pky"></td>
            <td class="tg-0pky">r2</td>
            <td class="tg-0pky">r2</td>
            <td class="tg-0pky">r2</td>
            <td class="tg-0pky"></td>
            <td class="tg-0pky"></td>
            <td class="tg-0pky"></td>
          </tr>
          <tr>
            <td class="tg-fymr">5</td>
            <td class="tg-0pky"></td>
            <td class="tg-0pky">s7</td>
            <td class="tg-0pky"></td>
            <td class="tg-0pky"></td>
            <td class="tg-0pky"></td>
            <td class="tg-0pky"></td>
            <td class="tg-0pky"></td>
          </tr>
          <tr>
            <td class="tg-fymr">6</td>
            <td class="tg-0pky"></td>
            <td class="tg-0pky">r2</td>
            <td class="tg-0pky">r2</td>
            <td class="tg-0pky">r2</td>
            <td class="tg-0pky"></td>
            <td class="tg-0pky"></td>
            <td class="tg-0pky"></td>
          </tr>
          <tr>
            <td class="tg-fymr">7</td>
            <td class="tg-0pky"></td>
            <td class="tg-0pky">r1</td>
            <td class="tg-0pky">r1</td>
            <td class="tg-0pky">r1</td>
            <td class="tg-0pky"></td>
            <td class="tg-0pky"></td>
            <td class="tg-0pky"></td>
          </tr>
        </tbody>
        </table>
        <br>
        This can be programmed using an if statement (for example: <code>if state == 0 and input == x, then state = 3</code>, etc.).  On reductions, you've read a whole statement.  By shifting the individual tokens, you also know what inputs were provided to those productions.  This is what tools like yacc do.
        </div>
      title: "Generating an LR(0) Parse Table"
      questions: 
        - "Draw the state machine (the states and transitions) implemented by this LR(0) parse table."
        - "Augment the grammar and generate an LR(0) parse table for the grammar <code>S -> XX; X -> xXy; X -> x; X -> y</code>"
        - "Notice that each state of the machine can have duplicate transitions.  The state machine of an LR parser allows states to represent multiple possible parser rules.  Why is this acceptable in an LR parser but required a revision to the grammar of an LL(1) parser, even though both can be represented with Deterministic Finite Automata?  Specifically, why must we revise the underlying LL(1) grammar even though we could simply convert the Nondeterministic Finite Automaton into a Deterministic Finite Automaton by combining states via the transitive closure, as we do with an LR parser and grammar?  (As a hint - it has something to do with the 1 in LL(1)!)"
    - model: |
        <a href="https://en.wikipedia.org/wiki/LR_parser#Bottom-up_parse_steps_for_example_A*2_+_1"><img alt="Wikipedia" src="https://upload.wikimedia.org/wikipedia/en/thumb/5/5f/Shift-Reduce_Parse_Steps_for_A%2A2%2B1.svg/512px-Shift-Reduce_Parse_Steps_for_A%2A2%2B1.svg.png"></a>
        <br>
        <!-- From https://en.wikipedia.org/wiki/LR_parser -->
        <style type="text/css">
        .tg  {border-collapse:collapse;border-spacing:0;}
        .tg td{border-color:black;border-style:solid;border-width:1px;font-family:Arial, sans-serif;font-size:14px;
          overflow:hidden;padding:10px 5px;word-break:normal;}
        .tg th{border-color:black;border-style:solid;border-width:1px;font-family:Arial, sans-serif;font-size:14px;
          font-weight:normal;overflow:hidden;padding:10px 5px;word-break:normal;}
        .tg .tg-c2ay{background-color:#EAECF0;border-color:inherit;color:#202122;font-style:italic;font-weight:bold;text-align:center;
          vertical-align:top}
        .tg .tg-x6qv{background-color:#EAECF0;border-color:inherit;color:#202122;font-weight:bold;text-align:center;vertical-align:top}
        .tg .tg-89jx{background-color:#F8F9FA;border-color:inherit;color:#202122;font-weight:bold;text-align:left;vertical-align:top}
        .tg .tg-ft3i{background-color:#F8F9FA;border-color:inherit;color:#202122;text-align:left;vertical-align:top}
        .tg .tg-0pky{border-color:inherit;text-align:left;vertical-align:top}
        </style>
        <table class="tg">
        <thead>
          <tr>
            <th class="tg-x6qv"><span style="background-color:#EAECF0">Curr</span></th>
            <th class="tg-x6qv"></th>
            <th class="tg-x6qv" colspan="5"><span style="background-color:#EAECF0">Lookahead</span></th>
            <th class="tg-x6qv"></th>
            <th class="tg-x6qv" colspan="3"><span style="background-color:#EAECF0">LHS Goto</span></th>
          </tr>
        </thead>
        <tbody>
          <tr>
            <td class="tg-x6qv"><span style="background-color:#EAECF0">State</span></td>
            <td class="tg-x6qv"><span style="background-color:#EAECF0">Current Rules</span></td>
            <td class="tg-c2ay">int</td>
            <td class="tg-c2ay">id</td>
            <td class="tg-x6qv"><span style="background-color:#EAECF0">*  </span></td>
            <td class="tg-x6qv"><span style="background-color:#EAECF0">+  </span></td>
            <td class="tg-c2ay">eof</td>
            <td class="tg-x6qv"></td>
            <td class="tg-x6qv"><span style="background-color:#EAECF0">Sums</span></td>
            <td class="tg-x6qv"><span style="background-color:#EAECF0">Products</span></td>
            <td class="tg-x6qv"><span style="background-color:#EAECF0">Value</span></td>
          </tr>
          <tr>
            <td class="tg-89jx">0</td>
            <td class="tg-ft3i">Goal → <span style="color:#F7F">•</span> Sums eof</td>
            <td class="tg-ft3i">8</td>
            <td class="tg-ft3i">9</td>
            <td class="tg-ft3i"></td>
            <td class="tg-ft3i"></td>
            <td class="tg-ft3i"></td>
            <td class="tg-ft3i"></td>
            <td class="tg-ft3i">1</td>
            <td class="tg-ft3i">4</td>
            <td class="tg-ft3i">7</td>
          </tr>
          <tr>
            <td class="tg-89jx">1</td>
            <td class="tg-ft3i">Goal → Sums <span style="color:#F7F">•</span> eof<br>Sums → Sums <span style="color:#F7F">•</span> + Products</td>
            <td class="tg-ft3i"></td>
            <td class="tg-ft3i"></td>
            <td class="tg-ft3i"></td>
            <td class="tg-ft3i">2</td>
            <td class="tg-ft3i">done<br> </td>
            <td class="tg-ft3i"></td>
            <td class="tg-ft3i"></td>
            <td class="tg-ft3i"></td>
            <td class="tg-ft3i"></td>
          </tr>
          <tr>
            <td class="tg-ft3i">2</td>
            <td class="tg-ft3i">Sums → Sums + <span style="color:#F7F">•</span> Products</td>
            <td class="tg-ft3i">8</td>
            <td class="tg-ft3i">9</td>
            <td class="tg-ft3i"></td>
            <td class="tg-ft3i"></td>
            <td class="tg-ft3i"></td>
            <td class="tg-ft3i"></td>
            <td class="tg-ft3i"></td>
            <td class="tg-ft3i">3</td>
            <td class="tg-ft3i">7</td>
          </tr>
          <tr>
            <td class="tg-ft3i">3</td>
            <td class="tg-ft3i">Sums → Sums + Products <span style="color:#F7F">•</span><br>Products → Products <span style="color:#F7F">•</span> * Value</td>
            <td class="tg-ft3i"></td>
            <td class="tg-ft3i"></td>
            <td class="tg-ft3i">5</td>
            <td class="tg-ft3i">r1<br> </td>
            <td class="tg-ft3i">r1<br> </td>
            <td class="tg-ft3i"></td>
            <td class="tg-ft3i"></td>
            <td class="tg-ft3i"></td>
            <td class="tg-ft3i"></td>
          </tr>
          <tr>
            <td class="tg-ft3i">4</td>
            <td class="tg-ft3i">Sums → Products <span style="color:#F7F">•</span><br>Products → Products <span style="color:#F7F">•</span> * Value</td>
            <td class="tg-ft3i"></td>
            <td class="tg-ft3i"></td>
            <td class="tg-ft3i">5</td>
            <td class="tg-ft3i">r2<br> </td>
            <td class="tg-ft3i">r2<br> </td>
            <td class="tg-ft3i"></td>
            <td class="tg-ft3i"></td>
            <td class="tg-ft3i"></td>
            <td class="tg-ft3i"></td>
          </tr>
          <tr>
            <td class="tg-ft3i">5</td>
            <td class="tg-ft3i">Products → Products * <span style="color:#F7F">•</span> Value</td>
            <td class="tg-ft3i">8</td>
            <td class="tg-ft3i">9</td>
            <td class="tg-ft3i"></td>
            <td class="tg-ft3i"></td>
            <td class="tg-ft3i"></td>
            <td class="tg-ft3i"></td>
            <td class="tg-ft3i"></td>
            <td class="tg-ft3i"></td>
            <td class="tg-ft3i">6</td>
          </tr>
          <tr>
            <td class="tg-ft3i">6</td>
            <td class="tg-ft3i">Products → Products * Value <span style="color:#F7F">•</span></td>
            <td class="tg-ft3i"></td>
            <td class="tg-ft3i"></td>
            <td class="tg-ft3i">r3</td>
            <td class="tg-ft3i">r3</td>
            <td class="tg-ft3i">r3</td>
            <td class="tg-ft3i"></td>
            <td class="tg-ft3i"></td>
            <td class="tg-ft3i"></td>
            <td class="tg-ft3i"></td>
          </tr>
          <tr>
            <td class="tg-ft3i">7</td>
            <td class="tg-ft3i">Products → Value <span style="color:#F7F">•</span></td>
            <td class="tg-ft3i"></td>
            <td class="tg-ft3i"></td>
            <td class="tg-ft3i">r4</td>
            <td class="tg-ft3i">r4</td>
            <td class="tg-ft3i">r4</td>
            <td class="tg-ft3i"></td>
            <td class="tg-ft3i"></td>
            <td class="tg-ft3i"></td>
            <td class="tg-ft3i"></td>
          </tr>
          <tr>
            <td class="tg-ft3i">8</td>
            <td class="tg-ft3i">Value → int <span style="color:#F7F">•</span></td>
            <td class="tg-ft3i"></td>
            <td class="tg-ft3i"></td>
            <td class="tg-ft3i">r5</td>
            <td class="tg-ft3i">r5</td>
            <td class="tg-ft3i">r5</td>
            <td class="tg-ft3i"></td>
            <td class="tg-ft3i"></td>
            <td class="tg-ft3i"></td>
            <td class="tg-ft3i"></td>
          </tr>
          <tr>
            <td class="tg-ft3i">9</td>
            <td class="tg-ft3i">Value → id <span style="color:#F7F">•</span></td>
            <td class="tg-ft3i"></td>
            <td class="tg-ft3i"></td>
            <td class="tg-ft3i">r6</td>
            <td class="tg-ft3i">r6</td>
            <td class="tg-ft3i">r6</td>
            <td class="tg-ft3i"></td>
            <td class="tg-ft3i"></td>
            <td class="tg-ft3i"></td>
            <td class="tg-0pky"></td>
          </tr>
        </tbody>
        </table>
      title: "LR(0) Parse Tables"
      questions:
        - "Click on the image to see the steps involved with parsing <code>A*2 + 1</code>."
        - "Describe these steps in terms of the LR(0) parse table."
        - "What does the L, R, and 0 refer to in LR(0)?"
      
tags:
  - parser
  - interpreter
  
---

