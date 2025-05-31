# Type Erasure

## Example

Let's say we have this Java code:

```java
public static <T> HashSet<T> create(int size) {  
    return new HashSet<T>(size);  
} 
 
HashSet<Integer> s = create(5);
```

During compilation, types are envorced/validated & generic types are removed:

```java
public static HashSet create(int size) {  
    return new HashSet(size);  
} 

HashSet s = create(5);
```