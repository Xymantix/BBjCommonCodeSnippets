# BBj Development Tips

 This file contains tips and tricks to help you write BBj code more effectively.  While some of the tips are generic and apply to all BBj applications, many of them apply to running your BBj application in a web browser via BBj's Dynamic Web Client (DWC).



## 1. Using Java Data Sets With `BBjListButtons` and `BBjListBoxes`

### Loading a `BBjListButton` or `BBjListEdit` with data from a Java `HashMap` or `BBjVector`

#### To load a `BBjListEdit` with the contents of a `BBjVector`:
The following code creates a string that's filled with the contents of your vector and uses the `$0A$` character as a delimiter:

```java
options! = java.lang.String.join($0a$, java.util.Arrays.asList(myVector!))
myListEdit! = myWindow!.addListEdit(myWindow!.getAvailableControlID(),x,y,w,h, options!)
```

#### To load a `BBjListEdit` with all the keys of a `HashMap`:

```java
options! = java.lang.String.join($0a$, myHashMap!.keySet())
myListEdit! = myWindow!.addListEdit(myWindow!.getAvailableControlID(),x,y,w,h, options!)
```

#### To load a `BBjListEdit` with all the values of a `HashMap`:

```java
options! = java.lang.String.join($0a$, myHashMap!.values())
myListEdit! = myWindow!.addListEdit(myWindow!.getAvailableControlID(),x,y,w,h, options!)
```

#### To load a `BBjListButton` with all the  items of a `BBjVector`:
Normally you would build a list of strings using the `$0A$` character as a delimitor.  This can 
be done easily in a single line of code as shown below:

```java
myListButton! = win!.addListButton(String.join($0a$, cast(Iterable, myVector!)))
```

## 2. Using Java data sets with a `BBjVector`

#### To fill a `BBjVector` with the keys from a `HashMap`:

```java
vect! = BBjAPI().makeVector()
vect!.addAll(hash!.keySet())
```

#### Converting a `BBjVector` to comma-delimited string

```java
method protected BBjString getBBjVectorAsString(BBjVector vector!)
    methodret String.join(",", cast(Iterable, vector!))
methodend
```

#### Filling a BBjVector via the Java String `split()` method

```java
rem Fill a BBjVector by splitting up a BBjString using a comma as a delimiter
declare BBjVector colorVector!

colorVector! = bbjapi().makeVector()
colorVector!.addAll(java.util.Arrays.asList(p_colorString!.split(",")))
```

```java
rem Fill a BBjVector by splitting up a BBjString using the linefeed character as a delimiter
declare BBjVector textLines!

textLines! = BBjAPI().makeVector()
textLines!.addAll(java.util.Arrays.asList(text!.split($0a$)))
```

## 3. Java `HashMap` and `TreeMap` Code

#### Getting the keys from a `LinkedHashMap` in reverse order as a linefeed-delimited String to fill a BBjList (`BBjListBox` or `BBjListButton`) control.  
You may want to get the keys in reverse order to populate a Most-Recently-Used type of list, where your app provides a history of the last X chosen items/paths/etc.

```java
method public BBjString getMruForListButton(LinkedHashMap mru!)
    list! = new java.util.ArrayList(mru!.keyset())
    java.util.Collections.reverse(list!)
    methodret java.lang.String.join($0a$, list!)
methodend
```

#### Getting the last key from a `Map`

```java
rem /**
rem  * Returns the last object in a Map
rem  */
method private Object getLastMapKey(Map map!)
#####     declare auto java.util.stream stream!
##### 		
#####     stream! = map!.keySet().stream()
    lastIndex = new java.lang.Long(stream!.count() - 1)
    methodret stream!.skip(lastIndex).findFirst().get()
methodend
```

#### Using a Java `TreeMap` for sorted collections (map)
See the `TreeMap` docs here: [https://docs.oracle.com/en/java/javase/21/docs/api/java.base/java/util/TreeMap.html](url).

**Note**: that the last line in the example code instantiates the TreeMap specifying that it should sort its items in a case-insensitive manner.
```java
rem /* USE statements so that we can refer to Java classes without their package information
use java.util.TreeMap
use java.lang.String

*rem /* Create a TreeMap to store sorted key/value pairs */*
treeMap! = new TreeMap()

rem /* Create a case-insensitive sorted TreeMap, so that it will treat lower and uppercase letters the same */
treeMap! = new TreeMap(String.CASE_INSENSITIVE_ORDER)
```

# Documentation Links:

- **BBjVector**: [https://documentation.basis.com/BASISHelp/WebHelp/gridctrl/bbjvector_bbj.htm](url)
- **Java Collection**: [https://docs.oracle.com/javase/8/docs/api/java/util/Collection.html](url)
- **Java List**: [https://docs.oracle.com/javase/8/docs/api/java/util/List.html](url)
- **Java HashMap**: [https://docs.oracle.com/en/java/javase/21/docs/api/java.base/java/util/HashMap.html](url)
- **Java TreeMap**: [https://docs.oracle.com/en/java/javase/21/docs/api/java.base/java/util/TreeMap.html](url).
- **Java String**: [https://docs.oracle.com/en/java/javase/21/docs/api/java.base/java/lang/String.html](url)

