text - # <div align=center>		Executing Code in The Python Virtual Machine</div>
added - ***
added - ## <a name="top"></a> Table of Contents
added - ***
text - 
text - Levels of abstraction are important in computing. We can look at a computer
text - (and the software it runs) at many levels of abstraction: as quantum devices
text - implementing logical components, as collections of these components implementing
text - digital circuits (described by Boolean formulas), as digital circuits combined
text - into an instruction set architecture (which we'll cover in today's lecture)
text - as an architecture capable of running "translated" computer programs written in
text - a high-level language, as large applications written in high-level programming
text - languages, and as distributed/networked applications running on multiple
text - machines communicating over a network.
text - 
text - It is difficult to hold all these levels of abstraction in our minds while we
text - are exploring and creating computing systems/software. We often focus on just
text - one level of abstraction for our jobs, while knowing some -but less- about the
text - levels directly below and above. We have spent most of ICS 31-33 at the level
text - of discussing computing via the high-level programming language Python. In this
text - lecture we will look down one level, at the architecture into which Python
text - programs are translated and run on: the Python Virtual Machine.
text - 
text - Some programming languages (e.g., C++) are translated into instructions that
text - run directly on hardware. We speak of compiling programs in that language onto
text - a specific hardware architecture. Other programming languages (e.g., Java and
text - Python) are translated to run on a common architecture: interpreters (or
text - virtual machines) for that architecture can be written in a low-level language
text - (like C, which targets most architectures) or in the machine's hardware language
text - itself for maximal speed, so that exactly the same translated programs can run
text - on any architecture that has the virtual machine program implemented on it.
text - 
text - The compiling approach has the advantage of producing programs that execute
text - speedily on real machines. But it is a large undertaking to produce a good
text - compiler for a new architecture, and compiling a program can take a long time.
text - The virtual machine approach is much easier to port to new architectures (it is
text - just one smallish program that must be written to run on the new architecture)
text - but the extra layer of software (even as well as we know how to write
text - interpreters) above the machine reduces performance, often by a factor 2-10 or
text - more. So there is no right way to write language translators: each approach
text - comes with its own advantages and disadvantages, and each can be used/abused in
text - situations.
text - 
text - In fact, hot-spot compilers do a bit of both. They profile programs while
text - interpreting them to find their hots spots (small amounts of code that are
text - executed frequently) and then they compile just those small pieces -all while
text - running the program. Language translation is and has always been an important
text - area in Computer Science. At UCI this topic is covered initially in
text - COMPSCI 142A/B: Language Processor Construction. The computer architectures for
text - real machines (into which compiled programs are compiled) are covered first in
text - ICS-51.
text - 
text - In this lecture we will discuss the code that Python is translated into and how
text - this code is run on the Python Virtual Machine. There is no way to cover this
text - topic fully in one lecture, so my goal is just to introduce this material and
text - show you an interesting vertical slice through it. We will leverage off Python's
text - dis.py module, whose dis function (dis means disassembly) shows us, in a
text - readable form, the Virtual Machine instructions that Python functions, classes,
text - and modules are translated into. At the very end of this lecture we will return
text - to analysis of algorithms by counting the instructions our Python functions are
text - translated into.
text - 
text - Finally, see Section 31.12 in the Python Library documentation for many more
text - details about today's lecture. Once you "get" the big picture, you might find
text - it quite interesting to "dis" a variety of software components. I tried to find
text - information about the Python Virtual Machine on the web, but it is pretty
text - sparse. So I put together this lecture based on general principles and the
text - documentation I could find. There is much more information available for the
text - Java virtual machine.
text - 
text - ------------------------------------------------------------------------------
text - 
text - Basics: 
text - 
text - Every function object (what we are mostly concerned with in this lecture) is
text - associated with 3 tuples:
text - 
text -   (1) its local variable names including parameters (in .\_\_code\_\_.co\_varnames)
text -   (2) its global names (in .\_\_code\_\_.co\_names)
text -   (3) it constant (in .\_\_code\_\_.co\_consts)
text - 
text - These tuples are built at the time Python defines the function and they are
text - stored in the \_\_code\_\_ object associated with the function.
text - 
text - For example, if we define
text - 
added - ```python
code-start - def minimum(alist):
code -     m = None if len(alist) == 0 else alist[0]
code -     for v in alist[1:]:
code -         if v < m:
code -             m = v
code -     return m
code-end - 
added - ```
text - Then,
text -   minimum.\_\_code\_\_.co\_varnames is ('alist', 'm', 'v')
text -   minimum.\_\_code\_\_.co\_names is    ('len', 'None')
text -   minimum.\_\_code\_\_.co\_consts is   (None, 0, 1)
text - 
text - Load operations (e.g., LOAD\_FAST, LOAD\_GLOBAL, LOAD\_CONST which are all
text - discussed in more detail below) are followed by an integer that indexes these
text - tuples. So for example, in the function addup addup.\_\_code\_\_.co\_varnames is the
text - list ('alist', 'sum', 'v'), so the operation LOAD\_FAST  0 loads/pushes onto the
text - stack the value the name alist refers to. LOAD\_FAST  1 loads/pushes onto the
text - stack the value the name sum refers to. LOAD\_FAST  2 loads/pushes onto the
text - stack the value the name v refers to.
text - 
text - ------------------------------------------------------------------------------
text - 
text - The Python Virtual Machine (PVM) and an example trivial calculation
text - 
text - The main data structure in the PVM is the "regular" stack (which is like a
text - restricted a list push=append, pop=pop). A stack's primary operations are
text - load/push and store/pop. We load/push a value on the top of an upwardly-growing
text - stack (incrementing the stackp -stack pointer- that indexes the top); we
text - store/pop a values from the top of a stack (decrementing the stackp).
text - 
text - There is a secondarily important block stack that is used to store information
text - about nested loops, try, and with statements. For example, a break statement
text - is transated into code that uses the block stack to determine which loop to
text - break out of (and how to continue executing at the first statement outside the
text - loop). As loops, try/except, and with statements are started, information about
text - their blocks are pushed onto the block stack; as they terminate, this
text - information is popped off the bock stack. The block stack is too complicated
text - for today's lecture and is not needed to understand it: so when we run across
text - block stack instructions we will point out the fact that we are ignoring them.
text - 
text - Here is an example of a simple sequence of stack operations to perform the
text - calculation d = a+b\*c, assuming that a, b, c, and d are local variables inside
text - a function: assume co\_varnames is ('a', 'b', 'c', 'd') and the actual values
text - for these names are stored in a parallel tuple (1, 2, 3, None): e.g., the value
text - for 'a' is 1, the value for 'b' is 2, the value for 'c' is 3, and the value for
text - 'd' is None. Generally the value for a name at index i in the co\_varnames tuple
text - is stored in index i in the tuple of actual values.
text - 
text - As we will see in more detail below the meaning of 
text - 
text -   LOAD\_FAST N
text -     load/push onto the stack the value stored in co\_varnames[N],
text -     written stackp += 1, stack[stackp] = co\_varnames[N] 
text - 
text -   STORE\_FAST N
text -     store/pop the value on the top of the stack into co\_varnames[N],
text -     written co\_varnames[N] = stack[stackp], and stackp -= 1
text - 
text -   BINARY\_MULTIPLY
text -     load/push onto the stack the \* of the two values on the top,
text -     written stack[stackp-1] = stack[stackp-1] \* stack[stackp]; stackp -= 1
text -     (turns the two values on the top of the stack into their product)
text - 
text -   BINARY\_ADD
text -     load/push onto the stack the + of the two values on the top
text -     written stack[stackp-1] = stack[stackp-1] + stack[stackp]; stackp -= 1
text -     (turns the two values on the top of the stack into their sum)
text - 
text - The PVM code for d = a+b\*c is
text - 
text -   LOAD\_FAST 0
text -   LOAD\_FAST 1
text -   LOAD\_FAST 2
text -   BINARY\_MULTIPLY
text -   BINARY\_ADD
text -   STORE\_FAST 3
text - 
text - Here is what happens step by step.
text - 
text - Initially
text -          ....
text -    +--------------------+
text -  3 |                    |
text -    +--------------------+
text -  2 |                    |
text -    +--------------------+
text -  1 |                    |
text -    +--------------------+
text -  0 |                    |
text -    +--------------------+
text - stack (with stackp = -1, meaning is empty stack)
text - 
text - execute LOAD\_FAST 0
text -          ....
text -    +--------------------+
text -  3 |                    |
text -    +--------------------+
text -  2 |                    |
text -    +--------------------+
text -  1 |                    |
text -    +--------------------+
text -  0 | 1: value of a      |
text -    +--------------------+
text - stack (with stackp = 0)
text - 
text - execute LOAD\_FAST 1
text -          ....
text -    +--------------------+
text -  3 |                    |
text -    +--------------------+
text -  2 |                    |
text -    +--------------------+
text -  1 | 2: value of b      |
text -    +--------------------+
text -  0 | 1: value of a      |
text -    +--------------------+
text - stack (with stackp = 1)
text - 
text - execute LOAD\_FAST 2
text -          ....
text -    +--------------------+
text -  3 |                    |
text -    +--------------------+
text -  2 | 3: value of c      |
text -    +--------------------+
text -  1 | 2: value of b      |
text -    +--------------------+
text -  0 | 1: value of a      |
text -    +--------------------+
text - stack (with stackp = 2)
text - 
text - execute BINARY\_MULTIPLY
text -          ....
text -    +--------------------+
text -  3 |                    |
text -    +--------------------+
text -  2 |                    |
text -    +--------------------+
text -  1 | 6: value of b\*c    |
text -    +--------------------+
text -  0 | 1: value of a      |
text -    +--------------------+
text - stack (with stackp = 1)
text - 
text - execute BINARY\_ADD
text -          ....
text -    +--------------------+
text -  3 |                    |
text -    +--------------------+
text -  2 |                    |
text -    +--------------------+
text -  1 |                    |
text -    +--------------------+
text -  0 |7: value of a+b\*c   |
text -    +--------------------+
text - stack (with stackp = 0)
text - 
text - execute STORE\_FAST 3
text -          ....
text -    +--------------------+
text -  3 |                    |
text -    +--------------------+
text -  2 |                    |
text -    +--------------------+
text -  1 |                    |
text -    +--------------------+
text -  0 |                    |
text -    +--------------------+
text - stack (with stackp = -1)
text - 
text - At this point d's value is 7, the value that was at the top of the stack when
text - STORE\_FAST was executed. The actual values for these names are stored in the
text - tuple (1, 2, 3, 7).
text - 
text - Problem: show (by drawing what I drew above) how the following instructions
text - compute d = (a+b)\*c
text - 
text -   LOAD\_FAST 0
text -   LOAD\_FAST 1
text -   BINARY\_ADD
text -   LOAD\_FAST 2
text -   BINARY\_MULTIPLY
text -   STORE\_FAST 3
text - 
text - Any expression can be translated into similar code for the PVM to evaulate its
text - value.
text - 
text - -------------------------
text - 
text - Control (the fetch/execute cycle) of the PVM
text - 
text - Each instruction (some of which are shown above) in the PVM consists of 1 or 3
text - bytes of information (a byte is 8 bits, and can represent numbers from 0 to
text - 255). The first byte is the operation or byte code; the second two bytes are
text - the operand for that byte code (but not all byte codes require operands:
text - LOAD\_FAST does; BINARY\_ADD doesn't). Two bytes (16 bits) can represent the
text - numbers 0 to 65,536: so it is probably the case that Python function cannot
text - have more than 65,536 different local variable names.
text - 
text - The instructions are stored in memory: think of memory too as a kind of list
text - named m which stores sequential data in one location after the other. We can
text - illustrate this by writing the instruction sequence above as follows:
text - 
text -   Memory     Instruction
text -   Location   
text - --------------------------
text -    0         LOAD\_FAST 0
text -    3         LOAD\_FAST 1
text -    6         LOAD\_FAST 2
text -    9         BINARY\_MULTIPLY
text -   10         BINARY\_ADD
text -   11         STORE\_FAST 3
text - 
text - Note that the first instruction is stored in m[0], and each subsequent
text - instruction is stored in a location that is 3 higher (if the instruction has an
text - explicit operand, as the load/store instructions do; most instructions have
text - implicit operands: stack and pc) or 1 higher (if the instruction has no explicit
text - operands, as the binary operator instructions do).
text - 
text - Technically, the operation name (e.g., LOAD\_FAST) represents a one byte value
text - (an integer from 0 to 255). The next two bytes are typically the higher and
text - lower bits in an integer between 0 and 2\*\*16-: 65,535.
text - 
text - Once a program (instruction sequence) is loaded into memory, the PVM executes
text - it according to the following simple rules. This control/execute cycle is what
text - animates computers, allowing then to execute programs. It is fundamental in
text - Computer Science.
text - 
text -  (1) Fetch the operation and it operand (if present) starting at m[pc]
text -  (2) pc += 3 (if operand is present) or pc += 1 (if no operand is present)
text -  (3) Execute operation code (possibly change its operand, stack, stackp, or pc)
text -  (4) Go to step 1
text - 
text - Some operations manipulate the stack/stackp and the tuples that store values,
text - others can change the pc (examples of such jump instructions appear in later
text - sections, changing the locus of execution of the code).
text - 
text - So, when the pc is initially 0, the PVM executes the code above as follows
text - 
text -  1. fetches the operation a m[0] and the operand at m[1] and m[2]
text -  2. increments pc to 3
text -  3. manipulates the stack (see above)
text -  4. goes back to step 1
text - 
text -  1. fetches the operation a m[3] and the operand at m[4] and m[5]
text -  2. increments pc to 6
text -  3. manipulates the stack (see above)
text -  4. goes back to step 1
text - 
text -  1. fetches the operation a m[6] and the operand at m[7] and m[8]
text -  2. increments pc to 9
text -  3. manipulates the stack (see above)
text -  4. goes back to step 1
text - 
text -  1. fetches the operation a m[9]: it has no operand
text -  2. increments pc to 10
text -  3. manipulates the stack (see above)
text -  4. goes back to step 1
text - 
text -  1. fetches the operation a m[10]: it has no operand
text -  2. increments pc to 11
text -  3. manipulates the stack (see above)
text -  4. goes back to step 1
text - 
text - At this point there is no more code to execute. In the next example we will
text - see how the PVM executes more complicated code, specified by full Python
text - functions.
text - 
text - ------------------------------------------------------------------------------
text - 
text - The dis function in the dis module
text - 
text - As described briefly in the intoduction, we can print a symbolic/annotated
text - desciption of any Python function (and module/class too; but in this lecture
text - note we will stick with just functions) by using the dis function in the dis.py
text - module. Here "dis" means "disassemble the code in the function object into a 
text - form that is readable by people.
text - 
text - Here is an example function (with line numbers) and the result dis.dis displays
text - for it). All the operation codes and their meanings are covered in detail in
text - the next section; we will look ahead at the relevant ones.
text - 
text - 1 def addup(alist):
text - 2     sum = 0
text - 3     for v in alist:
text - 4         sum = sum + v
text - 5    return sum
text - 
text - Actually, I wrote the following simple function to show useful information
text - about any function object (its name, its three tuples, and the dis information:)
text - labelled.
text - 
added - ```python
code-start - def func_obj(fo):
code -     print(fo.__name__)
code -     print('  co_varnames:',fo.__code__.co_varnames)
code -     print('  co_names   :',fo.__code__.co_names)
code -     print('  co_consts  :',fo.__code__.co_consts,'\n')
code -     print('Source Line  m  operation/byte-code      operand (useful name/number)\n'+69*'-')
code -     dis.dis(fo)
code-end - 
added - ```
text - calling func\_obj(addup) prints
text - 
text - addup
text -   co\_varnames: ('alist', 'sum', 'v')
text -   co\_names   : ()
text -   co\_consts  : (None, 0)
text - 
text - Source Line  m  op/byte-code             operand (useful name/number)
text - ---------------------------------------------------------------------
text -   2           0 LOAD\_CONST               1 (0) 
text -               3 STORE\_FAST               1 (sum) 
text - 
text -   3           6 SETUP\_LOOP              24 (to 33) 
text -               9 LOAD\_FAST                0 (alist) 
text -              12 GET\_ITER             
text -         >>   13 FOR\_ITER                16 (to 32) 
text -              16 STORE\_FAST               2 (v) 
text - 
text -   4          19 LOAD\_FAST                1 (sum) 
text -              22 LOAD\_FAST                2 (v) 
text -              25 BINARY\_ADD           
text -              26 STORE\_FAST               1 (sum) 
text -              29 JUMP\_ABSOLUTE           13 
text -         >>   32 POP\_BLOCK            
text - 
text -   5     >>   33 LOAD\_FAST                1 (sum) 
text -              36 RETURN\_VALUE         
text - 
text - Note that any line prefaced by >> means that some other instruction in the
text - function can jump to it (start executing code at it). Jumping in the PVM (by
text - setting pc) is how loops and if statements in Python do their computation.
text - 
text - Here is a high-level description how this function executes. For more details,
text - see the exact description of each instruction in the next section.
text - 
text - Line 2:
text -  m[ 0]: loads the value 0 (co\_consts[1]) on the stack
text -  m[ 3]: stores the value 0 into sum (co\_varnames[1])
text - 
text - Line 3:
text -  m[ 6]: setup for the loop by pushing the size of the loop onto the block stack
text -           (recall that we won't be doing anything with the block stack)
text -  m[ 9]: loads the value of alist (co\_varnames[0]) on the stack
text -  m[12]: replaces its value on the stack by its iterator (by popping and pushing)
text -  m[13]: loads the next iterator value on the stack, jumping to m[32] if
text -            StopIteration raised
text -           (code in m[29] jumps back to this location to make the loop)
text -  m[16]: stores the next value into v (co\_varnames[2]), popping it off the stack
text - 
text - Line 4:
text -  m[19]: loads the value of sum (co\_varnames[1]) on the stack
text -  m[22]: loads the value of v (co\_varnames[2]) on the stack
text -  m[25]: removes from stack/adds two values, pushes the result on the stack
text -  m[26]: stores the value into sum (co\_varnames[1]), popping it off the stack
text -  m[29]: sets pc to 13, so the next instruction executed in at m[13]
text -           (jumps back to a previous location to make the loop loop)
text -  m[32]: pops what m[6] pushed onto the block stack
text -           (recall that we won't be doing anything with the block stack)
text -           (code in m[13] jumps here, on StopIteration, terminating the loop)
text - 
text - Line 5:
text -  m[33]: load the value of sum (co\_varnames[1]) on the stack to return
text -  m[36]: return from the function with the result on the top of the stack
text - 
text - ------------------------------------------------------------------------------
text - 
text - Operation/Byte Codes
text - 
text - Below is a list of many important operations and how they manipulate the PVM.
text - The complete list is available in section 31.12 of the Python documenation. 
text - Recall that many operations manipulate stack, stackp, and pc.
text - 
text - Loading/Storing
text - 
text -   LOAD\_CONST   N
text -     stackp += 1, stack[stackp] = co\_consts[N] 
text -   LOAD\_FAST    N
text -     stackp += 1, stack[stackp] = co\_varnames[N] 
text -   LOAD\_GLOBAL  N
text -     stackp += 1, stack[stackp] = co\_names[N] 
text -   STORE\_CONST  N
text -     co\_consts[N] = stack[stackp], and stackp -= 1
text -   STORE\_FAST   N
text -     co\_varnames[N] = stack[stackp], and stackp -= 1
text -   STORE\_GLOBAL N
text -     co\_names[N] = stack[stackp], and stackp -= 1
text - 
text - There are general Load and Store operations that look up names based on the
text - the LEGB rules, if Python is unsure where these names will be found. Generally
text - it uses the operations above.
text - 
text - Operators
text -   UNARY\_POSITIVE 
text -     stack[stackp] = +stack[stackp]
text -   UNARY\_NEGATIVE 
text -     stack[stackp] = -stack[stackp]
text -   UNARY\_NOT 
text -     stack[stackp] = not stack[stackp]
text -   UNARY\_INVERT 
text -     stack[stackp] = ~stack[stackp]
text - 
text -   BINARY\_ADD
text -     stack[stackp-1] = stack[stackp-1] + stack[stackp]; stackp -= 1
text -   BINARY\_SUBTRACT 
text -     stack[stackp-1] = stack[stackp-1] - stack[stackp]; stackp -= 1
text -   BINARY\_MULTIPLY
text -     stack[stackp-1] = stack[stackp-1] \* stack[stackp]; stackp -= 1
text -   BINARY\_TRUE\_DIVIDE 
text -     stack[stackp-1] = stack[stackp-1] / stack[stackp]; stackp -= 1
text -   BINARY\_FLOOR\_DIVIDE 
text -     stack[stackp-1] = stack[stackp-1] // stack[stackp]; stackp -= 1
text -   BINARY\_MODULO 
text -     stack[stackp-1] = stack[stackp-1] % stack[stackp]; stackp -= 1
text -   BINARY\_POWER 
text -     stack[stackp-1] = stack[stackp-1] \*\* stack[stackp]; stackp -= 1
text - 
text -   BINARY\_SUBSCR (indexing)
text -     stack[stackp-1] = stack[stackp-1] [ stack[stackp] ]; stackp -= 1
text - 
text -   BINARY\_LSHIFT 
text -     stack[stackp-1] = stack[stackp-1] << stack[stackp]; stackp -= 1
text -   BINARY\_RSHIFT 
text -     stack[stackp-1] = stack[stackp-1] >> stack[stackp]; stackp -= 1
text - 
text -   BINARY\_AND 
text -     stack[stackp-1] = stack[stackp-1] & stack[stackp]; stackp -= 1
text -   BINARY\_OR 
text -     stack[stackp-1] = stack[stackp-1] | stack[stackp]; stackp -= 1
text -   BINARY\_XOR 
text -     stack[stackp-1] = stack[stackp-1] ^ stack[stackp]; stackp -= 1
text - 
text - There are in-place versions of the binary operators: e.g., x += 1 vs x = x + 1
text - I will show the meanin of INPLACE\_ADD and just list the others here:
text - 
text -   INPLACE\_ADD
text -     stack[stackp-1] += stack[stackp]; stackp -= 1
text - 
text - INPLACE\_SUBTRACT, INPLACE\_MULTIPLY, INPLACE\_FLOOR\_DIVIDE, INPLACE\_TRUE\_DIVIDE,
text - INPLACE\_MODULO, INPLACE\_POWER, INPLACE\_LSHIFT, INPLACE\_RSHIFT, INPLACE\_AND,
text - INPLACE\_XOR, and INPLACE\_OR
text - 
text - Iteration:
text - 
text -   GET\_ITER 
text -     stack[stackp] = iter(stack[stackp])
text -   FOR\_ITER N
text -     stackp += 1; stack[stackp] = next(stack[stackp-1])
text -       but if StopIteration exception is raised in part 2: pc += N
text -   
text - Jumping (changing the pc from where the next instruction is fetched):
text - 
text -   JUMP\_ABSOLUTE     N
text -     pc = N
text -   JUMP\_FORWARD      N
text -     pc += N
text -   POP\_JUMP\_IF\_TRUE  N
text -     if stack[stackp] is True, pc = N (always stackp =- 1)
text -   POP\_JUMP\_IF\_FALSE N
text -     if stack[stackp] is False, pc = N (always stackp =- 1)
text - 
text - Calling Functions and Returning/Yielding
text -   CALL\_FUNCTION N
text -     The first operand byte is a count of the position arguments: pcount
text -     The second operand byte is a count of the keyword arguments: kcount
text -     There are kcount values on the top of the stack followed by pcount values
text -       followed by the function to call
text -     This operation pops all function arguments off the stack to store them into
text -       the co\_varnames tuple, and pops off the the function itself
text -     The function should leave its answer on the top of the stack
text - 
text -   RETURN\_VALUE
text -     Return to the location that called this function (its retuned answer on the
text -       top of the stack)
text - 
text -   YIELD\_VALUE
text - 
text - In the Library Reference, they use the notation TOS to mean the location at
text - the top of the stack, and TOSn to mean n down from the top of the stack. So
text - TOS is our stack[stackp] and TOS1 is our stack[stackp-1]
text - 
text - ------------------------------------------------------------------------------
text - 
text - Here is a slightly more interesting function. It contains both a conditional/if
text - statement (lines 4-5) and a conditional expression (line 6), which translate
text - into a variety of jump instructions. Also note how the tuple assignment in line
text - 2 is translated via the UNPACK\_SEQUENCE N instruction (which takes any sequence
text - of values (tuple or list) and pushes N of them onto the stack right to left:
text - so if (0,1) is on the stack, UNPACK\_SEQUENCE 2 pops this value off the stack,
text - pushing first 1 then 0 onto the stack (which is why the first value below is
text - stored into sum and the second is stored into count).
text - 
text - 1 def average\_positive(alist):
text - 2    sum,count = 0,0
text - 3    for v in alist:
text - 4        if v > 0:
text - 5            sum = sum + v
text - 6            count += 1
text - 7    return sum / (1 if count == 0 else count)
text - 
text - average\_positive
text -   co\_varnames: ('alist', 'sum', 'count', 'v')
text -   co\_names   : ()
text -   co\_consts  : (None, 0, 1, (0, 0)) 
text - 
text - Source Line  m  operation/byte-code      operand (useful name/number)
text - ---------------------------------------------------------------------
text -  2            0 LOAD\_CONST               3 ((0, 0)) 
text -               3 UNPACK\_SEQUENCE          2 
text -               6 STORE\_FAST               1 (sum) 
text -               9 STORE\_FAST               2 (count) 
text - 
text -  3           12 SETUP\_LOOP              49 (to 64) 
text -              15 LOAD\_FAST                0 (alist) 
text -              18 GET\_ITER             
text -         >>   19 FOR\_ITER                41 (to 63) 
text -              22 STORE\_FAST               3 (v) 
text - 
text -  4           25 LOAD\_FAST                3 (v) 
text -              28 LOAD\_CONST               1 (0) 
text -              31 COMPARE\_OP               4 (>) 
text -              34 POP\_JUMP\_IF\_FALSE       19 
text - 
text -  5           37 LOAD\_FAST                1 (sum) 
text -              40 LOAD\_FAST                3 (v) 
text -              43 BINARY\_ADD           
text -              44 STORE\_FAST               1 (sum) 
text - 
text -  6           47 LOAD\_FAST                2 (count) 
text -              50 LOAD\_CONST               2 (1) 
text -              53 INPLACE\_ADD          
text -              54 STORE\_FAST               2 (count) 
text -              57 JUMP\_ABSOLUTE           19 
text -              60 JUMP\_ABSOLUTE           19 
text -         >>   63 POP\_BLOCK            
text - 
text -  7      >>   64 LOAD\_FAST                1 (sum) 
text -              67 LOAD\_FAST                2 (count) 
text -              70 LOAD\_CONST               1 (0) 
text -              73 COMPARE\_OP               2 (==) 
text -              76 POP\_JUMP\_IF\_FALSE       85 
text -              79 LOAD\_CONST               2 (1) 
text -              82 JUMP\_FORWARD             3 (to 88) 
text -         >>   85 LOAD\_FAST                2 (count) 
text -         >>   88 BINARY\_TRUE\_DIVIDE   
text -              89 RETURN\_VALUE         
text - 
text - Please feel free to type in all sorts of small functions to see how they are
text - translated to run on the PVM. The project folder that you can download includes
text - some simple functions and the func\_obj function.
text - 
text - Note that the two simple functions shown did not call functions on their inside:
text - see addup1 in the project folder, which uese range and len to iterate of the
text - list indexes to compute the sum. The relevant line in the function is
text - 
text -     for i in range(len(alist)):
text - 
text - The local/global names are
text - 
text -   co\_varnames: ('alist', 'sum', 'i')
text -   co\_names   : ('range', 'len')
text - 
text - and sequence of instructions is
text - 
text -               9 LOAD\_GLOBAL              0 (range) 
text -              12 LOAD\_GLOBAL              1 (len) 
text -              15 LOAD\_FAST                0 (alist) 
text -              18 CALL\_FUNCTION            1 (1 positional, 0 keyword pair) 
text -              21 CALL\_FUNCTION            1 (1 positional, 0 keyword pair) 
text -              24 GET\_ITER             
text -         >>   25 FOR\_ITER                20 (to 48) 
text -              28 STORE\_FAST               2 (i) 
text - 
text - Here the range function, to call last, is loaded/pushed on the stack; then the
text - len function, to call first, is loaded/pushed on the stack; then the alist
text - variable is pushed on the stack. The stack looks as follows
text - 
text -          ....
text -    +--------------------+
text -  3 |                    |
text -    +--------------------+
text -  2 | alist value        |
text -    +--------------------+
text -  1 | len function       |
text -    +--------------------+
text -  0 | range function     |
text -    +--------------------+
text - stack (with stackp = 2)
text - 
text - The first CALL\_FUNCTION 1 says using 1 positional argument (stackp=2), call the
text - function specified at stackp-1 (len) on the stack, leaving the answer on the
text - stack. The stack looks as follows
text - 
text -          ....
text -    +--------------------+
text -  3 |                    |
text -    +--------------------+
text -  2 |                    |
text -    +--------------------+
text -  1 | len(alist) value   |
text -    +--------------------+
text -  0 | range function     |
text -    +--------------------+
text - stack (with stackp = 1)
text - 
text - The second CALL\_FUNCTION 1 says using 1 positional argument (stackp=1), call the
text - function specified at stackp-1 (range) on the stack, leaving the answer on the
text - stack. The stack looks as follows
text - 
text -          ....
text -    +--------------------+
text -  3 |                    |
text -    +--------------------+
text -  2 |                    |
text -    +--------------------+
text -  1 |                    |
text -    +--------------------+
text -  0 | range(len(alist))  | The range iterator 
text -    +--------------------+
text - stack (with stackp = 1)
text - 
text - Then GET\_ITER is called to do the iteration, which uses FOR\_ITER to produce
text - the first value (which is stored in local variable i).
text - 
text - ------------------------------------------------------------------------------
text - 
text - Return to Analysis of Algorithms:
text - 
text - We can use the result of dis to compute the worst case count of the numnber of
text - instructions executed in a function. Let's return to (and review) the addup
text - function for our analysis.
text - 
text - 1 def addup(alist):
text - 2     sum = 0
text - 3     for v in alist:
text - 4         sum = sum + v
text - 5    return sum
text - 
text - addup
text -   co\_varnames: ('alist', 'sum', 'v')
text -   co\_names   : ()
text -   co\_consts  : (None, 0)
text - 
text - Source Line  m  op/byte-code             operand (useful name/number)
text - ----------------------------------------------------------------------
text -   2           0 LOAD\_CONST               1 (0) 
text -               3 STORE\_FAST               1 (sum) 
text - 
text -   3           6 SETUP\_LOOP              24 (to 33) 
text -               9 LOAD\_FAST                0 (alist) 
text -              12 GET\_ITER             
text -         >>   13 FOR\_ITER                16 (to 32) 
text -              16 STORE\_FAST               2 (v) 
text - 
text -   4          19 LOAD\_FAST                1 (sum) 
text -              22 LOAD\_FAST                2 (v) 
text -              25 BINARY\_ADD           
text -              26 STORE\_FAST               1 (sum) 
text -              29 JUMP\_ABSOLUTE           13 
text -         >>   32 POP\_BLOCK            
text - 
text -   5     >>   33 LOAD\_FAST                1 (sum) 
text -              36 RETURN\_VALUE         
text - 
text - Here is how to account for the number of instructions executed when Python runs
text - this function. As always we will assume that the len(alist) is N. Here there are
text - no conditional statements so the worst case always executes all the code in the
text - function (and the loop N times).
text - 
text - Instructions done once (not in the loop proper)
text -   2 for line 2, code before the loop: initialize sum
text -   3 for line 3, setup for the loop; not repeated in the loop proper at m[13]
text -   1 for line 4, at m[32], jumped to when StopIteration exception is raised
text -   2 for line 5, code after the loop: load/push sum on the stack and return
text - 
text - Instructions done in the loop
text -   2 for line 3, m[13] and m[16]; note loop back to m[13]
text -   5 for line 4, sum = sum + v and jump back to m[13] to get next iterator value
text - 
text - Finally, note that the instruction at m[13] is executed N+1 times: N times it
text - continues in the loop body and 1 time it raises StopIteration and jumps to
text - m[32].
text - 
text - So, the I(N) is 8 (instructions done once) + 7N (instructions done in the loop)
text -  + 1 (m[13] done on N+1 iteration raising StopInteration) instructions.
text - 
text - I(N) = 7N + 9 instructions
text - 
