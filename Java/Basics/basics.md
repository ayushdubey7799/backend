# Primitive Data Types

Type   | Size (bits) | Default Value | Range
byte   | 8           | 0             | -128 to 127
short  | 16          | 0             | -32,768 to 32,767
int    | 32          | 0             | -2^31 to 2^31-1
long   | 64          | 0L            | -2^63 to 2^63-1
float  | 32          | 0.0f          | IEEE 754 Floating point
double | 64          | 0.0d          | IEEE 754 Floating point
char   | 16          | '\u0000'      | 0 to 65,535 (Unicode)
boolean| 1           | false         | true or false


Important points:
- float needs f suffix (3.14f) and long needs L (100000L).
- char can store a single character like 'A' or Unicode symbols.
- Java primitives are stored directly in the stack (for local variables).


# Wrapper Classes
Integer, Double, Character, etc
- Needed for working with collections (List<Integer>, not List<int>).
- Support methods like Integer.parseInt("123").
- Nullable — you can assign null to a wrapper.
- ## Autoboxing: 
     - Java automatically converts primitives → wrappers.
- ## Unboxing: 
     - Java automatically converts wrappers → primitives.
- Unboxing null wrappers —> leads to NullPointerException.

# String, StringBuilder, StringBuffer
- ## String 
    - Immutable
    - Interned (String Pool)
    - Heavy concatenation ->  memory wastage - new string creation everytime
- ## StringBuilder: Java 1.5
    - Mutable.
    - Faster for multiple concatenations.
    - Not thread-safe.
- ## StringBuffer: Java 1.0
    - Like StringBuilder, but thread-safe (synchronized).

