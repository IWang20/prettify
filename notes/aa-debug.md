text - # <div align=center>	Analysis of Algorithms (Complexity Classes and Big-O Notation)</div>
added - ***
added - ## <a name="top"></a> Table of Contents
added - ***
text - 
text - 
text - Analysis of Algorithms is a mathematical area of Computer Science in which we
text - analyze the resources (mostly time, but sometimes space) used by algorithms to
text - solve problems. An algorithm is a precise procedure for solving a problem,
text - written in any notation that humans understand (and thus can carry-out the
text - algorithm): if we write an algorithm as code in some programming language, then
text - a computer can execute it too.
text - 
text - The main tool that we use to analyze algorithms is big-O notation: it means
text - "growth on the order of". We use big-O notation to characterize the performance
text - of an algorithm by placing it in a complexity class (most often based on its
text - WORST-CASE behavior -but sometimes on its AVERAGE-CASE behavior) when solving a
text - problem of size N: we will learn how to characterize the size of problem, which
text - is most often as simple as N is the number of values in a list/set/dictionary.
text - Once we know the complexity class of an algorithm, we have a good handle on
text - understanding its actual performance (within certain limits). Thus, in AA we
text - don't necessarily compute the exact resources needed, but typically an
text - approximate bound on the resources, based on the problem size.
text - 
text - ------------------------------------------------------------------------------
text - 
text - Getting to Big-O Notation: Throwing away Irrelevant Details
text - 
text - Here is a simple Python function for computing the maximum of a list (or
text - returning None if there are no values in the list).
text - 
added - ```python
code-start - def maximum(alist):
code -     answer = None if alist == [] else alist[0]
code -     for i in range(1,len(alist)):
code -         if alist[i] > answer
code -             answer = alist[i]
code -     return answer
code-end - 
added - ```
text - Often, the problem size is the number of values processed: e.g., the number of
text - values in a list or in a file. But we can use other metrics as well: it can be
text - the count of number of digits in an integer value, when looking at the
text - complexity of multiplication based on the size of the numbers. Thus, there is
text - no single measure of size that fits all problems: instead, for each problem we
text - try to choose a measure that makes sense and is natural for that problem.
text - 
text - Python translates functions like maximum into a sequence of instructions that
text - the computer executes (the subject of one of the last lectures). To solve a
text - problem, the computer always executes an integer number of instructions. For
text - simplicity, we will assume that all instructions take the same amount of time
text - to execute. So to compute the amount of time it takes to solve a problem is
text - equivalent to computing how many instructions the computer must execute to
text - solve it (which we can divide by the number of instructions/second the machine
text - executes to compute the time taken).
text - 
text - Again, we typically look at the worst case behavior of algorithms. For maximum
text - the worst case occurs if the list is in increasing order. In this case, each
text - new value examined in the list will be bigger than the previous maximum, so the
text - if statement's condition will always be True, which always requires updating
text - answer. If any value was lower, it wouldn't have to update answer and thus take
text - fewer instructions/less time to execute.
text - 
text - It turns out that for a list of N values, the computer executes 14N + 9
text - instructions in the worst case for this function. You need to know more CS than
text - you do at this time to determine this formula, but you will get there by
text - ICS 51 (and I expect to cover the basics during the last week of the quarter,
text - when I cover Python Machine Code).
text - 
text - A simple way to think about this formula is that there are 14 computer
text - instructions that are executed each time Python exceutes the body of the for
text - loop and 9 instructions that are executed only once: they deal with starting
text - and terminating the loop and the entire function. We can say I(N) = 14N + 9 for
text - the worst case of the maximum function, where I(N) is the number of
text - instructions the computer executes when solving a problem on a list with N
text - values.
text - 
text - I would like to argue now that if simplify this function to just I(N) = 14N we
text - have lost some information, but not much. Specifically, as N gets bigger (i.e.,
text - we are dealing with very big problems - the kinds computers are used to solve),
text - 14N and 14N+9 are relatively close. Let's look at the result of this function vs.
text - the original as N gets for values of N increasing by a factor of 10.
text - 
text -     N     |   14N + 9  |    14N     | error: (14N+9 - 14N)/(14N+9) as a % of N
text -  ---------+------------+------------+---------------------------
text -         1 |         23 |         14 |   61%         or 39%       accurate
text -        10 |        149 |        140 |    6%            94%       accurate
text -       100 |       1409 |       1400 |     .6%          99.4%     accurate
text -      1000 |      14009 |      14000 |     .06%         99.94%    accurate
text -      ...
text - 1,000,000 | 14,000,009 | 14,000,000 |     .00006%      99.99994% accurate
text -      ...
text - 
text - Each line shows the % error of computing 14N when the true answer is 14N + 9.
text - So by the time we are processing a list of 1,000 values, using the formula
text - 14N instead of 14N+9 is 99.94% accurate. For computers solving real problems,
text - a list of 1,000 values is small: a list of millions is more normal. For
text - 1,000,000 values 14N is off by just 9 parts in 14 million. So the 9 doesn't
text - affect the formula much.
text - 
text - Analysis of Algorithms really should be referred to as ASYMPTOTIC Analysis of
text - Algorithms, as it is mostly concerned with the performance of algorithms as
text - the problem size gets very big (N -> infinity). We see here that as N->Infinity
text - 14N is a better and better approximation to 14N+9: dropping the extra 9 becomes
text - less and less important.
text - 
text - A simple function for sorting is the following. This is much simpler than the
text - real sort method in Python (and the simplicity results in the function taking
text - much more time, but it is a good starting point for understanding sorting now).
text - If you are interested in how this function accomplishes sorting, hand simulate
text - it working on a list of 5-10 values (try increasing values, decreasing values,
text - values in random order): basically each execution of the outer loops mutates the
text - list so that the next value in the list is in the correct position.
text - 
added - ```python
code-start - def sort(alist):
code -     for base in range(len(alist)):
code -         for check in range(base+1,len(alist)):
code -             if alist[base] > alist[check]:
code -                 alist[base], alist[check] = alist[check],alist[base]
code -     return None  # list is mutated
code-end - 
added - ```
text - 
text - It turns out that for a list of N values, the computer executes 8N\*\*2 + 12N + 6
text - instructions in the worst case for this function. The outer loop executes N
text - times (N is len(alist)) and inner loop on average executes N/2 times, so the
text - if statement in the inner loop is executed a quadratic number of times. We can
text - say I(N) = 8N\*\*2 + 12N  + 6 for the worst case of the sort function, where I(N)
text - is again the number of instructions the computer executes. Or to be more
text - specific we would write Isort(N) = 8N\*\*2 + 12N  + 6.
text - 
text - I would like to argue in the same way that if simplify this function to just
text - I(N) = 8N\*\*2, we have not lost much information. Let's look at the result of
text - this this function vs. the original as N gets bigger and bigger
text - 
text -     N     | 8N\*\*2+12N+6|      8N\*\*2  | error: (12N+6)/(8N\*\*2+12N+6) as a % of N
text -  ---------+------------+-------------+---------------------------
text -         1 |         26 |           8 |   70%      or 30%    accurate
text -        10 |        926 |         800 |   14%         86%    accruate
text -       100 |     81,206 |      80,000 |    1.5%       98.5%  accurate
text -      1000 |  8,012,006 |   8,000,000 |     .15%      99.85% accurate
text - 
text - So by the time we are processing a list of 1,000 values, using the formula
text - 8N\*\*2 instead of 8N\*\*2 + 12N + 6 is 99.85% accurate. For 1,000,000 values
text - (10\*\*6), 8N\*\*2 is 8\*10^12 while 8N\*\*2 + 12N + 6 is 8\*10^12 +12\*10^6 + 6; the
text - simpler formula is is 99.999985% accurate. 
text - 
text - CONCLUSION (though not proven): If the real formula I(N) is a sum of a bunch of
text - terms, we can drop any term that doesn't grow as quickly as the most quickly
text - growing term. In computing the maximum, the linear term 14N grows more quickly
text - than the next term, the constant 9, which doesn't grow at all (as N grows) so
text - we drop the 9 term. In sorting, the quadratic term 8N\*\*2 grows more quickly
text - than the next two terms, the linear term 12N and the constant 6, so we drop the
text - 12N and 6 terms.
text - 
text -     In fact note that we can prove that the Limit as N-> of 12N/8N\*\*2 = 
text -     3/(2N) -> 0, which means we can discard the 12N term as growing more slowly
text -     thatn the 8N\*\*2 term.
text - 
text - The result is a simple function that is still an accurate approximation of the
text - number of computer instructions executed for lists of various large sizes.
text - 
text - We now will explain a rationale for dropping the constant in front of N and
text - N\*\*2, and classifying these algorithms as O(N) growing at a linear rate and
text - O(N\*\*2) growing at a quadratic rate. Again O means "grows on the order of",
text - so O(N) means grows on the order of N and O(N\*\*2) means grows on the order of
text - N\*\*2.
text - 
text - 1) If we assume that every instruction in the computer takes the same amount
text -    of time to execute. Then the time taken for maximum is about 14N/speed and
text -    time for sort is about 8N\*\*2/speed. We should really think about these
text -    formulas as (14/speed)N and (8/speed)N\*\*2. We know the 14 and 8 came from
text -    the number of instructions inside loops that Python needed to execute: but a
text -    different Python interpreter (or a different language) might generate a
text -    different number of instructions and therefore a different constant. Thus,
text -    this number is based on technology, and we want our analysis to be
text -    independent of technology. And, of course, "speed" changes based on
text -    technology too.
text - 
text - Since we are trying to come up with a "science" of algorithms, we don't want
text - our results to depend on technology, so we are also going to drop the constant
text - in front of the biggest term as well. For the reason explained above (relating
text - to instructions generated by Python and the speed of the machine), this number
text - is based solely on technology.
text - 
text - Here is another justification for not being concerned with the constant in
text - front of the biggest term.
text - 
text - 2) The fundamental question that we want answered about any algorithm is, "how
text -    much MORE resources does it need when solving a problem TWICE AS BIG". In
text -    maximum, when N is big (so we can drop the +9 without losing much accuracy)
text -    the ratio of time to solve solve a problem of size 2N to the time to solve a
text -    problem of size N is easily computed:
text - 
text -     Imaximum(2N)     14(2N)
text -    -------------- ~ -------- ~ 2
text -     Imaximum(N)      14 N
text - 
text -    The ratio is a simple number (no matter how many instructions are in the
text -    loop, since the constant 14 appears as a multiplicative factor in both the
text -    numerator and denominator, and cancels itself out).  So, we know for this
text -    code, if we double the size of the list, we double the number of instructions
text -    that maximum executes to solve the problem, and thus double the amount of
text -    time (for whatever the speed of the computer is).
text - 
text -    Thus, the constant 14 is irrelevant when asking this "doubling" question. 
text - 
text -    Likewise, for sorting we can write
text - 
text -     Isort(2N)     8(2N)\*\*2
text -    ----------- ~ ---------- ~ 4
text -     Isort(N)      8 N\*\*2
text - 
text -    Again, the ratio is is a simple number, with the constant (no matter what it
text -    is, disappearing).  So, we know for this code that if we double the size of
text -    the list, we increase by a factor of 4 the number of instructions that are
text -    executed, and thus increase by a factor of 4 the amount of time (for
text -    whatever the speed of the computer is).
text - 
text -    Thus, the constant 8 is irrelevant when asking this "doubling" question. 
text - 
text -    Note if we didn't simplify, we'd have
text - 
text -     I(2N)     8(2N)\*\*2 + 12(2N) + 6     32N\*\*2 + 24N + 6
text -    ------- = ----------------------- = -----------------------
text -     I(N)      8N\*\*2 + 12N + 6            8N\*\*2 + 12N + 6
text - 
text -    which doesn't simplify easiy; although, as N->inifinty, this ratio gets
text -    closer and closer to 4 (and is very close even for small-sized problems).
text - 
text - As with air-resistance and friction in physics, typically ignoring the
text - contribution of these negligible factors (for big, slow-moving objects) allows
text - us to quickly solve an approximately correct problem.
text - 
text - Using big-O notation, we say that the complexity class of the code to find the
text - maximum is O(N). The big-O means "grows on the order of" N, which means a
text - linear growth (double the input size, double the time). For the sorting code,
text - its complexity class is O(N\*\*2), which means grows on the order of N\*\*2, which
text -  means a quadratic growth rate (double the input size, quadruple the time).
text - 
text - ----------
text - IMPORTANT: A Quick way to compute the complexity class of an algorithm
text - 
text - To analyze a Python function's code and compute its complexity class, we
text - approximate the number of times the most frequently executed statement is
text - executed, dropping all the lower (more slowly growing) terms and dropping the
text - constant in front of the most frequently executed statement (the fastest
text - growing term). We will show how to do this much more rigorously in the next
text - lecture.
text - 
text - The maximum code executes the if statement N times, so it is O(N). The sorting
text - code executes the if statement N(N-1)/2 times (we will justify this number
text - below), which is N\*\*2/2 - N/2, so dropping the lower term and the constant 1/2,
text - yields a complexity class of O(N\*\*2)
text - ----------
text - 
text - ------------------------------------------------------------------------------
text - 
text - Comparing Algorithms by their complexity classes
text - 
text - Primarily from this definition we know that if two algorithms, a and b, both
text - solve some problem, and a is in a lower complexity class than b, then for all
text - BIG ENOUGH N, Ta(N) < Tb(N): here Ta(N) means the Time it takes for algorithm
text - a to solve the problem. Note that nothing here is said about small N; which
text - algorithms uses fewer resources on small problems depends on the actual
text - constants we dropped (and even on the terms that we dropped), so complexity
text - classes have little to say for small problem sizes.
text - 
text - For example, if algorithm a is O(N) with a constant of 100, and algorithm b
text - is O(N\*\*2) with a constant of 1, then for values of N in the range [1,100],
text - 
text -    Tb(N) = 1N\*\*2 <= 100N = Ta(N)
text - 
text - but for all values bigger than 100,
text - 
text -    Ta(N) = 100N <= 1N\*\*2 = Tb(N)
text - 
text - As a second example, if algorithm a is O(N) with a constant of 1, and algorithm
text - b is O(N\*\*2) with a constant of 10, then for all values of N
text - 
text -    Ta(N) = 1N <= 10N\*\*2 = Tb(N)
text - 
text - So in some cases a lower complexity class can be worse for small N, and in some
text - cases it can be better. We need to go beyond complexity classes to find out.
text - But it is guaranteed that FOR ALL SIZES BEYOND SOME VALUE OF N, the algorithm
text - in the lower complexity will run faster.
text - 
text - Again, we use the term "asymptotic" analysis of algoritms to indicate that we
text - are mostly concerned with the time taken by the code when N gets very large
text - (going towards infinity). In both cases, because of their complexity classes,
text - algorithm a will be better.
text - 
text - What about the constants? Are they likely to be very different in practice? It
text - is often the case that the constants of different algorithms are close. (They
text - are often just the number of instructions in the main loop of the code). So the
text - complexity classes are a good indication of faster vs slower algorithms for all
text - but the smallest problem sizes.
text - 
text - Although all possible mathematical functions might represent complexity classes
text - (and many strange ones do), we will mostly restrict our attention to the
text - following complexity classes. Note that complexity classes can interchangably
text - represent computing time, the # of machine operations executed, and such more
text - nebulous terms as "effort" or "work" or "resources".
text - 
text - As we saw before, a fundamental question about any algorithm is, "What is the
text - time needed to solve a problem twice as big". We will call this the SIGNATURE
text - of the complexity class (knowing this value empirically often allows us to know
text - the complexity class as well). 
text - 
text - Class   |  Algorithm Example				| Signature
text - --------+-----------------------------------------------+----------------------
text - O(1)	| pass argument->parameters/copying a reference | T(2N) = T(N)
text - O(LogN) | binary searching of a sorted list		| T(2N) = c + T(N)
text - O(N)	| linear searching a list (the in operator)	| T(2N) = 2T(N)
text - O(NLogN)| Fast sorting					| T(2N) = cN + 2T(N)
text - 
text -   Fast algorithms come before here; NLogN grows a bit more slowly than linearly
text -   (because logarithms grow so slowly compared to N) and nowhere near as fast as
text -    O(N\*\*2)
text - 
text - O(N\*\*2) | Slow sorting; scanning N times list of size N | T(2N) = 4T(N)
text - O(N\*\*3) | Slow matrix multiplication		        | T(2N) = 8T(N)
text - O(N\*\*m) | for some fixed m: 4, 5, ...			| T(2N) = 2\*\*mT(N)
text - 
text -   Tractable algorithms come before here; their work is polynomial in N.
text -   In the complexity calss below N is in an exponent.
text - 
text - O(2\*\*N) | Finding boolean values that satisfy a formula | T(2N)=2\*\*N T(N)
text - 
text - For example, for an O(N\*\*2) algorithm, doubling the size of the problem
text - quadruples the time required: T(2N) ~ c(2N)\*\*2 = c4N\*\*2 = 4cN\*\*2 = 4T(N).
text - 
text - Note that in Computer Science, logarithms are mostly taken to base 2. (Remember
text - that algorithms and logarithms are very different terms). All logarithms are
text - implicitly to base 2 (e.g., Log N = Log2 N). You should memorize and be able to
text - use the following facts to approximate logarithms without a calculator.
text - 
text - Log 1000 ~ 10
text -   Actually, 2\*\*10 = 1,024, 2\*\*10 is approximatley 1,000 with < a 3% error.
text - 
text - Log a\*\*b = b Log a, or more usefully, Log 1000\*\*N = N Log 1000; so ...
text -   Log 1,000,000     = 20 : 1,000,000     = 1,000\*\*2; Log 1000\*\*2 = 2\*Log 1000
text -   Log 1,000,000,000 = 30 : 1,000,000,000 = 1,000\*\*3; Log 1000\*\*3 = 3\*Log 1000
text - 
text - So note that Log is a very slowly growing function. When we increase from
text - Log 1,000 to Log 1,000,000,000 (the argument grows by a factor of 1 million)
text - the result only grows by from 10 to 30 (by a factor or 3).
text - 
text - In fact, we can compute these logarithms on any calculator that computes Log
text - in any base. For example,
text - 
text - Log (base b) X = Log (base a) X / Log (base a) b
text - 
text - So, Log (base b) X is just a constant (1/Log (base a) b) times Log (base a) X,
text - so logarithms to any base are really all the same complexity class (regardless
text - of the base) because they differ only by a multiplicative constant (which we
text - ignore in complexity classes). For example,
text - 
text - Log(base 10) X = Log(base 2) X  /  Log(base 2) 10 ~ .3 Log(base 2) X
text - 
text - ----------
text - IMPORTANT: Determining the Complexity Class Empirically - from a Signature
text - 
text - If we can demonstrate that doubling the size of the input approximately
text - quadruples the time of an algorithm, then the algorithm is O(N\*\*2). We can use
text - the signatures shown above for other complexity classes as well. Thus, even if
text - we cannot mathematically analyze the complexity class of an algorithm based on
text - inspecting its code (something we will highlight in the next lecture), if we
text - can measure it running on various sized problems (doubling the size over and
text - over again), we can use the signature information to approximate its complexity
text - class. In this approach we don't have to understand (even look at) the code.
text - ----------
text - 
text - ------------------------------------------------------------------------------
text - 
text - Computing Running Times from Complexity Classes
text - 
text - We can use knowledge of the complexity class of an algorithm to predict its
text - actually running time on a computer as a function of N easily. For example, if
text - we know the complexity class of algorithm a is O(N\*\*2), then we know that
text - Ta(N) ~ cN\*\*2 for some constant c. The constant c represents the "technology"
text - used: the language, interpreter, machine speed, etc.; the N\*\*2 (from O(N\*\*2))
text - represents the "science/math" part: the complexity class. Now, given this
text - information, we can time the algorithm for some large value N. Let's say for
text - N = 10,000 (which is actually a pretty small N these days) we find that
text - Ta(10,000) is 4 seconds.
text - 
text - First, if I asked you to estimate Ta(20,000) you'd  immediately know it is
text - about 16 seconds (doubling the input of an O(N\*\*2) algorithm approximately
text - increases the running time by a factor of 4). Second, if we solve for c we have
text - 
text -   Ta(N) ~ cN\*\*2, substituting 10,000 for N and 4 for Ta(N) we have
text -   Ta(10,000) = 4 ~ c 10,000\*\*2 (from the formula), so solving for the the
text -   technology constant c ~ 4x10\*\*-8.
text - 
text - So, by measuring the run-time of this code, we can calculate the constant "c",
text - which involves all the technology (language, interpreter, computer speed, etc).
text - Roughly, we can think of c as being the amount of time it takes to do one loop
text - (# of instructions per loop/speed of executing instructions) where the
text - algorithm requires N\*\*2 iterations through the loops to do all its work.
text - 
text - Therefore, Ta(N) ~ 4x10\*\*(-8) x N\*\*2. So, if asked to estimate the time to
text - process 1,000,000 (10\*\*6) values (100 times more than 10,000), we'd have
text - 
text -   Ta(10\*\*6) ~ 4x10\*\*(-8) x (10\*\*6)\*\*2
text -   Ta(10\*\*6) ~ 4x10\*\*(-8) x 10\*\*12
text -   Ta(10\*\*6) ~ 4x10\*\*4, or about 40,000 seconds (about 1/2 a day)
text - 
text - Notice that solving a problem 100 times as big take 10,000 (which is 100\*\*2)
text - times as long, which is based on the signature for an O(N\*\*2) algorithm when
text - we increase the problem size by a factor of 10. If we go back to our sorting
text - example,
text - 
text -     I(100N)    8(100N)\*\*2
text -    ------- ~ ------------- ~ 10,000
text -     I(N)       8 N\*\*2
text - 
text - In fact, while we often anaylze code to determine its complexity class, if we
text - don't have the code (or find it too complicated to analyze) we can double the
text - input sizes a few times and see whether we can "fit the resulting times" to any
text - of the standard signatures to estimate the complexity class of the algorithm.
text - We should do this for some N that is as large as reasonble (taking some number
text - of seconds to solve on the computer).
text - 
text - Note for an O(2\*\*N) algorithms, if we double the size of the problem from 100
text - to 200 values the amount of time needed goes up by a factor of 2\*\*100, which is
text - ~ 1.3x10\*\*30. Notice that adding one more value to process doubles the time:
text - this "exponential" time is the inverse function of logarithmic time, in terms
text - of its growth rate: it grows incredibly quickly while logarithms grow incredible
text - slowly.
text - 
text - 
text - ------------------------------------------------------------------------------
text - 
text - Odds and Ends:
text - 
text - Note too that it is important to be able to analyze the following code. Notice
text - that the upper bound of the inner loop (i) is changed by the outer loop. 
text - 
text - for i in range(N):
text -   for j in range(i):
text -      body
text - 
text - How many times does the "body" of the loop get executed? When the outer loop
text - index i is 0, "body" gets executed 0 times; when the outer loop index i is 1,
text - "body" gets executed 1 time; when the outer loop index i is 2, "body" gets
text - executed 2 times; .... when the outer loop index i is N-1 (as big as i gets),
text - "body" gets executed N-1 times. So, in totality, "body" gets executed
text - 0 + 1 + 2 + 3 + ... + N-1 times, or dropping the 0, just 1 + 2 + 3 + ... + N-1
text - times. Is there a simpler way to write this sum?
text - 
text - There is a simple, general closed form solution of adding up consecutive
text - integers. Here is the proof that 1 + 2 + 3 + ... + N =N\*(N+1)/2. This is a
text - direct proof, but this relationship can also be proved by induction.
text - 
text - Let
text - 
text - S = 1 + 2 + 3 + ... + N-1 + N.
text - 
text - Since the order of the numbers makes no difference in the sum, we also have
text - 
text - S = N + N-1 + ... + 3 + 2 + 1.
text - 
text - If we add the left and right side (column by column) we have
text - 
text - S   =   1  +    2  +   ...  +   N-1  +  N
text - S   =   N  +   N-1 +   ...  +   2    +  1
text - -------------------------------------
text - 2S  = (N+1) + (N+1) +  ... +  (N+1) + (N+1)
text - 
text - That is, each pair in the column sums to N+1, and there are N pairs to sum.
text - Since there are N pairs, each summing to N+1, the right hand side can be
text - simplified to just N\*(N+1). so
text - 
text - 2S = N\*(N+1), therefore S = N(N+1)/2 = N\*\*2/2 + N/2
text - 
text - Thus, S is O(N\*\*2): with a constant of 1/2 and a term of N/2 that is dropped
text - (because its order is lower than that N\*\*2). Note that either N or N+1 is
text - an even number, so dividing their product by 2 is always a integer as it must
text - be for the sum of integers: 6\*7/2 = 21.
text - 
text - So, looking back at the example of the code above, the total number of times
text - the body gets executed is 0 + 1 + 2 + ... + N-1 which is the same as
text - 1 + 2 + ... + N-1 so plugging N-1 in for N we have (N-1)(N-1+1)/2 = 
text - N\*\*2/2 - N/2 which is still O(N\*\*2) for the same reason.
text - 
text - We can apply this formula for putting N values at the end of a linked list
text - that is initially empty (and has no cache reference to the last node). To put
text - in the 1st value requires skipping 0 nodes; to put in the 2nd value requires
text - skipping 1 nodes; to put in the 3rd value requires skipping 2 nodes; ...
text - to put in the Nth value requires skipping N-1 nodes. So the number of nodes
text - skipped is 0 + 1 + ... + N-1, which by the formula is (N-1)N/2: so building a
text - linked list in this way is in the O(N\*\*2) complexity class.
text - 
text - 
text - Fast Searching and Sorting:
text - 
text - There are obvious algorithms for searching a list in complexity O(N) and 
text - sorting a list in complexity O(N\*\*2). But, there are surprisingly faster
text - algorithms for these tasks: searching a list can be O(Log N) if the list is
text - sorted (binary search); and sorting values in a list can be in O(N Log N).
text - 
text - In lecture, I will briefly discuss the binary searching algorithm, for searching
text - a sorted list: its complexity class is O(Log N). In fact the constant is 1,
text - which means that when searching a list of 1,000,000 values, we must access the
text - list at most about 20 times to either (a) find the index of the value in the
text - list or (b) determine the value is not in the list. This is potentially 50,000
text - times faster than a loop checking one index after another (which we must do if
text - the list is not sorted)! On large problems, algorithms in a lower complexity
text - class typically execute much faster. Also note that when increasing the size of
text - the list by 1,000 (to 1 billion values) the maximum number of accesses goes from
text - 20 to 30.
text - 
text - You should know that Python's sorting method on lists is O(N Log N), but we
text - will not discuss the algorithm. As with binary searching, we will discuss the
text - details in ICS-46.
text - 
text - Note that we CANNOT perform binary searching efficiently on linked lists,
text - because we cannot quickly find the middle of a linked list (for lists we just
text - compute the middle index and access the list there). In fact, we have seen
text - another self-referential data structure, trees, that can be used to perform
text - efficient searches. 
text - 
text - Sorting is one of the most common tasks performed on computers. There are
text - hundreds of different sorting algorithms that have been invented and studied. 
text - Many small and easy to write sorting algorithms are in the O(N\*\*2) complexity
text - class (see the sorting code above, for example). Complicated but efficient 
text - algorithms are often in the O(N Log N) complexity class. We will study sorting
text - in more detail in ICS-46, including a few of these efficient algorithms.
text - 
text - For now, memorize that fast (binary) searching is in the O(Log N) complexity
text - class and fast sorting algorithms are in the O(N Log N) complexity class. If
text - you are ever asked to analyze the complexity class of a task that requires
text - sorting data as part of the task, assume the sorting function/method is in the
text - O(N Log N) complexity class.
text - 
text - 
text - Closing:
text - 
text - To close for now, finding an algorithm to solve a problem in a lower complexity
text - class is a big accomplishment; a more minor accomplishment might be decreasing
text - the constant for an algorithm in the same complexity class (certainly useful,
text - but often based more on technology than science). By knowing the complexity
text - class of an algorithm we know a lot about the performance of the algorithm
text - (especially if we measure the time it takes to solve certain sized problems:
text - using this information we can accurately predict the time to solve other size
text - problems). Finally, We can also reverse the process, and use a few measurements,
text - doubling the size of the problem each time, to approximate the complexity class
text - of an algorithm.
text - 
text - ------------------------------------------------------------------------------
text - 
text - Coming up: In the next two lectures we will first learn the complexity classes
text - of many of the operations in Python, so we can better directly analyze Python
text - code that use these operations to determine its complexity class. The second
text - lecture includes code for easily timing Python functions, including dicussion
text - of hashing that will reveal why dicts and sets behave the way they do in Python.
text - 
text - ------------------------------------------------------------------------------
text - 
text - Problems:
text - 
text - 1) A PC in the 1980s executed 10\*\*6 operations per second; a PC today executes
text - 1,000 times faster 10\*\*9 operations per second. Assume a slow sorting method
text - takes exactly N\*\*2 operations to sort a list of length N; assume a fast sorting
text - method takes exactly 5 N Log N operations to sort a list of length N. When
text - sorting a small amount of data, the fast machine runinng the slow algorithm
text - (call this configuration 1) is better; when sorting a large amount of data the
text - slow machine running the fast algorithm is better (call this configuration 2).
text - For approxmiately what size array does the transition occur: for smaller arrays
text - configuration 1 is faster, but for larger arrays configuration 2 is better.
text - 
text - 2) Suppose an algorithm is O(N Log N) and it takes .003 seconds to run on a
text - computer for a problem of size 1000. Determine the function Ta(N) and compute
text - how long it would take to run on a problem of size 1 million. Hint: us the
text - simple approximation shown in this lecture for logarithms.
text - 
text - 3) Suppose that we time a function f by doubling the size of its input and we
text - get the following data
text - 
text -    N   |  Time to compute f(n)
text - -------+----------------------
text - 1,000  |     .32
text - 2,000  |    1.31
text - 4,000  |    5.12
text - 8,000  |   20.5
text - 
text - What complexity class has a signature that matches this data?
text - 
