``` java
// Primitive type
int.class.getName();          // -> int
int.class.getCanonicalName(); // -> int
int.class.getSimpleName();    // -> int
 
// Standard class
Integer.class.getName();          // -> java.lang.Integer
Integer.class.getCanonicalName(); // -> java.lang.Integer
Integer.class.getSimpleName();    // -> Integer
 
// Inner class
Map.Entry.class.getName();          // -> java.util.Map$Entry
Map.Entry.class.getCanonicalName(); // -> java.util.Map.Entry
Map.Entry.class.getSimpleName();    // -> Entry     
 
// Anonymous inner class
Class<?> anonymousInnerClass = new Cloneable() {}.getClass();
anonymousInnerClass.getName();          // -> somepackage.SomeClass$1
anonymousInnerClass.getCanonicalName(); // -> null
anonymousInnerClass.getSimpleName();    // -> // An empty string
 
// Array of primitives
Class<?> primitiveArrayClass = new int[0].getClass();
primitiveArrayClass.getName();          // -> [I
primitiveArrayClass.getCanonicalName(); // -> int[]
primitiveArrayClass.getSimpleName();    // -> int[]
 
// Array of objects
Class<?> objectArrayClass = new Integer[0].getClass();
objectArrayClass.getName();          // -> [Ljava.lang.Integer;
objectArrayClass.getCanonicalName(); // -> java.lang.Integer[]
objectArrayClass.getSimpleName();    // -> Integer[]
```
