
JAVA DESIGN PATTERN <br>
What is a Singleton Design Pattern ?

-  There should be only one instance allowed for a class and
- We should allow global point of access to that single instance.

All Scenarios - 

## Thread-Safe Singleton (Eager Initialization)

  In eager initialization instance is created during class loading. If your application requirement is to create instance everytime and it is not a problem then use this solution.

  **If two objects are equal, they MUST have the same hash code.**

## Singleton Violation on Using Reflection

  Java Reflection is a process of examining or modifying the run time behavior of a class at run time.

  Example- set constructor SingletonR to private to become accessible at runtime

``` Java  
package com.dev.dp.creational.singleton;

import java.lang.reflect.Constructor;

public class SingletonR {
	
	public static SingletonR instance= new SingletonR();
	
	private SingletonR() {
		System.out.println("creating instance.....");
	}
	
	public static SingletonR getInstance() { return instance }

	public static void main(String[] args) throws Exception{
		SingletonR s1 = SingletonR.getInstance();
		SingletonR s2 = SingletonR.getInstance();
		System.out.println("Hashcode of Object s1: " +s1.hashCode());
		System.out.println("Hashcode of Object s2: " +s2.hashCode());
		
		Class clazz = Class.forName("com.dev.dp.creational.singleton.SingletonR");
		Constructor<SingletonR> ctr= clazz.getDeclaredConstructor();
		ctr.setAccessible(true);
		SingletonR s3 = ctr.newInstance();
		System.out.println("Hashcode of Object s3: " +s3.hashCode());
	}
}
  ```

```creating instance.....
Hashcode of Object s1: 366712642
Hashcode of Object s2: 366712642
creating instance.....
Hashcode of Object s3: 1829164700
```
singleton violation occurs due to different hashcodes(instance for the class). To fix this lets check if an instance already exists before creating a new one and return it, else throw an error.

``` Java
private SingletonR() {
		System.out.println("creating instance.....");
		if(instance != null) {
			throw new RuntimeException("Can't create instance. Please use getInsance() to create it.");
		}	
}
```