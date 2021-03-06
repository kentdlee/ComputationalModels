=========================================
Pushdown Automata
=========================================

In this chapter we cover parsing as well as pushdown automata. One of the topics Linz does not cover is on the construction of a pushdown automaton from a context-free grammar. To begin, we must build the automaton which is called an LR0 machine for a grammar :math:`G = (N,T,P,S)`.

Nondeterminstic Pushdown Automata
===================================
From a theoretical standpoint, nondeterministic pushdown automata are capable of recognizing context-free languages. However, from a practical standpoint they are of little interest. Nevertheless, the implementation of a nondeterministic pushdown automaton helps to illustrate how search can be implemented both recursively and iteratively. The code listing below contains two versions of the **accepts** method, the first (commented out) is the recursive version with an acceptsSuffix nested function much like the nondeterministic NFA from chapter 2. The second version of **accepts** is an iterative version which maintains a stack of unexplored instantaneous descriptions.

.. container:: figboxcenter

	.. code-block:: python
		:linenos:

		import streamreader
		import io
		import copy
		import stack


		epsilon = ""

		class NPDA:
		    def __init__(self, delta, startStateId, stackStartSym, finalStates):
		        self.delta = delta
		        self.startStateId = startStateId
		        self.stackStartSym = stackStartSym
		        self.finalStates = finalStates

		    # This is a working recursive version of accepts
		    #def accepts(self,strm):

		        #def matchTop(aStackTop, stack):
		            #return stack[-1] == aStackTop

		        #def getTransitions(stateId, stack):
		            #transitionList = []

		            #for aStateId, anInputSym, aStackTop in self.delta.keys():
		                #if stateId == aStateId and matchTop(aStackTop, stack):
		                    #transitionList.append((anInputSym, aStackTop))

		            #return transitionList

		        #def popPush(aStackTop, pushOnStack, stack):
		            #newstack = stack[:]
		            #for x in aStackTop:
		                #newstack.pop()
		            #for x in pushOnStack[::-1]:
		                #newstack.append(x)
		            #return newstack


		        #def acceptsSuffix(stateId, stack):

		            #c = strm.readChar()
		            ##print(stateId, c, stack)

		            #if strm.eof() and stateId in self.finalStates:
		                ##print(stateId)
		                #return True

		            #strm.unreadChar(c)

		            #for anInputSym, aStackTop in getTransitions(stateId, stack):
		                #for toStateId, pushOnStack in self.delta[(stateId, anInputSym, aStackTop)]:
		                    #if anInputSym == epsilon and acceptsSuffix(toStateId,popPush(aStackTop,pushOnStack,stack)):
		                            #return True
		                    #else: # not an epsilon transition
		                        #c = strm.readChar()
		                        #if c == anInputSym and acceptsSuffix(toStateId,popPush(aStackTop,pushOnStack,stack)):
		                            #return True
		                        #strm.unreadChar(c)

		            #return False

		        #return acceptsSuffix(self.startStateId, [self.stackStartSym])

		    # This is an iterative version of accepts that works. It maintains
		    # a stack of instantaneous descriptions that have yet to be explored.
		    def accepts(self,strm):

		        def matchTop(aStackTop, stack):
		            return stack[-1] == aStackTop

		        def getTransitions(stateId, stack):
		            transitionList = []

		            for aStateId, anInputSym, aStackTop in self.delta.keys():
		                if stateId == aStateId and matchTop(aStackTop, stack):
		                    transitionList.append((anInputSym, aStackTop))

		            return transitionList

		        def popPush(aStackTop, pushOnStack, stack):
		            newstack = stack[:]
		            for x in aStackTop:
		                newstack.pop()
		            for x in pushOnStack[::-1]:
		                newstack.append(x)
		            return newstack


		        stateId = self.startStateId
		        pdaStack = [self.stackStartSym]

		        # This is the instantaneous description stack. It starts with the start state
		        # instantaneous description.
		        ID = stack.Stack()
		        ID.push((stateId,strm,pdaStack))

		        while not ID.isEmpty():
		            stateId, strm, pdaStack = ID.pop()
		            print((stateId, strm, pdaStack))
		            c = strm.readChar()

		            if strm.eof() and stateId in self.finalStates:
		                return True

		            strm.unreadChar(c)

		            for anInputSym, aStackTop in getTransitions(stateId, pdaStack):
		                for toStateId, pushOnStack in self.delta[(stateId, anInputSym, aStackTop)]:
		                    if anInputSym == epsilon:
		                        ID.push((toStateId,copy.deepcopy(strm),popPush(aStackTop,pushOnStack,pdaStack)))
		                    else: # not an epsilon transition
		                        c = strm.readChar()
		                        if c == anInputSym:
		                            ID.push((toStateId,copy.deepcopy(strm),popPush(aStackTop,pushOnStack,pdaStack)))
		                        strm.unreadChar(c)

		        return False


		def main():

		    delta = {}

		    # delta[(statedId, inputsym, stacktop)] = (newstateid, stacktop')
		    #This is a^n b^n
		    #delta[(0, "a", "0")] = set([(1,"10")])
		    #delta[(0, epsilon, "0")] = set([(3, "")])
		    #delta[(1,"a","1")] = set([(1,"11")])
		    #delta[(1,"b","1")] = set([(2,"")])
		    #delta[(2,"b","1")] = set([(2,"")])
		    #delta[(2,epsilon,"0")] = set([(3,"")])
		    #npda = NPDA(delta,0,"0",set([3]))

		    #This is ww^R the a word with its reverse appended. Language of a's and b's.
		    delta[(0,"a","a")] = set([(0,"aa")])
		    delta[(0,"b","a")] = set([(0,"ba")])
		    delta[(0,"a","b")] = set([(0,"ab")])
		    delta[(0,"b","b")] = set([(0,"bb")])
		    delta[(0,"a","z")] = set([(0,"az")])
		    delta[(0,"b","z")] = set([(0,"bz")])

		    delta[(0,epsilon,"a")] = set([(1,"a")])
		    delta[(0,epsilon,"b")] = set([(1,"b")])

		    delta[(1,"a","a")] = set([(1,"")])
		    delta[(1,"b","b")] = set([(1,"")])

		    delta[(1,epsilon,"z")] = set([(2,"z")])

		    npda = NPDA(delta,0,"z",set([2]))

		    x = input("Please enter a string of a's and b's: ")

		    if npda.accepts(streamreader.StreamReader(io.StringIO(x))):
		        print("Yes, in the language.")
		    else:
		        print("Nope! Not in the language.")

		if __name__ == "__main__":
		    main()


LL(1) Grammars
===============

LL(1) grammars are grammars that can be used to determinitically build a left-most derivation of a word, w, by looking ahead at the next symbol of w. Consider the grammar of prefix expressions.

	:math:`E \rightarrow + E E \mid - E E \mid * E E \mid / E E \mid integer`

To construct a parser for such a language, we can build a series of functions, one for each nonterminal. Each nonterminal, in this case E, becomes a function in an LL(1) parser. The body of the function is given by the right hand side (rhs) of each of its productions. For instance, in this case we get a token and if it is a '+' we call the E function two more times.

LALR(1) Grammars
==================
An LALR(1) grammar is a grammar that is suitable for deterministic parsing using one symbol of lookahead to deterministically make choices with the pushdown automaton. An example of an LALR(1) grammar is the list expression grammar.

	* :math:`LE \rightarrow LE @ LT`
	* :math:`LE \rightarrow LT`
	* :math:`LT \rightarrow int :: LT`
	* :math:`LT \rightarrow Lit`
	* :math:`Lit \rightarrow [ ListSeq ]`
	* :math:`Lit \rightarrow nil`
	* :math:`ListSeq \rightarrow int , ListSeq`
	* :math:`ListSeq \rightarrow int`


Building the LR0 Machine
==========================

To build the LR0 machine we consider the productions of a context-free grammar and build what are called *item sets*. An item is a production with a dot (i.e. period) placed somewhere on the right hand side (rhs) of the production.

We start by first augmenting the grammar to include one more production of :math:`GOAL \rightarrow S~eof` where eof stands for end of file. Sometimes the pound sign (i.e. #) is used to represent eof in the literature on this construction. Then we start the construction of LR0 machine with the item :math:`GOAL \rightarrow \bullet S~eof`. For each new item we call the :math:`Completion` function on it to add any other items that belong to the same state of the LR0 machine. If :math:`Completion(itemSet)` results in a new LR0 State, then the state is added to the LR0 Machine and added to the stack of unexplored states. The first unexplored state is

	:math:`Completion(GOAL \rightarrow \bullet StartSymbol~eof`

The Completion Function
------------------------

The Completion or closure function adds items to an item set to *complete* an item set. Completion is given an item set and completes it by examining each item of the form :math:`A \rightarrow \alpha \bullet X \beta`. To this item set, each item of the form :math:`X \rightarrow \bullet \gamma` is added, :math:`\alpha, \beta, \gamma \in \{T \cup N\}^*`.

The Successor Function
-----------------------

For the completed item set in an LR0 state, the Successor function builds a set of next LR0 states. Assume :math:`X \in \{T \cup N\}` in an item :math:`A \rightarrow \alpha \bullet X \beta` associated with some LR0 state :math:`U`. Then there is a transition from :math:`U` to a state :math:`V` containing the item :math:`A \rightarrow \alpha  X \bullet \beta`. Again, :math:`\alpha, \beta \in \{T \cup N\}^*`.

The algorithm for building the LR0 machine proceeds by finding all the successors of an LR0 State. If any of these successors are new states, they are added to the stack of unexplored states. This stack is popped repeatedly finding successors for the unexplored states until the stack is empty.

.. container:: figboxcenter

	.. _lr0machine:

	.. figure:: listexp.png

		The List Expression LR(0) Machine with Lookahead Sets

The LR0State Class and its Components
======================================

The code for LR0 States includes an LR0State class. Each LR0State is composed of one or more LR0Items. Each LR0Item is a Production with a dot on the right hand side. The classes for all three: LR0State, LR0Item, and Production are all included in the code here.

 .. container:: figboxcenter

	.. code-block:: python
		:linenos:

		epsilon = "EPSILON"
		NoTransition = -1

		class LR0State:
		    def __init__(self, id, items = frozenset(), transitions = {}, accepting = False):
		        # An id of -1 is not allowed since that indicates there is
		        # no transition on a value.
		        if id == -1:
		            raise Exception("A state id of -1 is not allowed.")

		        self.id = id
		        # A copy of the list of transitions needs to be created
		        # because Python makes only one instance of a default
		        # parameter. Changing the list later, by adding a transition
		        # would add that transition to every state. Wow! Talk about
		        # bad semantics...
		        self.transitions = dict(transitions)
		        self.predecessors = dict()
		        self.items = items
		        self.accepting = accepting
		        self.tnts = None

		    def __hash__(self):
		        val = 0
		        for item in self.items:
		            val = (val + hash(item)) % 999999999

		        return val

		    def __eq__(self,other):
		        if len(self.items) != len(other.items):
		            return False

		        for item in self.items:
		            if not item in other.items:
		                return False

		        return True

		    def isAccepting(self):
		        return self.accepting

		    def setClasses(self, classes):
		        self.classes = classes

		    def addTransition(self, onClass, aState):
		        self.transitions[onClass]=aState.id
		        if onClass in aState.predecessors:
		            aState.predecessors[onClass].add(self.id)
		        else:
		            aState.predecessors[onClass] = set([self.id])

		    def hasTransition(self, onClass):
		        return onClass in self.transitions

		    def onClassGoTo(self, onClass):
		        if onClass in self.transitions:
		            return self.transitions[onClass]

		        return NoTransition

		    def pred(self, onClass):
		        if onClass in self.predecessors:
		            return self.predecessors[onClass]

		        return -1

		    def getTransitions(self):
		        return self.transitions

		    def getId(self):
		        return self.id

		    #def productions(self):
		    #    prodSet = set()
		    #    for item in self.items:
		    #        prodSet.add(item.production.id)
		    #
		    #    return prodSet

		    def __repr__(self):
		        return "LR0State(" + repr(self.id) + "," + repr(self.items) + "," + \
		            repr(self.transitions) + "," + repr(self.accepting) + ")"

		    def __str__(self):
		        val = ""
		        val = "LR0State " + str(self.id)
		        if self.accepting:
		            val += " is accepting\n"
		        else:
		            val += "\n"

		        for on in self.transitions:
		            toStateId = self.transitions[on]
		            val += "    On " + str(self.tnts[on]) + " Go To " + str(toStateId) + "\n"

		        for item in self.items:
		            val += "    Item: " + str(item) + "\n"

		        return val

		class Production:
		    def __init__(self, id, lhsId = None, rhs = [], returnValue = ""):
		        self.id = id
		        self.rhs = list(rhs)
		        self.lhsId = lhsId
		        self.returnValue = returnValue
		        self.tnts = None


		    def addRHSItem(self, tntId):
		        self.rhs.append(tntId)

		    def __str__(self):
		        if self.tnts != None:
		            s = self.tnts[self.lhsId] + " --> "
		            for k in range(len(self.rhs)):
		                s += self.tnts[self.rhs[k]] + " "

		            return s

		        return repr(self)

		    def __repr__(self):
		        return "Production(" + repr(self.id) + "," + repr(self.lhsId) + "," + repr(self.rhs) + "," + repr(self.returnValue) + ")"

		class LR0Item:
		    def __init__(self,id,production=None,dotIndex=0,la=frozenset()):
		        # make a copy of the set and list below
		        # because sets are mutable and
		        # default arguments are only created once.
		        self.id = id
		        self.production = production
		        self.dotIndex = dotIndex
		        self.tnts = None
		        self.la = set(la)

		    def __eq__(self,other):
		        if type(self)!=type(other):
		            raise Exception("Comparison of LR0Item " + str(self) + " with " + str(other))

		        return self.dotIndex == other.dotIndex and self.production.id == other.production.id

		    def __hash__(self):
		        # the 100000 is really just a random number, but it
		        # does allow for a unique hash code up to 100,000 elements
		        # (i.e. terminal and nonterminals) on the right hand
		        # side of any production.
		        return self.production.id * 100000 + self.dotIndex

		    def __str__(self):
		        if self.tnts != None:
		            s = self.tnts[self.production.lhsId] + " ::= "
		            if len(self.production.rhs) == 0:
		                s += "(*)"
		            else:
		                for k in range(len(self.production.rhs)):
		                    if k == self.dotIndex:
		                        s += "(*) "

		                    s += self.tnts[self.production.rhs[k]] + " "

		                if self.dotIndex == len(self.production.rhs):
		                    s += "(*)"

		            if len(self.la) != 0:
		                s+= "\n        Lookaheads: "
		                first = True
		                for termId in self.la:
		                    if first:
		                        first = False
		                    else:
		                        s+=", "
		                    s+= self.tnts[termId]
		            s+="\n"
		            return s

		        return repr(self)

		    def __repr__(self):
		        return "LR0Item(" + repr(self.id) + "," + repr(self.production) + "," + repr(self.dotIndex) + "," + repr(self.la) + ")"


Computing Lookahead Sets
==========================
Lookahead sets can be computed using the algorithm presented in a paper titled "Methods for Computing LALR(k) Lookahead" by Bent Bruun Kristensen and Ole Lehrmann Madsen of Aarhus University, ACM Transactions on Programming Languages and Systems, Vol. 3, No. 1, January 1981, Pages 60-82. This paper can be found in the `ACM Digital Library <http://dl.acm.org/citation.cfm?id=357126>`_ and a copy is available for my students.

Running the LALR(1) Machine
=============================
A bottom-up parser for an LALR(1) language is a deterministic pushdown automaton. The parser constructs a right-most derivation of a sentence in reverse order. Consider the list expression 3::[1,2]. A right-most derivation for this sentence is given below (using the augmented grammar).

	:math:`Goal \rightarrow LE~eof \rightarrow LT~eof \rightarrow 3 :: LT~eof \rightarrow 3 :: Lit~eof`
	:math:`\rightarrow 3 :: [ ListSeq ] ~eof\rightarrow 3 :: [ 1 , ListSeq ] ~eof \rightarrow 3 :: [ 1 , 2 ] ~eof \Box`

You can think of their being a dot that moves from left to right through the sentence. All items to the left of the dot are shifted onto the PDA stack. The items to the right of the dot must be read yet. The dot starts at the left hand side of the sentence and ends at the right hand side of the sentence. The dot corresponds to the dot in the items of the LR(0) machine.

	:math:`\bullet 3 :: [ 1 , 2 ] ~eof`

The machine proceeds by starting in state 0 and following transitions when it can to shift items onto the stack. *Shift* is one of the two operations of the parser. When no *shift* is possible in a state, given the next input symbol, then a *reduce* operation occurs if the next input symbol is in a lookahead set. The reduce operation is described below.

The machine starts by pushing the state identifier and the Goal identifier onto the stack as a tuple. Then it proceeds as follows looping until the accepting state is found.

	1. Peek at the top of the stack to find the (stateId, RHS symbol) pair.
	2. If a token is needed then get a token from the scanner.
	3. Examine the token and the current state to determine if there is a transition on the token to a new state. If so, then push the (new stateid, token) pair onto the stack. This is called a shift operation.
	4. If there is no transition from the given state on the given token, then look for an item with a lookahead set containing the item. If an LR(0) item is found whose lookahead set contains the token, then *reduce* by popping the length of the right hand side of the item from the stack. Then peek at the top of the stack to see what state is now one top. Do a transition from that state on the LHS of the item by which you are reducing. Push that (newStateId, LHS) onto the stack.

A parser constructed in such a way can answer *yes* or *no* as to the membership of a sentence in the language of a grammar. More interestingly, an abstract syntax tree can be built by evaluating a return value for each reduction that occurs in the parsing of a sentence. The code below provides an outline for the parser code along with a method called {\em buildReturnValue} that builds value for the return value of a parser.

 .. container:: figboxcenter

	.. code-block:: python
		:linenos:

		from lr0state import *
		import stack
		import streamreader
		import io
		import sys

		class Parser:
		    def __init__(self,states,tnts):
		        self.states = states
		        self.tnts = tnts
		        for stateId in states:
		            theState = states[stateId]
		            theState.tnts = tnts
		            for item in theState.items:
		                item.tnts = tnts


		    def buildReturnValue(self,item,prodStack):
		        # We found an item to reduce by.
		        prodNames = {} # this is a map from return value names to locations with rhs
		        tntCount = {}
		        rhsVals = {} # this is a map from index location of rhs to value from the stack.

		        # this loop builds a map from names of rhs terminals or nonterminals to
		        # their index value for the rhs
		        for i in range(len(item.production.rhs)):
		            tntId = item.production.rhs[i]
		            tnt = self.tnts[tntId]
		            if not tnt in tntCount:
		                tntCount[tnt] = 1
		                prodNames[tnt] = i
		                prodNames[tnt+"1"] = i
		            else:
		                numVal = tntCount[tnt]+1
		                tntCount[tnt] = numVal
		                prodNames[tnt+str(numVal)]=i


		        # this loop builds a map from index value of rhs location to
		        # the actual value popped from the pda stack.
		        for i in range(len(item.production.rhs)-1,-1,-1):
		            stateId, val = prodStack.pop()
		            rhsVals[i] = val

		        returnValue = ""
		        rvStrm  = streamreader.StreamReader(io.StringIO(item.production.returnValue))

		        # Here we iterate through the parts of the return value replacing any
		        # non-terminal token with the actual value popped from the stack
		        # used in parsing. This builds the actual return value for the expression
		        # being parsed.

		        token = rvStrm.getToken()

		        while not rvStrm.eof():
		            if token in prodNames:
		                returnValue += rhsVals[prodNames[token]]
		            else:
		                returnValue += token

		            token = rvStrm.getToken()

		        # Here we call the overridden eval method to evaluate
		        # the return value string in the context of the parser's
		        # back end module. This is because each parser instance
		        # inherits from this class to define its own parser
		        # and backend code.

		        val = repr(self.eval(returnValue))
		        return val

		    # This is called in case of an error because no shift or reduce was possible.
		    def error(self,stateId,tokenId,prodStack):
		        sys.stderr.write("No Transition Found\n")
		        sys.stderr.write("\nState is " + str(stateId) + "\n\n")
		        sys.stderr.write("Stack Contents\n")
		        sys.stderr.write("==============\n\n")
		        sys.stderr.write(str(prodStack)+"\n\n\n")
		        sys.stderr.write("Next Input Symbol is "+lex+" with tokenId of "+str(tokenId)+"\n\n")
		        raise Exception("No transition on state!")


		    # This algorithm comes from page 218, Algorithm 4.7 from Aho,
		    # Sethi, and Ullman.
		    # The modification from this algorithm has the stack a stack of
		    # tuples of (stateId, val) where val is the return value
		    # for a terminal or nonterminal.
		    def parse(self, theScanner):

		        # create a stack called prodStack.

		        # push the start state and return value. The initial return value can be None.

		        while True:
		            # Peek at the top of the stack to get the stateId and return value.

		            # Get a token if needed.

		            # Do a shift operation if there is a transition on the tokenId. If we also had the same
		            # tokenId in a lookahead set we would have a shift/reduce conflict, but we'll always shift
		            # anyway in that case so don't worry about detecting shift/reduce conflicts.

		            # If no shift operation is possible then look for a reduce possibility.
		            # we look in the items of the state for an item that would
		            # work to reduce by. If more than one item is found then we have a
		            # reduce/reduce conflict.

		            # If an item to reduce by is found then call buildReturnValue which will build the return
		            # value (e.g. usually an AST) for the reduction.
		            # NOTE: buildReturnValue pops off the correct number of values from the stack, but does not
		            # push on the new state found by following the transition on the LHS of the chosen item.

		            # if no item was found for a shift or reduce operation, then tere is a problem so raise an exception
		            # by calling the error function.
		            pass

Exercises
==========

1. Using the prefix grammar presented in this chapter, build a parser for prefix expressions that returns an abstract syntax tree of an expression. Then, traverse the abstract syntax tree to print the postfix form of that expression.

   To complete this program you must build an LL(1) parser along with classes for the abstract syntax tree nodes of expressions. Write an eval method for each node in your abstract syntax tree that when called builds a string of the postfix expression. So your program should use a StreamReader over the string entered by the user to get the tokens of the string. The parser is called to parse the entered input and the parser should return an abstract syntax tree of the expression. Then eval is called on your abstract syntax tree to build the postfix expression which is then printed.
2. For the following grammar, generate the LR0 machine by generating each item set for each state of the machine. Then identify the transitions of the machine in table form where rows are the state identifiers and columns are the various :math:`\{T \cup N\}` vocabulary symbols. Be sure to augment the grammar first!

	* :math:`E \rightarrow E + T`
	* :math:`E \rightarrow E - T`
	* :math:`E \rightarrow T`
	* :math:`T \rightarrow integer`
	* :math:`T \rightarrow ( E )`

3. Complete the `calculator project which can be downloaded here <../../../examplefiles/calc.tar.gz>`_. This project is an infix calculator with one memory location. The primary purpose of this project is to implement the scanner and the parser (i.e. pushdown automaton). The project gives you the finite state machine and the LALR(1) machine. Your job is only to write the getToken method of the scanner and the parse method of the parser. You can do this incrementally, writing the getToken method first, if you change the calculator.py main function to create the scanner and then construct a while loop to get all the tokens from the input until you encounter the EOF (endoffile) token.

The finite state machine of the scanner and the LR0 Machine with lookaheads is provided below for your reference but is already provided in the project download as coded in both calcscanner.py and calcparser.py. Interacting with the calculator works like this.

 .. container:: figboxcenter

	.. code-block:: text
		:linenos:

		Kent's Mac> calculator
		Enter expressions with +,-,*,/,S (for store), and R (for recall) and terminated by a semicolon (i.e. ;).
		S 5 * R;
		25.0
		4*(5+3);
		32.0
		39/2;
		19.5
		24 + 3*2;
		30.0
		S5 + 3*R;
		20.0

Here is the Finite State Machine of the Scanner and the LR0 Machine with Lookaheads of the parser.

 .. container:: figboxcenter

	.. code-block:: python
		:linenos:

		The MINIMAL DFA CREATED FOR THE REGULAR EXPRESSIONS IS:

		The start state is: 0

		STATE     ON CLASS     GO TO     ACCEPTS
		-----     --------     -----     -------
		    0
		                 S        10
		                 /         6
		                 ;         7
		                 *         3
		                 (         1
		                 -         5
		                 R         9
		                 +         4
		             digit        11
		               EOF         8
		                 )         2

		    1                                  (

		    2                                  )

		    3                                  *

		    4                                  +

		    5                                  -

		    6                                  /

		    7                                  ;

		    8                          endoffile

		    9                                  R

		   10                                  S

		   11                             number
		            period        12
		             digit        11

		   12
		             digit        13

		   13                             number
		             digit        13

		Scangen Completed.
		Running Parsegen
		Warning: There is a shift/reduce conflict in state 11 on terminal '*'
		         Conflict will be resolved by shifting.
		Warning: There is a shift/reduce conflict in state 11 on terminal '/'
		         Conflict will be resolved by shifting.
		Warning: There is a shift/reduce conflict in state 18 on terminal '*'
		         Conflict will be resolved by shifting.
		Warning: There is a shift/reduce conflict in state 18 on terminal '/'
		         Conflict will be resolved by shifting.
		Warning: There is a shift/reduce conflict in state 19 on terminal '*'
		         Conflict will be resolved by shifting.
		Warning: There is a shift/reduce conflict in state 19 on terminal '/'
		         Conflict will be resolved by shifting.
		LR0State 0
		    On Prog Go To 1
		    Item: Start ::= (*) Prog endoffile

		    Item: Prog ::= (*) Prog Stmt ';'

		    Item: Prog ::= (*)
		        Lookaheads: 'R', '(', endoffile, number, 'S'


		LR0State 1
		    On St Go To 2
		    On F Go To 3
		    On '(' Go To 4
		    On number Go To 5
		    On 'S' Go To 6
		    On 'R' Go To 7
		    On endoffile Go To 8
		    On Stmt Go To 9
		    On E Go To 10
		    On T Go To 11
		    Item: Stmt ::= (*) E

		    Item: Start ::= Prog (*) endoffile

		    Item: Prog ::= Prog (*) Stmt ';'

		    Item: E ::= (*) E '+' T

		    Item: E ::= (*) E '-' T

		    Item: E ::= (*) T

		    Item: T ::= (*) T '*' St

		    Item: T ::= (*) T '/' St

		    Item: T ::= (*) St

		    Item: St ::= (*) 'S' F

		    Item: St ::= (*) F

		    Item: F ::= (*) 'R'

		    Item: F ::= (*) '(' E ')'

		    Item: F ::= (*) number


		LR0State 2
		    Item: T ::= St (*)
		        Lookaheads: ')', '+', '-', '*', '/', ';'


		LR0State 3
		    Item: St ::= F (*)
		        Lookaheads: ')', '+', '-', '*', '/', ';'


		LR0State 4
		    On St Go To 2
		    On F Go To 3
		    On '(' Go To 4
		    On number Go To 5
		    On 'S' Go To 6
		    On 'R' Go To 7
		    On E Go To 22
		    On T Go To 11
		    Item: E ::= (*) E '+' T

		    Item: F ::= '(' (*) E ')'

		    Item: E ::= (*) E '-' T

		    Item: E ::= (*) T

		    Item: T ::= (*) T '*' St

		    Item: T ::= (*) T '/' St

		    Item: T ::= (*) St

		    Item: St ::= (*) 'S' F

		    Item: St ::= (*) F

		    Item: F ::= (*) number

		    Item: F ::= (*) 'R'

		    Item: F ::= (*) '(' E ')'


		LR0State 5
		    Item: F ::= number (*)
		        Lookaheads: ')', '+', '-', '*', '/', ';'


		LR0State 6
		    On number Go To 5
		    On F Go To 21
		    On 'R' Go To 7
		    On '(' Go To 4
		    Item: F ::= (*) number

		    Item: St ::= 'S' (*) F

		    Item: F ::= (*) '(' E ')'

		    Item: F ::= (*) 'R'


		LR0State 7
		    Item: F ::= 'R' (*)
		        Lookaheads: ')', '+', '-', '*', '/', ';'


		LR0State 8 is accepting
		    Item: Start ::= Prog endoffile (*)


		LR0State 9
		    On ';' Go To 20
		    Item: Prog ::= Prog Stmt (*) ';'


		LR0State 10
		    On '+' Go To 16
		    On '-' Go To 17
		    Item: Stmt ::= E (*)
		        Lookaheads: ';'

		    Item: E ::= E (*) '+' T

		    Item: E ::= E (*) '-' T


		LR0State 11
		    On '*' Go To 12
		    On '/' Go To 13
		    Item: E ::= T (*)
		        Lookaheads: ')', '+', '-', '*', '/', ';'

		    Item: T ::= T (*) '*' St

		    Item: T ::= T (*) '/' St


		LR0State 12
		    On St Go To 15
		    On F Go To 3
		    On number Go To 5
		    On '(' Go To 4
		    On 'R' Go To 7
		    On 'S' Go To 6
		    Item: St ::= (*) 'S' F

		    Item: St ::= (*) F

		    Item: T ::= T '*' (*) St

		    Item: F ::= (*) number

		    Item: F ::= (*) '(' E ')'

		    Item: F ::= (*) 'R'


		LR0State 13
		    On St Go To 14
		    On F Go To 3
		    On number Go To 5
		    On '(' Go To 4
		    On 'R' Go To 7
		    On 'S' Go To 6
		    Item: St ::= (*) 'S' F

		    Item: St ::= (*) F

		    Item: T ::= T '/' (*) St

		    Item: F ::= (*) number

		    Item: F ::= (*) '(' E ')'

		    Item: F ::= (*) 'R'


		LR0State 14
		    Item: T ::= T '/' St (*)
		        Lookaheads: ')', '+', '-', '*', '/', ';'


		LR0State 15
		    Item: T ::= T '*' St (*)
		        Lookaheads: ')', '+', '-', '*', '/', ';'


		LR0State 16
		    On St Go To 2
		    On F Go To 3
		    On '(' Go To 4
		    On number Go To 5
		    On 'S' Go To 6
		    On 'R' Go To 7
		    On T Go To 19
		    Item: T ::= (*) T '*' St

		    Item: T ::= (*) T '/' St

		    Item: E ::= E '+' (*) T

		    Item: T ::= (*) St

		    Item: St ::= (*) 'S' F

		    Item: St ::= (*) F

		    Item: F ::= (*) number

		    Item: F ::= (*) '(' E ')'

		    Item: F ::= (*) 'R'


		LR0State 17
		    On St Go To 2
		    On F Go To 3
		    On '(' Go To 4
		    On number Go To 5
		    On 'S' Go To 6
		    On 'R' Go To 7
		    On T Go To 18
		    Item: T ::= (*) T '*' St

		    Item: T ::= (*) T '/' St

		    Item: E ::= E '-' (*) T

		    Item: T ::= (*) St

		    Item: St ::= (*) 'S' F

		    Item: St ::= (*) F

		    Item: F ::= (*) number

		    Item: F ::= (*) '(' E ')'

		    Item: F ::= (*) 'R'


		LR0State 18
		    On '*' Go To 12
		    On '/' Go To 13
		    Item: T ::= T (*) '*' St

		    Item: T ::= T (*) '/' St

		    Item: E ::= E '-' T (*)
		        Lookaheads: ')', '+', '-', '*', '/', ';'


		LR0State 19
		    On '*' Go To 12
		    On '/' Go To 13
		    Item: T ::= T (*) '*' St

		    Item: T ::= T (*) '/' St

		    Item: E ::= E '+' T (*)
		        Lookaheads: ')', '+', '-', '*', '/', ';'


		LR0State 20
		    Item: Prog ::= Prog Stmt ';' (*)
		        Lookaheads: 'R', '(', endoffile, number, 'S'


		LR0State 21
		    Item: St ::= 'S' F (*)
		        Lookaheads: ')', '+', '-', '*', '/', ';'


		LR0State 22
		    On ')' Go To 23
		    On '+' Go To 16
		    On '-' Go To 17
		    Item: E ::= E (*) '+' T

		    Item: F ::= '(' E (*) ')'

		    Item: E ::= E (*) '-' T


		LR0State 23
		    Item: F ::= '(' E ')' (*)
		        Lookaheads: ')', '+', '-', '*', '/', ';'
