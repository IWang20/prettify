text - # <div align=center> 	Python Review (everything you should have learned in ICS-31/32)</div>
added - ***
added - ## <a name="top"></a> Table of Contents
added - ***
text - 
text - When reading the following material, I suggest that you have Eclipse open,
text - including a project with an empty module; then copy/paste some of the code
text - below into it, to see what it does. Explore the code by experimenting with
text - (changing) it and predicting what results from the changes. I always have an
text - Eclipse folder/module named "experiment" open for this purpose. I believe
text - this approach is superior to me typing/executing code during lecture.
text - 
text - This lecture note is long (it is really three lectures, covered during the first
text - week), but the information is critical for getting started. I hope that this is
text - mostly a review for ICS-31/32 students, but there are likely to be many things
text - that come up as new (or as new perspectives and connections to the material
text - that you already know). Pay close attention to the terminology used here, as I
text - will use it (I hope consistently) throughout the entire quarter. If you do not
text - know any of these technical terms, try looking them up online, or post a
text - message in the "Lecture Material" Piazza Q&A Folder: if you didn't understand a
text - term, probably other students didn't as well. Here are 3 quotes relevant to
text - this lecture:
text - 
text -   1) The first step towards wisdom is calling things by their right names.
text - 
text -   2) He/She who is ashamed of asking is ashamed of learning.
text - 
text -   3) The voyage of discovery is not in seeking new landscapes but in having new
text -      eyes. - M. Proust
text - 
text - ------------------------------------------------------------------------------
text - 
text - Python in Four Sentences:
text - 
text - 1. Names (in namespaces) are bound to objects.
text - 
text - 2. Every object has its own namespace.
text -    (a dictionary that binds its internal names to other objects)
text - 
text - 3. Everything that Python computes with is an object.
text -    (examples are instance/data, function, module, and class objects)
text - 
text - 4. Python has rules about how things work.
text - 
text - In some sense these four sentences tell you very little about Python, but in
text - another sense they are a tiny framework in which to interpret every idea -small
text - and large- in Python and how Python works.
text - 
text - 1+2) Every name appears in the namespace of some object (when we define names in
text - a module, for example, these names appear in the module object's namespace); and
text - every name is itself bound to some object: names are mostly bound when defined
text - on the left of the = symbol. They can be rebound to another object by using
text - them on the left of the = symbol again. Names are also bound in imports,
text - function definitions, and class definitions: these names/bindings are discussed
text - in ICS-33 throughout the quarter.
text - 
text - 3) Objects are the fundamental unit with which Python computes. For example...
text - 
text -   a) We can compute with int objects (instance objects from the int class) by
text -      using operators; for the int object bound to x by x = 1 we can rebind it to
text -      another int object, one bigger, by writing x = x + 1. We will learn later
text -      that Python translates x+1 into the method call x.\_\_add\_\_(1) and then into
text -      int.\_\_add\_\_(x,1) -assuming, as it does here, x stores a reference to an
text -      int object: the name int is itself a reference to a class object (not an
text -      instance of a class object).
text - 
text -   b) We can compute with function objects by calling them; for the function
text -      object bound to print, we can write print(x); we will learn later in these
text -      notes that we can also pass functions as arguments to other functions and
text -      return functions as results from other functions. Python allows us to do
text -      many interesting things with function objects, beyond just calling them.
text -    
text -   c) We can compute with module objects by importing them (and/or the objects
text -      bound to the names in their namespaces).
text -      import random			or     from random import randint
text -      x = random.randint(1,6)		       x = randint(1,6)
text - 
text -      The name random (in the current           The name randint (in the current
text -      module) is bound to the random module     module) is bound to the function
text -      object, which defines a randint	       object that the name randint (in
text -      function in its namespace, which we       the random module's namespace)
text -      can refer to/call using random.randint.   is bound to, which is called.
text - 
text -   d) We can compute with class objects by constructing instance objects and
text -      using the instances to call class methods. For example, we can write
text -      timer = Stopwatch()
text -      timer.start()
text - 
text - 4) Python follows rules that determine how names are bound (e.g., how arguments
text - are bound to parameters), how operators compute, how control structures execute
text - the code blocks they control, etc. This course is designed to demystify how
text - Python executes scripts and how you can better use its features to write
text - compact/powerful scripts. We understand a programming language in most part by
text - knowing the rules it uses to execute its code.
text - 
text - ------------------------------------------------------------------------------
text - 
text - Binding (and Drawing Names and their associated Objects)
text - 
text - The process of making a name refer to a value is called BINDING: e.g., x = 1
text - binds the name x to the value 1 (which is an object/instance of the int class).
text - A name can store only one binding. Later, we can rebind x to another value
text - (possibly from another class) in another assignment: e.g., x = 'abc'. At this
text - point its first binding is forgotten. We speak about "the binding (noun) of a
text - name" to mean the value (such values are always objects) that the name is
text - currently associated with (or the object the name refers to or is bound to).
text - 
text - In Python, every data instance, module, function, and class is an object that
text - has a dictionary that stores its namespace: all its internal bindings. We will
text - learn much more about namespaces (and how to manipulate them) later in the
text - quarter, when we study classes in more detail.
text - 
text - Typically we illustrate the binding of a name to an object (below, x = 1) as
text - follows. We write the name over a square-edge rectangle, in which the tail of
text - an arrow is INSIDE, specifying the current reference stored for that name: the
text - arrow's head refers to a rounded-edge rectangle object labeled above by its
text - type (here, the class it is constructed from) with its value inside.
text - 
text -   x          int
text - +---+	    (---)
text - | --+------>| 1 |
text - +---+	    (---)
text - 
text - Technically, if we we write x = 1 inside the module m, Python has already
text - created an object for module m (we show all objects as rounded-edge rectangles)
text - and it puts x, its box, and its binding in the namespace of module m: here, the
text - name x is defined inside module m and bound to object 1. That is, we would more
text - formally write the result of x = 1 in module m as
text - 
text -              module
text -           (---------)
text -   m       |    x    |      int
text - +---+     |  +---+  |     (---)
text - | --+---->|  | --+--+---->| 1 |
text - +---+     |  +---+  |     (---)
text -           (---------)
text - 
text - But often we revert to the previous simpler diagram, when we don't care in what
text - module x is defined, focusing solely on x and the object it refers to.
text - 
text - Note that the del command in Python (e.g., del name) removes a name from the
text - current namespace/dictionary of the object in which name is bound. So, writing
text - del x inside module m would remove x, its box, and its arrow from m's
text - namespace/dictionary (but it DOES NOT do anything with the object to which x is
text - currently bound: del affects names  not the objects the are bound to). If there
text - were no name x in this module, Python would raise a NameError exception.
text - 
text - More generally, if m1 (a name defined in module m) were to refer to another
text - module (resulting from executing import m1 in module m), and that module
text - defined the name x (see the picture below) 
text - 
text -              module          module
text -           (---------)     (---------)
text -   m       |    m1   |     |    x    |      int
text - +---+     |  +---+  |     |  +---+  |     (---)
text - | --+---->|  | --+--+---->|  | --+--+---->| 1 |
text - +---+     |  +---+  |     |  +---+  |     (---)
text -           (---------)     (---------)
text - 
text - then writing del m1.x inside module m would remove x, its box, and arrow from
text - m1's namespace/dictionary. In such a case, m1.x refers to the name x in m1's
text - namespace/dictionary. Such names are called "qualified names". If there were no
text - name x in this namespace/dictionary, Python would raise a NameError exception.
text - 
text - If we write
text - 
text -   x = ['a','b']
text -   y = ['a','b']
text - 
text - Python creates two list objects, binding index 0 to the value 'a' in each and
text - binding index 1 to the value 'b' in each.
text - 
text -  Technically, the 0 and 1 indexes of the list refer to the SAME string objects
text - ('a' and 'b') as shown below. This a subtle point in Python that is special to
text - most (not even all) int and str objects, which are written in Python by special
text - constants. It is not true for objects constructed from other, more complicated
text - classes. So, we picture the result of executing this code as
text - 
text -                 list
text -             (-----------)
text -   x         |   0   1   |       str
text - +---+	    | +---+---+ |      (---)
text - | --+------>| | | | --+-+----->|'b'|<- +
text - +---+	    | +-+-+---+ |      (---)   |
text - 	    (---+-------)              |
text -                 |               str    |
text -                 |              (---)   |
text - 		+------------->|'a'|   |
text - 	                       (---)   |
text -                                  ^     |
text -                 list             |     |
text -             (-----------)        |     |
text -   y         |   0   1   |        |     |
text - +---+	    | +---+---+ |        |     |
text - | --+------>| | | | --+-+--------+-----+
text - +---+	    | +-+-+---+ |        |
text - 	    (---+-------)        |
text -                 |                |
text -                 |                |
text - 		+----------------+
text - 
text - The "is" operator in Python determines whether two references refer to the same
text - object (it is called the (object)identity operator); the == operator determines
text - whether two references refer to objects that store the same internal state. If
text - a is b is True then a == b must be True, because both a and b refer to the same
text - object if a is b is True, and every object has the same state as itself.
text - 
text - In the above picture, x is y is False and x == y is True: the names x and y are
text - bound to/refer to two different list objects (each use of [] creates a new
text - list), but these two different list objects store the same state ('a' at index
text - 0; 'b' at index 1).
text - 
text - Generally, when using int/str objects in pictures, I don't care whether you
text - share (as above) or duplicate (as below) such values.
text - 
text -                 list
text -             (-----------)
text -   x         |   0   1   |        str
text - +---+	    | +---+---+ |       (---)
text - | --+------>| | | | --+-+-----> |'b'|
text - +---+	    | +-+-+---+ |	(---)
text - 	    (---+-------)
text -                 |
text - 		v
text - 	       str		
text - 	      (---)
text - 	      |'a'|
text - 	      (---)
text - 
text -                 list
text -             (-----------)
text -   y         |   0   1   |        str
text - +---+	    | +---+---+ |       (---)
text - | --+------>| | | | --+-+-----> |'b'|
text - +---+	    | +-+-+---+ |	(---)
text - 	    (---+-------)
text -                 |
text - 		v
text - 	       str		
text - 	      (---)
text - 	      |'a'|
text - 	      (---)
text - 
text - Both the pictures above have x is y as False and x == y as True.
text - 
text - We can use the "is" operator or the list indexes to tell us the truth: which
text - picture is technically correct. Copy/paste the following code into a Python
text - module in Eclipse; then predict the results (and then execute the code to
text - see what Python says).
text - 
text - x = ['a','b']
text - y = ['a','b']
text - print(x is y, x == y, x[0] is y[0], x[1] is y[1], x[0] == y[0], x[1] == y[1])
text - 
text - If we instead write
text - 
text -   x = ['a','b']
text -   y = x
text - 
text - Python creates one list object, and binds it to first to x and then to y (the
text - assignment y = x just COPIES into name y the same reference that is in name x,
text - making y and x refer to the same object). Here, x is y is True (and x == y is
text - therefore True): the two different names x and y refer to the same list object.
text - So, we picture the result of executing this code as
text - 
text -   x
text - +---+           list
text - | --+------>(-----------)
text - +---+       |   0   1   |        str
text -             | +---+---+ |       (---)
text -   y         | | | | --+-+-----> |'b'|
text - +---+	    | +-+-+---+ |	(---)
text - | --+------>(---+-------)
text - +---+           |
text - 		v
text - 	       str		
text - 	      (---)
text - 	      |'a'|
text - 	      (---)
text - 
text - Therefore, pictures can help us understand the differences between these two
text - code fragments. You should be able to draw a simple picture of these names and
text - objects (both the list and str objects) to illustrate the difference between
text - the "is" and == operators.
text - 
text - The == operator, especially in simple programs, is used much more frequently
text - than the the "is" operator.
text - 
text - What happens in each example (what picture results) if we execute y[0] = 'c'?
text - What would be printed by print(x,y,x[0]) in each after such an assignment?
text - Copy and paste each of the following code fragments into a Python module in
text - Eclipse, and predict the results (and then execute the code to see what Python
text - says).
text - 
text - x = ['a','b']
text - y = ['a','b']
text - y[0] = 'c'
text - print(x, y, x[0], x is y, x == y)
text - 
text - vs.
text - 
text - x = ['a','b']
text - y = x
text - y[0] = 'c'
text - print(x, y, x[0], x is y, x == y)
text - 
text - Finally, if we are in module m1 and we write 
text - 
text - import m2
text - 
text - then we can create/delete names in m2's namespace by writing
text - 
text - m2.x = 3  # create a name x in the namespace of module m2 (present? use that
text -           # name); bind that new/existing name to the int 3
text - 
text - del m2.x  # delete name x in the namespace of module m2; absent? raise exception
text - 
text - 
text - ------------------------------
text - Recent change to Python: 
text - 
text - Python recently (version 3.8) added the := binding operator, which binds a name
text - to a value but also evaluates to a result; so the result of this binding
text - operator can be used INSIDE an EXPRESSION (unlike the = binding operator, which
text - must be used as its own statement). The result it evaluates to is the object
text - computed for the right side of the := operator (which is also the value bound
text - to the left side). When we use the := operator, we must put it in parentheses,
text - as in (var := value) which evaluates value, binds var to that object, and then
text - has that object as the value of the parenthesized expression.
text - 
text - For example, we can use := to avoid recomputing a function or avoid creating a
text - new line of code to store it temporarily. Assume f() is an expensive to compute
text - function call, so we don't want to call it twice in the following code.
text - 
text - if f() > 0:
text -   return f()
text - 
text - In old Python, to avoid calling it twice, we could write
text - 
text - temp = f()
text - if temp > 0:
text -   return temp
text - 
text - In new Python we can write this code just as
text - 
text - if (temp:= f()) > 0: # compute f(), bind that object to temp, check temp > 0
text -   return temp        # if so, return temp, the object computed by f()
text - 
text - As we learn more Python features (e.g., comprehensions) we will find other
text - uses for the := operator. But, I am not a big fan of using this operator in
text - Python. The meaning of = in C++ is the meaning of := (now) in Python. So, you
text - should at least be aware of := even if you elect not to use it. You can read
text - all the details (and there are some strange ones) at 
text - 
text - https://www.python.org/dev/peps/pep-0572/
text - ------------------------------
text - 
text - ------------------------------------------------------------------------------
text - 
text - Statements vs Expressions
text - 
text - The basic unit of speech in English is a sentence: sentences are built from
text - noun and verb phrases. In Python the basic unit of code is a statement:
text - statements are built from expressions (like a boolean expression in an if
text - statement) and other statements (like block statements inside an if statement).
text - 
text - Statements are executed to cause an effect (e.g., binding/rebinding a name or
text - producing output). Expressions are evaluated to compute a result (e.g.,
text - computing some formula, numeric, string, boolean, list, etc.). For example, the
text - statement x = 1, when executed, causes a binding of the name x to an object
text - representing the integer 1. The expression x+1, when evaluated, computes the
text - object representing the integer 2. Typically we write expressions inside
text - statements: two examples are x = x+1 and print(x+1): both "do something" with
text - the computed value x+1 (the first rebinds x to it; the second prints it). The
text - distinction between statements and expressions is important in most programming
text - languages.  Learn the technical meaning of these terms and be able to recognize
text - statements and expressions in statements.
text - 
text - The print function is a bit strange. We call it only for an effect (printing a
text - sequence of characters in the Console window); but, like every function, it also
text - returns a value: when print is called successfully (doesn't raise any
text - exceptions) it returns the value None. We typically don't do anything with this
text - value. So, we write calls to print as if they were statements.
text - 
text - print(x)
text - 
text - which calls the print function (printing something in the Console window) and
text - does nothing with the None value print returns. What do you think calling the
text - print function in the following way prints in the console?: print(print(x))
text - 
text - Control structures (even simple sequential ones, like blocks) are considered
text - to be statements in Python. Python has rules describing how to execute
text - statements. Control structures might contain both statements and expressions.
text - The following is a conditional statement using if/else
text - 
text -   if x == None:
text -       y = 0
text -   else:
text -       y = 1
text - 
text - This if statement contains the expression x == None and the statements y = 0 and
text - y = 1. Technically, x, None, 0 and 1 are "trivial" expressions (which trivially
text - compute the object to which x is currently bound, and the object values None,
text - 0, and 1 which are literals: preconstructed objects "named" by None, 0, and 1).
text - 
text - The following statement includes a conditional expression that binds a value to
text - y: the conditional expression includes the expressions 0 (yes, an object by
text - itself, or just a name referring to an object, is a simple expression),
text - x == None, and 1.
text - 
text -   y = (0 if x == None else 1)
text - 
text - This single statement has the same meaning in Python as the if statement above.
text - We will discuss conditional statements vs. conditional expressions in more
text - detail later in this lecture note.
text - 
text - ------------------------------------------------------------------------------
text - 
text - None:
text - 
text - None is a value (object/instance) of NoneType; it is the ONLY VALUE of that
text - type. Sometimes we use None as a default value for a parameter's argument;
text - sometimes we use it as a return value of a function: in fact, a Python function
text - that terminates without executing a return statement automatically returns the
text - value None.
text - 
text - If None shows up as an unexpected result printed in your code or more likely in
text - a raised exception, look for some function whose return value you FORGOT to
text - specify explicitly (or whose return statement somehow didn't get executed
text - before Python executed the last statement in a function).
text - 
text - ------------------------------------------------------------------------------
text - 
text - pass:
text - 
text - pass is the simplest statement in Python; semantically, it means "do nothing".
text - Sometimes when we write statements, we aren't sure exactly which ones to write,
text - so we might use pass as a placeholder until we write the actual statement we
text - need. Often, in tiny examples, we use pass as the meaning of a function.
text - 
text -   def f(x) : pass     or     def f(x) :
text -       	     	      	         pass
text - 
text - If you are ever tempted to write the first if statement below, don't; instead
text - write the second one, which is simpler: they both produce equivalent results.
text - 
text -   if a in s:			# DON'T write this code
text -       pass
text -   else:
text -       print(a,'is not in',s)
text - 
text - 
text -   if a not in s:		# Write this code instead; it is equivalent
text -       print(a,'is not in',s)    # and simpler; strive to use Python simply
text - 
text - ------------------------------------------------------------------------------
text - 
text - Importing: 5 Forms
text - 
text - There are five import forms; you should know what each does, and start to think
text - about which is appropriate to use in any given situation. Note that in EBNF (a
text - notation used to describe programming languages, which we will discuss soon)
text - [...] means option and {...}  means repeat 0 or more times, although this
text - second form is sometimes written (...)\* when describing the syntax of Python.
text - 
text - Fundamentally, import statements bind names to objects (one or both of which
text - come from the imported module).
text - 
text - "import module-name" form: one or more modules, optionally renaming each
text -   1. import module-name{,module-name}
text -   2. import module-name [as alt-name] {,module-name [as alt-name]}
text - 
text - "from module-name import" form: one or mor attributes, optionally renaming each
text -   3. from module-name import attr-name{,attr-name}
text -   4. from module-name import attr-name [as alt-name] {,attr-name [as alt-name]}
text -   5. from module-name import \*
text - 
text - Above, alt-name is an alternative name to be bound to the imported object;
text - attr-name is an attribute name already defined in the namespace of the module
text - from which attr-name is imported.
text - 
text - The "import module-name" forms import the names of modules (not their attribute
text - names). (1) bind each module-name to the object representing that imported
text - module-name. (2) bind each alt-name to the object representing its preceding
text - imported module-name. Using a module name, we can refer to its attributes using
text - periods in qualified name, such as by module-name.attribute-name
text - 
text - The "from module-name import" forms don't import a module-name, but instead
text - import some/all attribute names defined/bound inside module-name. (3) bind each
text - attr-name to the object bound to that same attr-name in module-name. (4) bind
text - each alt-name to the object bound to the preceding attr-name in module-name.
text - (5) bind every name that is bound in module-name to the same object it is bound
text - to in module-name.
text - 
text - Import (like an assignment, and a def, and a class definition) defines a name
text - (if that name is not already in the namespace) and then binds (or rebinds) it
text - to an object: the "import module-name" form binds names to module objects; the
text - "from module-name import" form binds names to objects (instances, functions,
text - modules, classes, etc.) defined inside modules (so now two modules contain
text - names -maybe the same, maybe different, depending on which of form 3 or 4 is
text - used- bound to the same objects).
text - 
text - The key idea as to which kind of import to use is to make it easy to refer to a
text - name but not pollute the name space of the module doing the importing with too
text - many names (which might conflict with other names defined in that module).
text - 
text - If a lot of different names in the imported module are to be used, or we want
text - to make it clear when an attribute name in another module is used, use the
text - "import module-name" form (1) and then qualify the names when used: for example
text - 
text -   import random
text -   # use:  random.choice(...)  and  random.randint(...)
text - 
text - If the imported module-name is too large for using repeatedly, use an
text - abbreviation by importing using an alt-name (2) : for example
text - 
text -   import high\_precision\_math as hp\_math
text -   # use:  hp\_math.sqrt(...)
text - 
text - If only one name (or a few names) are to be used from a module, use the
text - form (3):
text - 
text -   from random import choice, randint
text -   # use: choice(...)  and  randint(...)
text - 
text - Again, use alt-name to simplify either form, if the name is too large and
text - unwieldy to use. Such names are often very long to be clear, but awkward to use
text - many times at that length. Generally we should apply the Goldilocks principle:
text - name lengths shouldn't be too short (devoid of meaning) or too long (awkward to
text - read/type) but their length should be "just right". Better to make them too
text - long, because there are ways (such as alt-name) to abbreviate them.
text - 
text - We almost never write the \* form of importing. It imports all the names defined
text - in module-name, and "pollutes" our namespace by defining all sorts of names we
text - may or may not use (and even worse, which might conflict and redefine names that
text - we have already defined). Better to explicitly import the names needed/used.
text - Eclipse marks with a warning any names that are imported but unused.
text - 
text - ------------------------------------------------------------------------------
text - 
text - Directly iterating over values in a list vs.
text -   Using a range to iterate over indexes of values in a list
text - 
text - We know that we can iterate (with a for loop) over ranges: e.g., if alist is a
text - list, we can write
text - 
text - alist = [5, 2, 3, 1, 4, 0]
text - for i in range(len(alist)):          # same as for i in range(0,len(alist)):
text -     print(alist[i])
text - 
text - Here i takes on the values 0, 1, ... , len(alist)-1 which is 5, but not
text - len(alist), which is 6. The code above prints all six values in alist:
text - alist[0], alist[1], .... alist[5].
text - 
text - Often we want to iterate over all the values in a list (alist) but don't need to
text - know/use their indexes at all: e.g., to print all the values in a list we can
text - use the loop
text - 
text - for v in alist:
text -     print(v)
text - 
text - which is much better (simpler/clearer/more efficient) than the loop
text - 
text - for i in range(len(alist)):
text -     print(alist[i])
text - 
text - although both produce the same result. Generally, choose to write the simplest
text - loop possible (the one with the fewest details) for all your code. Sometimes you
text - might write a loop correctly, but then realize that you can also write a
text - simpler loop correctly: change your code to the simpler loop. Sometimes (when
text - doing more complicated index processing) we must iterate over indexes. Always
text - try to use the simplest tool needed to get the job done.
text - 
text - In many cases where using range is appropriate, we want to go from the low to
text - high value INCLUSIVELY. I have written function named irange (for Inclusive
text - RANGE) that we can import from the goody module and use like range.
text - 
text - from goody import irange
text - for i in irange(1,10):
text -     print(i)
text - 
text - prints the values 1 through 10 inclusive; range(1,10) would print only 1
text - through 9. One goal for ICS-33 is to show you how to write alternatives to
text - built-in Python features; we will study how irange is written later in the
text - quarter, but you can import and use it now (and examine its definition in the
text - goody module).
text - 
text - I have also written frange in goody, which allows iteration over
text - floating point (not int) values.
text - 
text - from goody import frange
text - for x in frange(0., 1., .5):
text -     print(x)
text - 
text - prints (iterating from 0 to 1 in steps of .5
text - 
text - 0.0
text - 0.5
text - 1.0
text - 
text - There are some interesting numerical issues to think about when we use floating
text - point numbers:
text - 
text - from goody import frange
text - for x in frange(0., 1., .2):
text -     print(x)
text - 
text - which prints
text - 
text - 0.0
text - 0.2
text - 0.4
text - 0.6000000000000001
text - 0.8
text - 1.0
text - 
text - Python tries to print floating point values simply, but sometimes it cannot
text - exactly represent (in binary) a value, so it prints a value that is a tiny bit
text - lower or higher. Don't be confused by such numbers. Generally, when we perform
text - computations we won't worry about such small errors. The field of Numerical
text - Analysis studies all sorts of issues related to representing numbers and
text - doing computations on them that accumulate the least amount of error.
text - 
text - ------------------------------------------------------------------------------
text - 
text - Arguments and Parameters (and Binding): Terminology (much more details later)
text - 
text - Whenever we DEFINE a function (and define methods in classes), we specify the
text - names of its PARAMETERS in its header (in parentheses, separated by commas).
text - Whenever we CALL a function we specify the values of its ARGUMENTS (also in
text - parentheses, separated by commas). The definition below
text - 
added - ```python
code-start - def f(x,y):
code -     return x**y
code-end - 
added - ```
text - defines a function of two parameters: x and y. When we define a function we
text - (re)bind the function's name to the function object. This is similar to what
text - happens when we write x = 1 which (re)binds a name to a data object: here, an
text - instance of the int class; if we later write x = 2 we rebind x to a different
text - object; in fact, if we wrote x = f then x would be rebound to the same function
text - object that f is bound to!
text - 
text - Calling  f(5,2\*6) calls this function with two arguments: the arguments 5 and
text - 2\*6 are evaluated (producing objects) and the values/objects computed from
text - these arguments (5 and 12) are bound to their matching parameters in the
text - function header (and then the body of the function is executed, which can refer
text - to the parameter names). Therefore, function calls happen in three steps (this
text - rule is an example of "Python has rules about how things work"): 
text -   (1) evaluate the arguments
text -   (2) bind the argument values (objects) to the parameter names
text -   (3) execute the body of the function, using the values bound to the parameter
text -       names; so parameters are names inside the name-space of the function,
text -       along with any local variables the function defines.
text - 
text - We will discuss the details of argument/parameter binding in much more detail
text - later in these lecture notes, but in a simple example like this one, parameter/
text - argument binding occurs sequential, binding the first evaluated argument to the
text - first parameter, the second evaluated argument to the second parameter (etc.).
text - It is like writing: x = 5 and then y = 2\*6 which binds the parameter x to the
text - object 5 and then the parameter y to the object 12.
text - 
text - Sometimes we can use the parameter defined in one function as an argument when
text - calling another function inside its body. If we define
text - 
added - ```python
code-start - def factorial_sum(a,b):
code -     return factorial(a) + factorial(b)
code-end - 
added - ```
text - Here the parameters a and b of factorial\_sum function are used as arguments in
text - the two calls in its body to the factorial function.
text - 
text - Parameters are always names. Arguments are always expressions that evaluate to
text - objects. Very simple expressions include literals (such as 1 and 'abc') and
text - names (bound to values, such as a and b in the calls to factorial above).
text - More complicated expressions involve literal, names, and operators/function
text - calls.
text - 
text - It is important that you understand the distinction between the technical term
text - PARAMETER and ARGUMENT, and that calling a function first BINDS each argument
text - value to its associated parameter name in the function's  HEADER (we will
text - discuss the exact rules later in this lecture note) and then EXECUTES the BODY
text - of the function. Be familiar with these related technical terms.
text - 
text - ------------------------------------------------------------------------------
text - 
text - Function calls ... always include ()
text -   how we can pass a function as an argument to another function by not using ()
text - 
text - Any time a reference to an object is followed by (...) it means to perform a
text - function call on that object. Some objects will raise an exception if they do
text - not support function calls: writing 3() or 'abc'() will both raise exceptions.
text - Calling 'abc'() raises the TypeError exception, with the description: 'str'
text - object is not callable.
text - 
text - While this rule is simple to understand for the functions that you have been
text - writing and calling, there are some much more interesting ramifications of this
text - simple rule. Run the following code to define these three functions.
text - 
added - ```python
code-start - def double(x):
code -     return 2*x
code-end - 
added - ```
added - ```python
code-start - def triple(x):
code -     return 3*x
code-end - 
added - ```
added - ```python
code-start - def times10(x):
code -     return 10*x
code-end - 
added - ```
text - Note that each def defines the name of a function and binds that name to the
text - function object that follows it.
text - 
text - If we then wrote
text - 
text - f = double
text - 
text - then f would become a defined name that is bound to the same (function) object
text - that the name double is bound to (just like writing y = x if x were bound to an
text - integer). The expression "f is double" would evaluate to True, because these
text - two names are bound to the same function object.
text - 
text - Note that there are no () in the code above, so there is NO FUNCTION CALL here:
text - we are just binding names to objects (that happen to be function objects). If
text - we instead wrote f = double(3), then Python would bind (a) evaluate the argument
text - 3, (b) bind it to the parameter x in the function object bound to double, (c)
text - execute the body of the function object bound to double, which computes and
text - returns the value 6. The net result is that the name f is bound to the int
text - object 6.
text - 
text - Given the assignment f = double above, 
text - 
text - print( f(5) )
text - 
text - Python would print 10, just as it would if we wrote
text - 
text -  print( double(5) )
text - 
text - because f and double refer to the same function object, and it makes no
text - difference whether we call that function object using the name f or double. The
text - function call does not occur until we use (). Of course the () in print(...)
text - means that the print function is also called: print's argument is the result
text - returned by calling f(5). So the two sets of () in print( f(5) ) means that
text - Python calls two functions while executing the print statement.
text - 
text - Here is a more interesting example, but using exactly the same idea.
text - 
text - for f in [double, triple, times10]:
text -     print( f(5) )
text - 
text - Here f is a variable that iterates over (is successively bound to) each value
text - in a list (we could also have used a tuple to store these three functions):
text - each value in the list is a reference to a function object (the objects referred
text - to by the names double, triple, and times10). The code in the loop's body prints
text - the values computed by calling each of these function objects with the argument
text - 5. Note that these functions are NOT called when creating the list (there are no
text - parentheses): the list is just built to contain references to these three
text - function objects: again, when their names are not followed by () there is no
text - function call. The function is called in the print statement that is in the
text - body of the loop: called 3 times. So, this code prints 10, 15, and 50.
text - 
text - See the "Creating;/Using a List of Functions" link on the Weekly Schedule to see
text - a picture of how this code works.
text - 
text - Using the same logic, we could also write a dictionary whose keys are strings
text - and whose associate values are function objects; then, we can use a string as a
text - key needed to retrieve and call its associated function object.
text - 
text - fs = {'x2' : double, 'x3' : triple, 'x10' : times10}
text - print( fs['x3'](5) )
text - 
text - Here fs is a dictionary that stores keys that are strings and associates each
text - with a function object: there are no calls to functions when building the
text - dictionary for fs in the first statement - no (); we then can retrieve the
text - function associated with any key (here the key 'x3') and then call the resulting
text - function object (here fs['x3']) with the argument 5 (here fs['x3'](5)): in the
text - second statement we use the () to call the function selected from the
text - dictionary.
text - 
text - We can also pass (uncalled) functions (objects) as arguments to other functions.
text - 
added - ```python
code-start - # p is a predicate: a 1-argument function returning a bool (True or False)
code - # alist is a list of values on which p can be/is called
code - # count_true returns the number of values in alist for which p returns True
code - def count_true(p, alist): 
code -     count = 0
code -     for x in alist:
code -         if p(x):
code -             count += 1
code-end - 
added - ```
text -     return count
text - 
text - if we wrote the following import statement (predicate is a module in the ICS-33
text - course library)
text - 
text - from predicate import is\_prime
text - 
text - it imports the is\_prime function from the predicates module: a PREDICATE is
text - a function of one argument that returns a boolean value. Then calling
text - 
text -   count\_true(is\_prime, [2,3,4,5,6,7,8,9,10])
text - 
text - returns 4: only the numbers 2, 3, 5, and 7, are prime. 
text - 
text - Note that the parameter p is bound to the function object that is\_prime is
text - bound to (is\_prime is not called when it is specified as an argument), and p
text - is called many times inside the count\_true function: one for each value in the
text - list. We call count\_true a "functional" because it is a function that is passed
text - a function as an argument.
text - 
text - In summary
text - 
added - ```python
code-start - def f(x):
code -     return 2*x
code-end - 
added - ```
text - is really just a special syntax for creating a name f and binding it to a new
text - function object (not using an = sign, which we normally use for defining names).
text - 
text - Note the difference between the following, which both print 6. Both g and f
text - refer to the same function object (by the "is" operator, f is g is True). The
text - call g(3) is calling the same function object as the call f(3).
text - 
text - x = f(3)
text - print(x)
text - 
text - and 
text - 
text - g = f
text - print(g(3))
text - 
text - A large part of this course deals with understanding functions better, including
text - but not restricted to function calls: the main thing -but not the only thing-
text - one does with functions; the other useful things are passing functions as
text - arguments to other functions (as in count\_true) and returning functions from
text - other functions (discussed soon).
text - 
text - So, functions are objects just like integers are objects. Both can have one or
text - more names bound to them. In fact, is some branches of mathematics 3 is
text - considered just the name of a parameterless function: we can call 3() to get
text - its value (although this does not work in Python). 
text - 
text - In fact, there are 7 categories of things we can CALL in Python. We will have
text - discussed all 7 "callables" by the end of ICS-33; Here is a list for reference.
text -   1) Built-in functions: e.g., len('abc')
text -   e) User-defined (by def/lambda) functions
text -   3) Built-in methods: e.g., 'abc'.uppercase()
text -   4) User-defined methods (in user defined classes)
text -   5) Classes: C() invokes \_\_new\_\_ and then \_\_init\_\_ in user-defined class C
text -   6) Class instances (any object): via \_\_call\_\_
text -   7) Generator functions: return generator objects/iterators to call next on
text - 
text - ------------------------------------------------------------------------------
text - 
text - Scope: Global and Local Names (generalized in the next section)
text - 
text - The topic of SCOPE concerns the visibility of variable names: when a name is
text - written in Python code, to what definition of that name does it refer? In this
text - section, we discuss whether naming a variable in a simple context refers to a
text - name defined in the GLOBAL scope or LOCAL scope; in the next section, we will
text - generalize this principle in a powerful way, allowing us to write functions
text - that return other functions.
text - 
text - Names defined in a module are global definitions; names defined in a function
text - (including its parameters) are local definitions. In a module, we can refer to
text - global names, but not any local names inside the functions that are defined
text - there. In functions we can refer both to their local names or to global names.
text - Of particular interest is what happens when a name is defined both globally and
text - locally.
text - 
text - Here are Python's rules (not all the rules yet, but the ones we focus on
text - now, and correct/expand later), which we will discuss and illustrate below.
text - 
text -   1) A name used (to lookup or bind) in a module is global
text - 
text -   2) A name used (to lookup or bind) in a function is global if 
text -       a) that name is explicitly declared global (e.g., global x) PRIOR to its
text -            use in the function, OR
text -       b) that name is only looked-up (and never bound) in the function
text - 
text -   3) A name used (to lookup or bind) in a function is local, otherwise
text -        (e.g., bound in a function AND not declared global before its use;
text -         also, all parameters are considered to be local)
text - 
text - As a result of these rules, all uses of a name in a given function must be the
text - same: all uses are global or all uses are local.
text - 
text - Let's start by looking at Example 1: 
text - 
text - x = 1
text - print(x)
text - 
text - which prints 1. Here the module defines a global name x (by rule 1) and binds
text - it to the object 1 and prints the value bound to that global name.
text - 
text - Now let's look at Example 2: 
text - 
added - ```python
code-start - x = 1
code - def f():
code -     print(x)
code-end - 
added - ```
text - f()
text - print(x)
text - 
text - which prints 1 and then 1. Here the module defines a global name x (by rule 1)
text - and binds it to the object 1; then it defines a global name f (by rule 1) and
text - binds it to a function object: note the function uses the name x, which is
text - global (by rule 2b). When we call f(), it prints the value bound to the global
text - name x. Then, returning to the module, it prints the value bound to the global
text - name x (again, by rule 1).
text - 
text - In this example, all the uses (there is just one) of x inside the function is
text - global.
text - 
text - Now let's look at Example 3: 
text - 
added - ```python
code-start - x = 1
code - def f():
code -     x = 2
code -     print(x)
code-end - 
added - ```
text - f()
text - print(x)
text - 
text - which prints 2 and then 1. Here the module defines a global name x (by rule 1)
text - and binds it to the object 1; then it defines a global name f (by rule 1) and
text - binds it to a function object: note the function uses the name x twice, which
text - is local (by rule 3: it is bound in the function and is not declared global in
text - the function). When we call f(), it binds the local name x to 2 and then
text - prints the value bound to the local name x. Then, returning to the module, it
text - prints the value bound to the global name x (by rule 1), which is still 1.
text - 
text - What is the primary difference between this example and the preceding one? Here,
text - both uses of the name x in this function are local; in the preceding one, the
text - use of the name x in this function is global.
text - 
text - Now let's look at Example 4: 
text - 
added - ```python
code-start - x = 1
code - def f():
code -     y = 2
code -     print(x,y)
code-end - 
added - ```
text - f()
text - print(x)
text - 
text - which prints 1 2 and then 1. Here the module defines a global name x (by rule 1)
text - and binds it to the object 1; then it defines a global name f (by rule 1) and
text - binds it to a function object: note the function uses the names x and y; x is
text - global (by rule 2b) and y is local (by rule 3). When we call f(), it binds the
text - local name y to 2 and then prints the values bound to the global name x and the
text - local name y. Then, returning to the module, it prints the value bound to the
text - global name x (by rule 1).
text - 
text - Note that if we replaced print(x) at the end of the module with print(x,y)
text - Python would raise a NameError exception because there is no global name y;
text - y is a local name defined only in the function f, and usable only there.
text - 
text - Something interesting happens by the rules above, if we try to get function f
text - to not only print the original value of the global name x, but also try to
text - rebind that global name to 2. Let's see why this fails to be the effect of the
text - code below (later we will see how to accomplish this task). In fact, it fails in
text - an interesting way.
text - 
text - Now let's look at Example 5: 
text - 
added - ```python
code-start - x = 1			# wrong code to print a global and then change it
code - def f():		# wrong code to print a global and then change it
code -     y = 2		# wrong code to print a global and then change it
code -     print(x,y)		# wrong code to print a global and then change it
code -     x = 2		# wrong code to print a global and then change it
code -       			# wrong coed to print a global and then change it
code - f()   			# wrong code to print a global and then change it
code - print(x)		# wrong code to print a global and then change it
code-end - 
added - ```
text - Here, the module defines a global name x (by rule 1) and binds it to the object
text - 1; then it defines a global name f (by rule 1) and binds it to a function
text - object: note the function uses the names x and y; x is local (by rule 3; it is
text - bound in the function and not declared global) and y is local (also by rule 3,
text - ditto). When we call f(), it binds the local name y to 2 and then tries to
text - print the values bound to the local names x and y; but here there is no binding
text - (yet) for x -even though Python knows that x must be local- so Python raises the
text - UnboundLocalError exception: local variable 'x' referenced before assignment.
text - 
text - So, it does NOT look-up x as a global in the print and then define x as a local!
text - The name x is either always global or always local in a function. Here, by rule
text - 3 it is always local.
text - 
text - ---------------
text - If we put the x = 2 before the print, it doesn't raise an exception, but doesn't
text - rebind the global name x either.
text - 
text - Now let's look at Example 6: 
text - 
added - ```python
code-start - x = 1			# wrong code
code - def f():		# wrong code
code -     y = 2		# wrong code
code -     x = 2		# wrong code
code -     print(x,y)		# wrong code
code -     			# wrong code
code - f()   			# wrong code
code - print(x)		# wrong code
code-end - 
added - ```
text - Python would print 2 2 and then 1. Here, the module defines a global name x
text - (by rule 1) and binds it to the object 1; then it defines a global name f (by
text - rule 1) and binds it to a function object: note the function uses the names x
text - and y; x is local (by rule 3; it is bound in the function and not declared
text - global) and y is local (also by rule 3, ditto). When we call f(), it binds the
text - local name y to 2 and binds the local name x to 2. Then it print the values
text - bound to the local names x and y. Then, returning to the module, it prints the
text - value bound to the global name x (which has remained bound to 1).
text - ---------------
text - 
text - We can change the value in the global name x by using a "global" declaration
text - before x is used inside f. This declaration means that x should always be
text - treated as a global name inside this function (whether it is examined or
text - bound to an object, or both) by rule 2.
text - 
text - Now let's look at Example 7: 
text - 
added - ```python
code-start - x = 1
code - def f():
code -     global x
code -     y = 2
code -     print(x,y)
code -     x = 2
code-end - 
added - ```
text - f()
text - print(x)
text - 
text - Python would print 1 2 and then print 2 (thus the global name x has changed
text - inside function f).
text - 
text - Here, the module defines a global name x (by rule 1) and binds it to the object
text - 1; then it defines a global name f (by rule 1) and binds it to a function
text - object: note the function uses the names x and y; x is global (by rule 2a; it is
text - declared global PRIOR to its use in the function) and y is local (by rule 3, it
text - is bound in the function but not declared global). When we call f(), it binds
text - the local name y to 2 and then prints the values bound to the global name x and
text - the local name y. Then it rebinds the global name x to 2. Finally, returning to
text - the module, it prints the value bound to the global name x (which has now been
text - rebound to 2).
text - 
text - So, if a function must (re)bind a global name, the function must explicitly
text - declare that name global PRIOR to its use in the function.
text - 
text - Note that if we wrote "global x" after print(x,y) in f, this x would be
text - considered a local name (rule 2a does not apply), so Python would instead raise
text - a SyntaxError: name 'x' is used prior to global declaration.
text - 
text - To repeat, when Python defines a function object, it first scans the whole
text - function to determine whether each name refers to a global name or a local
text - name, looking for global declarations and bindings to make this determination,
text - using the rules stated above. The rule for 2a hinges on the word PRIOR. So if
text - is declared global subsequent to its first use, it is considered to be in error.
text - 
text - In summary, we can always lookup (find the value of, evaluate) a global name
text - inside a function to find its value without doing anything special, but if we
text - want to (re)bind the global name to a new value inside the function, we must
text - declare the name global somewhere in the function (before its first use). If we
text - forget to declare the name global and (re)bind the name, Python will instead
text - define it as a local name (rule 3) and all occurrences of that name in the
text - function will use it as a local name (so it better be defined first too).
text - 
text - What do you think the following code will do? Notice that is similar to the code
text - above, but it does not define a global name x before defining f. Can you explain
text - why (in terms of the rules) it does what it does?
text - 
text - Now let's look at Example 8: 
text - 
added - ```python
code-start - def f():
code -     global x
code -     x = 2
code -     y = 2
code -     print(x,y)
code-end - 
added - ```
text - f()
text - print(x)
text - 
text - Also can you explain what would happen if the print statement print(x,y) were
text - moved between the statements y = 2 and x = 2, or moved before both?
text - 
text - Bottom Line: You should understand how Python decides whether names in a
text - function are global or local. TYPICALLY, no global names should be used: all
text - information that a function uses should be passed into the function and bound to
text - one of its parameters. BUT if a function must (re)bind a global name either
text - 
text -   (1) the name should be declared global (so it can be examined/changed), or 
text - 
text -   (2) the function should be called like global-name = f(...) so that the
text -       global name is not declared/changed as a global inside the function, but 
text -       is instead changed as a result of an assignment statement made after
text -       calling the function. We can even use this mechanism for rebinding
text -       multiple global names: gn1, gn2, ..., gnN = f(...) where f returns an
text -       N-tuple or N-list (see the Parallel/Tuple/List Assignment -aka sequence
text -       unpacking- section below).
text - 
added - ```python
code-start - Problem 1: Explain why we cannot write a general function in Python that
code - exchanges the references in its argument names. That is, we cannot write
code - def f(x,y):
code -   ...
code - such that
code - a = 1
code - b = 2
code - f(a,b)
code - print(a,b) # prints 2 and 1
code-end - 
added - ```
text - Such a function cannot be written in Java either (but can be written in C/C++).
text - 
text - Problem 2: Write a SPECIFIC function in Python that exchanges the references
text - ONLY FOR THE NAMES a AND b. Hint: use global. That is,
text - 
added - ```python
code-start - def f():
code -   ...
code - such that
code - a = 1
code - b = 2
code - f()
code - print(a,b) # prints 2, and 1
code-end - 
added - ```
text - Problem 3: Write a GENERAL function in Python that we can CALL IN A SPECIAL WAY
text - to exchange the references of its two argument names. Hint: read the paragraph
text - above about multiple return values.
text - 
added - ```python
code-start - def f(x,y):
code -   ...
code - a = 1
code - b = 2
code - # some Python statement, calling f on a and b such that
code - print(a,b) # prints 2, and 1
code-end - 
added - ```
text - These local/global rules are generalized in the next section to include 4
text - different locations/scopes: LEGB
text -   1) functions (Local scope -as seen above)
text -   2) enclosing functions (Enclosing scope)
text -   3) modules (Global scope -as seen above)
text -   4) the special builtins module (Builtins scope)
text - 
text - ------------------------------------------------------------------------------
text - 
text - Generalizing Scope: Local, Enclosing, Global, and Builtins
text - (application: a function call that return a reference to a function object
text -  that refers to information in the original function call)
text - 
text - Look at the following functions. They all define a local function (and define
text - parameters/local variables too) and then return, as their value, that local
text - function object. This feature in Python (and the feature of passing function
text - objects as arguments to parameters; see the count\_true function defined earlier)
text - is very powerful and useful. In this section, we will generalize the scope
text - rules discussed above and learn how these new rules apply when functions defined
text - and returned in functions are called.
text - 
added - ```python
code-start - def bigger_than(age):
code -     def test_it(x):
code -         return x > age
code-end - 
added - ```
text -     return test\_it
text - 
text - 
added - ```python
code-start - def running_average1():
code -     data = []              # Mutated, but not rebound, in include_it
code-end - 
added - ```
text -     def include\_it(x):
text -         data.append(x)     #   mutate data's list; rebinding is not needed/used
text -         return sum(data)/len(data)
text - 
text -     return include\_it
text - 
text - 
added - ```python
code-start - def running_average2():
code -     sum   = 0              # rebound in include_it
code -     count = 0              # rebound in include_it
code-end - 
added - ```
text -     def include\_it(x):
text -         nonlocal sum,count # allow rebinding of locals in outer function's scope
text -         sum += x           # rebind sum
text -         count+= 1          # rebind count
text -         return sum/count
text - 
text -     return include\_it
text - 
text - 
text - Note that the body of these functions, and the bodies of their inner functions
text - (test\_it and include\_it) can also use global names (as discussed in the last
text - section; if they needed to rebind global names we must declare such names
text - global); but none of these do, which follows good practice for defining any
text - functions.
text - 
text - We will now write the complete rules Python uses to look up and bind names.
text - These rules generalize what we learned in the previous section: we will now
text - define any name in a function as either local, nonlocal/enclosing, global, or
text - LEGB; then we will learn how such names are looked-up and bound. Here are the
text - rules from the previous sections, restated and extended using these new terms.
text - We separate rules into how names are classified and how they are
text - looked-up/bound.
text - 
text -   1) A name, when DEFINED in a module, is global
text - 
text -   2) A name used in a function is global if that name is explicitly declared
text -       global (e.g., global x) PRIOR to its use in the function
text - 
text -   3) A name used in a function is nonlocal/enclosing if that name is explicitly
text -       declared nonlocal (e.g., nonlocal x) prior to its use in that function
text - 
text -   4) A name used in a function is local if
text -        a) that name is a parameter in that function, or
text -        b) that name is bound anywhere in that function (and is neither declared
text -           global nor nonlocal)
text - 
text -   5) Otherwise, a name is LEGB (if it is none of the above)
text -      NOTE: an LEGB name can NEVER be bound in a function (if it is bound, it
text -        must be declared global, or nonlocal, or will be local by 4a or 4b). 
text -      So LEGB names are ONLY LOOKED-UP.
text - 
text - If we import a module name (call it M), then we can lookup/rebind any global
text - name defined in that module (call it n) by writing M.n: e.g., print(M.n) or
text - M.n = ...
text - 
text - Here are the corresponding rules for looking-up/binding all these names.
text - 
text -   A+B) When a name is global,
text -        1) Python LOOKS UP that name, in order, 
text -              a) in the Global scope; and if not found
text -              b) in the Builtins scope (in the builtins module); and if not found
text -              c) raises NameError
text -        2) Python BINDS that name in the Global scope
text -      
text -   C) When a name is nonlocal/enclosing, Python looks for the name in the scope
text -      of the function enclosing it (if there is one); if it doesn't find it
text -      there, it looks in the scope of the function enclosing the enclosing
text -      function (if there is one). This process continues outward until it finds
text -      the name and then uses the name in that scope. If it cannot find the name
text -      and reaches the global scope, Python raises a SyntaxError exception: no
text -      binding for nonlocal ... found
text - 
text -   D) When a name is local, Python looks-up/binds that name in the local scope.
text - 
text -   E) When a name is LEGB, Python looks for the name, in order,
text -        1) in the        Local scope of the function; and if not found 
text -        2) in any of the Enclosing scopes (see rules in C); and if not found
text -        3) in the        Global scope; and if not found
text -        4) in the        Builtins scope(in the builtins module); and if not found
text -        5) raises NameError
text - 
text - For example, the code
text - 
added - ```python
code-start - x = 1
code - def f(y):
code -     print(x,y)
code-end - 
added - ```
text - f(2)
text - print(x)
text - 
text - prints 1 2 and then 1. Here the module defines a global name x (by rule 1)
text - and binds it to the object 1; then it defines a global name f (by rule 1) and
text - binds it to a function object: note the function uses the names print, x and y
text - (yes, now we even look at where the name print comes from); print is LEGB (by
text - rule 5), x is LEGB (by rule 5) and y is local (by rule 4a). After the function
text - definition the name f is LEGB (by rule 5) and the the name print is LEGB (by
text - rule 5) and the name x is LEGB (by rule 5).
text - 
text - When we call f(2),
text - 
text - Python looks up the LEGB name f and finds it in the global scope where it is
text - declared and calls that function, first binding its local name y to the object
text - 2; then it executes the body of the function. It looks up the object associated
text - with the LEGB name print (which it finds not in the local scope, not in an
text - enclosing scope -there is none-, not in the global scope, but finally in the
text - builtins scope): the print function is defined in the builtins module. It then
text - calls the print function after looking up the binding of the LEGB name x (which
text - it finds not in the local scope, not in an enclosing scope -there is none-, but
text - finally in the global scope, with a binding 1) and the local name y (it finds
text - its binding 2, in the local scope). After printing 1 2 the function terminates.
text - 
text - Now, Python looks up the object associated with the LEGB name print (which it
text - finds not in the local scope -there is none-, not in the enclosing scope -there
text - is none-, not in the global scope, but finally in the builtins scope). It then
text - looks up the LEGB name x (which it finds not in the local scope -there is none-,
text - not in the enclosing scope -there is none-, but in the global scope, with a
text - binding of 1). It then calls the print function, printing the value 1. The
text - module then terminates.
text - 
text - \*\*\*\*\*\*
text - This is an incredibly detailed explanation of what happens, but it is an
text - accurate description. We will do many more examples in class, but it is up to
text - you to study these and be able to know/describe/hand-simulate exactly what
text - happens. Eventually it becomes 2nd nature, except where strange behavior occur.
text - \*\*\*\*\*\*
text - 
text - -----Back to understand a function defined in a function
text - 
text - Now let's return to the three concrete examples of a function defined inside a
text - function. None of these functions use global or builtin names, so all names are
text - defined locally or nonlocally (in an enclosing scope, found by the LEGB rule).
text - 
text - The first is function is
text - 
added - ```python
code-start - def bigger_than(age):
code -     def test_it(x):
code -         return x > age
code-end - 
added - ```
text -     return test\_it
text - 
text - Note that inside the function bigger\_than, age is a local name (this name is
text - used only inside test\_it). Inside the function test\_it, x is a local name (by
text - rule 4a) and age is an LEGB name (by rule 5); this is the only place age is
text - used.
text - 
text - Lets look at the following sequence of statements
text - 
text - old = bigger\_than(60)
text - print(old(65))
text - 
text - Python executes the first statement by calling bigger\_than and then binding the
text - object that it returns to the variable name old. When calling bigger\_than, 
text - Python first binds the argument 60 to the parameter age, and then executes its
text - body, whose two statements
text - 
text -   (1) define test\_it to refer to a NEW function object (new each time
text -       bigger\_than is called and test\_it is defined). That function object,
text -       being enclosed in bigger\_than, stores a CLOSURE consisting of all the
text -       local variables and their bindings in bigger\_than: here just age bound to
text -       60. A function stores a closure only if it is defined inside an enclosing
text -       function, so global functions store no closures. We can think of a
text -       closure as just a dictionary storing variable names from the enclosing
text -       scope and their current bindings.
text - 
text -   (2) return a reference to the function object bound to the local variable
text -       test\_it. In the statement above, the global name old is bound to this
text -       returned function object.
text - 
text - When old(65) is called, Python first binds the argument 65 to the parameter x
text - (in the function object defined by test\_it, returned by bigger\_than) and then
text - executes its body (the body of function object defined by test\_it, returned by
text - bigger\_than). It is just one return statement, which compares the local name x
text - to the LEGB name age; it looks-up the value for age inside the closure of old's
text - function object (where age is bound to 60). Since 65>60 valuates to True,
text - old(65) returns True, which is printed.
text - 
text - See a link to the picture of a Function returning a function in the lectures
text - and/or weekly schedule on the course website.
text - 
text - If we wrote the following
text - 
text - old     = bigger\_than(60)
text - ancient = bigger\_than(90)
text - print (old(10), old(70), old(95), ancient(70), ancient(95))
text - 
text - Python prints: False True True False True
text - 
text - Note than old and ancient return different results for the argument 70, because
text - old compares this argument to 60 and ancient compares this argument to 90.
text - 
text - Each assignment statement binds its name (old and ancient) to a different
text - function object that is defined/returned by different calls to the bigger\_than
text - function. Each of these function objects has its own closure: age is bound to
text - 60 in old's function object's closure; age is bound to 90 in ancient's function
text - object's closure. So the LEGB name age in each different function will get its
text - value from a different closure, so the functions can return different results.
text - 
text - In fact, we even could have avoided the local variable old and written
text - 
text - print ( bigger\_than(60)(70) )
text - 
text - We know that bigger\_than(60) calls bigger\_than with the argument 60, which
text - returns a result that is a reference to the test\_it function it defined and
text - returned, whose closure age is bound to 60; so by writing (70) after that, we
text - are calling the returned function object (the one bigger\_than(60) returned).
text - When calling this returned function object using the argument 70, it returns
text - True.
text - 
text - Now, let's examine another example. In the function 
text - 
added - ```python
code-start - def running_average1():
code -     data = []              # data is mutated, not rebound, in include_it
code-end - 
added - ```
text -     def include\_it(x):
text -         data.append(x)     # mutate data's list in enclosing scope
text -         return sum(data)/len(data)
text - 
text -     return include\_it
text - 
text - the include\_it function refers to data, which is an LEGB name, multiple times.
text - Note that it is an LEGB name in include\_it, because it is not bound in that
text - function. So, each time running\_average1 is called, it defines and returns a
text - new function object bound to include\_it, whose closure will contain the name
text - data bound to an empty list. Each time we call it, it will mutate data by
text - appending another value to it, and then computing/returning the average of all
text - the values it has been called with (all the values now in the list). For example
text - 
text - mileage\_per\_tank = running\_average1()  # list is now []; include\_it returned
text - print(mileage\_per\_tank(300))           # list is now [300]; 300 returned
text - print(mileage\_per\_tank(200))           # list is now [300,200]; 250 returned
text - print(mileage\_per\_tank(400))	       # list is now [300,200,400]; 300 returned
text - 
text - prints 300 (300/1) followed by 250 (300+200)/2 followed by 300 (300+200+400)/3
text - 
text - So, using functions returning functions, a function can remember information
text - from its previous calls! include\_it stores such information in its closure, in
text - data defined in the scope for running\_average, its enclosing function (why the
text - name "closure" is meaningful). Note that data in include\_it is found by the
text - LEGB rule: data is never rebound, but its list is mutated when append is called.
text - 
text - Finally, in the function
text - 
added - ```python
code-start - def running_average2():
code -     sum   = 0              # sum   is rebound, not mutated, in include_it
code -     count = 0              # count is rebound, not mutated, in include_it
code-end - 
added - ```
text -     def include\_it(x):
text -         nonlocal sum,count # sum/count are nonlocal; can rebind in enclosing scope
text -         sum   += x         # rebind sum   in enclosing scope
text -         count += 1         # rebind count in enclosing scope
text -         return sum/count
text - 
text -     return include\_it
text - 
text - the include\_it function refers to sum and count, which because they are REBOUND
text - inside include\_it (using +=) would normally be local names. But as local names
text - they wouldn't be initialized and that would cause an error. Instead, we want the
text - include\_it function to use the sum and count initialized in running\_average2, so
text - we explicitly declare them nonlocal, so their values will be LOOKED-UP and
text - REBOUND in the closure for the running\_average2 function call, in which the
text - include\_it function was defined and returned.
text - 
text - So, each time running\_average2 is called, it defines and returns a new function
text - object bound to include\_it, whose closure will contain the names sum and count
text - bound to 0. Each time we call it, it will rebind sum and count by incrementing
text - them, and returning the average of all the values it has been called with.
text - This function has the same behavior as running\_average1 by storing just two
text - integers, without having to remember a potentially long list of values. But
text - doing so requires us to use nonlocal variables.
text - 
text - The bottom line here is that there are complicated rules that govern the scope
text - of variable names. But they are often used in standard ways, to get powerful 
text - behavior from simple Python code.
text - 
text - Problem: Predict what the following prints. Drawing a diagram might help. Then,
text - copy/paste the following code into a Python module in Eclipse and execute it
text - to see what Python says. If you got the answer wrong, how can you debug your
text - understanding to get the right answer.
text - 
added - ```python
code-start - def f():
code -     prev = None
code -     
code -     def g1(x):
code -         nonlocal prev
code -         temp = prev
code -         prev = x+5
code -         return temp
code -     def g2(x):
code -         nonlocal prev
code -         temp = prev
code -         prev = 5*x
code -         return temp
code -     return g1,g2
code-end - 
added - ```
text - f1,f2 = f()
text - f3,f4 = f()
text - print(f1(1))
text - print(f2(2))   
text - print(f3(5))
text - print(f4(6))
text - 
text - Problem: What would the difference be if we defined a global variable prev = 100
text - and change the nonlocal declaration in g2 to be a global declaration?
text - 
text - ------------------------------------------------------------------------------
text - 
text - Functions vs Methods: The Fundamental Equation of Object-Oriented Programming
text -   ...and the concept of a bound function
text - 
text - Functions are typically called f(...) while methods are called on objects like
text - o.m(...). For example, the following statements
text - 
text - x = 'candide'
text - print( x.replace('d','p') )
text - 
text - prints the string 'canpipe'.
text - 
text - In reality, a method call is just a special syntax to write a function call. The
text - special "argument" o prefixes the method name; in functions, all arguments must
text - appear inside the parentheses. Methods and functions are related by what
text - I call "The Fundamental Equation of Object-Oriented Programming".
text - 
text -   o.m(...)     =     type(o).m(o,...)
text - 
text - On the right side
text -   1) type(o) returns a reference to the class object o was constructed from
text -   2) .m means call the method m declared inside that class: look for
text -        def m(self,...): .... in the class
text -   3) pass the prefix o as the first argument to m (matching the parameter self):
text -        recall, that when defining methods in classes we write def m(self, ....):
text -        where does the argument matching the  self parameter come from? It comes
text -        from the object o, the prefix in calls like  o.m(...)
text - 
text - So, calling the method 'candide'.replace('d','p') is exactly the same as calling
text - the function str.replace('candide','d','p'), because type('candide') returns a
text - reference to the str class, which defines many string methods, including 
text - replace: we can call it as a method or function. Python knows how to call each.
text - 
text - How well do you understand the self parameter in a method (or your-self, for
text - that matter:)? This equation is the key. I believe a deep understanding of this
text - equation is the key to clarifying many aspects of object-oriented programming
text - in Python (whose objects are constructed from classes). Just my two cents. But
text - we will often return to this equation throughout this course. I've never seen
text - any books that talk about this equation explicitly. We will revisit FEOOP when
text - we spend a week discussing how to write more sophisticated classes than the
text - ones you wrote in ICS-32.
text - 
text - In fact, the actual process of calling the method has three separate stages:
text - first looking up the method, second creating a "bound function" object from it, 
text - and third calling that function object. Let's start by looking at the first two
text - stages separately. Suppose that we start by writing (note: no () so no call yet)
text - 
text -   f = 'candide'.replace
text - 
text - When Python computes 'candide'.replace, it looks up the replace method in the
text - str class and then it returns a "bound function" object: a function object with
text - its first parameter already bound to the object/argument 'candide'. 
text - 
text - So, f now refers to this "bound function" object. At this point, if we print(f)
text - it displays <built-in method replace of str object at 0x000001F22F149D70>. We
text - can then call f('d','p') which returns the string object 'canpipe'. Even if we
text - wrote 'candide'.replace('d','p') directly, it would still do all three stages:
text - looking up the method, creating a bound function object, and then calling it.
text - 
text - In contrast (and this will be reviewed in week 3), accessing
text -   f =  str.replace
text - makes f refer to an unbound function object. In this case to call f we write
text -   f('candide','d','p')
text - That is when calling this f, we must now explicitly bind 'candide' to the first
text - (self) parameter of the replace function in the str module.
text - 
text - So, the way we refer to a function, prefacing it by either an object or the
text - class of the function, produces something different: a bound or unbound function
text - object. And each of these is called differently: the former already has the
text - first (self) parameter bound; in the latter we must bind it explicitly.
text - 
text - Oh, by the way, I must say that this equation is true, but not completely true.
text - As we will later see: (a) if m is in the object o's namespaces (rare), it will
text - be called directly (bypassing looking in o's class/type), and (b) when we learn
text - about class inheritance, the inheritance hierarchy of a class provides other
text - classes in which to look for m if it is not declared directly in o's class/type.
text - So, as in courtroom testimony, we need to distinguish the truth, the whole
text - truth, and nothing but the truth. We have learned the truth, but not the whole
text - truth.
text - 
text - But this version of FEOOP is simple, clear (once you understand it), and is
text - useful in many examples, so it is worth memorizing, even if it is not the whole
text - truth. We will leverage off this simple equation when we extend the FEOOP later
text - in the quarter.
text - 
text - ------------------------------------------------------------------------------ 
text - 
text - lambda: a simple way to write simple functions
text - 
text - We use lambdas when we need to use a very simple function object: so simple,
text - it doesn't even need a name; often we use lambdas when passing functions to
text - other functions. The form of a lambda is the word "lambda", followed by its
text - parameter names (separated by commas), then a colon, followed by one EXPRESSION
text - that computes the value that the lambda returns. The expression does not need to
text - appear in a return statement; in fact, lambdas cannot include any any 
text - statements or control structures, not even a block of sequential statements.
text - 
text - So, writing ...(lambda x,y : x+y)... in some context
text - 
text - Is just like first defining
text - 
added - ```python
code-start - def f(x,y):
code -     return x+y
code-end - 
added - ```
text - and then writing ...f... in the same context. That is calling
text - print( (lambda x,y : x+y)(2,3) ) is the same as calling print( f(2,3) ).
text - 
text - A lambda produces an UNNAMED function object: there is no name bound to it. But
text - we can always use an assignment statement to define a name and bind it to the
text - function object created by a lambda. For example, we can also write the
text - following code, whose first line binds to the name f the unnamed lambda/function
text - object, and whose second line calls the unnamed lambda object via the name g.
text - 
text - g = lambda x,y : x+y  # lambdas have one expression after : without a return
text - print( g(1,3) )
text - 
text - and Python will print 4. I often put lambdas in parentheses, to more clearly
text - denote the start and end of the lambda, for example writing
text - 
text - g =  (lambda x,y : x+y)
text - 
text - Using this form, we can write code code above without defining g, writing just
text - 
text - print( (lambda x,y : x+y)(1,3) )
text - 
text - In my prompt module (MY PREFERRED WAY OF DOING PYTHON USER-INPUT), there is a
text - function called for\_int that has a parameter that specifies a boolean function
text - (a function taking one argument and returning a boolean value: we will repeated
text - use the word "predicate" to describe this kind of function) that the int value
text - must satisfy, to be returned by the prompt (otherwise the for\_int function
text - prompts the user again, for a legal value).
text - 
text - That is, we pass a function object (without calling it) to prompt.for\_int,
text - which in its body CALLS that function on the value the user enters to th
text -  prompt, to verify that the function returns True for that value.
text - 
text - So the following code fragment is guaranteed to store a value between 0 to 5
text - inclusive in the variable x. If the user enters a value like 7, an error will
text - be printed and the user re-prompted for the information.
text - 
text - import prompt
text - x = prompt.for\_int('Enter a value in [0,5]', is\_legal = (lambda x : 0<=x<=5))
text - print(x)
text - 
text - Execute this code in Python. Enter bad values (not only out-of-range integers,
text - but text that doesn't describe an integer), finally followed by a good value.
text - 
text - In a later part of this lecture note, we will frequently use lambdas to simplify
text - sorting lists (or simplify iterating through any data structures according to
text - any ordering function).
text - 
text - Again, I often put lambdas in parentheses for clarity: see the prompt.for\_int
text - example above. But there are certain places where lambdas are required to be in
text - parentheses: assume def h(f): ... then the call h((lambda x : x[1],x[0]))
text - requires the lambda to be in parentheses, because without the parentheses it
text - would read as h( lambda x : x[1], x[0] ). In this function call, Python would
text - think that x[0] was a second argument to h (not part of the lambda) to function
text - h, which would raise an exception when called, because h defined only one
text - parameter. Of course, we could also use different parentheses in the call and
text - write h( lambda x : (x[1],x[0]) ). We also wrote this call above as
text - h( (lambda x : x[1],x[0]) ). In both cases the parentheses remove the ambiguity
text - about the lambda.
text - 
text - In a previous section (functions returning functions) we wrote the code
text - 
added - ```python
code-start - def bigger_than(age):
code -     def test_it(x) :
code -         return x > age
code -     return test_it
code-end - 
added - ```
text - We defined the test\_it function just so we could return a reference to it. Now,
text - because the test\_it function is so simple, and the only place we refer to it by
text - name is in the return statement, we can simplify this code by returning a
text - lambda instead of defining/returning a named function:
text - 
added - ```python
code-start - def bigger_than(age) :
code -     return (lambda x : x > age)
code-end - 
added - ```
text - So, in each version of the bigger\_than function, it returns a reference to a
text - function object.
text - 
text - ------------------------------------------------------------------------------
text - 
text - Parallel/Tuple/List Assignment (aka sequence unpacking)
text - 
text - Note that we can write code like the following: here both x,y and 1,2 are
text - implicit tuples in Python; and we can unpack them as follows
text - 
text - x,y = 1,2
text - 
text - In fact, you can replace the left hand side of the equal sign by either (x,y) or
text - [x,y] and replace the right hand side of the equal sign by either (1,2) or [1,2]
text - and x gets assigned 1 and y get assigned 2: even (x,y) = [1,2] works correctly.
text - 
text - In most programming languages (including Java and C/C++), to exchange the values
text - stored in two variables x and y we must write three assignment statements.
text - 
text - temp = x
text - x    = y
text - y    = temp
text - 
text - Problem: why would writing just x = y followed by y = x not exchange the values?
text - What would it do?
text - 
text - In Python we can write this code using one tuple assignment
text - 
text - x,y = y,x
text - 
text - To do any tuple/parallel assignment, Python
text -   (a) first computes all the values of the expressions/objects on the right
text -         (1 and 2 from the top)
text -   (b) second binds each name on the left (in order: x then y) to these computed
text -         values/objects (binds x to 2, then bind y to 1)
text - 
text - This is similar to how Python calls functions: first evaluating all the
text - arguments and then binding them in order to all the parameters.
text - 
text - This is also called "sequence unpacking assignment": both tuples and list are
text - considered kinds of sequences (where order is important) in Python. Note that
text - x,y = 1,2,3 and x,y,z = 1,2 would both raise ValueError exceptions, because
text - there are different numbers of names and values to bind to them. We will
text - frequently use simple forms of parallel/unpacking assignment when looping
text - through items in dictionaries (illustrated below; used extensively later in this
text - lecture note), but even more complicated forms are possible: for example.
text - 
text - l,m,(n,o) = (1, 2, [3,4])
text - print(l,m,n,o)
text - 
text - prints: 1 2 3 4
text - 
text - Here, each name is bound to one int value. Python also allows writing
text - 
text - a,\*b,c = [1,2,3,4,5]
text - print(a,b,c)
text - 
text - which prints as: 1 [2, 3, 4] 5
text - 
text - Here, \* can preface any name; the name is bound to a sequence of any number of
text - values, so as to correctly bind a and c. That is, \*b would need to bind to a
text - sequence of all but the first and last value. We could not write
text - 
text - \*a,\*b,c = [1,2,3,4,5]
text - 
text - because there is no unique way to bind a, b, and c: for example a=1, b=[2,3,4],
text - and c=5 works; but so does a=[1,2],b=[3,4],c=5. 
text - 
text - In fact, we can write complicated parallel assignments with multiple \*s like
text - 
text - l,(\*m,n),\*o  = (1, ['a','b','c'], 2, 3,4)
text - print(l,m,n,o)
text - 
text - so long as there is a unique way to decide what the \* variables bind to.
text - 
text - The above statement prints as: 1 ['a', 'b'] c [2, 3, 4]
text - 
text - Generally sequence unpacking assignment is useful if we have a complex
text - tuple/list structure and we want to bind names to its components, to refer to
text - these components more easily by these names: each name binds to part of the
text - complicated structure.
text - 
text - As another example, if we define a function that returns a tuple
text - 
added - ```python
code-start - def reverse(a,b) :
code -     return (b,a)    # we could also write just return b,a
code-end - 
added - ```
text - we can also write x,y = reverse(x,y) to also exchange these values.
text - 
text - Finally, one of the most common ways to use unpacking assignment is in for
text - loops: for example, we can write the following for loop to process a list of
text - dates in the form (month, day, year).
text - 
text - for month,day,year in [(9,4,1980), (11,29,1980), (8,8,2014)]:
text -     do something with month, day, and year
text - 
text - This is simpler and clearer than using one name for the entire tuple, and then
text - repeatedly indexing that one name.
text - 
text - for t in [(9,4,1980), (11,29,1980), (8,8,2014)]:
text -     do something with t[0], t[1], and t[2]
text - 
text - When you write for loops that loop over complicated data, use multiple names
text - for the individual parts of the data. Multiple names for the parts often make
text - the loop body's code easier to read/write/understand. Students often fail to
text - follow this advice and eventually get docked points for writing code that is
text - more cryptic than necessary, even if it is correct: who can easily remember
text - what t[1] is? Better to use a good name for the the data it represents.
text - 
text - We will see more about such assignments when iterating though items in
text - dictionaries. The following code preview how to iterate over the tuples that
text - are key/value items in a dictionary, by unpacking them into the names key and
text - value.
text - 
text - for key, value in d.items():  #for every (key,value) item in dictionary d
text -     print(key, '->', value)   #  print it
text - 
text - This code prints each key and its associated value in dictionary d. If a
text - specific dictionary stored keys that were letters and associated values that
text - were their frequencies, we could write.
text - 
text - d = {'a':1, 'b':2, 'c':3, 'd':4}
text - for letter, frequency in d.items():
text -     print(letter,'->',frequency)
text - 
text - which prints
text - 
text - d -> 4
text - a -> 1
text - c -> 3
text - b -> 2
text - 
text - This is easier to understand  than the following loop, which iterates over the
text - unpacked item tuples.
text - 
text - for t in d.items():         #for every tuple item in dictionary d
text -     print(t[0], '->', t[1]) #  print its key (t[0])and associated value (t[1])
text - 
text - In fact, if we know there is a specific structure to the key, value, or both,
text - then we can use unpacking to help. Suppose that keys are dates in the form
text - (year, month day) and associated values are orders from a warehouse in the form
text - (item, quantity). We could write 
text - 
text - for (month,day,year),(item,quantity) in d.items():
text -     process all 5 quantities, which have reasonable names from unpacking
text - 
text - ------------------------------------------------------------------------------
text - ------------------------------------------------------------------------------
text - ------------------------------------------------------------------------------ 
text - 
text - End of 1st Lecture on this material
text - 
text - ------------------------------------------------------------------------------
text - ------------------------------------------------------------------------------
text - ------------------------------------------------------------------------------ 
text - 
text - Iterable
text - 
text - When we specify that an argument of a function must be iterable, it might be one
text - of the standard data structures in Python: str, list, tuple, set, dict. All
text - these data structures are iterable: we can iterate over the values they contain
text - by using a simple for loop (for dicts, there are two other forms of iteration,
text - which are discussed later in these notes).
text - 
text - But we will learn other Python features (classes and generators) that are also
text - iterable. The difference is, that for standard Python data structures we can
text - call the "len" function on them and index them  [...]; but for general iterable
text - arguments we CANNOT call "len" nor index them: we can only iterate over their
text - values (typically with a for loop): getting their first value, their second
text - value, etc. (never knowing when there won't be a next value). Later in the
text - quarter we will learn how to call iter and next explicitly on iterables; for
text - loops implicitly call these two functions on iterables.
text - 
text - Also, we can always use constructors and comprehensions (discussed later in this
text - lecture) to transform any iterable into a list or tuple of its values, although
text - doing so takes up extra time and space, and should be avoided unless necessary.
text - But learning how to convert any iterable into a list or tuple is instructive.
text - 
text - ------------------------------------------------------------------------------ 
text - 
text - sort (a list method)/sorted (a function): and their "key"/"reverse" parameters
text - 
text - First, we will examine the following similar definitions of sort/sorted. Then we
text - will learn the differences between these similar and related features below.
text - 
text - (1) sort is a method defined on arguments that are LIST objects; it returns
text -       the value None, but it MUTATES its list argument with its values in some
text -       specified order: e.g., alist.sort() or alist.sort(reverse=True); note that
text -       calling  print(alist.sort()) sorts (mutates)  alist and then prints None
text -       (the sort method's returned value). The result of writing
text -       alist = alist.sort() binds alist to None (the sorted list is gone).
text - 
text - (2) sorted is a function defined on arguments that are ITERABLE objects; it
text -       returns a LIST of its argument's values, with its values in some specified
text -       order. The argument itself isn't  MUTATED: e.g., sorted(adict) or
text -       sorted(adict,reverse=True). Neither changes adict to be sorted, because
text -       dictionaries are never sorted; both function calls produce/return a LIST
text -       of sorted keys in adict. We can also call sorted with an argument that is
text -       a str, list, tuple, set, etc.: anything that is iterable in Python.
text - 
text - So, the sort method can be applied only to lists, but the sorted function can
text - be applied to any iterable (str, list, tuple, set, dict, etc.) 
text - 
text - As a first example, if votes is the list of 2-tuples (candidates, votes)
text - specified below, we can execute the following code.
text - 
text - votes = [('Charlie', 20), ('Able', 10), ('Baker', 20), ('Dog', 15)]
text - votes.sort()        # call sort method with prefix argument votes
text - for c,v in votes:   # note parallel/unpacking assignment
text -     print('Candidate', c, 'received', v, 'votes')
text - print(votes)
text - 
text - The call votes.sort() uses the sort METHOD to sort the LIST (MUTATE it); then
text - the for loop iterates over this newly-ordered list and prints the information
text - in the list, in the order it now appears. Then, the entire votes list is printed
text - after the loop: we will see that the list has been MUTATED and is now sorted.
text - Python prints the information sorted alphabetically, in increasing order.
text - 
text - Candidate Able received 10 votes
text - Candidate Baker received 20 votes
text - Candidate Charlie received 20 votes
text - Candidate Dog received 15 votes
text - [('Able', 10), ('Baker', 20), ('Charlie', 20), ('Dog', 15)]
text - 
text - We will see why it sorts by name soon, and learn how to use the key argument
text - (which is bound to a function) to sort in any other way (say by decreasing
text - votes). Contrast this with the following code that falls the sorted function.
text - 
text - votes = [('Charlie', 20), ('Able', 10), ('Baker' ,20), ('Dog', 15)]
text - for c,v in sorted(votes):  # call sorted function on votes (which is iterable)
text -     print('Candidate', c, 'received', v, 'votes')
text - print(votes)
text - 
text - Here we never sort/mutate the votes list. Instead we use the sorted FUNCTION,
text - which takes an ITERABLE argument (a LIST is iterable) and returns a NEW LIST
text - that is sorted (leaving votes unchanged). Then we iterate over that returned
text - list to print its information (which is a sorted version of votes). But here,
text - when we print the votes list at the end, we see that the votes list remained
text - unchanged. Python prints
text - 
text - Candidate Able received 10 votes
text - Candidate Baker received 20 votes
text - Candidate Charlie received 20 votes
text - Candidate Dog received 15 votes
text - [('Charlie', 20), ('Able', 10), ('Baker', 20), ('Dog', 15)]
text - 
text - The sorted function can be thought of as first iterating over the parameter,
text - using its value to create a temporary list; then sorting that temporary list;
text - and finally returning the sorted temporary list: sorted mutates the list it
text - creates internally; it does not mutate the argument matching iterable.
text - 
text - So, we can think of the sorted function to be defined by
text - 
added - ```python
code-start - def sorted (iterable):
code -   alist = list(iterable)  # construct a list with all the iterable's values
code -   alist.sort()		  # sort that list using the sort method for lists
code -   return alist		  # return the sorted list
code-end - 
added - ```
text - Question: What would the following code do? If you understand the definitions
text - above, you won't be fooled and the answer won't surprise you. Hint: what does
text - votes.sort() return vs. sorted(votes)? If you cannot figure out the answer,
text - copy/paste the following code into a Python module in Eclipse and execute it
text - to see what Python says.
text - 
text - votes = [('Charlie', 20), ('Able', 10), ('Baker' ,20), ('Dog', 15)]
text - for c,v in votes.sort():
text -     print('Candidate', c, 'received', v, 'votes')
text - print(votes)
text - 
text - If we were going to print some list in a sorted form many times, it would be
text - more efficient to sort/mutate it once, and then use a regular for loop on that
text - sorted list (either mutate the original or construct a new, sorted list and use
text - it multiple times). But if we needed to keep the list in a certain order and
text - care about space efficiency and/or don't care as much about time efficiency, we
text - would not sort/mutate the list and instead call the sorted function whenever we
text - need to process the list in a sorted order.
text - 
text - Note that if we store votes\_dict as a dict data structure (a dictionary
text - associating candidates with the number of votes they received), and then tried
text - to call the sort method on it, Python would raise an exception
text - 
text - votes\_dict = {'Charlie': 20, 'Able': 10, 'Baker': 20, 'Dog': 15}
text - votes\_dict.sort()
text - 
text -   AttributeError: 'dict' object has no attribute 'sort'
text - 
text - The problem is: the sort METHOD is defined only for LIST objects, not for DICT
text - objects.
text - 
text - So, Python cannot execute votes\_dict.sort()! It makes no sense to sort a
text - dictionary. In fact, we cannot sort strings (they are immutable); we cannot
text - sort tuples (their order is immutable); we cannot sort sets (they have no order,
text - which actually allows them to operate more efficiently; we'll learn how and why
text - later in the quarter); we cannot sort dicts (like sets, they have no order,
text - which allows them to operate more efficiently; ditto). 
text - 
text - BUT, WE CAN CALL THE SORTED FUNCTION ON ALL FOUR OF THESE DATA STRUCTURES, and
text - generally on anything that is iterable: As we saw, Python executes the sorted
text - function by creating a temporary list from all the values produced by the
text - iterable, then it sorts that temporary list, and finally it returns the sorted
text - list. Here is one example of how the sorted function processes votes\_dict. Here,
text - executing sorted(votes\_dict) is the same as executing sorted(votes\_dict.keys())
text - which returns a sorted list of the dict's keys, which we can then iterate over,
text - printing every key in votes\_dict and its associated integer.
text - 
text - votes\_dict = {'Charlie' : 20, 'Able' :  10, 'Baker' : 20, 'Dog' : 15}
text - for c in sorted(votes\_dict):  # same as: for c in sorted(votes\_dict.keys())
text -     print('Candidate', c, 'received', votes\_dict[c], 'votes')
text - print(votes\_dict)
text - 
text - Python prints
text - 
text - Candidate Able received 10 votes
text - Candidate Baker received 20 votes
text - Candidate Charlie received 20 votes
text - Candidate Dog received 15 votes
text - {'Charlie': 20, 'Able': 10, 'Baker': 20, 'Dog': 15}
text - 
text - -----Start alternative example
text - Note: if we wrote
text - 
text - votes\_dict = {'Charlie' : 20,  'Able' : 10,  'Baker' : 20,  'Dog' : 15}
text - print(sorted(votes\_dict))
text - 
text - Python prints a list, the list returned by sorted: the dictionary's keys in
text - sorted order:
text - 
text - ['Able', 'Baker', 'Charlie', 'Dog']
text - -----Stop alternative example
text - 
text - This example shows one "normal" way to iterate over a sorted list built from a
text - dictionary. We can also iterate over sorted "items" in a dictionary as follows
text - (the difference is in the for loop, its unpacked indexes, and the print). We
text - will examine more about dicts and the different ways to iterate over them later
text - in this lecture. Recall that each item in a dictionary is a 2-tuple consisting
text - of one key and its associated value.
text - 
text - votes\_dict = {'Charlie' : 20,  'Able' : 10,  'Baker' : 20,  'Dog' : 15}
text - for c,v in sorted(votes\_dict.items()): # note parallel/unpacking assignment
text -     print('Candidate', c, 'received', v, 'votes')
text - print(votes\_dict)
text - 
text - Python prints a result identical to the original example using dicts.
text - 
text - Notice that this print doesn't access votes[c] to get the votes, which is the
text - second item in each 2-tuple being iterated over using .items(). This is because
text - iterating over votes\_dict.items() produces a sequence of 2-tuples, each
text - containing one key and its associated value. The order that these 2-tuples
text - appear in the list is unspecified, but using the sorted function ensures that
text - the keys appear in order.
text - 
text - If you are going to iterate over a dict and use both the key and its associated
text - value, it is better (simpler and more efficient) to iterate over items than to
text - iterate over keys and then dict[key] to access key's associated item.
text - 
text - -----Start alternative example
text - Note: if we wrote
text - 
text - votes\_dict = {'Charlie' : 20,  'Able' : 10,  'Baker' : 20,  'Dog' : 15}
text - print(list(votes\_dict.items()))
text - print(sorted(votes\_dict.items()))
text - 
text - Python first prints a list of the dictionary's items in an unspecified order,
text - then it prints a list of the same dictionary's items, in sorted order:
text - 
text - [('Charlie', 20), ('Able', 10), ('Baker', 20), ('Dog', 15)]
text - [('Able', 10), ('Baker', 20), ('Charlie', 20), ('Dog', 15)]
text - 
text - both list(...) and sorted(...) produce a list of the items in the dictionary,
text - with only sorted(....) producing a list that is guaranteed to be sorted.
text - -----Stop alternative example
text - 
text - Now we discuss how can specify a "key" parameter to either a call to the sort
text - method or sorted function, to sort its values in any arbitrary order.
text - 
text - How does sort/sorted work? How do they know how to compare the values they are
text - sorting: which should come before which? There is a standard way to compare any
text - data structure, but we can also use the "key" and "reverse" parameters (which
text - must be used with their names, not positionally) to tell Python how to do the
text - sorting. The reverse parameter is simpler, so let's look at it first; writing
text - sorted(votes,reverse=True) sorts, but in the reverse order (writing
text - reverse=False is like not specifying reverse at all). So, returning to votes as
text - a list,
text - 
text - votes = [('Charlie', 20), ('Able', 10), ('Baker' ,20), ('Dog', 15)]
text - print(sorted(votes,reverse=True))
text - 
text - Python prints the list of 2-tuples sorted alphabetically, but now in decreasing
text - (reversed) order:
text - 
text - [('Dog', 15), ('Charlie', 20), ('Baker', 20), ('Able', 10)]
text - 
text - What sort is doing is comparing each 2-tuple value in the list to the others
text - using the standard meaning of <. You should know how Python compares str values,
text - int values, etc. But how does Python compare whether one 2-tuple is less than,
text - equal to, or greater than another? Or whether one list is less than, equal to,
text - or greater than another? The algorithm in analogous to strings, so let's first
text - re-examine comparing strings. The resulting alphabetical ordering, by the way,
text - is called "lexicographic ordering", aka "dictionary ordering": it is the order
text - in which words appear in dictionaries (books), not Python dicts/dictionaries.
text - 
text - ---------------
text - Interlude: Comparing Strings in Python:
text - 
text - String comparison: x to y (high level: how does x compare to y):
text - 
text - Find the first/minimum index i such that the ith character in x and y are
text - different (e.g., x[i] != y[i]). If that character in x is less than that
text - character in y (by the standard ASCII table) then x is less than y; if that
text - character in x is greater than that character in y then x is greater than y;
text - if there is no such different character, then how x compares to y is the same as
text - how  x's length compares to y's length (either less than, equal to, or greater
text - than).
text - 
text - Here are three examples:
text - 
text -   (1) How do we compare x = 'aunt' and y = 'anteater'? The first/minimum i such
text -   that the characters are different is 1: x[1] is 'u' and y[1] is 'n'; 'u' is
text -   greater than 'n', so x > y, so 'aunt' > 'anteater'. The word 'anteater'
text -   appears in an English dictionary before the word 'aunt'.
text - 
text - 
text -   (2) How do we compare x = 'ant' and y = 'anteater'? There is no first/minimum
text -   i such that the characters are different; len(x) < len(y), so x < y, so
text -   'ant' < 'anteater'. The word 'ant' appears in an English dictionary before
text -   the word 'anteater' ('ant' is a prefix of 'anteater').
text - 
text -   (3) How do we compare x = 'ant' and y = 'ant'? There is no first/minimum i
text -   such that the characters are different; len(x) == len(y), so x == y, so
text -   'ant' == 'ant'. I show this example, which is trivial, just to be complete.
text - 
text - See the Handouts(General) webpage showing the ASCII character set, because
text - there are some surprises. You should memorize that "the digits, lower, and upper
text - case letters compare in order", and "all digits < all upper-case letters < all
text - lower-case letters". So 'TINY' < 'huge' is True because the index 0 characters
text - different, and 'T' is < 'h' (all upper-case letters have smaller ASCII values
text - than any lower-case letters). Likewise, 'Aunt' < 'aunt' because 'A' < 'a'.
text - 
text - Note that we can always use the upper() method (as in x.upper() < y.upper()) or
text - lower() method to perform a comparison that ignores the case of the letters in
text - a string: 'TINY'.lower() > 'huge'.lower() because 'tiny' > 'huge' because
text - 't' > 'h'. Such is called a case-insensitive comparison, although Python is
text - really doing a case-sensitive comparison on strings that have had all their
text - letter characters appearing in upper-case or lower- case.
text - 
text - Use these rules to determine whether '5' < '15' by comparing the string '5' to
text - '15'. What is your answer; is it the correct answer?
text - ---------------
text - 
text - Back to comparing tuples (or lists). We basically do the same thing. We look at
text - what is in index 0 in the tuples; if different, then the tuples compare in the
text - same way as the values in index 0 compare; if the values at this index are
text - equal, we keep going until we find a difference, and compare the tuples the same
text - way that the differences compare; if there are no differences, the tuples
text - compare the same way their lengths compare.
text - 
text - So ('UCI', 100) < ('UCSD', 50) is True because at index 0 of these tuples, the
text - string values are different; and 'UCI' < 'UCSD' (because at index 2 of these
text - strings, the character values are different, and 'I' < 'S').
text - 
text - Whereas ('UCI', 100) < ('UCI', 200) is True because at index 0 of these tuples,
text - the string values are equal ('UCI' == 'UCI'), so we go to index 1, where we
text - find the first different values, and 100 < 200.
text - 
text - Finally, ('UCI', 100) < ('UCI', 100, 'extra') is True because at index 0 of
text - these tuples the values are equal ('UCI' == 'UCI'); and at index 1 of these
text - tuples the values are equal (100 == 100), so there are no indexes with different
text - values in these tuples; but, the second tuple has a larger length, so it
text - compares greater than the first tuple.
text - 
text - Recall the sorting code from above.
text - 
text - votes = [('Charlie', 20), ('Able', 10), ('Baker' ,20), ('Dog', 15)]
text - for c,v in sorted(votes):
text -     print('Candidate', c, 'received', v, 'votes')
text - print(votes)
text - 
text - The reason that the values come out in the order they do in the code above
text - (increasing alphabetical order of the names) is because the names that are in
text - the first index in each 2-tuple are all different, so Python ensures the tuples
text - are sorted in increasing alphabetical order by their first indexes. Python
text - never gets to looking at the second value in each tuple, because the first
text - values (the candidate names) are always different.
text - 
text - Now, what if we don't want to sort by the natural tuple ordering. We can
text - specify a "key" function that computes a key value for every value in what is
text - being sorted, and the COMPUTED KEY VALUES ARE USED FOR COMPARISON, not the
text - original values themselves. The results returned by this function are the "keys"
text - for comparison.
text - 
text - See the by\_vote\_count function below; it takes a 2-tuple argument and returns
text - only the second value in the tuple (recall indexes start at 0) for the key on
text - which Python will compare. For the argument 2-tuple ('Baker' ,20) by\_vote\_count
text - returns 20.
text - 
added - ```python
code-start - def by_vote_count(t):
code -     return t[1]            # remember t[0] is the first index, t[1] the second
code-end - 
added - ```
text - So, when we sort with key=by\_vote\_count, we are telling the sorted function to
text - determine the order of values by calling the by\_vote\_count function on each
text - value: so in this call of sorted, we are comparing tuples based solely on their
text - vote part, from index 1. So writing
text - 
text - votes = [('Charlie', 20), ('Able', 10), ('Baker' ,20), ('Dog', 15)]
text - for c,v in sorted(votes, key=by\_vote\_count):
text -     print('Candidate', c, 'received', v, 'votes')
text - 
text - produces 
text - 
text - Candidate Able received 10 votes
text - Candidate Dog received 15 votes
text - Candidate Charlie received 20 votes
text - Candidate Baker received 20 votes
text - 
text - First, notice that by writing "key=by\_vote\_count" Python didn't CALL the
text - by\_vote\_count function (there are no parentheses) it just passed its associated
text - function object to the key parameter in the sorted function. Inside the sorted
text - function, by\_vote\_count's function object automatically is called where needed,
text - to compare two 2-tuples, to determine in which order these values will appear in
text - the returned list.
text - 
text - Also, because Charlie and Baker both received the same number of votes, they
text - both appear at the bottom; in the current Python sorted function, the order
text - equal values appear in (in the list returned by sorted) is the same order they
text - appear in the argument. Since Charlie appears before Baker in votes, and both
text - compare == using by\_vote\_count, Charlie appears before Bake in the result list
text - returned by sorted (more on this later in this section).
text - 
text - Finally, notice that these tuples are printed in ascending order by votes;
text - generally for elections we want the votes to be descending, so we can combine
text - using key and reverse (with reverse=True) in the call to the sorted function.
text - 
text - votes = [('Charlie', 20), ('Able', 10), ('Baker' ,20), ('Dog', 15)]
text - for c,v in sorted(votes, key=by\_vote\_count, reverse=True):
text -     print('Candidate', c, 'received', v, 'votes')
text - 
text - which produces 
text - 
text - Candidate Charlie received 20 votes
text - Candidate Baker received 20 votes
text - Candidate Dog received 15 votes
text - Candidate Able received 10 votes
text - 
text - Again, since the top two candidates both received the same number of votes, they
text - appear in the same order they appear in votes.
text - 
text - Now, rather than define this simple by\_vote\_count function, we can use a lambda
text - instead, and write the following.
text - 
text - ...
text - for c,v in sorted(votes, key=(lambda t : t[1]), reverse=True):
text -     ...
text - 
text - So, now we don't have to write/name that extra by\_vote\_count function. Of
text - course, if we did write it, we could re-use it wherever we wanted, instead of
text - rewriting the lambda (but the lambdas are pretty small). Also, by writing a
text - lambda, the relevant code is all inside the call to sorted: we don't have to go
text - look elsewhere for its function definition. So, we now know how to use function
text - and lambda arguments to match the key parameter, when calling sorted.
text - 
text - Such a key function is used the same way when calling the sort method. If votes
text - is the list shown at the beginning of this section (not the dictionary above),
text - we can call  votes.sort(key=(lambda t : t[1]), reverse=True) to sort this list,
text - mutating it.
text - 
text - Another way to sort in reverse order (for integers) is to use the "negation"
text - trick illustrated below (and omit reverse=True).
text - 
text - ...
text - for c,v in sorted(votes, key=(lambda t : -t[1]) ):
text -     ...
text - 
text - Here we have negated the vote count part of the tuple, and removed reverse=True.
text - So it is sorting from smallest to largest, but the key function returns the
text - negative of the vote values (because that is what the lambda says to do). It is
text - comparing -20, -10, -20, and -15 when sorting. So here the biggest vote count
text - corresponds to the smallest negative number (so the tuple with that number will
text - appear first). The tuples appear in increasing order, specified by the values
text - their key function returns: -20, -20, -15, -10 (smallest to largest). These
text - negative values returned by the key function are used only to determine the
text - order inside sorted; they do not appear in the sorted results.
text - 
text - Finally, typically when multiple candidates are tied for votes, we want to print
text - their names together (because they all have the same number of votes) but also
text - in alphabetical order. We do this in Python by specifying a key function that
text - returns a tuple in which the vote count is checked first and sorted in
text - descending order; and only if the votes are equal will the names be checked and
text - sorted in ascending order. Because we want the tuples in decreasing vote
text - counts but increasing names, we cannot just use reverse=True: it would reverse
text - both the vote AND name comparisons; we need to resort to the "negation trick"
text - above and write
text - 
text - votes = [('Charlie', 20), ('Able', 10), ('Baker' ,20), ('Dog', 15)]
text - for c,v in sorted(votes, key=(lambda t : (-t[1],t[0])) ):
text -     print('Candidate',c,'received',v,'votes')
text - 
text - So it compares the 2-tuple keys (-20, 'Charlie'), (-10, 'Able'), (-20, 'Baker'),
text - and (-15, 'Dog') when ordering the actual 2-tuples, and by what we have learned
text - 
text - (-20, 'Baker') < (-20, 'Charlie') < (-15, 'Dog') < (-10, 'Able')
text - 
text - which produces 
text - 
text - Candidate Baker received 20 votes
text - Candidate Charlie received 20 votes
text - Candidate Dog received 15 votes
text - Candidate Able received 10 votes
text - 
text - So with this key function, the previous order is GUARANTEED, even though Baker
text - and Charlie both received the same number (20) of votes, 'Baker' < 'Charlie' so
text - that 2-tuple will appear first.
text - 
text - Finally, note that the lambda in the key ensures Python compares ('Charlie', 20)
text - and ('Dog', 15) as if they were (-20, 'Charlie') and (-15, 'Dog'), so the first
text - tuple will be less (and appear earlier) in the sorted list (the one with the
text - highest votes has the lowest negative votes). And when Python compares
text - ('Charlie', 20) and ('Baker' ,20) as if they were (-20, 'Charlie') and
text - (-20, 'Baker') so the second tuple will be less and the tuple it was produced
text - from will appear earlier (equal votes and then 'Baker' < 'Charlie').
text - 
text - So think of sorting as follows. Python first computes the keys of all values
text - to sort.
text - 
text - ('Charlie', 20)   ('Able', 10)   ('Baker' ,20)   ('Dog', 15)
text -        |                |               |              |  using the key function
text -        |                |               |              |  it really compares
text -        V                V               V              V  
text - (-20, 'Charlie')  (-10, 'Able')  (-20, 'Baker')  (-15, 'Dog')
text - 
text - It then sorts these keys naturally, and substitutes back the original values to
text - sort into the resulting list, based on the order of the original keys.
text - 
text - (-20,'Baker')     (-20,'Charlie')   (-15,'Dog')    (-10,'Able')
text -        |                |               |              | actual values in list
text -        |                |               |              | (without key function)
text -        V                V               V              V
text - ('Baker',20)      ('Charlie', 20)   ('Dog',15)     ('Able', 10)
text - 
text - Finally,  Python returns the a list of these values
text - 
text - [('Baker', 20), ('Charlie', 20), ('Dog', 15),  ('Able', 10)]
text - 
text - which here is sorted by decreasing vote, with equal votes sorted alphabetically
text - increasing by name.
text - 
text - Our ability to use this "negation trick" works in SOME cases, but unfortunately
text - NOT IN ALL cases: we can sort arbitrarily using this trick exactly when "all
text - non-numerical data is sorted in the same way" (all increasing or all
text - decreasing). In such cases, we can negate any numerical data, if we need to
text - sort it decreasing.
text - 
text - So, we can sort the above example because we are sorting only one non-numerical
text - datum. By the requirement, all non-numerical data (there is only one on this
text - example, the name) is sorted the same way (here increasing).
text - 
text - Using the "negation trick" is therefore not generalizable to all sorting tasks:
text - for example, if we had a list of 2-tuples containing strings of candidates and
text - strings of the states they are running in
text - 
text -   [('Charlie', 'CA'), ('Able', 'NY'), ('Baker' ,'CA'), ('Dog', 'IL')]
text - 
text - there is no way to use the "negation trick" to sort them primarily by decreasing
text - state name, and secondarily by increasing candidate name (when two candidates
text - come from the same state). Here it is not the case that "all non-numerical data
text - is sorted the same way": each string data is sorted differently. In this case,
text - calling sorted(..., key = (lambda t : (t[1], t[0])), reverse = True) the sorted
text - list would be
text - 
text -   [('Able', 'NY'), ('Dog', 'IL'), ('Charlie', 'CA'), ('Baker' ,'CA')]
text - 
text - even though we wanted it to be (note the last two names, from the same state)
text - 
text -   [('Able', 'NY'), ('Dog', 'IL'), ('Baker', 'CA'), ('Charlie' ,'CA')]
text - 
text - We cannot apply the "negation trick" to either of the strings: we cannot negate
text - strings at all. But we can produce a list in this specified order by calling
text - sorted multiple times, as is illustrated below. 
text - 
text - ---------------
text - Arbitrary sorting: multiple calls to sorted with "stable" sorting
text - 
text - Python's sorted function (and sort method) are "stable". This property means
text - that "equal" values (decided naturally, or with the key function supplied) keep
text - their same "relative" order (left-to-right positions) in the data being sorted.
text - 
text - For example, assume that db is a list of 2-tuples, each specifying a student:
text - index 0 is a student's name; index 1 is that student's grade.
text - 
text -   db = [('Bob','A'),    ('Mary','C'), ('Pat','B'), ('Fred','B'), ('Gail','A'),
text -         ('Irving','C'), ('Betty','B'), ('Rich','F')]
text - 
text - If we call
text - 
text -   sorted(db, key = (lambda x : x[1]) )   # sorted by index 1 only: their grade
text - 
text - Python returns the following list, whose 2-tuples are sorted by grade. Because
text - sorting is "stable", all 2-tuples with the same grade (equal values by the key
text - function) keep their same relative order (left-to-right positions) in the list.
text - 
text -   [('Bob','A'),  ('Gail','A'),   ('Pat','B'), ('Fred','B'), ('Betty','B'),
text -    ('Mary','C'), ('Irving','C'), ('Rich','F')]
text - 
text - Notice that...
text - 
text -   (1) in the original list, the students with 'A' grades are 'Bob' and 'Gail',
text -       with 'Bob' to the left of 'Gail'. In the returned list, 'Bob' is also to
text -       the left of 'Gail'. They compare == by the lambda.
text - 
text -   (2) in the original list, the students with 'B' grades are 'Pat', 'Fred', and
text -       'Betty', with 'Pat' to the left of 'Fred' and 'Fred' to the left of
text -       'Betty'. In the returned list, 'Pat' is to the left of 'Fred' and 'Fred'
text -       is to the left of 'Betty'. They compare == by the lambda.
text - 
text -   (3) in the original list, the students with 'C' grades are 'Mary' and
text -       'Irving', with 'Mary' to the left of 'Irving'. In the returned list,
text -       'Mary' is also to the left of 'Irving'. They compare == by the lambda.
text - 
text -   (4) there is only one student with an 'F' grade.
text - 
text - Now, suppose that we wanted to sort this list so that primarily the 2-tuples
text - are sorted DECREASING by grade ('F's, then 'D's, then 'C's, then 'B's, then
text - 'A's); and for students who have equal grades, the 2-tuples are sorted
text - INCREASING alphabetically by student name. We can accomplish this task by
text - calling sorted twice, by writing
text - 
text - sorted( sorted(db, key=(lambda x : x[0])), key=(lambda x : x[1]), reverse=True )
text - 
text - The inner call to sorted produces the list
text - 
text -   [('Betty','B'), ('Bob','A'), ('Fred','B'), ('Gail','A'), ('Irving','C'),
text -    ('Mary','C'),  ('Pat','B'), ('Rich','F')]
text - 
text - which is sorted INCREASING by the student's name: all names are distinct
text - (different), so stability is irrelevant here.
text - 
text - The outer call to sorted, using the "sorted by name" db list as an argument,
text - produces the list
text - 
text -   [('Rich','F'), ('Irving','C'), ('Mary','C'), ('Betty','B'), ('Fred','B'),
text -    ('Pat','B'),  ('Bob','A'),    ('Gail','A')]
text - 
text - which is sorted DECREASING by the student's grade: for equal grades, the
text - stability property ensures that all names are still sorted INCREASING by the
text - student's name (because the argument of this call to sorted is sorted that way).
text - Here, the 'B' students are in the alphabetical order 'Betty', 'Fred', and 'Pat'.
text - Note that we could simplify this code to just
text - 
text -   sorted( sorted(db), key=(lambda x : x[1]), reverse = True )
text - 
text - because for the inner call to sorted, no key function is the same as the key
text - function that specifies sorting the 2-tuples in the natural way (alphabetically
text - by name). We could also use a temporary variable and write this code as
text - 
text -   temp = sorted(db)
text -   sorted( temp, key = lambda x : x[1], reverse = True )
text - 
text - Thus, we can sort complex structures in any arbitrary way (some data increasing,
text - some data decreasing) by calling sorted multiple times on it. The LAST call will
text - dictate the primary order in which it is sorted; each preceding/inner call
text - dictates how the data is sorted if values specified by the outer orders are
text - equal.
text - 
text - Experiment with these various forms of sorting, using reverse, keys, the 
text - negation trick, and multiple calls to sort.
text - ---------------
text - 
text - Finally, to solve the original problem above (with candidates and their states:
text - also involving two strings sorted primarily DECREASING on state and INCREASING
text - on name)
text - 
text -   db  = [('Charlie', 'CA'), ('Able', 'NY'), ('Baker' ,'CA'), ('Dog', 'IL')]
text - 
text - we would call 
text - 
text -   sorted( sorted(db), key=(lambda x : x[1]), reverse = True )
text - 
text - Bottom line on sorting:
text - 
text -   To achieve our sorting goal, if we can call sorted once, using a complicated
text -   key (possibly involving the "negation trick") and reverse, that is the
text -   PREFERRED APPROACH: it is shortest/clearest in code and fastest in computer
text -   time; it also uses up the least space, because it creates only one new list.
text - 
text -   But if we are unable to specify a single key function and reverse that
text -   do the job, we can always call sorted multiple times, using multiple key
text -   functions and multiple reverses, sometimes relying on the "stability"
text -   property to achieve our sorting goal.
text - 
text - We now have a general tool bag for sorting all types of information. Sorting
text - will be heavily tested in Quiz #1, Programming Assignment #1, and In-Lab #1.
text - 
text - ------------------------------------------------------------------------------
text - 
text - The print function
text - 
text - Notice how the sep and end parameters in print help control how the printed
text - values are separated and what is printed after the last one. Recall that print
text - can have any number of arguments (we will learn how  this is done in Python
text - later in this lecture), and it prints the str(...) called on parameter. By
text - default, sep=' ' (space) and end='\n' (newline). So the following
text - 
text - print(1,2,3,4,sep='--',end='/')
text - print(5,6,7,8,sep='x',end='\*\*')
text - 
text - prints
text - 
text - 1--2--3--4/5x6x7x8\*\*
text - 
text - Also recall that all functions must return a value. The print function returns
text - the value None: this function serves as a statement: it has an "effect" (of
text - displaying information in the console window; we might say changing the state
text - of the console) but returns no useful "value"; but all functions must return a
text - value, so this one returns None. Recall that the sort method on lists mutated
text - a list (its primarly action) but also had to return a value, so returned None.
text - 
text - Sometimes we use sep='' to control spaces more finely. In this case we must put
text - in all the spaces ourselves. If we want to separate only 2 and 3 by a space,
text - we write
text - 
text - print(1,2,' ',3,sep='')
text - 
text - which prints
text - 
text - 12 3
text - 
text - Still, other times we can concatenate values together into one string (which
text - requires us to explicitly use the str function on the things we want to print).
text - 
text - x = 10
text - print('Your answer of '+str(x)+' is too high.'+'\nThis is on the next line')
text - 
text - Note the use of the "escape" sequence \n to generate a new line. It doesn't
text - print a '\' followed by an 'n'; Python interprets '\n' to be the newline
text - character: the remaining output starts at the beginning of the next line.
text - 
text - Finally, we can also use the very general-purpose .format function. This
text - function is illustrated in Section 6.1.3 in the documentation of The Python
text - Standard Library. The following two statements print equivalently: for a string
text - with many format substitutions, I prefer the form with a name (here x) in the
text - braces of the string and as a named parameter in the arguments to format.
text - 
text - print('Your answer of {} is too high\nThis is on the next line'.format(10))
text - 
text - print('Your answer of {x} is too high\nThis is on the next line'.format(x=10))
text - 
text - In Python 3.6 and beyond we can use f-strings (formatted strings; google PEP
text - 498) to solve this problem even more simply.
text - 
text - print(f'Your answer of {x} is too high\nThis is on the next line')
text - 
text - Here, any expression in {} in the f-string is evaluated and turned into a string
text - embedded in the f-string. We could write {x+1} or any other expression instead
text - of just the x in the {}. It is also easy to format f-strings: to print the
text - value of x, right-justified, in field of 10 spaces, we can use the f-string
text - f'{x: >10}' or f'{x:>10}'; the colon separates the value to be printed from the
text - specification for printing it. We can replace > by < to left-justify the number
text - in the field. If we wrote f'{x:\*>10}' it would fill the field width with leading
text - \*s not spaces. Finally, if fw is the calculated the field width we could write
text - f'{x:>{fw}}' note that fw is enclosed in another {} to evaluate it.
text - 
text - ------------------------------------------------------------------------------
text - 
text - String/List/Tuple (SLT) slicing:
text - 
text - SLTs represent sequences of indexable values, whose indexes start at index 0,
text - and go up to -but do not include- the length of the SLT (which we can compute
text - using the len function).
text - 
text - 1) Indexing: We can index a SLT by writing SLT[i], where i is in the range 0
text -   to len(SLT)-1 inclusive, or i is negative and i is in the range
text -   -len(SLT) to -1 inclusive: if an index is not in these ranges, Python raises
text -   an exception. Note SLT[0] is the value store in the first index, SLT[-1] is
text -   the value stored in the last index, SLT[-2] is the value stored in the 2nd
text -   to last index.
text - 
text - 2) Slicing: We can specify a slice by SLT[i:j] which includes SLT[i] followed
text -    by SLT[i+1], ... SLT[j-1]. Slicing a string produces another string;
text -    slicing a list produces another list; and slicing a tuple produces another
text -    tuple. The resulting structures has j-i elements if indexes i through j are
text -    in the SLT (or 0 if j-i is <= 0); for this formula, both indexes must be
text -    non-negative or both positive; if they are different, convert the negative
text -    to it non-negative (or the non-negative to its negative) equivalent.
text - 
text -    If the slice contain no values it is empty (an  empty string, an empty list,
text -     and empty tuple).
text -   
text -    If the first index come before index 0, then index 0 is used; if the second
text -    index comes after the biggest index, then index len(SLT) is used. Finally,
text -    if the first index is omitted it is 0; if the second index is omitted it is
text -    len(SLT).
text - 
text -    Here are some examples
text - 
text - s = 'abcde'
text - x = s[1:3]
text - print (x)     # prints 'bc' which is 3-1 = 2 values
text - x = s[-4:-1]
text - print (x)     # prints 'bcd' same as s[1:4] which is 4-1 = 3 values
text - x = s[1:-2]
text - print (x)     # prints 'bc' same as s[1:3] which is 3-1 = 2
text - x = s[-4:-2]
text - print (x)     # prints 'bc' same as s[1:3] which is 3-1 = 2 values
text - x = s[1:10]
text - print (x)     # prints 'bcde' which is len(x)-1 = 4 values
text - 
text -    likewise
text - 
text - s = ('a','b','c','d','e')
text - x = s[1:3]
text - print (x)     # prints ('b','c') which is 3-1 = 2 values
text - x = s[-4:-1]
text - print (x)     # prints ('b','c','d') which is -1-(-4) = 3 values
text - 
text -    s[:i] is index 0 up to but not including i (can be positive or negative)
text -    s[i:] is index i up to and including the last index; so s[-2:] is the last
text -          two values.
text - 
text - 3) Slicing with a stride: We can specify a slice by SLT[i:j:k] which includes
text -    SLT[i] followed by SLT[i+k], SLT[i+2k] ... SLT[j-1]. This follows the rules
text -    for slicing specified above, and allows negative numbers for indexing.
text - 
text - s = ('a','b','c','d','e')
text - x = s[::2]
text - print (x)     # prints ('a','c','e')
text - x = s[1::2]
text - print (x)     # prints ('b','d')
text - 
text - x = s[3:1:-1]
text - print (x)     # prints ('d','c')
text - x = s[-1:1:-1]
text - print (x)     # prints ('e','d','c')
text - 
text -   When the stride is omitted, it is +1. If the stride is negative, if the first
text -   index is omitted it is len(SLT); if the last index is omitted it is -1.
text -   Compare these to the values if omitted above in part 2, which assumed a
text -   positive stride.
text - 
text - Experiment with various slices of strings (the easiest data structure to
text - experiment with) to verify you understand what results they produce.
text - 
text - ------------------------------------------------------------------------------
text - 
text - Conditional statement vs. Conditional expression
text - 
text - Python has an if/else STATEMENT, which is a conditional statement. It also has a
text - conditional EXPRESSION that uses the same keywords (if and else), which while
text - not as generally useful, sometimes is exactly the right tool to simplify a
text - programming task.
text - 
text - A conditional statement uses a boolean expression to decide which indented
text - block of statements to execute; a conditional expression uses a boolean
text - expression to decide which one of exactly two other expressions to evaluate:
text - the value of the chosen evaluated expression is the value of the conditional
text - expression.
text - 
text - The form of a conditional expression is
text - 
text -   resultT if test else resultF
text - 
text - This says, the expression evaluates to the value of resultT if test is True
text - and the value of resultF if test is False; first it evaluates test, and then
text - evaluates either resultT or resultF (but only one, not the other) as necessary.
text - Like other short-circuit operators in Python (do you know which?) it evaluates
text - only the sub-expressions it needs to compute its final value.
text - 
text - I often write conditional expressions inside parentheses for clarity (as I did
text - for lambdas; sometimes the parentheses are required, as with lambdas). See the
text - examples below.
text - 
text - Here is a simple example. We start with a conditional statement, which always
text - stores a value into min: either x or y depending on whether x <= y. Note that
text - regardless of the test, min is bound to some value.
text - 
text -   if x <= y:
text -       min = x
text -   else:
text -       min = y
text - 
text - We can write this using a simpler conditional expression, capturing the fact
text - that we are always storing into min, and just deciding which of two values to
text - store in it.
text - 
text -   min = (x if x <= y else y)
text - 
text - Not all conditional statements can be converted into conditional expressions;
text - typically only simple ones can: ones with a single statement in their indented
text - blocks, and then only if their statements are similar (see the example above).
text - But using conditional expressions in these cases simplifies the code even more.
text - So attempt to use conditional expression, but use good judgement after you see
text - what the code looks like. Write the simplest looking code.
text - 
text - Here is another example; it always prints x followed by some message
text - 
text -   if x % 2 == 0:
text -       print(x,'is even')
text -   else:
text -       print(x,'is odd')
text - 
text - We can rewrite it as one call to print, with a conditional expression inside:
text - deciding what string to print at the end.
text - 
text -       print(x, ('is even' if x%2 == 0 else 'is odd'))
text - 
text - We can also write it as a one argument print using concatenation:
text - 
text -       print(str(x) + ' is ' + ('even' if x%2 == 0 else 'odd'))
text - 
text - Note that all conditional expressions REQUIRE using BOTH THE if AND else
text - KEYWORDS (AND CANNOT USE elif) along with the test boolean expression and the
text - two expressions needed in the cases where the test is True or the test is
text - False. All conditional statements use if; some others use else or elif.
text - 
text - Using f-strings, we can write this as code as 
text - 
text -       print(f"{x} is {'even' if x%2 == 0 else 'odd'}")
text - 
text - because we can write a conditional EXPRESSION inside {}, because we can put
text - any expression there. Note that I had to write the outside string using " to
text - be able specify 'even' and 'odd' as strings INSIDE the f-string.
text - 
text - ------------------------------------------------------------------------------
text - 
text - The else: block-else option in for/while loops
text - 
text - The synax of the for and while looping statements are described as follows. The
text - else: block-else is optional, and not often used. But we will explore its
text - meaning here.
text - 
text - for\_statement   <= for index(es) in iterable:
text -                        block-body
text -                    [else:
text -                        block-else]
text - 
text - while\_statement <= while <bool-expression>:
text -                        block-body
text -                    [else:
text -                        block-else]
text - 
text - Here are the semantics of else: block-else.
text - 
text -    If the else: block-else option appears, and the loop terminated normally,
text -    (not with a break statement) then execute the block-else statements.
text - 
text - Here is an example that makes good use of the else: block-else option. This
text - code prints the first/lowest value (looking at the values 0 to 100 inclusive)
text - for which the function special\_property returns True (and then breaks out of
text - the loop); otherwise it prints that no value in this range had this property:
text - so it prints exactly one of these two messages. Note that you cannot run this
text - code, because there is no special\_property function: I'm using it for
text - illustration only.
text - 
text - for i in irange(100):
text -     if special\_property(i):
text -         print(i,'is the first value with the special property')
text -         break
text - else:
text -     print('No value in the range had the special property')
text - 
text - Without the else: block-else option (languages like Java and C/C++ don't have
text - it), the simplest code that I can write that has equivalent meaning is
text - 
text - found\_one = False
text - for i in irange(100):
text -     if special\_property(i):
text -         print(i,'is the first with the special property')
text -         found\_one = True
text -         break
text - if not found\_one:
text -     print('No value in the range had the special property')
text - 
text - This solution requires an extra name (found\_one), two assignment statements to
text - set and possibly reset the name, and an if statement. Although I came up with
text - the good example above, I have not used the else: block-else option much in
text - Python. Most programming languages that I have used don't have this special
text - feature, so I'm still exploring its usefulness. But every so often I have found
text - an elegant use of this construct. 
text - 
text - Can you predict what would happen if I removed the break statement in the
text - bigger code above? Substitute the expression i%10 == 5 for the call to
text - special\_property, and then run the code to check your answer.
text - 
text - ------------------------------------------------------------------------------
text - 
text - Argument/Parameter Matching (leaves out \*\*kargs, discussed later)
text - 
text - Recall that when functions are called, Python first evaluates the arguments,
text - then matches/binds them with parameters, and finally executes the body of the
text - function using the names of parameters (bound to the values of arguments).
text - 
text - Let's explore the argument/parameter matching rules. First we classify
text - arguments and parameters, according to the options that they include. Remember
text - that arguments appear in function CALLS and parameters appear in function
text - HEADERS (the first line in a function definition).
text - 
text - Arguments
text -   positional argument: an argument NOT preceded by the name= option
text -   named      argument: an argument preceded by the name= option
text - 
text - Parameters
text -   name-only parameter       :a parameter not followed by =default argument value
text -   default-argument parameter:a parameter followed by =default argument value
text - 
text - When Python calls a function, it must define every parameter name in the
text - function's header (binding each to the argument value object matching that
text - parameter's name, just like an assignment statement). In the rules below, we
text - will learn exactly how Python matches arguments to parameters according to
text - three criteria: positions, parameter names, and default arguments for parameter
text - names. We will also learn how to write functions that can receive an arbitrary
text - number of arguments (so, for example, we can write our own print function).
text - 
text - Here is a concise statement of Python's rules for matching arguments to
text - parameters. The rules are applied in this order (e.g., once you reach M3 we
text - cannot go back to M1).
text - 
text - Recently Python has added two other parameter options:
text - 
text - M1. Match positional argument values in the call sequentially to the
text -     parameters named in the header's corresponding positions (both name-only
text -     and default-argument parameters are OK to match). Stop when reaching any
text -     named argument in the call, or the \*parameter (if any) in the header.
text - 
text - M2. If matching a \*parameter in the header, match all remaining positional
text -     argument values to it. Python creates a tuple that stores all these
text -     arguments. The parameter name (typically args) is bound to this tuple.
text -     Only one parameter can be prefaced by \*.
text - 
text - M3. Match named-argument values in the call to their like-named parameters
text -     in the header (both name-only and default-argument parameters are OK).
text - 
text - M4. Match any remaining default-argument parameters in the header (unmatched
text -     by rules M1 and M3) with their specified default argument values.
text - 
text - M5. Exceptions: If at any time (a) an argument cannot match a parameter (e.g.,
text -     a named argument matches a parameter preceding a / (if present), or a
text -     position argument follows a \* (by itself, if present), or a
text -     positional-argument follows a named-argument) or (b) a parameter is matched
text -     multiple times by arguments; or if at the end of the process (c) any
text -     parameter has not been matched or (d) if a named-argument does not match the
text -     name of a parameter, raise an exception: SyntaxError for (a) and TypeError
text -     for (b), (c), and (d). These exceptions report that the function call does
text -     not correctly match its header by these rules.
text - 
text - [When we examine a \*\*kargs as a parameter, we will learn what Python does when
text - there are extra named arguments in a function call: names besides those of
text - parameters: preview: it puts all remaining named arguments in a dictionary, with
text - their name as the key and their value associated with that key). The parameter
text - name (typically kargs or kwargs) is bound to this dictionary.]
text - 
text - When this argument-parameter matching process if finished, Python binds, (in the
text - function's namespace), a name for every parameter and binds each to the unique
text - argument it matched by using the above rules. Passing parameters is similar to
text - performing a series of assignment statements between parameter names and their
text - matching argument values, and the matching can be quite complex.
text - 
text - If a function call raises no exception, these rules ensure that each parameter
text - in the function header matches the value of exactly one argument in the
text - function call. After Python binds each parameter name to its matching argument,
text - it executes the body of the function, which computes and returns the result of
text - calling the function.
text - 
text - Here are some examples of functions that we can call to explore the rules for
text - argument/parameter matching specified above. These functions just print their
text - parameters, so we can see the arguments bound to them (or see which exception
text - they raise). Certainly you should try others to increase your understanding.
text - 
added - ```python
code-start - def f(a,b,c=10,d=None): print(a,b,c,d)
code - def g(a=10,b=20,c=30) : print(a,b,c)
code - def h(a,*b,c=10)      : print(a,b,c)
code-end - 
added - ```
text - Call              | Parameter/Argument Binding (matching rule)
text - ------------------+--------------------------------------------
text - f(1,2,3,4)	  | a=1, b=2, c=3, d=4(M1)
text - f(1,2,3)	  | a=1, b=2, c=3(M1); d=None(M4)
text - f(1,2)		  | a=1, b=2(M1); c=10, d=None(M4)
text - f(1)		  | a=1(M1); c=10, d=None(M4);
text -                        TypeError(M5c: parameter b not matched)
text - f(1,2,b=3) 	  | a=1, b=2(M1); b=3(M3); c=10, d=None(M4)
text -                       TypeError(M5b: parameter b matched twice)
text - f(d=1,b=2)	  | d=1, b=2(M3); c=10(M4);
text -                        TypeError(M5c: parameter a not matched)
text - f(b=1,a=2)	  | b=1, a=2(M3); c=10, d=None(M4)
text - f(a=1,d=2,b=3)	  | a=1, d=2, b=3(M3); c=10(M4)
text - f(c=1,2,3)	  | c=1(M3);
text -                        SyntaxError(M5a:2, a positional argument, follows c=1,
text -                                    a named argument)
text - 
text - g()		  | a=10, b=20, c=30(M4)
text - g(b=1)		  | b=1(M3); a=10, c=30(M4)
text - g(a=1,2,c=3)	  | a=1(M3);
text -                        SyntaxError(M5a:2, a positional argument, follows a=1,
text -                                    a named argument)
text - 
text - h(1,2,3,4,5)	  | a=1(M1); b=(2,3,4,5)(M2), c=10(M4)
text - h(1,2,3,4,c=5)    | a=1(M1); b=(2,3,4)(M2), c=5(M3)
text - h(a=1,2,3,4,c=5)  | a=1(M3);
text -                        SyntaxError(M5a:2, a positional argument, follows a=1,
text -                                    a named argument)
text - h(1,2,3,4,c=5,a=1)| a=1(M1); b=(2,3,4)(M2); c=5(M3);
text -                         TypeError(M5b:a matched twice)
text - 
text - There are two recent Updates to Python (U1 and U2)
text - 
text - U1) A / may appear once in a parameter list by itself, preceded by any number
text -     of parameter names (not including \*). In such cases all preceding
text -     parameters must bind to positional arguments. So def f(a,/,b) will
text -     not match f(a=1,b=2) but will match  f(1,2) or f(1,b=2). Generally, the
text -     names of preceding parameters cannot be used in named arguments.  Rule U1
text -     is used mostly when Python calls special function written in C (which does
text -     not allow such general argument/parameter matching; this topic is not
text -     used in ICS-33).
text - 
text - U2) A \* may appear once in a parameter list by itself (in this new rule, not
text -     followed by a parameter name). In such cases, no positional arguments are
text -     collected or bound (they would be if the \* prefixed a parameter name), but
text -     all subsequent arguments must be named arguments. So def f(a,\*,b) will not
text -     match f(1,2) but will match f(1,b=2) or f(a=1,b=2) or even  f(b=2,a=1). How
text -     is this feature actually used in Python? The sorted function has the
text -     following parameter structure:
text -       sorted(iterable, \*, key=(lambda x : x), reverse = False)
text -    When we call sorted, if we want to bind either key or reverse to a
text -    non-default argument, we must specify the key/reverse parameters by named
text -    arguments: we cannot call sorted([1,2,3],(lambda x : -x)) but instead must
text -    call is as sorted([1,2,3],key = (lambda x : -x)).
text - 
text - Here is a real but simple example of using \*args, showing how the max function
text - is implemented in Python; we don't really need to write this function, because
text - it already is in Python, but here is how it is written in Python. We will cover
text - raising exceptions later in this lecture note, so don't worry about that code.
text - 
added - ```python
code-start - def max(*args) :     	   # Can refer to args inside; it is a tuple of values
code -     if len(args) == 0:
code -         raise TypeError('max: expected >=1 arguments, got 0')
code-end - 
added - ```
text -     answer = args[0]       # len not 0, so guaranteed to exist
text -     for v in args[1:]:     # slice for every value but the first (at index 0)
text -         if v > answer:     # if v is bigger than the biggest value seen,
text -             answer = v     #   update answer to be the new biggest seen
text -     return answer
text - 
text - print(max(3,-4, 2, 8))     # call of max with many arguments; prints 8
text - 
text - In fact, the real max function in Python can take either (a) any number of
text - arguments or (b) one iterable argument. It is a bit more subtle to write
text - correctly to handle both parameter structures, but here is the code. This
text - version avoids slicing (which consume extra time and space).
text - 
added - ```python
code-start - def max(*args) :     	   # Can refer to args inside; it is a tuple of values
code -     if len(args) == 0:
code -         raise TypeError('max: expected >=1 arguments, got 0')
code-end - 
added - ```
text -     if len(args) == 1:     # Assume that if max has just one argument then
text -         args = args[0]     #   it's iterable, so take the max over its values
text - 
text -     answer = None
text -     for v in args:         # Avoid slice; see how Answer is used above and below
text -         if answer == None or v > answer:
text -             answer = v
text -     return answer
text - 
text - l = (3,-4, 2, 8)
text - print(max(l))              # call of max with one iterable argument; prints 8
text - 
text - Finally, because of this approach computing max(3) raises an exception, because
text - Python expects a single argument to be iterable. It might be reasonable in this
text - case to just return 3, but that is not the semantics (meaning) of the max
text - function built into Python.
text - 
text - Here is another real example of using \*args, where I show how the print function
text - is written in Python. The myprint calls a very simple version of print, just
text - once at the end, to print only one string that it builds from args along with
text - sep and end; it prints the same thing the normal print would print with the same
text - arguments. Notice the use of the conditional if in the first line, to initialize
text - s to either '' or the string value of the first argument.
text - 
added - ```python
code-start - def myprint(*args, sep=' ', end='\n'):
code -     s = (str(args[0]) if len(args) >= 1 else '') # handle 1st (if there) special
code -     for a in args[1:]:	 	      	     	 # all others come after sep
code -         s += sep + str(a)
code -     s += end					 # with end string at the end
code -     print(s,end='')				 # print the entire string s
code-end - 
added - ```
text - myprint('a',1,'x')		   # prints a line
text - myprint('a',1,'x',sep='\*',end='E') # prints a line but stays at end
text - myprint('a',1,'x')		   # continues at end of previous line
text - 
text - Together when executed, these print
text - 
text - a 1 x
text - a\*1\*xEa 1 x
text - 
text - This is what would be printed if the print (not myprint) function was called.
text - 
text - ------------------------------------------------------------------------------
text - 
text - Constructors operating on Iterable values to Construct Data
text - 
text - List, Tuples, Sets:
text - 
text - Python's "for" loops allow us to iterate through all the components of any
text - iterable data. We can even iterate through strings: iterating over their
text - individual characters. Later in the quarter, we will study iterator protocols
text - in detail, both the special iter/next functions and the \_\_iter\_\_/\_\_next\_\_
text - methods in classes, and generator functions (which are very similar to functions
text - with a small but powerful twist). Both will improve our understanding of
text - iteration and also allow us to write our own iterable data types (classes)
text - easily. 
text - 
text - Certainly we know about using "for" loops on iterable data (as illustrated by
text - lots of code above). What I want to illustrate here is how easy it is to
text - construct lists, tuples, and sets from anything that is iterable by using the
text - list, tuple, and set constructors (we'll deal with dict constructors separately,
text - later in this section). For example, in each of the following the constructor
text - for the list/tuple/set objects iterates over the string argument to get the
text - 1-char strings that become the values in the list/tuple/set object.
text - 
text - l = list ('radar') then l is ['r', 'a', 'd', 'a', 'r']
text - t = tuple('radar') then t is ('r', 'a', 'd', 'a', 'r')
text - s = set  ('radar') then s is {'a', 'r', 'd'} or {'d', 'r', 'a'} or ...
text - 
text - Note that lists/tuples are ORDERED, so whatever the iteration order of their
text - iterator argument is, the values in the list/tuple will be the same order.
text - Contrast this with sets, which have (no duplicates and) no special order. So,
text - set('radar') can print its three different values in any order.
text - 
text - Likewise, since tuples/sets are iterable, we can also compute a list from a
text - list, a list from a a tuple, or a list from a set. Using l, t, and s from above.
text - 
text - list(t) which is ['r', 'a', 'd', 'a', 'r']
text - list(s) which is ['r', 'd', 'a']  assuming s iterates in the order 'r', 'd', 'a'
text - list(l) which is ['r', 'a', 'd', 'a', 'r']
text - 
text - The last of these iterates over the list to create a new list with the same
text - values: note that "l is list(l)" is False, but "l == list(l)" is True: there are
text - two different lists, but they store the same contents (see the next section on
text - the details of "is" vs "==" for more discussion).
text - 
text - Likewise we could create a tuple from a list/set, or a set from a list/tuple.
text - All the constructors handle iterable data, producing a result of the specified
text - type by iterating over their argument. 
text - 
text - Note that students sometimes try to use a list constructor to create a list with
text - one value. They write list(1), but Python responds with "TypeError: 'int' object
text - is not iterable" because the list constructor is expecting an iterable argument.
text - The correct way to construct a new list with one value is [1]. Likewise for
text - tuples, sets, and dictionaries.
text - 
text - We have seen that opened file objects are also iterable, where each line in the
text - file is iterated over. So the statement
text - 
text -   contents =  list(open('test.txt'))
text - 
text - will result in contents binding to a list containing all the lines in the file
text - (with the special newline ('\n') character at the end of each line).
text - 
text - Program #1 will give you lots of experience with these data types and when and
text - how to use them. The take-away now is it is trivial to convert from one of these
text - data types to another, because the constructors for their classes all allow
text - iterable values as their arguments, and all these data types (and strings as
text - well) are iterable (can be iterated over).
text - 
text - 
text - Dictionary Constructors:
text - 
text - Before leaving this topic, we need to look at how dictionaries fit into the 
text - notion of iterable. There is NOT JUST ONE way to iterate through dictionaries,
text - but there are actually THREE ways to iterate through dictionaries: by keys, by
text - values, and by items (each item a 2-tuple with a key followed by its associated
text - value). 
text - 
text - We indicate how we want to iterate over a dictionary by calling a method name:
text - keys, values, or items. When called, these methods very quickly return a VIEW
text - of a dictionary. Views do not create a new data structure (hence, "quickly")
text - but, they allow us to iterate over just the keys of a dictionary (the same as
text - iterating over the dictionary itself), or just the associated values in a
text - dictionary, or 2-tuples containing each key and its associate value. Since
text - Python 3.6, Python established complicated rules determining the iteration
text - ORDER of sets and dictionaries, but it is best to not assume anything about
text - this order.
text - 
text - So if we write the following to bind d to a dict (we will discuss this "magic"
text - constructor soon)
text - 
text - d = dict(a=1,b=2,c=3,d=4,e=5) # the same as d = {'a':1,'b':2,'c':3,'d':4,'e':5}
text - 
text - Then we can create lists of three aspects of the dict:
text - 
text - list(d.keys  ()) is like ['c', 'b', 'a', 'e', 'd']
text - list(d.values()) is like [3, 2, 1, 5, 4]
text - list(d.items ()) is like [('c', 3), ('b', 2), ('a', 1), ('e', 5), ('d', 4)]
text - 
text - I said "is like" because sets and dicts are NOT ORDERED: in the first case we
text - get a list of the keys; in the second a list of the values; in the third a list
text - of item tuples, where each tuple contains one key and its associated value. But
text - in all three cases, the list's values can appear in ANY ORDER.
text - 
text - Note that the keys in a dict are always unique, but there might be duplicates
text - among the values: try the code above with d = dict(a=1,b=2,c=1). Items are
text - unique because they contain keys (which are unique).
text - 
text - Also note that if we iterate over a dict without specifying how, it is
text - equivalent to specifying d.keys(). That is
text - 
text - list(d) is the same as list(d.keys()) which is like ['a', 'c', 'b', 'e', 'd']
text - 
text - One way to construct a dict is to give it an iterable, where each value
text - is either a 2-tuple or 2-list: a key followed by its associated value. So, we
text - could have written any of the following to initialize d:
text - 
text - d = dict( [['a', 1], ['b', 2], ['c', 3], ['d', 4], ['e', 5]] ) #list of  2-list
text - d = dict( [('a', 1), ('b', 2), ('c', 3), ('d', 4), ('e', 5)] ) #list of  2-tuple
text - d = dict( (['a', 1], ['b', 2], ['c', 3], ['d', 4], ['e', 5]) ) #tuple of 2-list
text - d = dict( (('a', 1), ('b', 2), ('c', 3), ('d', 4), ('e', 5)) ) #tuple of 2-tuple
text - 
text - or, even (a tuple that has a mixture of 2-tuples and 2-lists in it)
text - 
text - d = dict( (('a', 1), ['b', 2], ('c', 3), ['d', 4], ('e', 5)) )
text - 
text - or even (a set of 2-tuples; we cannot have a set of 2-list (see hashable below)
text - 
text - d = dict( {('a', 1), ('b', 2), ('c', 3), ('d', 4), ('e', 5)} )
text - 
text - The bottom line is that a positional dict argument must be iterable, and each
text - value in the iterable must have 2 values (e.g., a 2-list or 2-tuple) that
text - represent a key followed by its associated value.
text - 
text - We can also combine the two forms writing 
text - d = dict( {('a', 1), ('b', 2), ('c', 3), ('d', 4), ('e', 5)}, f=6, g=7)
text - 
text - Finally, if we wanted to construct a dict using the keys/values in another
text - dict, here are two easy ways to do it
text - 
text - d\_copy = dict(d)
text - 
text - or
text - 
text - d\_copy = dict(d.items())
text - 
text - In both cases the dict constructor creates a new dictionary by iterating through
text - the (key,value) 2-tuples.
text - 
text - ------------------------------------------------------------------------------
text - 
text - Sharing/Copying:  is vs. ==
text -   (one more time: see Binding/Drawing Names and their associated Objects
text - 
text - It is important to understand the fundamental difference between two names
text - sharing an object (bound to the same object) and two names referring/bound to
text - "copies of the same object". Note that if we mutate a shared object, both names
text - "see" the change: both are bound to the same object which has mutated. But if
text - they refer to different copies of an object, only one name "sees" the change.
text - 
text - Note the difference between the Python operators is and ==. Both return boolean
text - values. The first asks whether two references/binding are to the same object
text - (the is operator is called the (object)identity operator); the second asks only
text - whether the two objects store the same values. See the different results
text - produced for the example below. Also note that if x is y is True, then x == y
text - must be True too: an object ALWAYS stores the same values as itself. But if
text - x == y is True, x is y may or may not be True.
text - 
text - For example, compare execution of the following scripts: the only difference
text - is the second statement in each: y = x vs. y = list(x)
text - 
text - x = ['a']
text - y = x		# Critical: y and x share the same reference
text - print('x:',x,'y:',y,'x is y:',x is y,'x == y:',x==y)
text - x [0] = 'z'	# Mutate x (could also append something to it)
text - print('x:',x,'y:',y,'x is y:',x is y,'x == y:',x==y)
text - 
text - This prints
text - x: ['a'] y: ['a'] x is y: True x == y: True
text - x: ['z'] y: ['z'] x is y: True x == y: True
text - 
text - x = ['a']
text - y = list(x)	# Critical: y refers to a new list with the same contents as x
text - print('x:',x,'y:',y,'x is y:',x is y,'x == y:',x==y)
text - x [0] = 'z'	# Mutate x (could also append something to it: x+)
text - print('x:',x,'y:',y,'x is y:',x is y,'x == y:',x==y)
text - 
text - This prints
text - x: ['a'] y: ['a'] x is y: False x == y: True
text - x: ['z'] y: ['a'] x is y: False x == y: False
text - 
text - You might have learned about the id function: it returns a unique integer for
text - any object. It is often implemented to return the first address in memory at
text - which an object is stored, but there is no requirement that it returns this int.
text - Checking "a is b" is equivalent to checking "id(a) == id(b)" but the first way
text - to do this check (with the is operator) is preferred (and is faster).
text - 
text - Finally there is a copy module in Python that defines a copy function: it
text - copies some iterable without us having to specify the specific constructor
text - (like list, set, tuple, or dict).
text - 
text - So we can import it as: from copy import copy
text - 
text - Assuming x is a list, we can replace y = list(x) by y = copy(x).
text - Likewise, if x is a dict we can replace y = dict(x) by y = copy(x).
text - 
text - We could also just: import copy (the module) and then write y = copy.copy(x)
text - but it is clearer in this case to write "from copy import copy". Here is a
text - simple implementation of copy:
text - 
added - ```python
code-start - def copy(x):
code -     return type(x)(x)
code-end - 
added - ```
text - which finds the type of x, then calls it as a constructor to construct a new
text - object of that type, initialized by x: iterating over all the values in x, as
text - we say in examples list list('abc') producing ['a', 'b', 'c'].
text - 
text - ----------
text - 
text - Note that copying in all the ways that we have discussed is SHALLOW. That means
text - the "copy" is a new object, but that object stores all the references from the
text - "object being copied". For example, if we write
text - 
text -   x = [1, [2]]
text -   y = list(x) # or y = copy(x) or y = x[:]
text - 
text - then we would draw the following picture for these assignments.
text - 
text -                 list              list
text -             (-----------)      (-------)        
text -   x         |   0   1   |      |   0   |        int
text - +---+	    | +---+---+ |      | +---+ |       (---)
text - | --+------>| | | | --+-+----->| | --+-+-----> | 2 |
text - +---+	    | +-+-+---+ |      | +---+ |       (---)
text - 	    (---+-------)      (-------)
text -                 |                  ^
text - 		v	           |
text - 	       int		   |
text - 	      (---)		   |
text - 	      | 1 |<--------+	   |
text - 	      (---)	    |	   |
text - 	      		    |	   |
text -                 list	    |	   |
text -             (-----------)   |	   |
text -   y         |   0   1   |   |	   |
text - +---+	    | +---+---+ |   |	   |
text - | --+------>| | | | --+-+---+------+
text - +---+	    | +-+-+---+ |   |
text - 	    (---+-------)   |
text -                 |           |
text -                 +-----------+
text - 
text - Here, x and y refer to DIFFERENT lists: but all the references in x's list are
text - identical to the references in y's list. Using the "is" operator we can state
text - that (1) x[0] is y[0] == True, and (2) x[1] is y[1] == True.
text - 
text - This means if we now write x[1][0] = 'a', then print(y[1][0]) prints 'a' too,
text - because x[1] and y[1] refer to the same list.
text - 
text - We will discuss how to do DEEP copying when we discuss recursion, later in the
text - course; it is implemented in the copy module by the function named deepcopy,
text - which does a deep copy of an object, by constructing an object of that type and
text - populating it with deep copies of all the objects in the original object.
text - 
text - But, if we wrote
text -   from copy import deepcopy
text -   x = [1, [2]]
text -   y = deepcopy(x)
text - 
text - then we would draw the following picture for these assignments.
text - 
text -                 list              list
text -             (-----------)      (-------)        
text -   x         |   0   1   |      |   0   |       int
text - +---+	    | +---+---+ |      | +---+ |       (---)
text - | --+------>| | | | --+-+----->| | --+-+-----> | 2 |
text - +---+	    | +-+-+---+ |      | +---+ |       (---)
text - 	    (---+-------)      (-------)         ^
text -                 |                                |
text - 		v                                |
text - 	       int                               |
text - 	      (---)				 |
text - 	      | 1 |<--------+                    |
text - 	      (---)	    |                    |
text - 	      		    |                    |
text -                 list	    |      list          |
text -             (-----------)   |   (-------)        |
text -   y         |   0   1   |   |   |   0   |        |
text - +---+	    | +---+---+ |   |   | +---+ |        |
text - | --+------>| | | | --+-+---+-->| | --+-+--------+
text - +---+	    | +-+-+---+ |   |   | +---+ |
text - 	    (---+-------)   |   (-------)
text -                 |           |   
text -                 +-----------+
text - 
text - In a deep copy, all the mutable objects referred to are copied, but the
text - immutable ones referred to are not copied. We will return to deep copying
text - when we discuss recursion (which is how it is implemented).
text - 
text - For more information on mutable/immutable, see the next section.
text - 
text - ------------------------------------------------------------------------------
text - 
text - Hashable vs. Mutable and how to Change Things:
text - 
text - Python uses the term Hashable, which has the same meaning as Immutable. So
text - hashable and mutable are OPPOSITES: You might see this message relating to
text - errors when using sets with UN-HASHABLE values or dicts with UN-HASHABLE keys:
text - since hashable means immutable, then un-hashable means un-immutable which
text - simplifies (the two negatives cancel) to mutable. So, un-hashable means the same
text - as mutable. 
text - 
text - hashable     means the same as immutable
text - un-hashalble means the same as mutable
text - mutable      means the same  as not immutable
text - 
text - Here is a quick breakdown of standard Python types
text - 
text - Hashable/immutable : numeric values, strings, tuples containing
text -                        hashable/immutable data, frozensets
text - mutable/un-hashable: list, sets, dict
text - 
text - The major difference between tuples and lists in Python is the former is
text - (mostly) hashable/immutable and the later is not. I say mostly because if a
text - tuple contains mutable data, you can mutate the mutable data part. So
text - 
text -   x = (1, 2, [3, 4])
text -   x[2][0] = 'a'
text -   print(x)
text - 
text - prints 
text - 
text -   (1, 2, ['a', 4])
text - 
text - So you have mutated the tuple because you mutated the list stored in index 2
text - of the tuple. On the other hand, you cannot append to a tuple, nor store a new
text - value in one of its indexes (x[0] = 10 is not allowed).
text - 
text - So technically, a tuple storing hashable/immutable values is hashable/immutable,
text - but a tuple storing un-hashable/mutable values is un-hashable/mutable. So if
text - some other datatype (e.g., values in a set, or keys in a dictionary) needs to be
text - hashable/immutable, use a tuple (storing hashable/immutable values) to
text - represent its value, not a list. Thus we cannot say, "Any value whose type is a
text - tuple is hashable/immutable." Instead we must say, "A value whose type is a
text - tuple is hashable/immutabile when the tuple stores only hashable/immutable
text - values."
text - 
text - A frozenset can do everything that a set can do, but doesn't allow any mutator
text - methods to be called (so we cannot add a value to or delete a value from a
text - frozenset). Thus, we can use a frozen set as a value in a set or as a key in a
text - dictionary.
text - 
text - The constructor for a frozenset is frozenset(...) not {}. Note that once you've
text - constructed a frozen set you cannot change it (because it is immutable). If you
text - have a set s and need an equivalent frozenset, just write frozenset(s).
text - 
text - The function hash in Python takes an argument that is hashable (otherwise it
text - raises TypeError, with a message about a value from an un-hashable type) and
text - returns an int.
text - 
text - We will study hashing towards the end of the quarter: it is a technique for
text - allowing very efficient operations on sets and dicts. ICS-46 (Data Structures)
text - studies hash tables in much more depth, in which you will implement the
text - equivalent of Python sets and dicts by using hash tables.
text - 
text - ------
text - Interlude: unique objects
text - 
text - Small integer objects are unique in Python. If we assign x = 1 and y = 1, then
text - x is y evaluates to True (but you should use == and != for comparing integers).
text - Likewise, if we write x += 1 and y \*= 2 then x is y evaluates to True. But if
text - we write x = 10\*\*100 and y = 10\*\*100. then x is y is False (but x == y is True, 
text - s expected).
text - 
text - To save space, Python allocates only one object for each small int. When a small
text - int is computed, Python first looks to see if an object with that value already
text - exists, and if it does, returns a reference to it; if it doesn't, it makes a new
text - object storing that values and returns a reference to it. This is a tradeoff
text - that minimizes the space (occupied by objects) but increases the time to do
text - computations (by having to first look to see if an object for a value already
text - exists). I don't know what rule Python uses to decide to re-use int objects.
text - 
text - Generally, because ints are immutable, there are no bad consequences in sharing
text - objects in this way: once a name is bound to an int object, that int object
text - will always represent the same value, because ints are immutable. So long as you
text - compare ints with ==, your code should work correctly for small and big ints.
text - 
text - Again, contrast this with writing lists, which always allocate new objects. If
text - we write 
text - 
text - x = ['a']
text - y = ['a']
text - 
text - Then x is y evaluates to False. Because lists are mutable, this is the only
text - correct way to implement lists.
text - 
text - The situation with strings (also immutable) is similar but even more
text - complicated. Whether "is" operating on two strings with the same characters is
text - True depends on things like the length of the strings and whether the string
text - was specified in code, computed in code, or entered by the user in the console.
text - If we write
text - 
text - x = 'ab'
text - y = 'a'+'b'
text - z = input('Enter string') # and enter the string ab when prompted
text - 
text - x is y evaluates to True, but x is z evaluates to False. As with integers, it is
text - best to compare strings with == and != and not use the "is" or "is not"
text - operators on them.
text - 
text - ------------------------------------------------------------------------------
text - ------------------------------------------------------------------------------
text - ------------------------------------------------------------------------------ 
text - 
text - End of 2nd Lecture on this material
text - 
text - ------------------------------------------------------------------------------
text - ------------------------------------------------------------------------------
text - ------------------------------------------------------------------------------ 
text - 
text - Comprehensions: list, tuple, set, dict
text - 
text - List and Set Comprehensions (Dict/Tuple comprehensions latter):
text - 
text - Comprehensions are compact ways to create complicated (but not too complicated)
text - lists, tuples, sets, and dicts. That is, they compactly solve some problems but
text - cannot solve all problems (for example, we cannot use them to mutate values in
text - an existing data structure, just to create values in a new data structure). The
text - general form of a list comprehension is as follows, where f means any function
text - using var (or expression using var: we can also write just var there because a
text - name by itself is a very simple expression) and p means any predicate (or bool
text - expression) using var.
text - 
text - [f(var,...) for var in iterable if p(var,...)]
text - 
text - Meaning: collect together into a list (list because of the outer []) all of
text - f(var,...) values, for var taking on every value in iterable, but only collect
text - an f(var,...) value if its corresponding p(var,...) is True.
text - 
text - For tuple or set comprehensions, we would use () and {} as the outermost
text - grouping symbol instead of []. We'll talk about dicts later in this section:
text - they use also use {} but also include a : inside (separating keys from values)
text - to be distinguished from sets, which use {} without any such : inside.
text - 
text - Note that the "if p(var,...)" part is optional, so we can also write the
text - simplest comprehensions as follows (in which case it has the same meaning as
text - p(var,...) always being True).
text - 
text - [f(var,...) for var in iterable]
text - 
text -    which has the same meaning as
text - 
text - [f(var,...) for var in iterable if True]
text - 
text - for example
text - 
text - x = [i\*\*2 for i in irange(1,10) if i%2==0]	# note: irange not range
text - print(x)
text - 
text - prints the squares of all the integers from 1 to 10 inclusive, but only if the
text - integer is even (computed as leaving a remainder of 0 when divided by 2). Run 
text - it. Change it a bit to get is to do something else. Here is another example
text - 
text - x = [2\*c for c in 'some text' if c in 'bcdfghjklmnpqrstvwxz']
text - print(x)
text - 
text - which prints a list with strings with doubled characters for all the consonants
text - (no aeiouy -or spaces for that matter) in the string 'some text':
text - ['ss', 'mm', 'tt', 'xx', 'tt'].
text - 
text - We can translate any list comprehension into equivalent code that uses more
text - familiar Python looping/if/list appending features.
text - 
text - x = []	       	  	   # start with an empty list
text - for var in iterable:	   # iterate through iterable
text -    if p(var):		   # if var is acceptable?
text -        x.append(f(var))	   # add f(var) next in the list
text - 
text - But often using a comprehension (in the right places: where you want to
text - create from scratch some list, tuple, set or dict) is simpler. Not all lists
text - that we build can be written as simple comprehensions, but the ones that can
text - are often very simple to write, read, and understand when written as
text - comprehensions. They tend to be more efficient too.
text - 
text - What comprehensions aren't good for is putting information into a data structure
text - and then mutating/changing it during the execution of the comprehension; for
text - that job you need code that is more like the for loop above. So when deciding
text - whether or not to use a comprehension, ask yourself if you can specify each
text - value in the data structure once, without changing it (as was done above, using
text - comprehensions). Or try to write the code as a comprehension first; if you
text - succeed, you'll have a compact to compute your results; if you fail, then try
text - to write it using more complicated statements in Python.
text - 
text - Note that we can add-to (mutate) lists, sets, and dicts, but not tuples. For
text - tuples we would have to write this code with x = () at the top and
text - x = x + (var,) in the middle: which builds an entirely new tuple by
text - concatenating the old one and a one-tuple (containing only x) and then binding
text - x to the newly constructed tuple. For very large tuples, this process is very
text - slow. Don't worry about these details, but understand that unlike lists and
text - sets, tuples have no mutator methods: so x.append(...)/x.add(...) is not
text - allowed in Python.
text - 
text - Here is something interesting (using a set comprehension: notice {} around the
text - comprehension.
text - 
text - x = {c for c in "I've got plenty of nothing"}	# note ' in str delimited by "
text - print(sorted(x))
text - 
text - It prints a set of characters (printing in a list, in sorted order, created by
text - sorted(x)) in the string but because it is a set, each character occurs one
text - time). So even though a few c's have the same value, only one of each appears in
text - the set because of the semantics/meaning of sets. Note it prints as as the list
text - 
text - [' ', "'", 'I', 'e', 'f', 'g', 'h', 'i', 'l', 'n', 'o', 'p', 't', 'v', 'y']
text - 
text - because sorted takes the iterable set created and produces a sorted list as a
text - result.
text - 
text - If we used a list comprehension instead of a set comprehension, the result would
text - be much longer because, for example, the character 't' would occur 3 times in a
text - list (but occurs only once in a set).
text - 
text - Note the following: binding values to for loop variables INSIDE of
text - comprehensions does not affect the same variable NAME OUTSIDE the scope of
text - the comprehension. So when running
text - 
text - x = 'ABC'     	       		x = 'ABC'
text - for x in range(1,5):		y = [x for x in range(1,5)]
text -    pass	 			print(x)
text - print(x)
text - 
text - the left code prints 4 but the right code prints ABC. In the right code, the
text - x outside the comprehension is considered a different variable than the x
text - inside the comprehension. The reason is similar to why
text - 
added - ```python
code-start - x = 'ABC'
code - def f():
code -   x = 1
code - f()
code - print(x)
code-end - 
added - ```
text - also prints ABC after the function call. So, a comprehension (like a function)
text - creates its own scope for the comprehension's index variable. A for loop does
text - not create its own scope, but a comprehension does.
text - 
text - 
text - Dict Comprehensions:
text - 
text - The form for dict comprehensions is similar, here k and v are functions (or
text - expressions) using var. Notice the {} on the outside and the : on the inside,
text - separating each key from its assocated value. That is how Python knows the
text - comprehension is a dict not a set.
text - 
text - {k(var,...) : v(var,...) for var in iterable if p(var,...)}
text - 
text - So,
text - 
text - x = {k : len(k) for k in ['one', 'two', 'three', 'four', 'five']}
text - print(x)
text - 
text - prints a dictionary that stores keys that are these five words whose associated
text - values are the lengths of these words. Because dicts aren't ordered, it could
text - print as {'four': 4, 'three': 5, 'one': 3, 'five': 4, 'two': 3}
text - 
text - Finally, we can write a nested comprehension, although they are harder to
text - understand than simple comprehensions.
text - 
text - x = {c for word in ['i', 'love', 'new', 'york'] for c in word if c not in 'aeiou'}
text - print(x)
text - 
text - It says to collect c's, by looking in each word in the list, and looking at
text - each character c in each word: so the c collected at the beginning is the c
text - being iterated over in the second part of the comprehension (for c in word...).
text - 
text - It prints a set of each different letter that is not a vowel, in each word
text - in the list. I could produce this same result by rewriting the outer part of the
text - comprehension as a loop, but leaving the inner one as a comprehension (union
text - merges two sets: does a bunch of adds).
text - 
text - x = set()                    # empty set: cannot use {} which is an empty dict
text - for word in ['i', 'love', 'new', 'york']:
text -     x = x.union( {c for c in word if c not in 'aeiou'} )
text - print(x)
text - 
text - or write it with no comprehensions at all
text - 
text - x = set()
text - for word in ['i', 'love', 'new', 'york']:
text -     for c in word:
text -         if c not in 'aeiou':
text -             x.add(c)
text - print(x)
text - 
text - So which of these is the most comprehendable: the pure comprehension, the
text - hybrid loop/comprehension, or the pure nested loops? What is important is that
text - we know how all three work, can write each correctly in any of these ways,
text - and then we can decide afterwords which way we want the code to appear. As we
text - program more, our preferences might change. I'd probably prefer the first one
text - (because I've seen lots of double comprehensions), but the middle one is
text - certainly reasonable. If you write the last one, I might think that you just
text - don't know how to use comprehensions.
text - 
text - What do you think the following nested (in a different way) comprehension
text - produces?
text - 
text - x = {word : {c for c in word} for word in ['i', 'love', 'new', 'york']}
text - 
text - Check your answer by executing this code in Python and printing x.
text - 
text - Finally, here is a version of myprint (written above in the section on the
text - binding of arguments and parameters) that uses a combination of the .join
text - function and a comprehension to create the string to print simply.
text - 
text - The .join function (discussed more in depth below) joins all the string values
text - in an iterable into a string using the prefix operand as a separator: so,
text - '--'.join( ['My', 'dog', 'has', 'fleas'] )
text - returns the string 'My--dog--has--fleas'
text - 
added - ```python
code-start - def myprint(*args,sep=' ',end='\n'):
code -     s = sep.join(str(x) for x in args)+end       # create string to print
code -     print(s,end='')				 # print the string
code-end - 
added - ```
text - WARNING: Once students learn about comprehensions, sometimes they go a bit
text - overboard as they learn about/use this feature. Here are some warning signs:
text - When writing a comprehension, you should (1) use the result produced in a later
text - part of the computation and (2) typically not mutate anything in the
text - comprehension. If the purpose of your computation is to mutate something,
text - don't use a comprehension. Over time you will develop good instincts for when
text - to use comprehensions.
text - 
text - 
text - Tuple Comprehensions are special:
text - 
text - The result of a tuple comprehension is special. You might expect it to produce
text - a tuple, but what it does is produce a special "generator" object that we can
text - iterate over. We will discuss generators in detail later in the quarter, so for
text - now we will examine just some simple examples. Given the code
text - 
text - x = (i for i in 'abc')  # tuple comprehension
text - print(x)
text - 
text - You might expect this to print as ('a', 'b', 'c') but it prints as
text - <generator object <genexpr> at 0x02AAD710>
text - 
text - The result of a tuple comprehension is not a tuple: it is actually a generator.
text - The only thing that you need to know now about a generator is just that you can
text - iterate over it, but ONLY ONCE. So, given the code
text - 
text - x = (i for i in 'abc')
text - for i in x:
text -     print(i)
text - for i in x:
text -     print(i)
text - 
text - it prints
text - a
text - b
text - c
text - 
text - Yes, it prints a, b, c and just prints it once: after the first loop finishes,
text - the generator is exhausted so the second loop prints no more values. We will
text - spend a whole lecture studying the details of generators later in the quarter.
text - 
text - Specifically, if x is defined as above, we cannot call len(x) or x[1]: it is
text - not a tuple, it is a generator. All we can do it iterate over x.
text - 
text - Recall our discussion of changing any iterable into a list, tuple, or set by
text - iterating over it; we can iterate over a tuple comprehension. So if we wrote
text - t  = tuple(x) or t = tuple( (i for i in 'abc') ), then print(t) would print
text - ('a', 'b', 'c'). In fact, we could even write t = tuple(i for i in 'abc')
text - because (1) by default, comprehensions are tuple comprehensions; (2) in a
text - function call with exactly one argument, we can omit the () specifying a tuple
text - comprehension (but with multiple arguments we must supply the parentheses).
text - 
text - Of course, we could also write things like :
text - l = list(i for i in 'abc')
text - s = set (i for i in 'abc')
text - 
text - but these are equivalent to writing the standard comprehensions more simply
text - (and efficiently) as:
text - l = [i for i in 'abc']
text - s = {i for i in 'abc'}
text - 
text - ------------------------------------------------------------------------------
text - 
text - Nine Important/Useful Functions: split/join, any/all, sum/min/max, zip/enumerate
text - 
text - The split/join methods
text - 
text - Both split and join are methods in the str class: if s is some string, we call
text - them by writing s.split(...) or s.join(....)
text - 
text - The split method also takes one str argument as .... and the result it returns
text - is a list of of str. For example 'ab;c;ef;;jk'.split(';') returns the list of
text - str
text - 
text - ['ab', 'c', 'ef', '', 'jk']
text - 
text - It uses the argument str ';' to split up the string (the one prefixing the
text - .split call), into slices that come before or after every ';'.
text - 
text - Note that there is an empty string between 'ef' and 'j' because two adjacent
text - semi-colons appear between them in the string. If we wanted to filter out such
text - empty strings, we can easily do so by embedding the call to split inside a
text - comprehension: writing [s for s in 'ab;c;ef;;jk'.split(';') if s != '']
text - produces the list ['ab', 'c', 'ef', 'jk'].
text - 
text - Because the prefix and regular arguments are both strings, students sometime
text - reverse these two operands: what does ';'.split('ab;c;ef;;jk') produce and why?
text - 
text - The split method is very useful to call after reading lines of text from a file,
text - to parse (split) these lines into their important constituent information.
text - You will use split in all 5 parts of Programming Assignment #1.
text - 
text - -----
text - 
text - The join method also takes one iterable argument (it must produce str values) 
text - as ....; the result it returns is a str. For example
text - ';'.join(['ab', 'c', 'ef', '', 'jk']) returns the str
text - 
text - 'ab;c;ef;;jk'
text - 
text - It merges all the strings produced by iterating over its argument into one big
text - string, with all the strings produced by iterating over its argument
text - concatenated together and separated from each other by the ';' string.
text - 
text - So, split and join are opposites. Unfortunately, the splitting/joining string
text - ';' appears as the argument inside the () in split, but it appears as the prefix
text - argument before the call in join. This inconsistency can be confusing.
text - 
text - ------------------------------------------------------------------------------
text - 
text - The all/any functions (and their use with tuple comprehensions)
text - 
text - The "all" function takes one iterable argument (and returns a bool value): it
text - returns True if ALL the bool values produced by the iterable are True; it can
text - stop examining values and return a False result when the first False is produced
text - (ultimately, if no False is produced it returns True).
text - 
text - The "any" function takes one iterable argument (and returns a bool value): it
text - returns True if ANY the bool values produced by the iterable are True; it can
text - stop examining values and return a True result when the first True is produced
text - (ultimately, if no True is produced it returns False).
text - 
text - These functions can be used nicely with tuple comprehensions. For example, if
text - we have a list l of numbers, and we want to know whether all these numbers are
text - prime, we can call
text - 
text -   all( predicate.is\_prime(x) for x in l )
text - 
text - which is the same as calling
text - 
text -   all( (predicate.is\_prime(x) for x in l) )
text - 
text - and similar (but more time/space-efficient) than calling
text - 
text -   all( [predicate.is\_prime(x) for x in l] )
text - 
text - The list comprehension computes the entire list of boolean values and then
text - "all" iterates over this list. When "all" iterates over a tuple comprehension,
text - the tuple comprehension computes values one at a time and "all" checks each: if
text - one is False, the tuple comprehension returns False immediately and does not
text - have to compute any further values in the tuple comprehension.
text - 
text - -----Efficiency Interlude
text - The tuple comprehension version can be much more efficient, if a False value is
text - followed by a huge number of other values.
text - 
text - For example, calling
text - 
text - all( predicate.is\_prime(x) for x in range(2,1000000) )
text - 
text - immediately returns False because it checks whether 2 is prime (True), 3 is
text - prime (True), 4 isn't prime (False: so all finishes executing, returning False).
text - 
text - But calling
text - 
text - all( [predicate.is\_prime(x) for x in range(2,1000000)] )
text - 
text - takes a very long time to return False. It first determines whether every value
text - from 2 to 1,000,000 is prime (putting all these boolean values in the list
text - comprehension), and then looks at the first (True), the second (True), the
text - third (False: all finishes executing, returning False).
text - 
text - This illustrates the very important difference between tuple comprehension and
text - other kinds of comprehensions.
text - -----End: Efficiency Interlude
text - 
text - Likewise for the "any" function, which produces True the first time it examines
text - a True value.
text - 
text - Here is how we can write these functions, which search for a False or a True
text - respectively:
text - 
added - ```python
code-start - def all(iterable):
code -     for v in iterable:
code -         if v == False:
code -             return False  # something was False; return False immediately
code -     return True           # nothing   was False; return True after loop
code-end - 
added - ```
added - ```python
code-start - def any(iterable):
code -     for v in iterable:
code -         if v == True:
code -             return True   # something was True; return True immediately
code -     return False          # nothing   was True; return False after loop
code-end - 
added - ```
text - Read these functions. What does each produce if the iterable produces no values?
text - Why is that reasonable?
text - 
text - ------------------------------------------------------------------------------
text - 
text - The sum/max/min functions (and their use with tuple comprehensions)
text - 
text - The simple versions of the sum function takes one iterable argument. The sum
text - function requires that the iterable produce numeric values that can be added.
text - It returns the sum of all the values produced by the iterable; if the iterable
text - argument produces no values, sum returns 0. Actually, we can supply a second
text - argument to the sum function; in this case, that value will be returned in the
text - special case when the iterable produces no values; if the iterable does produce
text - any values, the sum will be the actual sum plus this argument. We can think of
text - sum as defined by
text - 
added - ```python
code-start - def sum(values, init_sum=0):	alternative definition	def sum(values, sum=0):
code -     result = init_sum			    		    for v in values:
code -     for v in values:					        sum += v
code -         result += v					    return sum
code -     return result
code-end - 
added - ```
text - The simple versions of the max and min functions also each take one iterable
text - argument.
text - 
text - The min/max functions require that the iterable produce values that can be
text - compared with each other; so calling min([2,1,3]) returns 1; and calling
text - min(['b','a','c']) returns 'a';  but calling min([2,'a',3]) raises a TypeError
text - exception because Python cannot compare an integer to a string. These functions
text - return the minimum/maximum value produced by their iterable argument; if the
text - iterable produces no values (e.g., min([]), min and max each raise a ValueError
text - exception, because there is no minimum/maximum value that it can compute/return.
text - 
text - There are two more interesting properties to learn about the min and max
text - functions.
text - 
text - First, we can also call min/max specifying any number of arguments or if one
text - argument, it must be iterable: so calling min([1,2,3,4]) -using a tuple which it
text - iterable- produces the same result as calling min(1,2,3,4) -using 4 arguments.
text - 
text - We can also specify a named argument in min/max: a key function just like the
text - key function used in sort/sorted. The min/max functions return the
text - smallest/largest value in its argument(s), but if the key function is supplied,
text - it compares two values by calling the key function on each, not by directly
text - comparing these two values (as sort/sorted did).
text - 
text - For example, min('abcd','xyz') returns 'abcd', because 'abcd' < 'xyz'. But if
text - we instead wrote min('abcd','xyz',key = (lambda x : len(x)) ) it would return
text - 'xyz' because len('xyz') < len('abcd'): that is, it compares the key function
text - applied to all values in the iterable to determine which value to return.
text - 
text - Note that min('abcd','wxyz',key = (lambda x : len(x)) ) will return 'abcd'
text - because it is the first value that has the smallest result when the key
text - function is called: the key function returns 4 for both values. We can think of
text - the min function operating on an iterable to be written as follows (although we
text - will learn Python features later on that simplifies the actual definition of
text - this function)
text - 
added - ```python
code-start - def min(*args,key = (lambda x : x) ): # default key is the identity function
code -     if len(args) == 0:
code -         raise TypeError('min: expected >=1 arguments, got 0')
code -     if len(args) == 1:     # Assume that if min has just one argument
code -         args = args[0]     #   it's iterable, so take the max over its values
code-end - 
added - ```
text -     answer = None
text -     for v in args:
text -         key\_of\_v = key(v)
text -         if answer == None or key\_of\_v < key\_of\_answer:
text -             answer = v
text -             key\_of\_answer = key\_of\_v
text - 
text -     return answer
text - 
text - Calling min( ('abc','def','gh','ij'),key = (lambda x : len(x)) ) returns 'gh'.
text - Note that the default value for the key function is a lambda that returns the
text - value it is passed: this is called the identity function/lambda. 
text - 
text - ------------------------------------------------------------------------------
text - 
text - The zip and enumerate functions
text - 
text - Zip:
text - 
text - There is a very interesting function called zip that takes an arbitrary number
text - of iterable arguments and zips/interleaves them together (like a zipper does
text - for the teeth on each side). Let's start by looking at just the two argument
text - version of zip.
text - 
text - What zip actually produces is a generator -the ability to get the results of
text - zipping- not the result itself. See the discussion above about how tuple
text - comprehensions produce generators.
text - 
text - So to "get the result itself" we should use a for loop, or constructor (as shown
text - in most of the examples below), or comprehension to iterate over the generator
text - result of calling zip. The following code
text - 
text - z = zip( 'abc', (1, 2, 3) )           # String and tuple are iterator arguments
text - print('z:', z, 'list of z:', list(z))
text - 
text - prints
text - 
text - z: <zip object at 0x02A4D990> list of z: [('a', 1), ('b', 2), ('c', 3)]
text - 
text - Here, z refers to a zip generator object; the result of using z in the list
text - constructor is [('a', 1), ('b', 2), ('c', 3)] which zips/interleaves the values
text - from the first iterable and the values from the second:
text - [(first from first,first from second),(second from first,second from second),  
text -  (third from first,third from second)]
text - 
text - What happens when the iterables are of different lengths? Try it.
text - 
text - z = zip( 'abc', (1, 2) )  # String and tuple for iterables
text - print(list(z))		  # prints [('a', 1), ('b', 2)]
text - 
text - So when one iterable runs out of values to produce, the process stops. Here is
text - a more complex example with three iterable parameters of all different sizes.
text - Can you predict the result it prints: do so, and only then run the code.
text - 
text - z = zip( 'abcde', (1, 2, 3), ['1st', '2nd', '3rd', '4th'] )
text - print(list(z))
text - 
text - which prints the following (stopping after the 3 in the second argument)
text - 
text - [('a', 1, '1st'), ('b', 2, '2nd'), ('c', 3, '3rd')]
text - 
text - Of course, this generalizes for any number of arguments, interleaving them all
text - (from first to last) until any iterable runs out of values. So the number of
text - values in the result is the minimum of the number of values of the argument 
text - iterables.
text - 
text - Note one very useful way to use zip: suppose we want to iterate over values in
text - two iterables simultaneously, i1 and i2, operating on the first pair of values
text - in each, the second pair of values in each, etc. We can use zip to do this by
text - writing:
text - 
text - for v1,v2 in zip(i1,i2):
text -     process v1 and v2: the next pair of values in each
text - 
text - So
text - 
text - for v1,v2 in zip ( ('a','b','c'), (1,2,3) ):
text -     print(v1,v2)
text - 
text - prints
text - 
text - a 1
text - b 2
text - c 3
text - 
text - Using zip, we can write a small function that computes the equivalent of the
text - < (less than) operator for strings in Python (see the discussion above about
text - the meaning of < for strings).
text - 
added - ```python
code-start - def less_than(s1 : str, s2 : str)-> bool:
code -     for c1,c2 in zip(s1,s2):   # examine 1st, 2nd, ... characters of each string
code -         if c1 != c2:           # if current characters are different
code -             return c1 < c2     #   compute result by comparing characters
code-end - 
added - ```
text -     # if all character from the shorter are the same (as in 'a' and 'ab')
text -     # return a result based on the length of the strings
text -     return len(s1)<len(s2)
text - 
text - This precisely captures in Python code our prose discussion of the meaning of
text - comparing strings.
text - 
text - Enumerate:
text - 
text - Finally, this is a convenient time to toss in another important function:
text - enumerate. It also also produces a generator as a result, but has just one
text - iterable argument, and an optional 2nd argument that is a starting number. It
text - produces tuples whose first values are numbers (starting at the value of the
text - second parameter; omit a 2nd parameter and unsurprisingly, the starting number
text - is by default 0) and whose second values are the values in the iterable. So,
text - if we write
text - 
text - e = enumerate(['a','b','c','d'], 5)
text - print(list(e))
text - 
text - it prints [(5, 'a'), (6, 'b'), (7, 'c'), (8, 'd')]
text - 
text - Given l = ['a','b','c','d','e'] we could write the following code
text - 
text - for i in range(len(l)):
text -     print(i+1,l[i])
text - 
text - (which prints 1 a, 2 b, 3 c, 4 d, and 5 e on separate lines) more simply by
text - using enumerate (notice the use of parallel/tuple assignment for i,x):
text - 
text - for i,x in enumerate(l,1):
text -      print(i,x)
text - 
text - Another nice example illustrating enumerate is reading a file a line at a time,
text - and processing the line number and the line contents together.
text - 
text - Instead of writing 
text - 
text - line\_number = 1
text - for line\_text in open("file-name"):
text -     .... process line\_number and line\_text
text -     line\_number += 1
text - 
text - we can write just
text - 
text - for line\_number, line\_text in enumerate(open("file-name"),1):
text -     .... process line\_number and line\_text
text - 
text - You might ask now, why do these things (tuple comprehension, zip, enumerate)
text - produce generators and not just tuples or lists? That is an excellent question.
text - It goes right along with the excellent question why does sorted(...) produce a
text - list and not a generator? We will discuss these issues later in the quarter.
text - The generator question mostly has to do with space efficiency when iterating
text - over very  many values. The sorted question has to do with why there is no way
text - to do this operation in a space efficient way.
text - 
text - ------------------------------------------------------------------------------
text - 
text - \*\*kargs for dictionary of not-matched named arguments in function calls
text - 
text - Recall the use of \*args in a function parameter list. It combines into one
text - tuple a sequence of positional arugments. We can also write the symbol \*\*kargs
text - (we can write \*\* and any word, but kargs or kwargs are the standard ones). If
text - we use it to specify this kind of parameter, it must occur as the last
text - parameter. kargs stands for keyword arguments. Basically, if Python has any 
text - keywords arguments that do not match keyword parameters (see the large
text - discussion of argument/paramteter binding above, which includes \*args but
text - doesn't include \*\*kargs) they are all put in a dictionary that is stored in the
text - last parameter named kargs.
text - 
text - So, imagine we define the following function
text - 
added - ```python
code-start - def f(a,b,**kargs):
code -   print(a,b,kargs)
code-end - 
added - ```
text - and call it by
text - 
text - f(c=3,a=1,b=2,d=4)
text - 
text - it prints: 1 2 {'c': 3, 'd': 4}
text - 
text - Without \*\*kargs, using the rules specified before, Python would report a
text - TypeError exception (by rule M5(d)), because there are no parameter named
text - c or d. 
text - 
text - By the new rules, Python finds two named arguments (c=3 and d=4) whose names
text - did not appear as parameter names in the function header of f (which specifies
text - only a and b, and of course the special \*\*kargs), so while Python directly
text - binds a to 1 and b to 2 (the parameter names specified in the function header,
text - matched to similarly named arguments) it creates a dictionary with all the
text - other named arguments: {'c': 3, 'd': 4} and binds kargs to that dict.
text - 
text - The same result would be printed for the call
text - 
text - f(1,2,d=4,c=3)
text - 
text - We will use \*\*kargs to understand a special use of of dict constructors (below).
text - We will also use \*\*kargs (and learn something new in the process) when
text - discussing how to (1) call methods in decorators and (2) call overridden
text - methods using inheritance much later in the quarter.
text - 
text - Note that to write a perfectly general function that is called with any kinds
text - of (legal) arguments we can write
text - 
added - ```python
code-start - def g(*args,**kargs):
code -   print(args,kargs)
code-end - 
added - ```
text - Now, any legal call of g (one with any legal combination of positional and
text - named arguments) will populate the \*args and \*\*kargs structures appropriately.
text - 
text - Calling
text - 
text - g(1,2,c=3,d=4)     prints (1, 2) {'c': 3, 'd': 4}
text - g(a=1,b=2,c=3,d=4) prints () {'c': 3, 'b': 2, 'a': 1, 'd': 4}
text - 
text - Generally, when a parameter name is prefixed by \* and \*\* in a function header,
text - we omit the prefix when we use the parameter name inside the function. But
text - there are times where use use the \* and \*\* prefixes.
text - 
added - ```python
code-start - def h(a,b,c,d):
code -     print(a,b,c,d)
code-end - 
added - ```
added - ```python
code-start - def i(*args,**kargs):
code -   h(*args,**kargs)
code-end - 
added - ```
text - The arguments \*args and \*\*kargs in the call of h expand the args tuple to
text - be positional arguments and the \*\*kargs dictionary to be named arguments
text - 
text - Calling
text - 
text - i(1,2,c=3,d=4)     prints 1 2 3 4: it calls h(1,2,c=3,d=4)
text - i(a=1,b=2,c=3,d=4) prints 1 2 3 4: it calls h(c=3,b=2,a=1,d=4)
text - 
text - Summarizing:
text - 
text - 1) Writing \* and \*\* when specifying parameters makes those parameters names
text -    bind to a tuple/dict respectively.
text - 
text - 2) Using the parameter names by themselves in the function is equivalent to 
text -     using the tuple/dict respectively.
text - 
text - 3) Using \* and \*\* followed by the parameter name as ARGUMENTS IN FUNCTION
text -    CALLS expands all the values in the tuple/dict respectively to represent
text -    all the arguments.
text - 
text - Experiment with \*args/\*\*kargs as parameters of functions and args/kargs and
text - \*args/\*\*kargs as arguments to other function calls. Remember that recently
text - 
text - (rules U1/U2) Python allows / and \* to specify parameters, 
text -   / requires all earlier parameters to match position-arguments 
text -   \* requires all later   matching arguments to be named arguments
text - 
text - ------------------------------------------------------------------------------
text - 
text - Lists, Tuples, Sets, Dictionaries (frozenset and defaultdict)
text - 
text - You need to have pretty good grasp of these important data types, meaning how to
text - construct them and the common methods/operations we can call on them. Really
text - you should get familiar with reading the online documentation for all these
text - data types (see the Python Library Reference link on the homepage for the
text - course).
text - 
text - 4. 6: Sequence Types includes Lists (mutable) and Tuples (immutable)
text - 4. 9: Set Types includes set (mutable) and frozenset (immutable)
text - 4.10: Mapping Types includes dict and defaultdict (both mutable)
text - 
text - Here is a very short/condensed summary of these operations. Experiment with
text - them in Eclipse.
text - 
text - -----
text - 
text - 4.6: Sequence Types includes Lists (mutable) and Tuples (immutable)
text - These sequence operations (operators and functions) are defined in 4.6.1
text -    x in s, x not in s, s + t, s \* n, s[i], s[i:j], s[i:j:k], len(s), min(s), 
text -    max(s), s.index(x[, i[, j]]), s.count(x)
text - 
text - Mutable sequence allow the following operations, defined in 4.6.3
text -   s[i] = x , s[i:j] = t, del s[i], s[i:j:k] = t, del s[i:j:k], s.append(x)
text -   s.clear(), s.copy(), s.extend(t), s.insert(i, x), s.pop(), s.pop(i),
text -   s.remove(x), s.reverse()
text - 
text -   It also discusses list/tuple constructors and sort for list.
text - 
text -   note that the append method is especially important for building up sequences
text -   like lists. Also to return and remove a value from a sequence, we call
text -     x = s.pop(i)
text -   is equivalent to 
text -     x = s[i]
text -     del s[i]
text -   and calling s.pop() uses the value len(s)-1 (the last index in the sequence)
text - 
text - -----
text - 
text - 4. 9: Set Types includes set (mutable) and frozenset (immutable)
text - These set (operators and functions) are defined in 4.6.1.9
text -   len(s), x in s, x not in s, isdisjoint(other), issubset(other), set <= other, 
text -   set < other, issuperset(other), set >= other, set > other, union(other, ...),
text -   intersection(other, ...), difference(other, ...), symmetric\_difference(other),
text -   copy; also the operators | (for union), & (for intersection), - (for
text -   difference), and ^ (for symmetric difference)
text - 
text - Sets, which are mutable, allow the following operations
text -   update(other, ...), intersection\_update(other, ...),
text -   difference\_update(other, ...), symmetric\_difference\_update(other), add(elem),
text -   remove(elem), discard(elem), pop(), clear(); also the operators |= (union
text -   update), &= (intersection update), -= (difference update), ^= (symmetric
text -   difference update)
text - 
text - -----
text - 
text - 4.10: Mapping Types includes dict and defaultdict (both mutable)
text - These dict (operators and functions) are defined in 4.10
text -   d[key] = value , del d[key], key in d, key not in d, iter(d), clear(), copy(),
text -   fromkeys(seq[, value]), get(key[, default]), items(), keys(),
text -   pop(key[, default]), popitem(), setdefault(key[, default]), update([other]),
text -   values() 
text - 
text - Important Notes on dicts:
text -   d[k] returns the value associated with a key (raises exception if k not in d)
text - 
text -   d.get(k,default) returns d[k] if k in d; returns default if k not in d
text -     it is equivalent to the conditional expression (d[k] if k in d else default)
text - 
text -   d.setdefault(k,default) returns d[k] if k in d; if k not in d it
text -     (a) sets d[k] = default
text -     (b) returns d[k]
text -   writing d.setdefault(k,default) is equivalent to (but more efficient than)
text -   writing
text -       if k in d:
text -           return d[k]
text -       else
text -           d[k] = default
text -           return d[k]
text - 
text - There is a data structure called a defaultdict (see 8.3.4) whose constructor
text - generally takes an argument that is a reference to any object that CAN BE
text - CALLED WITH NO ARGUMENTS. Very frequently, we use a NAME OF A CLASS that when
text - called will CONSTRUCT A NEW VALUE OF THAT CLASS: if the argument is int, it 
text - will call int() producing the value 0; if the argument is list, it will call
text - list() producing an empty list; if the argument is set, it will call set()
text - producing an empty set; etc.
text - 
text - Whenever a key is accessed for the first time (i.e., that key is accessed but
text - not already associated with a value in the dictionary) in a defaultdict, it
text - will associate that key with the value created by calling the reference to
text - the object supplied to the constructor.
text - 
text - Here is an example of program first written using a dict, and simplified later
text - by using a defaultdict. Copy/paste the following code fragments into a Python
text - module in Eclipse; then use the debugger to step through the code a line at a
text - time, watching how freq\_dict is updated.
text - 
text - letters = ['a', 'x', 'b', 'x', 'f', 'a', 'x']
text - freq\_dict = dict()             # could use = {} instead
text - for l in letters:              # iterate through each list value: a letter
text -     if l not in freq\_dict:     # must check l in freq\_dict before freq\_dict[l]
text -         freq\_dict[l] = 1       # if not there, put with frequency of 1
text -     else:
text -         freq\_dict[l] += 1      # otherwise there, increment frequency
text - print(freq\_dict)
text - 
text - This would print the following dict: {'b': 1, 'x': 3, 'f': 1, 'a': 2}
text - 
text - As each letter in the loop is processed, if it is not already in the dict it is
text - associated with 1; if it is already in the dict, its associated value is
text - incremented by 1: freq\_dict[l]+=1 is equivalent to freq\_dict[l]=freq\_dict[l]+1,
text - so to execute that assignment without an exception, freq\_dict[l] must already
text - store a value.
text - 
text - We could solve this a bit more easily with a defaultdict.
text - 
text - from collections import defaultdict            # in same module as namedtuple
text - letters = ['a', 'x', 'b', 'x', 'f', 'a', 'x']
text - freq\_dict = defaultdict(int)   # int not int(); but int() returns 0 when called
text - for l in letters:              # iterate through each list value: a letter
text -     freq\_dict[l] += 1	       # in dict, exception raised if l not in d, but
text - print(freq\_dict)               #   defaultdict calls int() putting 0 there first
text - 
text - As each letter in the loop is processed, its associated key is looked up:
text - if the key is absent, it it placed in the dict associatiated with int()/0; then
text - then the associated value (possibly the one just put in the dict) is
text - incremented by 1.
text - 
text - The dict code below is equivalent to how the defaultdict code above works.
text - 
text - letters = ['a', 'x', 'b', 'x', 'f', 'a', 'x']
text - freq\_dict = dict()	       # could use = {}
text - for l in letters:              # iterate through each list value: a letter
text -     if l not in freq\_dict:     # must check l in freq\_dict before freq\_dict[l]
text -         freq\_dict[l] = int()   # int() constructor returns 0; could write 0 here
text -     freq\_dict[l] += 1	       # l is guaranteed in freq\_dict, either because
text - print(freq\_dict)	       #   it was there originally, or just put there
text - 
text - Another way to do the same thing (but also a bit longer and less efficient)
text - uses the setdefault method (listed above)
text - 
text - letters = ['a', 'x', 'b', 'x', 'f', 'a', 'x']
text - freq\_dict = dict()	       # note dict only
text - for l in letters:
text -     freq\_dict[l] = freq\_dict.setdefault(l,0) + 1
text - print(freq\_dict)	   	       
text - 
text - Here we evaluate the right side of the = first; if l is already in the dict,
text - its associated value is returned; if not, l is put in the map with an associated
text - value of 0, then this associated value is returned (and then incremented, and
text - stored back into freq\_dict[l] replacing the 0 just put there).
text - 
text - You should understand why each of these four scripts work and why they all
text - produce equivalent results. But, using a defaultdict is simpler (once we 
text - understand this data structure) and more efficient.
text - 
text - Often we use defaultdicts with list instead of int: just as int() produces the
text - object 0 associated with a new key, list() creates an empty list associated
text - with a new key (and later we add things to that list); likewise we can use
text - defaultdicts with set to get an empty set with a new key.
text - 
text - So, if we wrote x = defaultdict(list) and then immediately wrote
text - x['bob'].append(1) then x['bob'] is associated with the list [1] (the empty
text - list, with 1 appended to it). If we then wrote x['bob'].append(2), then
text - x['bob'] is associated with the list [1, 2]. So, we can always append to the
text - value associated with a key, because it will use an empty list to start the
text - process if there is nothing currently associated with a key.
text - 
text - How could we specify a defaultdict that initializes keys with the value 5?
text - Translation: in what simple way can we pass to defaultdict an argument that is
text - a function of no parameters, which returns the value 5. Generalizing a solution
text - can lead to all sorts of interesting variants, say for a defaultdict whose
text - unknown keys are automatically associated with another dict (or another
text - defaultdict). One simple answer is defaultdict((lambda : 5)): pass defaultdict
text - a reference to a function object that when called with no arguments returns 5.
text - 
text - Note that the ==/!= operators for dictionaries work correctly between dicts and
text - defaultdicts. They are considered equal so long as they have the equal keys, and
text - each key is associated with an equal value.
text - 
text - Just as with comprehensions, what is important is that we know how things like
text - defaultdicts and dicts (with the setdefault method) work, so that we can
text - correctly write code in any of these ways. We can decide afterwords which way
text - we want the code to appear. As we program more, our preferences might change.
text - I have found defaultdicts are mostly what I to use to simplify my code, but
text - every so often I must use a regular dict.
text - 
text - Later in the quarter we will use inheritance to show how to write the
text - defaultdict class simply, by extending the dict class.
text - 
text - ------------------------------------------------------------------------------
text - 
text - Printing Dictionaries in Order: An example of using comprehensions and sorted
text - 
text - In this section we will combine our knowledge about the sorted function,
text - comprehensions, and iterating over dictionaries to examine how we can print
text - (or generally process) dictionaries in arbitrary orders.
text - 
text - Generally, code the processes a dictionary in sorted order will be of the form
text - 
text -   for index(es) in sorted( iterable, key = (lambda x : ....) ):
text -      print( index(es) )
text - 
text - In these examples, we will use a simple dictionary
text - 
text -    d = {'x': 3, 'b': 1, 'a': 2, 'f': 1}
text - 
text - In each example, we will discuss the relationships among index(es), iterable,
text - and the (lambda x : ....) in the sorted function, bound to the key parameter.
text - 
text - 1) In the first example, we will print the dictionary keys in increasing
text - alphabetical order, and their associated values, by iterating over d.items().
text - 
text - for k,v in sorted( d.items(), key = (lambda item : item[0]) ):
text -    print(k,'->',v)
text - 
text - which prints as
text - 
text - a -> 2
text - b -> 1
text - f -> 1
text - x -> 3
text - 
text - Here, the iterable is d.items(), which produces 2-tuples storing each key and
text - its associated value, for every item in the dictionary; the parameter item in
text - the lambda must also be a key/value 2-tuple (specifying here to sort by item[0],
text - the key part in the 2-tuple); finally, the list returned by sorted also contains
text - key/value 2-tuples, which are unpacked into k and v and printed.
text - 
text - We can solve this same problem by iterating over just the keys in d as well.
text - 
text - for k in sorted( d.keys(), key = (lambda k : k) ):
text -    print(k,'->',d[k])
text - 
text - Here, the iterable is d.keys() which produces strings storing each key in the
text - dictionary; the parameter k in the lambda is also a key/str value (specifying
text - here to sort by k, the key itself: I could have omitted this identity lambda
text - when calling sorted); finally, the list returned by sorted also contains key/str
text - value, which are stored into k and printed along with d[k].
text - 
text - Generally, sorted returns a list of the values specified by its first parameter:
text - a list of 2-tuples for d.items() and a list of strings for d.keys().
text - 
text - This code is equivalent to the following, since d.keys() is the same as just d,
text - and lambda x : x is the default for the key parameter.
text - 
text - for k in sorted( d ):
text -    print(k,'->',d[k])
text - 
text - 
text - 2) In the second example, we will print the dictionary keys and their associated
text - values, in increasing order of the values, by iterating over d.items().
text - 
text - for k,v in sorted( d.items(), key = (lambda item : item[1]) ):
text -    print(k,'->',v)
text - 
text - which prints as
text - 
text - b -> 1
text - f -> 1
text - a -> 2
text - x -> 3
text - 
text - Here, iterable is d.items(), which produces 2-tuples storing each key and its
text - associated value, for every item in the dictionary; the parameter item in the
text - lambda is also a key/value 2-tuple (specifying here to sort by item[1], the
text - value part in the 2-tuple); also, the list returned by sorted also contains
text - key/value 2-tuples, which are unpacked into k and v and printed. Finally, by
text - this lambda either b or f might be printed first, because they have the same
text - value associated with them.
text - 
text - We can solve this same problem by iterating over just the keys in d as well.
text - 
text - for k in sorted( d.keys(), key = (lambda k : d[k]) ):
text -    print(k,'->',d[k])
text - 
text - Here, iterable is d.keys() which produces strings storing each key in the
text - dictionary; the parameter k in the lambda is also a key/str value (specifying
text - here to sort by d[k], the value associated with the key); finally, the list
text - returned by sorted also contains key/str values, which are stored into k and
text - printed along with d[k].
text - 
text - This code is equivalent to the following, since d.keys() is the same a d; the
text - lambda is still needed here because it is not the identity lambda.
text - 
text - for k in sorted( d, key = (lambda k : d[k]) ):
text -    print(k,'->',d[k])
text - 
text - In both cases, inside the lambda Python uses the LEGB rule: d is not a
text - parameter to the lambda, but it might be defined in the Enclosing or Global
text - scope (here the Global scope), so it can be used in the lambda.
text - 
text - 3) In the third example, we will compute a list that contains all the keys in a
text - dictionary, sorted by their associated values (in decreasing order of the
text - values), by iterating over d.keys() (really just d, to simplify the code)
text - 
text - We compute
text - 
text - ks = sorted(d, key = (lambda k : d[k]), reverse = True)
text - 
text - ks stores ['x', 'a', 'b', 'f'].
text - 
text - We could also compute this list less elegantly using d.items(); I say less
text - elegantly, because we need a comprehension to "throw away" the value part of
text - each item returned by sorted.
text - 
text - ks = [k for k,v in sorted(d.items(), key=(lambda item : item[1]), reverse=True)]
text - 
text - 
text - 4) Finally, in the fourth example, we will compute a list that contains all the
text - 2-tuples in the items of d, but the tuples are reversed (values before keys) and
text - sorted in increasing alphabetical order by keys.
text - 
text - We compute this list of 2-tuple in either of the following two ways
text - 
text - vks = [ (v,k) for k,v in sorted( d.items(), key = (lambda item : item[0]) ) ]
text - vks = sorted( ((v,k) for k,v in d.items()), key = (lambda item : item[1]) )
text - 
text - vks stores [(2, 'a'), (1, 'b'), (1, 'f'), (3, 'x')]
text - 
text - The top computation first creates a sorted version of d by keys, and then uses a
text - comprehension to create tuples that reverse the keys/values; the bottom
text - computation first uses a comprehension to create a tuple of reversed
text - keys/values, and then calls sorted to sort these reversed 2-tuples by the keys
text - that are now in the second part of each tuple.
text - 
text - ----------
text - Using \_ as a variable Name:
text - 
text - If you write 
text - 
text -   ks = [k for k,v in list\_of-2-tuples...]
text - 
text - Eclipse will likely give you warning about "variable v is not used". Since
text - there must be variable to store the second tuple index, but that variable does
text - not get used, we can write it as just \_ (a legal Python variable name)
text - 
text -   ks = [k for k,\_ in list\_of-2-tuples...]
text - 
text - In this case, Eclipse does not indicate a warning. Naming a variable \_ implies
text - you aren't going to to anything useful with it. Although, we can write code like
text - 
text - for \_ in [1,2,3]:
text -     print(\_)
text - 
text - and it will print 1, 2, and 3 (on their own lines). By using \_ Eclipse
text - doesn't expect this name to be used.
text - ----------
text - 
text - Assume l = ['x', a', 'f', 'b']; students often write code like sorted(list(l)).
text - The use of the list constructor is superfluous and wasteful. We can write this
text - code more simply sorted(l), which iterates though l to create values in a list
text - to sort; it doesn't create a new list (extra space and time) and then iterate
text - over the new list (extra time) to do the sorting.
text - 
text - ------------------------------------------------------------------------------
text - 
text - Exceptions: example from prompt\_for\_int
text - 
text - We do two things with exceptions in Python: we raise them (with the raise
text - statement) and we handle them (with the try/except statement). Exceptions were
text - not in early programming languages, but were introduced big time in the Ada
text - language in the early 1980s, and have been parts of most new languages since
text - then.
text - 
text - A function raises an exception if it cannot do the job it is being asked to
text - do. Rather than fail silently, possibly producing a bogus answer that gets used
text - to compute an even bigger bogus result, it is better that the computation
text - announces that a problem occurred; and if no way is known to recover from it
text - (see handling exceptions below) the computation terminates with an error
text - message to the user.
text - 
text - For example, if your program has a list l and you write l[5] but l has nothing
text - stored at index 5 (its length is smaller), Python raises the IndexError
text - exception. If you haven't planned for this possibility, and told Python how to
text - handle the exception and continue the calculation, then the program just
text - terminates and Python prints: 
text - 
text - IndexError: list index out of range
text - 
text - Why doesn't it print the index value 5 and the length of list l?. I don't know.
text - That certainly seems like important and useful information, and Python knows
text - those two values. I try to make my exception messages include any information
text - useful to the programmer.
text - 
text - There are lots of ways to handle exceptions. Here is a drastically simplified
text - example from my prompt class (this isn't the real code, but a simplification
text - for looking at a good use of exception handling). But it does a good job of
text - introducing the try/except control form which tries to execute a block of code
text - and handles any exceptions it raises.
text - 
text - If we write a try/except and do not specify the name of an exception, it will
text - handle any exception. The name Exception is the name of the most generic
text - exception.
text - 
text - We can write a try/except statement with many excepts, each one specifying a 
text - specific exception to handle, and what to do when that exception is raised. In
text - fact, int('x') raises the ValueError exception, so I use ValueError in the
text - except clause below to be specific, and not accidentally handle any other kind
text - of exception.
text - 
added - ```python
code-start - def prompt_for_int(prompt_text):
code -     while True:
code -         try:
code -             response = input(prompt_text+': ')    # response is used in except
code -             answer = int(response)
code -             return answer
code -         except ValueError:
code -             print('  You bozo, you entered "',response,'"',sep='')
code - 	    print('  That is not a legal int')
code-end - 
added - ```
text - print(prompt\_for\_int('Enter a positive number'))
text - 
text - So, this is an "infinite" while loop, but there is a return statement at the
text - bottom of the try-block; if it gets executed, it returns from the function,
text - thus terminating the loop. The loop body is a try/except; the body of the
text - try/except
text - 
text - 1) prompts the user to enter a string on the console (this cannot fail)
text - 
text - 2) calls int(response) on the user's input (which can raise the ValueError
text -    exception, if the user types characters that cannot be interpreted as an
text -    integer)
text - 
text - 3) if that exception is not raised, the next statement in the block is executed:
text -    the return statement returns an int object representing the integer the user
text -    typed as a string
text - 
text - But if the exception is raised, it is handled by the except clause, which
text - prints some information. Now the try/except is finished, but it is in an
text - infinite loop, so it goes around the loop again, re-prompting the user (over and
text - over until the user enters a legal int).
text - 
text - Actually, the body of try could be simplified (with the same behavior) to just
text - 
text -   response = input(prompt\_text+': ')    # response is used in except
text -   return int(response)
text - 
text - If an exception is raised while the return statement is evaluating the int
text - function, it still gets handled in except. We CANNOT write it in one line
text - because the name response is used in the except clause (in the first print).
text - If this WASN'T the case, we could write just
text - 
text -   return int(input(prompt\_text+': '))    # if response not used in except
text - 
text - For example, we might just say 'Illegal input' in the except, but in the example
text - above, it actually display the string (not a legal int) that the user typed.
text - 
text - Finally, in Java we throw and catch exceptions (obvious opposites, instead of
text - raise and handle) so I might sometimes use the wrong term. That is because I 
text - think more generally about programming than "in a language", and translate what
text - I'm thinking to the terminology of the language I am using, but sometime I get
text - it wrong.
text - 
text - Note that exception handling is very powerful, but we should avoid using it if a
text - boolean test can easily determine whether a computation will fail. For example.
text - 
text - l = [...]
text - if (0 <= i < len(l))
text -   print(l[i])
text - 
text - is preferred to
text - 
text - try:
text -   print(l[i])
text - except IndexError:
text -   pass
text - 
text - 
text - The general form for try/except is a bit more complicated, but easy to
text - understand. EorSE stand for the name of an exception or a sequence (tuple or
text - list) of exception names.
text - 
text - try:
text -     try-block
text - 
text - except EorSE:
text -     except-block
text - except EorSE:
text -     except-block
text - ...
text - except EorSE:
text -     except-block
text - 
text - else:
text -     else-block
text - 
text - finally:
text -     finally-block
text - 
text - Python tries to execute the statements in a try-block sequentially...
text - 
text -   (a) ...if NONE raise an exception, it executes the statements in else-block
text -       (if else: is present) and then the statements in finally-block (if 
text -       finally: is present)
text - 
text -   (b) ...if ANY raise an exception, no more statements in the try-block are
text -       executed; it executes the statements in except-block (if except: is
text -       present) for the first EorSE matching the raised exception (if any match),
text -       then the statements in finally-block (if finally: is present)
text - 
text -   (c) If no EorSE matches the raised exception, then after executing the
text -       finally-block (if finally: is present), Python propagates the exception.
text - 
text -       For example, if the try/except was in a function, it would propagate it
text -       out of the function to the location where the function was called, which
text -       might be in a try/except block that catches the exception.
text - 
text - Single-stepping in the debugger is an excellent way to watch try/except
text - statements execute to better understand them.
text - 
text - ------------------------------------------------------------------------------
text - 
text - Name Spaces (for objects): \_\_dict\_\_
text - 
text - Every object has a special variable named \_\_dict\_\_ that stores all its
text - namespace bindings in a dictionary. During this quarter we will systematically
text - study class names that start and end with two underscores. Writing x.a = 1
text - is similar to writing x.\_\_dict\_\_['a'] = 1; both associate a name with a value
text - in the object. We will explore the uses of this kind of knowledge in much more
text - depth later in the quarter.
text - 
text - Here is a brief illustration of the point above. Note that there is a small
text - Python script that illustrates the point. This is often the case.
text - 
text - class C:
text -     def \_\_init\_\_(self): pass
text - 
text - o = C()
text - o.a = 1
text - print(o.a)           # prints 1
text - 
text - o.\_\_dict\_\_['a'] = 2
text - print(o.a)           # prints 2
text - 
text - o.\_\_dict\_\_['b'] = 3
text - print(o.a,o.b)       # prints 2 3
text - 
text - ------------------------------------------------------------------------------
text - 
text - Trivial Things.
text - 
text - An empty dict is created by {} and empty set by set() (we can't use {} for an
text - empty set because Python would think it is a dict). Any non-empty dicts can
text - be distinguished from a non-empy set because a non-empty dict will always have
text - the : character inside {} separating keys from values. Suppose you want to
text - create a set containing the value 1: which works (and why)? {1} or set(1).
text - 
text - A one value tuple must be written like (1,) with that "funny" comma (we can't
text - write just (1) because that is just the value 1, not a tuple storing just 1).
text - 
text - ------------------------------------------------------------------------------
text - 
text - Questions:
text - 
text - 1. Describe, in term of binding, what happens if the first statement in a
text - module is x = 0, in terms of the module object and its namespace.
text - 
text - 2. Assume that we have executed the statement x = 1. Describe, in terms of
text - binding, what is the semantics of the statement x += 1.
text - 
text - 3. What is printed in print(f(0)) if we define f as follows?
text - 
text -   def f(x):
text -       pass
text - 
text - 4. Using the kind of pictures discussed in the Binding section above, illustrate
text - the meaning of a module named m that contains the following statements:
text - 
text -   x = 1
text -   y = 2
text -   z = y
text - 
text - And a module named script that contains the following statements:
text - 
text -   import m
text -   a = m.x
text -   m.y = 10
text -   from m import y
text -   from m import z as b
text -   z = y
text -   del m.z
text -   c = 10
text - 
text - The result will be two large rounded-rectangles (objects for modules m and 
text - script) which contain labeled boxes that refer to other rounded-rectangles
text - (objects for int values), some of which are shared (referred to by multiple
text - names)
text - 
text - 5. Predict what the following script will produce and explain why.
text - 
text - print(print(5))
text - print=1
text - print(print)
text - 
text - 5.1. Assume that f(x) is expensive to compute and the expression x < 5 is often
text - False. Simplify the following code to be efficient without using the :=
text - operator; continue simplifying it using the := operators.
text - 
text - if x < 5 and f(x) > 5:
text -   return f(x)
text - 
text - 6a. Write a simple function named count that returns the number of times a value
text - is in a list; write the a simple function named indexes that returns a list of
text - indexes in a list that a value returns. Use the appropriate kind of for loop.
text - For example, count(5, [5,3,4,5,1,2,5]) returns 3; indexes(5, [5,3,4,5,1,2,5])
text - returns [0,3,6].
text - 
text - 7. Suppose that we define the functions double, triple, and times10 (as done
text - above). Write the function call, such that call('double',5) returns the result
text - double(5); call('triple',5) returns the result triple(5); call('magnitude',5)
text - returns the result magnitude(5). Hint: use eval.
text - 
text - 8. Write a function named between that is defined with two parameters. It
text - returns a reference to a function that has one parameter, which returns whether
text - or not its one parameter is between (inclusive) the two arguments supplied when
text - between is called. For example
text - 
text - college\_age = between(18,22)
text - print( college\_age(20) )
text - 
text - print True because 18 <= 20 <= 22.
text - 
text - 8b. Rewrite the following function
text - 
added - ```python
code-start - def running_average2():
code -     sum   = 0              # rebound in include_it
code -     count = 0              # rebound in include_it
code-end - 
added - ```
text -     def include\_it(x):
text -         nonlocal sum,count # allow rebinding of locals in outer function's scope
text -         sum += x           # rebind sum
text -         count+= 1          # rebind count
text -         return sum/count
text - 
text -     return include\_it
text - 
text - so that we can omit the nonlocal declaration. Hint: make sum/count each a list
text - with one value (the real sum/count) so that the body of include\_it will change
text - the value in each list, but not rebind sum/count.
text - 
text - 9. Assume s = 'Fortunate'. Explain how Python evalues s.replace('t','c'). Sure,
text - the result it produces is 'Forcunace', but exactly how does Python know which
text - function to call and how to call that function with these arguments. Hint: Use
text - the Fundamenal Equation of Object-Oriented Programming.
text - 
text - 9.5 This is tricky: it requires a good understanding of exec, eval, and what
text - characters really appear in strings when you type them on the console.
text - (1) prompt the user to enter a string that defines a function (using \n where
text - new lines would be in the function); (2) use exec to define the function;
text - (3) prompt the user to enter a call to the function and print what it evaluates
text - to. The interaction might look like:
text - 
text - Enter function definition as string: def f(n):\n    return 2\*n
text - Enter function call as string      : f(3)
text - 6
text - 
text - Hint: when you enter \n in a string, what characters are entered? Use the
text - replace function to "fix" them to exec will work.
text - 
text - 10. Assume that you have a list of tuples, where each tuple is a name and
text - birthday: e.g., ('alex', (9, 4, 1988)) and ('mark', (11, 29, 1990)). The
text - birthday 3-tuple, consists of a month, followed by day, followed by year. Write
text - a for-loop using sorted and a lambda to print out the tuples in the list in
text - order of their birthdays (just months and days) so the 'alex' tuple prints
text - before the 'mark' tuple because September 4th comes before November 29th; all
text - people who have the same birthday should be printed in alphabetical order.
text - Hint write a lambda that uses a key that is a person's month value, followed by
text - a day value, followed by a name value.
text - 
text - 11. What does the following script print?
text - 
text - print('a','b','c',sep='.-x-.')
text - print('d','e','f',sep='')
text - print('g','j','i',sep=':',end='..')
text - print('j','k','l',sep='--',end='?')
text - print('m',end='\n\n')
text - print('n')
text - 
text - 12. Assume s is a string. What is the value of s[-1:0:-1]? Assume l is a list.
text - Write the simplest loop that prints all values in a list except the first and
text - last (if the list has2 or fewer values, it should print nothing).
text - 
text - 13. Write a single Python statement using a conditional expression that is
text - equivalent to the following two statements
text - 
text - t = 0
text - if x < 0:
text -     t = -1
text - 
text - 14. What can we say about
text -   len of set(d.keys())   compared to len of d.keys?
text -   len of set(d.values()) compared to len of d.values()?
text -   len of set(d.items())  compared to len of d.items()?
text - 
text - 15. Assume we import the copy function from the copy module. What is the
text - difference between the result produced by list((1,2,3)) and copy((1,2,3))?
text - 
text - 15.5 Using the kind of pictures discussed in the Binding section above,
text - illustrate the meaning of the following and determine what is printed.
text - 
text -   x = [ [0], [1]]
text -   y = list(x)
text -   x[0][0] = 'a'
text -   print(x,y)
text - 
text - 16a. Assume we have a list of students (ls) and a list of their gpas (lg).
text - Create a dictionary whose first key is the first student in ls and whose
text - associated value is the first gpa in the lg; whose second key is the second
text - student in ls and whose associated value is the second gpa in the lg; etc.
text - So if ls = ['bob','carol','ted','alice'] and lg = [3.0, 3.2, 2,8, 3.6] the
text - resulting dict might print as {'carol': 3.2, 'ted': 2, 'alice': 8, 'bob': 3.0}.
text - Hint: use the dict constructor and the zip function.
text - 
text - 16b. Assume that we have a list of runners in the order they finished the race.
text - For example ['bob','carol','ted','alice']. Create a dictionary whose keys are
text - the students and whose values are the place in which they finished. For this
text - example the resulting dict might print as 
text - {'carol': 2, 'ted': 3, 'alice': 4, 'bob': 1}. Hint: use a comprehension and
text - enumerate.
text - 
text - 16c. Assume that we have a list of values l and a function f, and we want to
text - define a function that computes the value x in l whose f(x) is the smallest.
text - For example, if l = [-2, -1, 0, 1, 2] and def f(x) : return abs(x+1), then
text - calling min\_v(l,f) would return -1. Hint: try to write the min\_v function in one
text - line: a returns statement calling min, on a special comprehension, and indexing.
text - 
text - 17. The following script uses many topics that are covered in the lecture.
text - 
text - Write a script that reads a multi-line file of words separated by spaces
text - (no punctuation) and builds a dictionary whose keys are the words and whose
text - values are lists of the line numbers that the words are on (with no duplicate
text - line numbers in each list). Print all the words (in sorted order, one per line)
text - with the list of their line numbers afterwards. For example, the 3 line file
text - 
text - to be or
text - not to be
text - that is the question
text - 
text - would print as
text - 
text - be [1, 2]
text - is [3]
text - not [2]
text - or [1]
text - question [3]
text - that [3]
text - the [3]
text - to [1, 2]
text - 
text - My solution was 8 lines long (including one import). It used a defaultdict,
text - three loops, three function calls (open, enumerate, and sorted), and four
text - method calls (rstrip, split, append).
text - 
text - 18. Assume that we have a list of 2-tuples representing a deck of cards, with
text - each 2-tuple specifying a suit ('clubs', 'diamonds', 'hearts', or 'spades') and
text - a rank ('2', '3, ... '10', 'jack', 'queen', 'king', or 'ace'). Also assume that
text - we have two dictionaries specifying
text - suit\_order = {'clubs' : 0, 'diamonds' " 1, 'hearts' : 2,'spades' : 3}
text - rank\_order = {'2' : 2, '3' : 3, ..., '10' : 10, 'jack' : 11, 'queen' : 12,
text -                   'king' : 13, 'ace' : 14}
text - Write a lambda for ... such that calling sorted(card\_list, key = ...) produces
text - a list sorted by suit then rank: [('clubs','2'), ('diamonds','2'),
text -   ('hearts','2'), ('spades','2'), ('clubs','3'), ('diamonds','3'),
text -   ('hearts','3'), ('spades','3'), ..., ('clubs','ace'), ('diamonds','ace'),
text -   ('hearts','ace'), ('spades','ace')]
text - What would we need to change the lambda to be for to produce a list sorted by
text - rank then suit?
text - 
text - 
