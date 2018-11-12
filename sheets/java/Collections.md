#### Contents
* [Comparable vs comparator](#comparable-vs-comparator)


### Comparable vs comparator

#### Comparable 

* Comparable is an interface which when implemented by a class allows it to compare this object with the specified object for order. The method ```int compareTo()``` returns:
  * a **negative** integer if this object is **less** than specified object
  * **zero** if this object is **equal** to the specified object.
  * **positive** integer if this object is **greater** than the specified object.
  
* Lists (and arrays) of **objects that implement** Comparable interface can be sorted automatically by  ```Collections.sort()```and  ```Arrays.sort().  

* Objects that implement this  interface can be used as keys in a ```SortedMap``` or as  elements in a ```SortedSet```without the need to specify a ```Comparator```.

* Following points must be kept in mind while implementing Comparable interface and overriding the ```compareTo``` interface:
  * ```signum(x.compareTo(y)) == -signum(y.compareTo(x))```
  * Transitivity: ```(x.compareTo(y) > 0 && y.compareTo(z) > 0)``` implies ```x.compareTo(z) > 0```
  * if ```x.compareTo(y) == 0``` then ```signum(x.compareTo(z)) == signum(y.compareTo(z))```
  
* It is strongly recommended but **not strictly required** that ```(x.compareTo(y)==0) == (x.equals(y))```



  
  
