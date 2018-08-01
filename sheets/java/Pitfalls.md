## Contents

* [Arrays.asList(arr)](#arrays.aslist(arr))
* [Remove an Element from a List Inside a Loop](#remove-an-element-from-a-list-inside-a-loop)


### Arrays.asList(arr) 

* When converting an array to ArrayList use 

```ArrayList<String> arrayList = new ArrayList<String>(Arrays.asList(arr));```

instead of this

```List<String> list = Arrays.asList(arr);```

* ```Arrays.asList()``` will return an ```java.util.Arrays.ArrayList``` which is a private static class inside ```Arrays```, it is not the ```java.util.ArrayList``` but a wrapper class that implements ```List<>```.
Nothing is copied and only a single wrapper object is created for you. Operations on the list wrapper are propagated to the original array.his means that if you shuffle the list wrapper, the original array is shuffled as well, if you overwrite an element, it gets overwritten in the original array, etc.
Some List operations aren't allowed on the wrapper, like adding or removing elements from the list,so its size is fixed.

* On the other hand when we use
``` new ArrayList<Integer>(Arrays.asList(arr)) ```
we create new ```java.util.ArrayList```, which is a full, independent copy of the original one. Although here you create the wrapper using Arrays.asList as well, 
it is used only during the construction of the new ArrayList and is garbage-collected afterwards. The structure of this new ```ArrayList``` is completely independent of the original array. 
It contains the same elements , but it creates a new, internal array, that holds the references. So when you shuffle it, add, remove elements etc., the original array is unchanged.

### Remove an Element from a List Inside a Loop

* Consider the follwing code: 
```java
ArrayList<String> list = new ArrayList<String>(Arrays.asList("a", "b", "c", "d"));
for (int i = 0; i < list.size(); i++) {
	list.remove(i);
}
System.out.println(list);
```
Output is:
```java
[b, d]
```
There is a serious problem in this method. When an element is removed, the size of the list shrinks and the index changes. So if you want to delete multiple elements inside a loop by using the index, that will not work properly.
If you use for each loop:
```java
ArrayList<String> list = new ArrayList<String>(Arrays.asList("a", "b", "c", "d"));
 
for (String s : list) {
	if (s.equals("a"))
		list.remove(s);
}
```
It will throw out ```ConcurrentModificationException```.


