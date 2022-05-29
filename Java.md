# Java

### Basics

1. `&` and `&&`

   1. `&&`会走捷径

2. **stack** and **heap**

   1. stack is faster, but less flexible.
   2. <u>normal objects</u> are on the heap; 
   3. <u>referrences</u> and <u>primitive types</u> are on the stack

3. High-precision numbers:`Biginteger` and `BigDecimal`

   `BigInteger` /`BigDecimal`supports <u>arbitrary-precision</u> integers/floats.

4. `null`

   1. A null referrence isn't pointing to a object.

5. **Scoping** and **Lifetime**

   1. The following is legal in C, but not in Java:

      ```java
      {
        int x = 12;
        {
          int x = 96;  // Illegal: x has been defined
        }
      }
      ```

   2. Scope of objects

      ```java
      {
        String s = new String("abc");
      } 
      ```

      the reference `s` vanished at the end of scope, but the `String` object is **<u>still occupying the memory</u>**. 

6. Default values for primitive members

   | Primitive type                       | Default          |
   | ------------------------------------ | ---------------- |
   | `boolean`                            | `false`          |
   | `char`                               | `\u0000`(`null`) |
   | `byte` `int` `long` `float` `double` | 0(0.0)           |

7. The `static` keyword
   1. All objects **share** the same static field
   2. Static methods can be **called directly** through the class.

8. 