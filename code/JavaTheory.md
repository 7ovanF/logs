# Java Technicals

A list of technical, "in-practice" quirks of Java

1. **Interaction between Primitives**
   - Hierarchy
     ```
     byte → short → int → long → float → double
                 ↘ char ↗
     ```
     Widening: valid
     Narrowing: no. Needs explicit casting by prepending `(type)`

   - Arithmetics and Compound Assignments
     - Normal arithmetics
       ``` Java
       byte a = 8;
       byte b = 12;
       byte sum = (byte) (a + b)2;
       ```
       That casting is required. All arithmetics of lower than integer are widened to integer arithmetics. 
    
     - Compound Assignments
       ```Java
       byte a = 8;
       a += 10; // no casting needed.
       // Basically, a += 10 is equal to a = (byte) a + 10;
       ```
       Compound assignments have **implicit casting**. Intermediary integer conversion is still done.
    
     - Increments and Decrements
       ```Java
       byte a = 8;
       a++; // also byte; no casting needed
       // Unlike the other one, 
       ```
       Increments and decrements, by definition, **don't** widen to integer before computing. No type casting is done, but explicit casting isn't needed.
    
      > All of these **still can overflow**.
      
     - `char` Arithmetics
     `char`s are also promoted to `int` in arithmetics; it behaves just like byte or short.
     That means type casting is needed for normal arithmetics and not needed for compound assignments and increments.
     ```Java
     char a = '1'; // ASCII code 49
     a + a; // 98
     (char) (a + a); // b
     a += a; // b
     a++; // 2 (ASCII code 50)
     ```
2. **&&, ||**
   In an expression `a && b`, if a is `false`, the conclusion is already `false`, so following expressions are not validated or computed. 
   In an expression `a || b`, if a is `true`, the conclusion is already `true`, so so following expressions are not validated or computed.
   Significance: look at this.
   ``` java
   int count = 0;
   boolean b = (count++ > 0) && (++count > 1);
   boolean c = (count == 1) || (count++ > 1);
   System.out.println(b + " " + c); // false true
   System.out.println("count: " + count); // count: 1
   ```
3. 

**Not learned:**
- String formatting