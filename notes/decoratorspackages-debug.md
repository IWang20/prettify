text - # <div align=center>				Decorators</div>
added - ***
added - ## <a name="top"></a> Table of Contents
added - ***
text - 
text - We have discussed decorators before. Typically we described them as a class
text - that takes an argument that supports some protocol (methods) and returns an
text - object that supports the same protocol (methods). When the decorator object
text - executes these methods, it performs a bit differently than for the decorated
text - object. The decorator object typically stores a reference to the decorated
text - object and calls it when necessary.
text - 
text - The examples in this lecture, and the Check\_Annotations class in Programming
text - Assignment #4, use classes to decorate functions by using the \_\_call\_\_ protocol
text - to decorate/augment how functions are called. Although, some examples don't
text - need classes (and just use functions) to do the decoration.
text - 
text - Finally, it is appropriate to put this material right after our discussion
text - of recursion and functional programming. Decorating function calls is most
text - interesting when the functions are called many times. And, one call to a
text - recursive function results in it calling itself many times.
text - 
text - We will use the recursive factorial and fib (Fibonacci) functions in our
text - examples below. Both require an argument >= 0.
text - 
text -     def factorial(n):
text -         if n == 0:
text -             return 1
text -         else:
text -             return n\*factorial(n-1)
text - 
text -     def fib(n):
text -         if    n == 0: return 1
text -         elif  n == 1: return 1
text -         else:         return fib(n-1) + fib(n-2)
text - 
text - ------------------------------------------------------------------------------
text - 
text - Special Python Syntax for Decorators
text - 
text - If Decorator is the name of a decorator (a class or a function) that takes one
text - argument, we can use it to decorate a function object, by writing either
text - 
text -   def f(params-annotation) -> result-annotation:
text -       ...
text -   f = Decorator(f)
text - 
text - or
text - 
text -   @Decorator
text -   def f(params-annotation) -> result-annotation:
text - 
text - which has the same meaning. We can also use multiple decorators on functions.
text - The meaning of
text - 
added - ```python
code-start - @Decorator1
code - @Decorator2
code - def f(...)
code -     ...
code-end - 
added - ```
text - is equivalent to writing
text - 
text - f = Decorator1(Decorator2(f))
text - 
text - so Decorator1 decorates the result of Decorator2 decorating f; the decorators
text - are applied in the reverse of the order they appear (with the closest one to
text - the decorated object applying first).
text - 
text - There is a decorator name staticmethod which we have used to decorate methods
text - defined in classes: those methods that don't have a self parameter. Currently
text - Eclipse marks these as syntax errors (methods with no self parameter) but still
text - runs the code correctly. By using the staticmethod decorator, the syntax error
text - disappears.
text - 
text - ------------------------------------------------------------------------------
text - 
text - Examples of Function Decorators
text - 
text - Here are three decorators for functions (three are classes; some can be written
text - easily as functions too).
text - 
text - (1) Track\_Calls remembers the function it is decorating and initializes the
text - calls counter to 0; the decorator object overloads the \_\_call\_\_ method so that
text - all calls to the decorator object increment its calls counter and then actually
text - calls the decorated function (which if recursive, increments the calls counter
text - for every recursive call). Once the function's value is computed and returned,
text - the calls counter instance name can be accessed (via called) and reset (via
text - reset) for tracking further calls.
text - 
text - class Track\_Calls:
text -     def \_\_init\_\_(self,f):
text -         self.f = f
text -         self.calls = 0
text -     
text -     def \_\_call\_\_(self,\*args,\*\*kargs):  # bundle arbitrary arguments
text -         self.calls += 1
text -         return self.f(\*args,\*\*kargs)   # unbundle arbitrary arguments
text - 
text -     def called(self):
text -         return self.calls
text -     
text -     def reset\_calls(self):
text -         self.calls = 0
text - 
text - So, if we wrote
text - 
added - ```python
code-start - @Track_Calls
code - def factorial(n):
code -     if n == 0:
code -         return 1
code -     else:
code -          return n*factorial(n-1)
code-end - 
added - ```
text - then factorial would refer to a Track\_Calls object, whose self.f refers to the
text - actual function object factorial defined and whose self.calls is 0.
text - 
text - If we called factorial(3), Python executes the factorial.\_\_call\_\_(3) method on
text - the factorial object, which would increment factorial.calls and then call
text - factorial.f(3) - the original factorial function object: its body would
text - recursively call factorial(2), which Python executes as factorial.\_\_call\_\_(2).
text - 
text - This process continues, ultimately factorial.\_\_call\_\_(3) returns 6 with
text - factorial.calls = 4. We have seen before that factorial(n) calls itself for
text - n, n-1, n-2, ... 0 for a total of n+1 times.
text - 
text - Examine the picture accompanying this lecture, showing the Fibonacci function
text - (whose body does two recursive calls) and all the recursive calls it generates
text - when called with a variety of numbers. Ignore the colors for now.For example
text - calling fib(2) does a total of 3 calls to fib (counting itself), fib(3) does a
text - total of 5 calls, fib(4) does a total of 9 calls, fib(5) does a total of 15
text - calls, fib(6) does a total of 25 calls. We can use Track\_Calls to verify these
text - numbers (and compute fib for bigger numbers). For example fib(10) = 89 does a
text - total of 177 calls fib(20) = 10,946 and does a total of 21,891 calls.
text - 
text - Run the program accompanying this lecture to see the value (and number of
text - function calls) needed to evaluate fib(0) through fib(30). Notice that the last
text - few calculations take a noticable amount of time: computing fib(30) requires
text - 2,692,537 function calls!
text - 
text - We can write this decorator as the following function instead
text - 
added - ```python
code-start - def track_calls(f):
code -     def call(*args,**kargs):
code -         call.calls += 1
code -         return f(*args,**kargs)
code-end - 
added - ```
text -     call.calls = 0         # define calls attribute on call function-object
text -     return call
text - 
text - Here we define an inner-function call, which is returned by track\_calls. Before
text - returning this function object, a calls instance name is defined for that object
text - and initialzed to 0; inside the call function that instance name is incremented
text - before the original function (f) is called and the value it computes returned.
text - 
text - I will continue to show equivalent class/function definitions for the decorators
text - described below, but at the end I will briefly explain the reason for using
text - classes instead of functions: the ability to overload the \_\_getattr\_\_ function
text - for using multiple decorators at the same time.
text - 
text - ------------------------------------------------------------------------------
text - 
text - (2) Memoize remembers the function it is decorating and initializes a dict to
text - {}. It will use this dict to cache (keep track of and be able to access quickly)
text - the arguments to calls and the value ultimately returned by the function. The
text - decorator object overloads the \_\_call\_\_ method so that all calls to the
text - decorator object first check to see if the arguments are already cached in the
text - dict, and if so their associated value is returned immediately, without
text - executing the code in the decorated function; if not the decorated function is
text - called, its answer is cached in the dict with the function's arguments, and the
text - answer is returned. 
text - 
text - For simplicity here, I'm assuming all arguments are positional (so no \*\*kargs).
text - Also,since the arguments are used as keys in a dictionary, they must be
text - immutable/hashable.
text - 
text - In this way, a function never has to compute the same value twice. Memoization
text - is useful for multiply recursive calls, as in the fibonacci function.
text - 
text - class Memoize:
text -     def \_\_init\_\_(self,f):
text -         self.f = f
text -         self.cache = {}
text - 
text -     def \_\_call\_\_(self,\*args):
text -         if args in self.cache:
text -             return self.cache[args]
text -         else:
text -             answer = self.f(\*args)
text -             self.cache[args] = answer
text - 	    return answer
text - 
text -     def reset\_cache(self):
text -         self.cache = {}
text - 
text - Examine the picture accompanying this lecture, showing the Fibonacci function.
text - When decorated by Memoize, the only calls that actually compute a result are
text - those in green. As each green function calls finishes, it caches its result in
text - the dictionary. Afterward, calling fib with that argument again will obtain the
text - result from the cache immediately (all the calls in white). With memoization,
text - calling fib(n) does a total of n+1 function calls.
text - 
text - Run the program accompanying this lecture to see the value (and number of
text - function calls) needed to evaluate fib(0) through fib(30). This time, uncomment
text - @Memoize and the fib.reset\_cache() call in the loop. Notice that all the
text - calculations are done instantaneously: with memoization, computing fib(30)
text - requires only 31 function calls (with cached values computed immediately
text - billions of times).
text - 
text - We can also write memoize as a function, to return a wrapper function (it can
text - be named anything) that does the same operations as the class above. Note that
text - cache, a name local to memoize, is used in wrapper but not accessible outside
text - wrapper (unlike what we did with call.calls above)
text - 
added - ```python
code-start - def memoize(f):
code -     cache = {}
code -     def wrapper(*args):
code -         if args in cache: 
code -             return cache[args]
code -         else:
code -             answer = f(*args)
code -             cache[args] = answer
code -             return answer
code -     return wrapper
code-end - 
added - ```
text - ------------------------------------------------------------------------------
text - 
text - (3) Illustrate\_Recursive remembers the function it is decorating and
text - initializes a tracing variable to False. The decorator object overloads the
text - \_\_call\_\_ method so that all calls to the decorator object just return the
text - result of calling the decorated function (if tracing is off). Calling
text - .illustrate(...) on the decorator calls the illustrate method, which sets up
text - for tracing, and then uses \_\_call\_\_ to trace all entrances and exists to the
text - decorated function printing indented/outdented information for each function
text - call/return.
text - 
text - class Illustrate\_Recursive:
text -     def \_\_init\_\_(self,f):
text -         self.f = f
text -         self.trace = False
text -         
text -     def illustrate(self,\*args,\*\*kargs):
text -         self.indent = 0
text -         self.trace = True
text -         answer = self.\_\_call\_\_(\*args,\*\*kargs)
text -         self.trace = False
text -         return answer
text -     
text -     def \_\_call\_\_(self,\*args,\*\*kargs):
text -         if self.trace:
text -             if self.indent == 0:
text -                 print('Starting recursive illustration'+30\*'-')
text -             print (self.indent\*"."+"calling", self.f.\_\_name\_\_+str(args)+str(kargs))
text -             self.indent += 2
text -         answer = self.f(\*args,\*\*kargs)
text -         if self.trace:
text -             self.indent -= 2
text -             print (self.indent\*"."+self.f.\_\_name\_\_+str(args)+str(kargs)+" returns", answer)
text -             if self.indent == 0:
text -                 print('Ending recursive illustration'+30\*'-')
text -         return answer
text - 
text - Run the program accompanying this lecture to example of this trace (and others
text - by changing the program. Here is what the program prints for a call to factorial
text - and fibonacci.
text - 
added - ```python
code-start - @Illustrate_Recursive
code - def factorial(n):
code -     if n == 0:
code -         return 1
code -     else:
code -         return n*factorial(n-1)
code-end - 
added - ```
text - Starting recursive illustration------------------------------
text - calling factorial(5,){}
text - ..calling factorial(4,){}
text - ....calling factorial(3,){}
text - ......calling factorial(2,){}
text - ........calling factorial(1,){}
text - ..........calling factorial(0,){}
text - ..........factorial(0,){} returns 1
text - ........factorial(1,){} returns 1
text - ......factorial(2,){} returns 2
text - ....factorial(3,){} returns 6
text - ..factorial(4,){} returns 24
text - factorial(5,){} returns 120
text - Ending recursive illustration------------------------------
text - 
text - Factorial is a linear recursive function (one recursive call in its body) so
text - the structure of its recursion is simple.
text - 
text - 
added - ```python
code-start - @Illustrate_Recursive
code - def fib(n):
code -     if    n == 0: return 1
code -     elif  n == 1: return 1
code -     else:         return fib(n-1) + fib(n-2)
code-end - 
added - ```
text - print(fib.illustrate(5))
text - 
text - Starting recursive illustration------------------------------
text - calling fib(5,){}
text - ..calling fib(4,){}
text - ....calling fib(3,){}
text - ......calling fib(2,){}
text - ........calling fib(1,){}
text - ........fib(1,){} returns 1
text - ........calling fib(0,){}
text - ........fib(0,){} returns 1
text - ......fib(2,){} returns 2
text - ......calling fib(1,){}
text - ......fib(1,){} returns 1
text - ....fib(3,){} returns 3
text - ....calling fib(2,){}
text - ......calling fib(1,){}
text - ......fib(1,){} returns 1
text - ......calling fib(0,){}
text - ......fib(0,){} returns 1
text - ....fib(2,){} returns 2
text - ..fib(4,){} returns 5
text - ..calling fib(3,){}
text - ....calling fib(2,){}
text - ......calling fib(1,){}
text - ......fib(1,){} returns 1
text - ......calling fib(0,){}
text - ......fib(0,){} returns 1
text - ....fib(2,){} returns 2
text - ....calling fib(1,){}
text - ....fib(1,){} returns 1
text - ..fib(3,){} returns 3
text - fib(5,){} returns 8
text - Ending recursive illustration------------------------------
text - 
text - The fib function is NOT a linear recursive function: its body contains two
text - recursive calls. So the structure of its recursion is more complicated. This is
text - why thefib function is a good one to test both Track\_Calls and Memoize. Even
text - for fairly small arguments (under 30), it produces a tremendous number of calls
text - and can be sped-up tremendously by memoizing it.
text - 
text - ----------
text - 
text - Delegation of attribute lookup
text - 
text - When using (multiple) decorators, we need a way to translate attribute accesses
text - on the decorator into attribute accesses on the decorated. There is no simple
text - mechanism to do this with functions, but it easy to do with classes: by
text - overloading the \_\_getattr\_\_ method as follows (which should be done to all three
text - classes above).
text - 
text -     def \_\_getattr\_\_(self,attr):        # if attr not here, try self.f
text -         return getattr(self.f,attr)
text - 
text - So, if we use the following two decorators
text - 
text -     @Track\_Calls
text -     @Illustrate\_Recursive
text -     def fib(....):
text -         ...
text - 
text - fib is a Track\_Calls object, whose .f attribute is an Illustrate\_Recursive 
text - object whose .f attribute is the actual fib function. If we then wrote
text - fib.illustrate(5) Python would try to find the illustrate attribute of the
text - Track\_Calls object; there is no such attribute there, so it fails and then
text - calls the \_\_getattr\_\_ of the Track\_Calls class, which "translates" the failed
text - attribute access into getting the same attribute from the .f object (from the
text - decorated class object, an object of the Illustrate\_Recursive class, which does
text - define such an attribute, as a method which can be called).
text - 
text - Generally this is called delegation: where an "outer" object that does not have
text - some attribute delegates the attribute reference to an inner object. Decorators
text - often use exactly this form of delegation, so the decorator object can process
text - its attributes and all the attributes of the decorated object.
text - 
text - ------------------------------------------------------------------------------
text - 
text - Now we will study an interesting combination of using decorators and the
text - functools.partial function (discussed in the previous lecture). Let's look at
text - the following simple decorator, which takes a function and its name as
text - arguments; every time the function is called the decorator prints the
text - function's name and the result it computes.
text - 
text - class Trace:
text -     def \_\_init\_\_(self,f,f\_name):
text -         self.f = f
text -         self.f\_name = f\_name
text -         
text -     def \_\_call\_\_(self,\*args,\*\*kargs):
text -         result = self.f(\*args,\*\*kargs)
text -         print(self.f\_name+'called: '+str(result))
text -         return result
text - 
text - Now suppose we want to decorate a function f defined simple as
text - 
added - ```python
code-start - def f(x):
code -     return 2*x
code-end - 
added - ```
text - If we write
text - 
added - ```python
code-start - @Trace
code - def f(x):
code -     return 2*x
code-end - 
added - ```
text - Python raises a TypeError exception, because the \_\_init\_\_ for Trace requires
text - two arguments, not one. We can avoid the @ form of decorators and write
text - 
added - ```python
code-start - def f(x):
code -     return 2*x
code - f = Trace(f,'f')
code-end - 
added - ```
text - to solve the problem, but without using the @Decorator form. Can we do something
text - to use this form. The problem is that we have have two arguments to \_\_init\_\_ but
text - @Decorator requires just one, so we can use functools.partial to pre-supply the
text - second argument.
text - 
text - We can write
text - 
added - ```python
code-start - f_Trace = functools.partial(Trace,f_name='f')
code - @f_Trace
code - def f(x):
code -     return 2*x
code-end - 
added - ```
text - But even that is a bit clunky, because we are defining the name f\_Trace but
text - using it only once (we are not likely to trace other functions named f). We
text - don't need this name; instead we can write
text - 
added - ```python
code-start - @functools.partial(Trace,f_name='f')
code - def f(x):
code -     return 2*x
code-end - 
added - ```
text - directly using the result returned from partial as the decorator. This allows
text - us to use the standard @Decorator form (with possibly more than one decorator).
text - 
text - Now calling f(1) would return the result 2 and cause Python to print
text - 
text -   f called: 2
text - 
text - ------------------------------------------------------------------------------
text - 
text - Problems:
text - 
text - 1) Define a class that decorates function calls so that it keeps count of how
text - many times the function was called with each combination of arguments. Write
text - a report function that returns a list of 2-tuples, each containing the argument
text - and the number of times the function was called with that argument, sorted from
text - the most freqeuently to least frequently called argument).  Hint: this is
text - similar to what Track\_Calls and Memoize does.
text - 
text - 2) Define a Memoize class whose constructor also has a max\_to\_remember argument,
text - which limits the size of the dict to that number of entries (if the argument is
text - None, memoize all calls in the ditc). It should remember only the most recent
text - max\_to\_remember arguments. Hint: do this by keeping a list (really representing
text - a queue with oldest and youngest values) and a dict in synch as follows:
text -   (a) if the args are IN the dict, just return the result.
text -   (b) if the args are NOT IN the dict, and the list HAS ROOM, add the args to
text -          the list (at the end: youngest) and to the dict (with their computed
text -          value)
text -   (c) if the args are NOT IN the dict, and the list has NO MORE ROOM, pops the
text -          value out of index 0 (oldest) in the list and delete that as a key from
text -          the dict, then add the args to the list (at the end) and to the dict
text -          (with their computed value) 
text - Finally, write a function that returns a 4-tuple containing the number of hits
text - (times the function was callled on memoized arguments), misses (times the
text - function was callled on arguments that weren't memoized), the current size of
text - the list/dict, and the maximum size.
text - 
