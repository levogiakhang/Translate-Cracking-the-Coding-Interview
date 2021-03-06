# Testing
## Concepts and Algorithms: Solutions

> **Example 1:**
	Find the mistake(s) in the following code:
  ```
  unsigned int i;
  for (i = 100; i >= 0; --i)
    printf("%d\n", i);
  ```

#### **Solution**

There are two mistakes in this code.

_First,_ note that an `unsigned int` is, bay definition, always greater than or equal to zero. 
The `for` loop condition will therefore always be true, and it will loop infinitely.

:point_right:  The correct code to print all numbers from 100 to 1, is `i > 0`. If we truly wanted to print zero, we could add an additional `printf` statement after the `for` loop.

   ```
  unsigned int i;
  for (i = 100; i > 0; --i)
    printf("%d\n", i);
  ```
  _Second_ One additional correction is to use `%u` in place of `%d`, as we are printing `unsigned int`:
  
  ```
  unsigned int i;
  for (i = 100; i > 0; --i)
    printf("%u\n", i);
  ```
 :purple_heart: This code will now correctly print the list of all numbers from 100 to 1, in descending order. :purple_heart:
  
  ___
  
  > **Example 2:** 
    You are given the source to an application which crashes when it is run. After running it ten times in a debugger, you find it never crashes in the same place. The application is single threaded, and uses only the C standard library. What programming errors could be causing this crash? How would you test each one?
  
 #### **Solution**
  
The question largely depends on the type of application being diagnosed. However, we can give some general causes of random crashes. 
  1. _"Random Variable:"_ The application may use some random number or variable component that may not be fixed for every execution of the program. 
      - Examples include user input, a random number generated by the program, or the time of day.
  2. _Uninitialized Variable:_ The application could have an uninitialized variable which, in some languages, may cause it to take on an arbitrary value. The Values of this variable could result in the code taking a slightly different path each time.
  3. _Memory Leak:_  The program may have run out of memory. Other culprits are totally random for each run since it depends on the number of processes running at that particular time. This also includes heap overflow or corruption of data on the stack.
  4. _External Dependencies:_  The program may depend on another application, machine, or resource. If there are multiple dependencies, the program could crash at any point.
  
 To track down the issue, we should start with learning as much as possible about the application. :joy::joy::joy:
  - "Who is running it?"
  - "What are they doing with it?"
  - "What kind of application is it?"
  - ...
  
:point_right:  Additionally, although the application doesn't crash in exactly the same place, it's possible that it is linked to specific components or scenarios.
- For example, it could be that the application never crashes if it's simply launched and left untouched, and that crashes only appear at some point after loading a file. Or, it may be that all the crashes take place within the lower level components, such as file I/O.

:point_right:  It may be useful to approach this by elimination. Close down all other applications on the system. Track resource use very careful. If there are parts of the program we can disable, do so. Run it on a different machine and see if we experience the same issue. The more we can eliminate (or change), the easier we can track down the issue.

:point_right:  Additionally, we may be able to use tools to check for specific situations.
-  For example, to investigate issue #2, we can utilize runtime tools which check for _uninitialized variables_.
         
:point_right:  These problems are as much about your brainstorming ability as they are about your approach. Do you jump all over the place, shouting out random suggestions? Or do you approach it in a logical, structured manner? Hopefully, it's the latter.

___

> **Example 3:**
   We have the following method used in a chess game: boolean canMoveTo(int x, int y). This method is part of the Piece class and returns whether or not the piece can move to position (x, y). Explain how you would test this method. 
   
#### **Solution**

In this problem, there are two primary types of testing: extreme case validation (ensuring that the program doesn't crash on bad input), and general case testing. We'll start with the first type.

**Testing Type #1: Extreme Case Validation**

We need to ensure that the program handles bad or unusual input gracefully. This means checking the following conditions:
- Test with negative numbers for x and y.
- Test with x larger than the width.
- Test with y larger than the heigh.
- Test with a completely full board.
- Test with an empty or nearly empty board.
- Test with far more white pieces than black.
- Test with far more black pieces than white.

For the error cases above, we should ask our interviewer whether we want to return false or throw an exception, and we should test accordingly. 

**Testing Type #2: General Testing**

General testing is much more expansive. Ideally, we would tes t every possible board, but there are far too many boards. We can, however, perform a reasonable coverage of different boards . 
There are 6 pieces in chess, so we can test each piece against every other piece, in every possible direction.This would look something like the below code: 
```
foreach piece a: 
  foreach other type of piece b (6 types + empty space ) 
    foreach direction d
      Create a board with piece a.
      Place piece b in direction d.
      Try to move - check return value.
```
The key to this problem is recognizing that we can't test every possible scenario, even if we would like to. So, instead, we must focus on the essential areas.

___

> **Example 4:**
  How would you load test a webpage without using any test tools?
     
#### **Solution**

Load testing helps to identify a web application's maximum operating capacity, as well as any bottlenecks that may interfere with its performance. Similarly, it can check how an application responds to variations in load.
To perform load testing, we must first identify the performance critical scenarios and the metrics which fulfill our performance objectives. Typical criteria include:
- Response time
- Throughput
- Resource utilization
- Maximum load that the system can bear. 

:point_right:  Then, we design tests to simulate the load, taking care to measure each of these criteria.

:point_right:  In the absence of formal testing tools, we can basically create our own.
- For example: we could simulate concurrent users by creating thousands of virtual users. We would write a multi-threaded program with thousands of threads, where each thread acts as a real-world user loading the page. For each user, we would programmatically measure response time, data I/O, etc.
 
:point_right:   We would then analyze the results based on the data gathered during the tests and compare it with the accepted values.

___

> **Example 5:**
  How would you test a pen?
        
#### **Solution**

This problem is largely about understanding the constraints and approaching the problem in a structured manner.
To understand the constraints, you should ask a lot of questions to understand the "who, what , where, when, how and why" of a problem (or as many of those as apply to the problem). Remember that a good tester understands exactly what he is testing before starting the work. 
To illustrate the technique in this problem, let us guide you through a mock conversation.

**Interviewer**: How would you test a pen?

**Candidate**: Let me find out a bit about the pen. Who is going to use the pen? 

**Interviewer**: Probably children. 

**Candidate**: Okay, that's interesting. What will they be doing with it? Will they be writing, drawing, or doing something else with it? 

**Interviewer**: Drawing. 

**Candidate**: Ok, great. On what? Paper? Clothing? Walls? 

**Interviewer**: On clothing. 

**Candidate**: Great. What kind of tip does the pen have? Felt? Ball point? Is it intended to wash off, or is it intended to be permanent? 

**Interviewer**: It's intended to wash off. 

Many questions later, you may get to this:

**Candidate**: Okay, so as I understand it , we have a pen that is being targeted at 5 to 10 year olds. The pen has a felt tip and comes in red, green, blue and black. It's intended to wash off when clothing is washed. Is that correct?

:point_right:  The candidate now has a problem that is significantly different from what it initially seemed to be. This is not uncommon. In fact, many interviewers intentionall y give a problem that seems clear (everyone knows what a pen is!), only to let you discover that it's quite a different problem from what it seemed. Their belief is that users do the same thing, though users do so accidentally.

Now that you understand what you're testing, it's time to come up with a plan of attack. The key here is structure. 

Consider what the different components of the object or problem, and go from there. In this case, the components might be:
- _Fact check_: Verify that the pen is felt tip and that the ink is one of the allowed colors. 
- _Intended use_: Drawing. Does the pen write properly on clothing?
- _Intended use_: Washing. Does it wash off of clothing (even if it's been there for an extended period of time)? Does it wash off in hot, warm and cold water? 
- _Safety_: Is the pen safe (non-toxic) for children?
- _Unintended uses_: How else might children use the pen? They might write on other surfaces, so you need to check whether the behavior there is correct. They might also stomp on the pen , throw it , and so on. You'll need to make sure that the pen holds up under these conditions.

:point_right:  Remember that in any testing question, you need to test both the intended and unintended scenarios. People don't always use the product the way you want them to.

___

> **Example 6:**
  How would you test an ATM in a distributed banking system?
        
#### **Solution**

The first thing to do on this question is to clarify assumptions. Ask the following questions: 
- Who is going to use-the ATM? Answers might be "anyone," or it might be "blind people," or any number of other answers.
- What are they going to use it for? Answers might be "withdrawing money", "transferring money", "checking their balance", or many other answers.
- What tools do we have to test? Do we have access to the code, or just to the ATM? 

Remember: a good tester makes sure she knows what she's testing!

Once we understand what the system looks like, we'll want to break down the problem into different testable components. These components include: 
- Logging in
- Withdrawing money
- Depositing money 
- Checking balance
- Transferring money 

We would probably want to use a mix of manual and automated testing.

Manual testing would involve going through the steps above, making sure to check for all the error cases (low balance, new account, nonexistent account, and so on). 

:point_right:  Automated testing is a bit more complex. We'll want to automate all the standard scenarios, as shown above, and we also want to look for some very specific issues, such as race conditions. Ideally, we would be able to set up a closed system with fake accounts and ensure that, even if someone withdraws and deposits money rapidly from different locations, he never gets money or loses money that he shouldn't. 

:point_right:  Above all, we need to prioritize security and reliability. People's accounts must always be protected, and we must make sure that money is always properly accounted for. No one wants to unexpectedly lose money! A good tester understands the system priorities.
