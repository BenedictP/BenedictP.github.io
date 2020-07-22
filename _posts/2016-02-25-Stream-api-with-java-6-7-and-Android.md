---
layout: post
title: "Stream API with Java 6/7 and Android"
date: 2016-02-25
comments: true
---
Java 8 has the new fancy and powerful Streams API but in the Android development world we are still stuck somewhere between Java 6 and 7 and can’t use the Streams API.
So there are two ways to solve this:

1. Use [Kotlin](https://kotlinlang.org/) ;)
2. Use the [StreamSupport Library](https://sourceforge.net/projects/streamsupport/)

The StreamSupport Library is a backport of the Java 8 java.util.function (functional interfaces) and java.util.stream (streams) API for users of Java 6 or 7. The Project is hosted at [Sourceforge](https://sourceforge.net/projects/streamsupport/) and can easily added to an Android project with gradle.
The [mvnrepository Link](http://mvnrepository.com/artifact/net.sourceforge.streamsupport/streamsupport/1.4.2).

```
compile 'net.sourceforge.streamsupport:streamsupport:1.4.2'
```

Streams in Java 8
---
In Java 8 we can easily call stream() on any collection that supports the Stream API.
After that call there are many ways to use the stream features. I can recommend this helpful Cheat Sheet: [Java 8 Streams Cheat Sheet](http://zeroturnaround.com/wp-content/uploads/2016/01/Java-8-Streams-cheat-sheet-v3.png)

Streams with the StreamSupport Library
---
To use the stream feature with the Library we can not easily call stream() on any collection like in Java 8. We must call StreamSupport.stream(@NotNull java.util.Collection<? extends T> c). But after that call we can use the same functionalities like in the Java Streams API! So we can easily transfer the knowledge from Java 8 Streams to the StreamSupport Library.

Some simple examples
---
Attention: I'm also using [retrolamda](https://github.com/orfjackal/retrolambda). Like StreamSupport it's a backport of the Lamdba Feature from Java 8. Thats why I can use arrows in my code ;)

The model:

```java
public class Person {
  private String name;
  private int    age;
  private int    zipCode;
  //...getter and setters
}
```
We want alle persons with age > 21:

```java
private ArrayList<Person> getPersonsOlderThan21(List<Person> persons) {
  return StreamSupport.stream(persons)
    .filter(person -> person.getAge() > 21)
    .collect(Collectors.toCollection(ArrayList::new));
}

```
With Collectors.toCollection(ArrayList::new) we want to save the result as ArrayList.

Now all persons starting with an "A" in the name:

```java
private List<Person> getPersonsStartingWithAnA(List<Person> persons) {
 return StreamSupport.stream(persons).filter(person ->
  person.getName().startsWith("A")).collect(Collectors.toList());
}
```
For a List as an Output we call .collect(Collectors.toList()) at the end.

Also we can edit a List. Every Person should be one year older:


```java
private void oneYearOlder() {
  List<Person> persons = new ArrayList<>();
  //code to fill in some datat
  //make each person one year older:
  StreamSupport.stream(persons).forEach(person -> 
    person.setAge(person.getAge() + 1));
  //now each person is one year older
}
```

Many more examples can be found in other Java 8 Streams Tutorials. I just wanted to show you how we can use Java 8 Streams with Java 6/7 and Android.