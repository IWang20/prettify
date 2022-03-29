text - # <div align=center>				Python Classes</div>
added - ***
added - ## <a name="top"></a> Table of Contents
added - ***
text - 
text - 
text - This lecture reviews basic material that you should know about defining and
text - using classes, although it also presents some new (hopefully easy to understand)
text - material that you may have not seen. Primarily this lecture discusses how to
text - use the namespaces in class objects and their instance objects to store data as
text - well as functions/methods.
text - 
text - ------------------------------------------------------------------------------
text - 
text - Defining Classes:
text - 
text - When we define a class in a Python, we are binding the class name to an object
text - representing the entire class. We call the class object to create a new object
text - that is an instance of the class: Python constructs an empty instance object
text - and then calls the \_\_\_init\_\_ method defined in the class, (passing the new/empty
text - instance object to the self parameter) to initialize the state of the instance
text - object. Recall that all names in Python refer/are-bound to objects; so defining
text - 
text -    class C:
text -        ...
text - 
text - creates the name C and binds it to an object representing the class C.
text - 
text - What names are attributes defined in a class object's namespace? I'm not
text - talking about the instance objects that will be constructed from the class C,
text - but the names in the namespace of the class object itself. Mostly, a class
text - defines names that are bound to its methods (\_\_init\_\_, etc.), but later in this
text - lecture we will discuss names representing class variables as well as class
text - methods.
text - 
text - When we want to construct an object that is an instance of a class, we refer to
text - a class object (typically by a name bound to it) followed by () and possibly
text - arguments in these parentheses. Python does three things when constructing new
text - instance objects:
text - 
text - (1) It calls a special function that creates an object that is a new instance of
text -     the class. Note that this object automatically has an dictionary associated
text -      with it, with the name \_\_dict\_\_, but that dictionary starts empty.
text - 
text - (2) It calls the \_\_init\_\_ method for the class, passing the object created in
text -     (1) to the first/self parameter of \_\_init\_\_ , and following this with all
text -     the other argument values in the parentheses used in the call to construct
text -     the state of this instance. Typically it assigns values to self/instance
text -     variables, storing the name/binding in \_\_dict\_\_ for the self object.
text - 
text - (3) A reference to the object that was created in (1) and initialized in (2) is
text -     returned as the result of constructing the object: most likely this
text -     reference will be bound to a name: e.g., c = C(...) which means c refers to
text -     a newly constructed/initialized object of class C.
text - 
text - So if we call c = C(1,2) Python calls C.\_\_init\_\_(reference to new object,1,2)
text - and binds c to refer to the newly constructed object somehow initialized by
text - 1 and 2.
text - 
text - Note that we can define other names that can bind to/share the same class
text - object. For example:
text - 
text - class C:
text -     def \_\_init\_\_(self):
text -         print('instance of C object initialized')
text -         self.m = 'starting' # initialize an instance/attribute name
text - 
text - D = C	# C and D share the same class object
text - x = C() # Use C to construct an instance of a class C object
text - y = D() # Use D to construct an instance of a class C object
text - 
text - print(C,D,x,y)
text - print(type(x), type(y), type(C), type(type(x)))
text - 
text - Running this script produces the following: the first two lines from calling
text - \_\_init\_\_ twice, the next two from calling the two print statements above
text - 
text -   instance of C object initialized
text -   instance of C object initialized
text -   <class '\_\_main\_\_.C'> <class '\_\_main\_\_.C'> <\_\_main\_\_.C object at 0x027B0E10> <\_\_main\_\_.C object at 0x02889C50>
text -   <class '\_\_main\_\_.C'> <class '\_\_main\_\_.C'> <class 'type'> <class 'type'>
text - 
text - Finally, if we print(x.m) or print(y.m) it prints 'starting'
text - 
text - We can use the type function to determine the type of any object. The objects x
text - and y refer to are instances of the class C, defined in the main script. C (and
text - all classes that we define) are instances of a special class called 'type'). So
text - x is an instance of C, and C is an instance of 'type'.
text - 
text - ------------------------------------------------------------------------------
text - 
text - Manipulating an object's namespace (and \_\_init\_\_):
text - 
text - All objects have namespaces, which are dictionaries in which names defined in
text - that object are bound-to/associated-with values. Typically we write something
text - like:
text - 
text -   self.name = value
text - 
text - in the \_\_init\_\_ or any other method, to defined a name in the dictionary of the
text - object self refers to, and bind that name (or update an exsting binding) to
text - refer to value.
text - 
text - Now we illustrate a cruder way to add names to the namespace of an object.
text - This way is not recommended now, but it furthers our understanding of objects
text - and their namespaces in Python, and we will find concrete uses for this
text - understanding later. Given class C defined above, we can write
text - 
text - c = C()
text - print(c.\_\_dict\_\_)
text - c.x = 1
text - c.y = 'two'
text - c.\_\_dict\_\_['z'] = 3
text - print(c.\_\_dict\_\_)
text - print(c.m, c.x, c.y, c.z)
text - 
text - Running this script produces
text - 
text -   instance of C object initialized
text -   {'m': 'starting'}
text -   {'x': 1, 'y': 'two', 'm': 'starting', 'z': 3}
text -   starting 1 two 3
text - 
text - So, we have used two different forms for adding three names to the
text - namespace/dictionary for the object that c refers to (which initially stores
text - the name m, initialized by self.m = 'starting'). Adding names updates the
text - object's \_\_dict\_\_.  Writing object.name = value is equivalent to writing
text - object.\_\_dict\_\_['name'] = value.
text - 
text - The object c has a \_\_dict\_\_ attribute that stores its namespace bindings.
text - Each identifier for which we define an attribute for the object appears as a
text - string keyword in \_\_dict\_\_.
text - 
text - Generally we don't initialize the namespace of an object this way; instead we
text - use the automatically-called \_\_init\_\_ method and its self parameter to do the
text - initialization. But really, the same thing is happening in \_\_init\_\_ below as
text - was shown above.
text - 
text - class C:
text -     def \_\_init\_\_(self,init\_x,init\_y):
text -         print('instance of C object initialized')
text -         self.x = init\_x		# value depends on the argument matching init\_x
text -         self.y = init\_y		# value depends on the argument matching init\_y
text -         self.z = 3		# always the same object: 3
text - 
text - c = C(1,'two')
text - print(c.\_\_dict\_\_)
text - 
text - Running this script produces the same results.
text - 
text -   instance of C object initialized
text -   {'z': 3, 'y': 'two', 'x': 1}
text - 
text - The purpose of the \_\_init\_\_ method is to create all the attribute names needed
text - by the object's methods and initializes them appropriately. Typically, once
text - \_\_init\_\_ is called, no new names are added to the object's namespace (although
text - Python allows additions, as illustrated in the prior section (c.x = 1) and some
text - useful examples are illustrated later in this lecture as well). Every object
text - constructed is likely to need exactly the same names: all the names used
text - in the methods defined in the class; methods that process instance objects
text - of the class. The \_\_init\_\_ method, which is automatically called by Python
text - when an object is constructed, is a convenient place to localize the creation
text - and initialization of all these attribute names.
text - 
text - So, for every assignment statement
text - 
text -   self.name = value
text - 
text - Python puts an entry into the object's namespace (self.\_\_dict\_) with the key
text - 'name' (keys are strings) associated with value. We can do this in the \_\_init\_\_
text - method or after the object is constructed: both ways are shown above. When
text - self.name appears in an expression (e.g., a = self.name), Python substitutes the
text - expression self.\_\_dict\_\_['name'] for the right hand side of the =, to retrieve
text - the value of that name from the self object's namespace/dictionary.
text - 
text - If we try to access a non-existent attribute name (c.mumble in the class C
text - above, which is translated into c.\_\_dict\_\_['mumble']), Python raises an
text - exception. print(c.mumble) prints the exception as
text - 
text -   AttributeError: 'C' object has no attribute 'mumble'
text - 
text - Note that some names defined in \_\_init\_\_ (z above) always receive the same
text - initialization, so we don't need a parameter to initialize them. But often
text - names need to be initialized to different values when different objects are
text - constructed, so typically we add just enough parameters to the \_\_init\_\_  method
text - to allow us to specify how those names with different values should be
text - initialized.
text - 
text - ----------
text - Interlude: assert in initialization
text - 
text - Sometimes \_\_init\_\_ will ensure that a parameter is matched to an argument that
text - stores a legal and reasonable value for it; if not, Python will raise an
text - exception to indicate that the object being constructed cannot be properly
text - initialized. Sometimes it raises an exception explicitly, using an if statement
text - that tests for an illegal value. Sometimes it uses an assert statement for this
text - purpose. Remember that the form of assert is:
text - 
text -   assert boolean-test, string
text - 
text - which is equivalent to the slightly more verbose
text - 
text -   if not boolean-test:
text -       raise AssertionError(string)
text - 
text - I suggest that the string argument to AssertionError should always contain 4
text - parts:
text - 
text -   (1) The name of the class (if the problem occurs in the method of a class) or
text -       the name of the module (if the problem occurs in a function in a module)
text - 
text -   (2) The name of the method/function that detects the problem (here \_\_init\_\_)
text - 
text -   (3) A short description of the problem...
text - 
text -   (4) ...including the values of the argument(s) that is causing the problem
text - 
text - For example, if class C included an  \_\_init\_\_ method that required x's argument
text - to be a positive integer, we could write \_\_init\_\_ as follows. We typically check
text - all the arguments FIRST in this method, before binding any self/instance names.
text - 
text -   def \_\_init\_\_(self,x):
text -       assert type(x) is int and x > 0, 'C.\_\_init\_\_: x('+str(x)+') must be an int and positive'
text -       ...
text - 
text - or
text - 
text -   def \_\_init\_\_(self,x):
text -       assert type(x) is int and x > 0, 'C.\_\_init\_\_: x({v}) must be an int and positive'.format(v=x)
text -       ...
text - 
text - If so, calling C(-1) would result in the error message
text - 
text -    C.\_\_init\_\_: x(-1) must be an int and positive
text - 
text - and calling C('abc') would result in the error message
text - 
text -    C.\_\_init\_\_: x(abc) must be an int and positive
text - 
text - 
text - Such a message provides useful information to whoever is writing/debugging the
text - program. In a well-written program, someone just using the program (possible
text - not a programer) should not have to read/interpret such a message.
text - 
text - Question: if we wrote the assert as just x > 0, what exception would be raised
text - by calling C('abc') and why? Why is the given assertion "better", even though it
text - is less efficient to check?
text - 
text - We could be even more descriptive and write
text -   def \_\_init\_\_(self,x):
text -       assert type(x) is int, 'C.\_\_init\_\_: x('{v}') must be an int'.format(v=x)
text -       assert x > 0, 'C.\_\_init\_\_: x({v}) must be positive'.format(v=x)
text - 
text - Note that int refers to the int class (not an instance of an int class: e.g.,
text - not 1), so writing "type(x) is int" is checking whether the type of the object
text - x refers to is the same as the object int refers to: meaning x is bound to an
text - object constructed from the int class.
text - ----------
text - 
text - Once an object is constructed and initialized, typically we use it by calling
text - methods (or possibly passing it to another function/method that calls its
text - methods). We call an object's method using the syntax object.method(arguments);
text - recall the Fundamental Equation of Object-Oriented Programming.
text - 
text -     o.m(...) is translated into type(o).m(o,...)
text - 
text - This means, the method m in the class specifying the type of object o is called,
text - and the object o used to call the method is passed to the method as the first
text - argument (matching the self parameter). For example, 'a;b;c'.split(';') is
text - translated into type('a;b;c').split('a;b;c',';') which is equivalent to
text - str.split('a;b;c',';'). It calls the split method defined in the str class with
text - the arguments 'a;b;c' (matching the self parameter) and ';' (matching the
text - second parameter).
text - 
text - Before finishing the discussion of objects and their dictionaries, recall that
text - C refers to a class object. As an object, it also has a \_\_dict\_\_ that stores its
text - namespace. Here is some code that shows what names are defined in the C object.
text - 
text - for (k,v) in C.\_\_dict\_\_.items():
text -     print(k,'->',v)
text - 
text - And here is what it prints.
text - 
text - \_\_weakref\_\_ -> <attribute '\_\_weakref\_\_' of 'C' objects>
text - \_\_dict\_\_ -> <attribute '\_\_dict\_\_' of 'C' objects>
text - \_\_doc\_\_ -> None
text - \_\_init\_\_ -> <function C.\_\_init\_\_ at 0x03197618>
text - \_\_qualname\_\_ -> C
text - \_\_module\_\_ -> \_\_main\_\_
text - 
text - Note that \_\_init\_\_ is the only function that we defined, and it is there (on
text - the 4th line). Because I ran this code as a script (the main module), its
text - \_\_module\_\_ variable is bound to '\_\_main\_\_', which you should know something
text - about, because you should have written (and understand) code like
text - 
text -   if \_\_module\_\_ == '\_\_main\_\_':
text -       ...
text - 
text - If this module were imported, the \_\_module\_\_ key would be associated with the
text - file/module name it was written in, when imported.  Only the module
text - corresponding to the script that started execution has its \_\_module\_\_ key bound
text - to '\_\_main\_\_'. We can test this name to run the script only when its module is
text - run, not when it is imported by another module. The if accomplishes this
text - behavior.
text - 
text - If I had defined C with a docstring it would appear as an attribute of \_\_doc\_\_.
text - Don't worry about \_\_weakref\_\_, the internal \_\_dict\_\_ or \_\_qualname\_\_.
text - 
text - ------------------------------------------------------------------------------
text - 
text - Different kinds of names: definition and use
text - 
text - Let's discuss four different kinds of names in relation to classes. We will
text - call all these names variables.
text - 
text -   (1) local variables: defined and used inside functions/methods to help with
text -       the computation; parameter variables are considered local variables too.
text - 
text -   (2) instance variables of an object: typically defined inside \_\_init\_\_ and
text -       used inside class methods (we saw other ways to define them above too).
text -       These are referred to as self.name.
text - 
text -   (3) class variables: typically defined in classes (at same level as methods,
text -       not inside methods) and typically used in methods; we use a class
text -       variable for information COMMON to all objects of a class (rather than
text -       putting the same local variable in each object constructed from the
text -       class). Methods are actually class variables, bound to function objects.
text - 
text -       All class variables are defined in the class object and they are found by
text -       the Fundamental Equation of Object-Oriented Programming through instances
text -       of that class. That is, if class C defines an attribute a (method or
text -       class variable) and x refers to an object constructed from class C, then
text -       x.a will find attribut a in class C, but only if it is not stored
text -       directly in x (in x's \_\_dict\_\_). For class variables, that is typically
text -       what we want.
text - 
text -   (4) global variables: typically defined in modules (outside of functions and
text -       classes) and used inside functions and/or class methods; we typically
text -       avoid using them (don't use global variables), and when we do use them,
text -       we do so in a cautious and limited manner.
text - 
text - You should know how to use all these kinds of variables (and their semantics).
text - Use local variables and instance variables as needed (most function/methods
text - have the former, and most classes define the later in \_\_init\_\_ and use them in
text - methods). Class variables are sometimes useful to solve certain kinds of
text - problems where common information is stored among all the instances, by storing
text - them just once in their common class object. Global variables are fine to use in
text - scripts, but are often frowned upon when declared in modules that are imported
text - (although they too have their uses there, but in more advanced settings).
text - 
text - The following script uses each kind of variable, appropriately named. Ensure
text - that you understand how each use of these variables works. The use of the
text - command named 'global' (see the two lines with #comments) is explained in more
text - detail below.
text - 
text - global\_var = 0
text - 
text - class C:
text - 
text -     class\_var = 0
text - 
text -     def \_\_init\_\_(self,init\_instance\_var):
text -         self.instance\_var = init\_instance\_var
text - 
text -     def bump(self,name):
text -         print(name,'bumped')
text -         #global\_var = 100    # comment out this line or the next
text -         global global\_var    # comment out this line or the previous 
text -         global\_var += 1 
text -         C.class\_var += 1     # self.class\_var get translated to C.class\_var
text -         self.instance\_var += 1
text - 
text -     def report(self,var\_name):
text -         print('instance referred to by', var\_name,
text -               ': global\_var =', global\_var,
text -               '/class\_var =', self.class\_var,     # can write as self.\_class\_var
text -               '/instance\_var=', self.instance\_var)
text - 
text - x=C(10)
text - x.report('x')
text - x.bump('x')
text - x.report('x')
text - print()
text - 
text - prints
text -   instance referred to by x : global\_var = 0 /class\_var = 0 /instance\_var= 10
text -   x bumped
text -   instance referred to by x : global\_var = 1 /class\_var = 1 /instance\_var= 11
text - 
text - print('y = x')
text - y = x
text - y.bump('y')
text - x.report('x')
text - y.report('y')
text - print()
text - 
text - prints
text -   y = x
text -   y bumped
text -   instance referred to by x : global\_var = 2 /class\_var = 2 /instance\_var= 12
text -   instance referred to by y : global\_var = 2 /class\_var = 2 /instance\_var= 12
text - 
text - print('y = C(20)')
text - y=C(20)
text - y.bump('y')
text - y.report('y')
text - print()
text - 
text - prints
text -   y = C(20)
text -   y bumped
text -   instance referred to by y : global\_var = 3 /class\_var = 3 /instance\_var= 21
text - 
text - C.report(x,'x')       # same as x.report('x') by the Fundamental Equation of OOP
text - type(x).report(x,'x') # ditto: the meaning of the Fundamental Equation of OOP
text - print()
text - 
text - prints
text -   instance referred to by x : global\_var = 3 /class\_var = 3 /instance\_var= 12
text -   instance referred to by x : global\_var = 3 /class\_var = 3 /instance\_var= 12
text - 
text - 
text - print(C.class\_var, x.class\_var)  # discussed below
text - print(x.instance\_var)
text - 
text - prints
text -   3 3
text -   12
text - 
text - So, the global variable is changing every time, as is the class variable,
text - because there is just one of each. But each object thta is an instance of C
text - has its own instance variable, which changes only when bump is called on that
text - instance.
text - 
text - If we instead commented as follows
text -   
text -         global\_var = 100     # comment out this line or the next
text -         #global global\_var   # comment out this line or the previous 
text - 
text - running the script would have the following result: By removing the statement
text - global global\_var, then the statement global\_var = 100 actually defines a local
text - variable in the bump method -despite its name- so its increment does not affect
text - the true global\_var, which stays at zero.
text - 
text - Recall, if a variable defined in a function/method has not been declared global,
text - it is created as a local variable inside the function/procedure.
text - 
text - Note that one can REFER to the value of a global\_var inside methods of class C
text - WITHOUT a global declaration (see the report method), but if a method wants to
text - CHANGE global\_var it must declare it global (then all reference and changes are
text - to the real global variable). With no global global\_var, the assignment
text - global\_var = 100 creates a new name local to the bump method and always binds
text - it to 100.
text - 
text -   instance referred to by x : global\_var = 0 /class\_var = 0 /instance\_var= 10
text -   x bumped
text -   instance referred to by x : global\_var = 0 /class\_var = 1 /instance\_var= 11
text - 
text -   y = x
text -   y bumped
text -   instance referred to by x : global\_var = 0 /class\_var = 2 /instance\_var= 12
text -   instance referred to by y : global\_var = 0 /class\_var = 2 /instance\_var= 12
text - 
text -   y = C(20)
text -   y bumped
text -   instance referred to by y : global\_var = 0 /class\_var = 3 /instance\_var= 21
text -   instance referred to by x : global\_var = 0 /class\_var = 3 /instance\_var= 12
text - 
text -   1 1
text -   11
text - 
text - Finally, it is clear what C.class\_var and x.instance\_var refer to, but what
text - about x.class\_var? As shown above this prints 1 just as C.class\_var does. This
text - meaning is a result of the Fundamental Equation of Object Oriented Programming
text - (but applied to variable attributes, not method attributes).
text -   
text - -----
text - Technically, when specifying o.attr, any access to an attribute name in object o
text - (whether a variable or method) Python first tries to find attr in the object o;
text - if it is not defined in o's namespace/\_\_dict\_\_ Python uses the FEOOP to try to
text - find it by checking type(o).attr, which attempts to find attr in type(o)'s
text - namespace/\_\_dict\_\_. So when trying to find x.class\_var if fails to find
text - class\_var in o's namespace/\_\_dict\_\_, so it tries type(o).class\_var or
text - C.class\_var and finds the class\_var attribute in C's namespace/\_\_dict\_\_.
text - 
text - When we study inheritance, we will learn more about how Python searches for
text - all attributes by generalizing this rule: if it is not in an instance, then it
text - tries in the class that instance was constructed from, and if not in that
text - class, it tries in its super classes, and if not in its superclass....
text - 
text - But for now remember: to find an attribute, look first in the namespace of the
text - object; if it isn't there, then look in the namespace of the class thta the
text - object was constructed from.
text - -----
text - 
text - ------------------------------------------------------------------------------
text - 
text - Strange Python (but now understandable):
text -   1) Defining/using a method for a class, AFTER the class has been declared:
text - 
text -   2) Defining a method for an instance (but not the whole class) after the
text -        instance has been constructed:
text - 
text - We will now discuss one more interesting thing a dynamic language like Python
text - can do (but languages like Java and C++ cannot). We can change the meaning of
text - a class as a program is running. Let's go back to a very simple class C, that
text - stores one instance variable, but has no methods that change it. The report
text - method prints the value of this instance variable.
text - 
text - class C:
text -     def \_\_init\_\_(self,init\_instance\_var):
text -         self.instance\_var = init\_instance\_var
text - 
text -     def report(self,var\_name):
text -         print('instance referred to by', var\_name,
text -               '/instance\_var=', self.instance\_var)
text - 
text - Now look at the following code. It defines x to refer to an object constructed
text - from the class C, which defines only a report method (and then it calls that
text - method to report). Next it defines the bump function with a  first parameter
text - named self: its body increments the instance\_var in self's namespace
text - dictionary. We call bump with x and it updates x's instance\_var (as seen by the
text - report).
text - 
text - Then we do something strange. We add the bump function into the namespace of
text - C's class object with the name cbump (that is just the same as writing
text - C.\_\_dict\_\_['cbump'] = bump, creating the cbump method). We could have used just
text - bump, but instead used a slightly different name. Then, we call x.cbump('x')
text - which by the Fundamental Equation of OOP is the same as calling C.cbump(x,'x')
text - and because we just made the cbump attribute of the object representing class C
text - refer to the bump function, its meaning is to call the same function object
text - that bump refers to.
text - 
text - x=C(10)
text - x.report('x') # By FEOOP, exactly the same as calling C.report(x,'x')
text - print()
text - 
text - prints
text -   instance referred to by x /instance\_var= 10
text - 
text - 
added - ```python
code-start - def bump(self,name):
code -     print(name,'bumped')
code -     self.instance_var += 1
code-end - 
added - ```
text - bump(x,'x')
text - x.report('x')
text - print()
text - 
text - prints
text -   x bumped
text -   instance referred to by x /instance\_var= 11
text - 
text - C.cbump = bump; # put bump in the namespace of C, with the name cbump
text - x.cbump('x')    # By FEOOP, exactly the same as calling C.cbump(x,'x')
text - x.report('x')
text - print()
text - 
text - prints
text -   x bumped
text -   instance referred to by x /instance\_var= 12
text - 
text - y=C(20)
text - y.cbump('y')
text - y.report('y')
text - 
text - prints
text -   y bumped
text -   instance referred to by y /instance\_var= 21
text - 
text - So, even after the class C has been defined, we can still add a method to it 
text - namespace and then can call it using any object that has already been (or will
text - be) constructed from the class C. That is, we can change the meaning of a class
text - C, and all the objects constructed from the class will respond to the change
text - through the FEOOP.
text - 
text - Recall that the del command gets rid of an association in a dict. If we wrote
text - del C.cbump, then if we tried to call that method by x.cbump('x') the result
text - would be that Python raises 
text - 
text -   AttributeError: 'C' object has no attribute 'cbump'
text - 
text - So we can both ADD and REMOVE names from a class object's namespace!
text - 
text - Note that we could have defined/written bump as follows (note use of p not self
text - as the first parameter of bump)
text - 
added - ```python
code-start - def  bump(p,name):
code -     print(name,'bumped')
code -     p.instance_var += 1
code-end - 
added - ```
text - and nothing changes, so long as every occurrence of self is changed to p inside
text - the bump function (as is done). We could likewise write
text - 
added - ```python
code-start - def report(p,var):
code -     ...
code-end - 
added - ```
text - inside the C class itself. The parameter name self is just the standard name 
text - used in Python code; but there is nothing magical about this name, and we can
text - substitute whatever name we want for the first parameter. This parameter will
text - be matched with the object used to call the method. Although any name is
text - allowed, I strongly recommend always following Python's convention and using
text - the name self. Eclipse always supplies the name self automatically as the first
text - parameter to any methods we define.
text - 
text - ------------------------------
text - 
text - Defining a method for an instance (but not the whole class) after the
text -   instance has been constructed:
text - 
text - In fact, Python also allows us to add a reference to the bump method to a single
text - instance of an object constructed from class C, not the class itself. Therefore
text - unlike the example above, bump is callable only on that one object it was
text - added to, not on all the other instances of that class. We can also use this
text - technique to add a method to an object that is different than the one defined
text - in the object's class.
text - 
text - Start with the class C as defined above, defining just \_\_init\_\_ and report.
text - Then
text - 
added - ```python
code-start - def bump(self,name):
code -     print(name,'bumped')
code -     self.instance_var += 1
code-end - 
added - ```
text - x = C(0)
text - y = C(100)
text - 
text - x.bump = bump;
text - 
text - x.bump(x,'x')
text - x.bump(x,'x')
text - x.report('x')
text - 
text - y.bump(y,'y')     # fails because there is no bump attribute in the object y
text - y.report('y')     #    refers to, either directly in y or in y's class C
text - 
text - Note that calling x.bump(..) finds the bump method in x's namespace, without
text - needing to translate it using the Fundamental Equation of Object-Oriented
text - Programming. Without the translation, we explicitly need to pass the x argument
text - which becomes bump's self parameter.
text - 
text - Generally when looking up the attribute x.attr (whether attr is a variable or
text - method name), Python first looks in the namespace of the object x refers to,
text - and if it doesn't find attr, it next looks in the namespace of the object
text - type(x) refers to.
text - 
text - When run, this script produces:
text - 
text - x bumped
text - x bumped
text - instance referred to by  x /instance\_var= 2
text - Traceback (most recent call last):
text -   File "C:\Users\pattis\Desktop\python32\test\script.py", line 21, in <module>
text -     y.bump(y,'y')     # fails because there is no bump attribute in the object y
text - AttributeError: 'C' object has no attribute 'bump'
text - 
text - The error message mentions C because after failing to find the bump attribute
text - in the object y refers to, it looks in the object C refers to; when it fails
text - there too, the raised exception reports the error.
text - 
text - So, we can add methods to
text - 
text -   (a) classes: methods that all the instances of that class can refer to, via
text -       FEEOP
text - 
text -   (b) an instance of the class, such that only that instance can call the method
text -       (and their self parameter must also be passed as an explicit argument),
text -       since FEOOP is not used.
text - 
text - ------------------------------
text - 
text - Combining both worlds using delegation
text - 
text - Suppose that we wanted to be able to call methods attached to objects, but do
text - so using the standard object.method(...) syntax, as we did in the first part of
text - this section. In the second part of this section, we had to duplicate the
text - object, e.g., calling x.bump(x,'x') instead of just x.bump('x'). We will see
text - how to return to this simpler behavior using the concept of delegation in
text - Python.
text - 
text - We start by defining C as follows, adding the bump function below
text - 
text - class C:
text -     def \_\_init\_\_(self,init\_instance\_var):
text -         self.instance\_var = init\_instance\_var
text - 
text -     def report(self,var):
text -         print('instance referred to by', var,
text -               '/instance\_var=', self.instance\_var)
text - 
text -     def bump(self,name):
text -         try:
text -             self.object\_bump(self,name)
text -         except AttributeError:
text -             print('could not bump',name) # or just pass to handle the exception;
text -                                          # or omit try/except altogther
text - 
text - In this definition of C, there is a bump method defined in the class C, for all
text - instances constructed from this class to execute, findable by FEEOP.
text - 
text - When bump is called here, it tries to call a method named object\_bump on the
text - instance it was supplied, passing the object itself to object\_bump (doing
text - explicitly what FEOOP does implicitly/automatically). If that instance defines
text - an object\_bump function, it is executed; if not, Python raises an attribute
text - exception, which at present prints a message, but if replaced by pass would
text - just silently fail. Of course, we could also remove the entire try/except so an
text - attribute failure would raise an exception and stop execution.
text - 
text - Note that in the call x.bump(...) Python uses the Fundamental Equation of OOP
text - to translate this call into C.bump(x,'x'), which calls the equivalent of
text - x.object\_bump(x,'x').
text - 
text - In the world of programming, this is called delegation (which we will see more
text - of): the bump method delegates to the object\_bump method (if present) to get
text - its work done.
text - 
text - Here is a script, using this class. It attaches different object\_bump methods
text - to the instance x refers to, and the instance y refers to, but not to the
text - instance z refers to (nor to the class C). It calls this object\_bump method not
text - directly, but through delegation by calling bump in the C class.
text - 
text - x = C(10)
text - y = C(20)
text - z = C(30)
text - 
added - ```python
code-start - def bump1(self,name):
code -     print('bump1',name)
code -     self.instance_var += 1
code-end - 
added - ```
added - ```python
code-start - def bump2(self,name):
code -     print('bump2',name)
code -     self.instance_var += 2
code -     
code - x.object_bump = bump1
code - y.object_bump = bump2
code - # No binding of z.object_bump
code-end - 
added - ```
text - x.report('x')
text - x.bump('x')
text - x.report('x')
text - print()
text - 
text - prints
text -   instance referred to by x /instance\_var= 10
text -   bump1 x
text -   instance referred to by x /instance\_var= 11
text - 
text - y.report('y')
text - y.bump('y')
text - y.report('y')
text - print()
text - 
text - prints
text -   instance referred to by y /instance\_var= 20
text -   bump2 y
text -   instance referred to by y /instance\_var= 22
text - 
text - z.report('z')
text - z.bump('z')
text - z.report('z')
text - 
text - prints
text -   instance referred to by z /instance\_var= 30
text -   could not bump z
text -   instance referred to by z /instance\_var= 30
text - 
text - ------------------------------------------------------------------------------
text - 
text - Redefinition of Function Names
text - 
text - Note that we can redefine a function or class. For example, we can write
text - 
added - ```python
code-start - def f(): return 0
code - def f(): return 1
code - print(f())
code-end - 
added - ```
text - Calling f() would return 1. Eclipse gets upset about this, and marks the
text - second definition as an error (duplicate signature), but there is nothing
text - technically wrong with this code (although the first definition is useless,
text - and there may be a mistake in the spelling of one of these functions). Python
text - will run the script. We can also write the following script, which Eclipse
text - won't complain about, and runs the same.
text - 
added - ```python
code-start - def f(): return 0
code - def g(): return 1
code - f = g
code - print(f())
code-end - 
added - ```
text - Calling f() returns 1. Again, def just makes a name refer to a function object;
text - if, as in the case of the two definitions of the f name above, the name already
text - refers to an object, the binding of f is just changed to refer to the function
text - object g refers to.
text - 
text - Conceptually, it is no different than writing x = 1 and then x = 2 (changing
text - what x refers to from the int object 1 to the int object 2).
text - 
text - We can do the same thing for classes, as we saw with the names C and D in the
text - first example in these notes.
text - 
text - In summary, def f or class C just define a name and binds it to a function/class
text - object. We can call the function or the class's constructor. We can rebind that
text - name later to any other object. We can even write
text - 
added - ```python
code-start - def f(): return 0
code - f = 0
code-end - 
added - ```
text - Now f is bound to an int instance object, not a function object.
text - 
text - ------------------------------------------------------------------------------
text - 
text - Accessor/Mutator Methods (or query/command methods) and Instance Variables
text -   ...single/double underscore prefix
text - 
text - If calling o.method(...) returns information about an o's state but does not
text - change o's state, it is called an accessor (or query). If o.method(...) changes
text - o's state and returns None (all functions/methods must return some value), it
text - is called a mutator (or command). Some method calls do both: they change o's
text - state but also return a non-None value.
text - 
text - A design question arises. Suppose that we know that an object o of a class C
text - has an instance variable name iv: should we directly refer to o.iv? Should we
text - use its value by writing o.iv or change it by writing o.iv = ....? The high
text - road says, no: a class should hide the actual instance variables from the
text - clients/users of the class; they should provide methods to manipulate the
text - objects under control of the class. What instance variables we need to
text - implement a class might change over time, but the methods that define the
text - behavior of objects created from that class should stay the same and always
text - work correctly with whatever instance variables we are using.
text - 
text - Python is a bit at odds with this philosophy. Some languages (Java/C++) have a
text - mechanism whereby instance variables can be tagged PRIVATE, so that they are
text - accessible only from methods defined in the class itself; these languages do
text - not allow new instance variables nor methods to be added dynamically to objects
text - as Python does (as we showed above).
text - 
text - Python's philosophy is a bit more open to accessing instance variables outside
text - of the class methods. But there is danger in doing so, and beginners often use
text - this convenience and end up taking longer to get their code to work correctly,
text - and also make it harder to understand and change the code, because they are
text - accessing information that is not guaranteed to be there in future changes to
text - the code.
text - 
text - In fact, Python does have a weaker form of tagging PRIVATE names, which we can
text - use for names that should not be referred to outside the methods in the
text - object's class. Below we explain the meaning of instance variable names that
text - begin with one or two underscores (but don't have two trailing underscores, so
text - are unlike \_\_init\_\_).
text - 
text - -----
text - Single Underscore: 
text - 
text - When a programmer uses a single underscore in a name in a class (for data or a
text - method), he/she is indicating  that the name should NOT BE ACCESSED outside of
text - the methods in the class. But, there is nothing in the Python interpreter that
text - stops anyone from accessing that name. We can use this convention for private
text - data and helper methods.
text - 
text - class C:
text -     def \_\_init\_\_(self):
text -         self.\_mv = 1
text - 
text -     def \_f(self):
text -         return self.\_mv == 1
text -     
text - x =  C()
text - print(x.\_mv, x.\_f())
text - 
text - When run, this script produces: 1 True
text - 
text - Still, if a class is written with names prefixed by a single underscore, it
text - indicates that objects constructed from that class should not access those
text - names outside of the methods defined inside the class.
text - 
text - -----
text - Double Underscore:
text - 
text - If a Python name in a class begins with two underscores, it can be referred to
text - by that name in the class, but not easily outside the class: but it can still be
text - referred to outside the class, but with a "mangled" name that includes the name
text - of the class. If a class C defines a name \_\_mv then the name outside the class
text - can be referred to as \_C\_\_mv. This is called a "mangled" name.
text - 
text - So, if we changed the code in the class C above by writing \_mv as \_\_mv and \_f as
text - \_\_f, and tried to execute
text - 
text - x =  C()
text - print(x.\_\_mv, x.\_\_f())
text - 
text - Python would complain by raising an AttributeError exception for the first
text - value in the print statement
text - 
text -   AttributeError: 'C' object has no attribute '\_\_mv'
text - 
text - It x.\_\_mv was remove, Python would complain about x.\_\_f(), indicating the
text - object has no attribute '\_\_f'.  
text - 
text - But given class C defines \_\_mv, and \_\_f we could execute the following code
text - 
text - x =  C()
text - print(x.\_C\_\_mv, x.\_C\_\_f())
text - 
text - by writing the mangled names. When we run this script it again produces: 1 True
text - 
text - In fact, if we printed the dictionary for x, it would show '\_C\_\_mv' as a key,
text - which is the true name of these functions outside of the module.
text - 
text - print(x.\_\_dict\_\_) would print: {'\_C\_\_mv': 1}
text - 
text - So, Python does contain two conventions for hiding names: the first (one
text - underscore) is purely suggestive; the second (two underscores) actually makes
text - it harder to refer to such names outside of a class.
text - 
text - But neither truly prohibits accessing the information by referring to the name.
text - 
text - When we discuss operator overloading (later this week) and inheritance (later
text - in the quarter) we will learn more about controlling access to names defined
text - in objects and classes.
text - 
text - ------------------------------------------------------------------------------
text - 
text - Defining classes in unconventional places:
text - 
text - We normally define classes in modules, and often a module does nothing but
text - define just one class (although some define multiple, related classes). Other
text - modules define lots of functions. All such modules are called library modules
text - (not scripts) because they typically don't run code themselves, but we import
text - them to gain access to the names (of classes and functions) that they define.
text - (If they do run code, it is inside the code if \_\_module\_\_ == '\_\_main\_\_': and
text - often the code there allows us to test the class or module).
text - 
text - We can declare a class in a function and call the function to return an object
text - constructed from the class (and even use the returned object if we know 
text - its defined instance variables or methods).
text - 
added - ```python
code-start - def f(x):
code -     class C:
code -         def __init__(self,x):  self.val = x
code -         def double  (self)  :  return 2*self.val  
code -     return C(x)
code-end - 
added - ```
text - y = f(1)
text - print(y, y.val, y.double())
text - 
text - When run, this script prints the following.
text - 
text - <\_\_main\_\_.f.<locals>.C object at 0x02829170> 1 2
text - 
text - The first value indicates (reading the first word after the left angle-bracket,
text - from back to front) that class C is defined local to function f, which
text - is in the script we ran (named by Python to be \_\_main\_\_).
text - 
text - We can also declare a class in a class, and call some method in the class to
text - return an object constructed from the inner class (and even use the object if
text - we know its defined instance variables or methods).
text - 
text - class C:
text -     def \_\_init\_\_(self,x,y):
text -         self.x = x
text -         self.y = y
text -     class Cinner:
text -         def \_\_init\_\_(self,x):  self.val = x
text -         def double  (self)  :  return 2\*self.val  
text -     def identity(self)   : return (self.x, self.y)
text -     def x\_construct(self): return C.Cinner(self.x)
text -     def y\_construct(self): return C.Cinner(self.y)
text - 
text - z = C(1,2)
text - a = z.x\_construct()
text - b = z.y\_construct()
text - print( z, a, b,sep='\n')
text - print(z.identity(), a.double(), b.double())
text - 
text - When run this script prints:
text -   <\_\_main\_\_.C object at 0x02A36310>
text -   <\_\_main\_\_.C.Cinner object at 0x02A36290>
text -   <\_\_main\_\_.C.Cinner object at 0x02A36E70>
text -   (1, 2) 2 4
text - 
text - z.x\_construct() returns a reference to an instance of class C.Cinner, that was
text - initialized by the x parameter in the \_\_init\_\_ method. When we call a.double(),
text - the double method defined in Cinner() returns twice the value it was
text - initialized with.
text - 
text - In fact, instead of the x\_construct and y\_construct functions, we could define
text - a more general construct function that takes either 'x' or 'y as arguments and
text - constructs an object for self.x or self.y. The first way to do this uses the
text - parameter (matching 'x' or 'y') as a key to access \_\_dict\_\_
text - 
text -     def construct(self,which): return C.Cinner(self.\_\_dict\_\_[which])
text - 
text - The second way to do this uses the eval function, whose argument is either
text - 'self.x' or 'self.y' depdending on the value of which.
text - 
text -     def construct(self,which): return C.Cinner(eval('self.'+which))
text - 
text - ------------------------------------------------------------------------------
text - 
text - Defining/Using Static Methods in Classes
text - 
text - A method defined in a class is considered "static" if it does not have a self
text - parameter. Sometimes it is useful to write methods in a class that have this
text - property. 
text - 
text - For example, suppose that we wanted to declare a Point2d class for storing the
text - x,y coordinates for Points in 2-d space. The \_\_init\_\_ method would take two such
text - arguments. But suppose that we also wanted to create Point2d objects by
text - specifying polar coordinates (i.e, using a distance and angle in radians). We
text - could write such a class (with this static method) as follows.
text - 
text - import math
text - 
text - Class Point2d:
text -     def \_\_init\_\_(self,x,y):
text -         self.x = x
text -         self.y = y
text - 
text -     @staticmethod
text -     def from\_polar(dist,angle):
text -         return Point2d( dist\*math.cos(angle), dist\*math.sin(angle) )
text - 
text -     ...more code
text - 
text - This method is preceded by @staticmethod (a decorator: we will discuss
text - decorators later). It is meant to be called from outside the class, to create
text - Point2d objects from polar coordinates. We can write calls like
text - 
text -   a = Point2d(0., 1.)
text -   b = Point2d.from\_polar(1.0, math.pi/4)
text - 
text - Notice that we call Point2d.from\_polar outside of the class by using the class
text - name Point2d and the static method name from\_polar defined in that class. It
text - has no self parameter.
text - 
text - Likewise, suppose that we wanted to write a helper function for computing the
text - distance between two Point2d objects as a static method in this class. We could
text - write it as
text - 
text -     @staticmethod
text -     def \_distance(x1,y1,x2,y2):
text -         return math.sqrt( (x1-x2)\*\*2 + (y1-y2)\*\*2 )
text - 
text -     def dist(self,another):
text -         return Point2d.\_distance(self.x, self.y, another.x, another.y)
text - 
text - Here this helper function is meant to be called only by the dist method in
text - this class, so we write its name with a leading underscore. Note that again we
text - call it using Point2d. Because of FEOOP, we could also call this helper as
text - self.\_distance(self.x, self.y, another.x, another.y) because type(self) is
text - Point2d. Because it is static, the call does not put self as the first argument.
text - 
text - Finally, we could also write this helper function as a global function defined
text - outside Point2d, in the module that Point2d is defined in. But it is better to
text - minimize any kinds of global names; so, it is better to define this name inside
text - the class. In this way it won't conflict with any other name the importing
text - module has defined.
text - 
text - ------------------------------------------------------------------------------
text - 
text - Look at my Dice class (in dice.py in the courselib). It has many interesting
text - simple class features; it doesn't use many of the features discussed here.
text - 
text - Later during this week, we will learn how to use methods to overload operators
text - that allow us to take advantage of Python's syntactic features. We will learn
text - about many other special functions named like \_\_init\_\_  with double underscores
text - front and back.
text - 
text - ------------------------------------------------------------------------------
text - 
text - Problems:
text - 
text - 1) What does the following script print?
text - 
added - ```python
code-start - class C:
code -     def __init__(self):
code -         print('C object created')
code - D = C
code - def C():
code -   print('C function called')
code-end - 
added - ```
text - x = C()
text - y = D()
text - 
text - 
text - 2) What does the following script print? Draw a picture of the object x refers
text - to using the graphical form for representing objects we learned during Week #1.
text - 
text - class C:
text -     def \_\_init\_\_(self,a):
text -         self.a = a
text - 
text - x = C(1)
text - C.\_\_init\_\_(x,2)
text - print(x.a)
text - 
text - 
text - 3) Write a class C whose \_\_init\_\_ method has a self parameter, followed by low
text - and high. \_\_init\_\_ should store these values in self's dictionary using the
text - same names, but do so only if low is strictly less than high (otherwise it
text - should raise the AssertionError exception with an appropriate string.
text - 
text - 
text - 4) Explain why each of the following code fragments does what it does: two
text - execute (with different results) and one raises an exception.
text - 
added - ```python
code-start - g = 0			g = 0			g = 0
code - def f():		def f():		def f():
code -   print(g)		  print(g)		  print(g)		
code -   			  global g		  g += 1
code -   			  g += 1 		  
code-end - 
added - ```
text - f()			f()  			f()
text - g += 1			g += 1			g += 1
text - f()			f()  			f()
text - 
text - 
text - 5) Write a class C that uses a class variable to keep track of how many objects
text - are created from C (remember that each object creation calls \_\_init\_\_)
text - 
text - That is, for a class C
text - 
text - a = C(...)
text - b = C(...)
text - print(C.instance\_count) prints 2
text - c = C(...)
text - print(C.instance\_count) prints 3
text - 
text - 
text - 6) What would the following script print; explain why. Also, explain why the
text - call self.object\_bump(name) in bump is not self.object\_bump(self,name) as it
text - was in the notes.
text - 
text - class C:
text -     def \_\_init\_\_(self,init\_instance\_var):
text -         self.instance\_var = init\_instance\_var
text - 
text -     def report(self,var):
text -         print('instance referred to by', var,
text -               '/instance\_var=', self.instance\_var)
text - 
text -     def bump(self,name):
text -         try:
text -             self.object\_bump(name)
text -         except AttributeError:
text -             print('could not bump',name) # or just pass to handle the exception
text - 
text - x = C(10)
text - y = C(20)
text - 
added - ```python
code-start - def bump(self,name):
code -     print('bumped',name)
code -     self.instance_var += 1
code -     
code - C.object_bump = bump    
code-end - 
added - ```
text - x.report('x')
text - x.bump('x')
text - x.report('x')
text - 
text - y.report('y')
text - y.bump('y')
text - y.report('y')
text - 
text - 
text - 7) The function print is bound to a function object. What is printed by the
text - following script (and why)?
text - 
text - print = 1
text - print(print)
text - 
text - 
