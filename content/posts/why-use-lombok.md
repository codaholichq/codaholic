+++
title = 'Should Lombok Still Be Used'
slug = 'why-use-lombok'
author = 'Codaholic'
date = 2023-11-05T12:41:20+01:00
draft = false
description = "Let's explore the relevance of Lombok in modern Java development. Let's see whether it's a valuable tool or if alternatives should be considered."
[cover]
image = 'img/00-should-lombok-still-be-used.webp'
alt = 'Should Lombok Still Be Used'
caption = 'Should Lombok Still Be Used'
+++

Lombok is an annotation-based Java library that reduces boilerplate code. It offers various annotations aimed at replacing Java code that is well known for being repetitive or tedious to write, such as constructors, `toString()`, `equals()`, and `hashCode()` methods.

[Lombok](https://projectlombok.org/) works by plugging into your build process and auto-generating the Java bytecode into your .class files required to implement the desired behavior, based on the annotations you used. This magic happens during the compile-time when the library injects the bytecode representing the desired and boilerplate code into your .class files.

To use Lombok, you need to add it to your dependencies. If your build tool is Gradle, add these lines to the dependencies section of your `build.gradle` file:

```groovy
compileOnly 'org.projectlombok:lombok:1.18.30'
annotationProcessor 'org.projectlombok:lombok:1.18.30'
```

If your build tool is Maven, add the following dependency to your `pom.xml` file:

```xml
<dependency>
     <groupId>org.projectlombok</groupId>
     <artifactId>lombok</artifactId>
     <version>1.18.30</version>
     <scope>provided</scope>
</dependency>
```

And add the Lombok dependency to the `maven-compiler-plugin` configuration section as follows:

```xml
<build>
     <plugins>
           <plugin>
                <groupId>org.apache.maven.plugins</groupId>
                <artifactId>maven-compiler-plugin</artifactId>
                <configuration>
                      <annotationProcessorPaths>
                            <path>
                                 <groupId>org.projectlombok</groupId>
                                 <artifactId>lombok</artifactId>
                                 <version>1.18.30</version>
                            </path>
                      </annotationProcessorPaths>
                </configuration>
           </plugin>
     </plugins>
</build>
```
<br/>

***Also see:**[How To Make Maven Faster](/make-maven-faster)*

<br/>

Lombok also comes with a `delombok` tool. This tool auto-generates Java source code containing the same features contained in the bytecode Lombok would have injected. This way, your Lombok annotated codebase will be simply replaced with a standard, non-Lombok Java code. As a result, your entire project will no longer be Lombok-dependent.

Here is an example of how Lombok simplifies code:

Without Lombok:

```java
public class Blogger {
   private int id;
   private String name;
   private String surname;

   public Blogger(int id, String name, String surname) {
     this.id = id;
     this.name = name;
     this.surname = surname;
 }
}
```

With Lombok:

```java
@Data
public class Blogger {
   private final int id;
   private String name;
   private String surname;
}
```

In the second example, Lombok automatically generates the `constructor, getters, setters, equals, hashCode, and toString` methods.

<br/>

### Why Java is reluctant to implement getters and setters?

Java's resistance to adopting `setters` and `getters` has led to the development of Lombok, a tool that provides a feature that C# has had for over two decades. This feature is not limited to niche applications but can benefit any class that has properties, making it a versatile and useful tool for Java developers.

However, here is why OpenJDK has been reluctant to implement this feature:

- JVM languages like Lombok or Kotlin, have little say or influence on the ecosystem so their features only address existing practices as they cannot change them. In Java we want to discourage the JavaBean style for server-side code, hence, `records` were introduced which is more powerful and, over time, will change how the ecosystem works.
- The features Lombok boasts of are too weak for inclusion in Java, which prefers more powerful ones. Java is not getting properties because it has `records`, which is more powerful and more useful for server-side code (properties, both in JavaBeans and in C# trace their origin to client-side GUI). Java's record feature will continue to evolve, making it even more potent.
- All languages don’t have the same design philosophy. Java does not have all of C#'s features (and C# certainly doesn't have all of Java's features) and that is because we don't want most of them. Java's strategy has always been to be a last-mover, only adopting features that have proven their worth elsewhere, and also adopting as few features as possible (preferably only the strongest features). Some portion of developers prefer more feature-rich languages, and the JVM offers such languages.
- Last but not least, Java, traditionally, has always preferred innovating on the runtime rather than the language.

<br/>

***Also see:**[Java Docker Best Practices](/java-docker-best-practices)*

<br/>

### Arguments For the use of Lombok
[Lombok](https://projectlombok.org/features/) provides several advantages that can enhance the efficiency and readability of your code:

1. **Reduced Boilerplate Code**: Lombok is particularly effective for reducing boilerplate code, which is often associated with Java's verbosity. For instance, if you're developing a Plain Old Java Object (POJO), you usually need to create private access directly to the class fields and create accessor methods—getters and setters—to read from and write to those fields. Lombok can handle this by generating these methods if a field is annotated with `@Getter` and `@Setter`. This makes the code more concise, easier to read, and less error-prone.
2. **Automatic Generation of Methods**: Lombok can automatically generate `equals()`, `hashCode()`, and `toString()` methods for your classes. This is particularly useful when working with data objects. By annotating a class with `@EqualsAndHashCode` and `@ToString`, Lombok will generate the respective methods, and they are customizable so that you can specify field exclusions and other factors.
3. **Immutable Classes**: Lombok offers the `@Value` annotation, which creates an immutable class with automatically generated getters for all private final fields. The annotation is applied at the class level, and it triggers the generation of getter methods for each private final field without generating any setters. Additionally, the class is marked as final and Lombok generates constructors with arguments for each field. Furthermore, Lombok also generates `toString()`, `equals()`, and `hashCode()` methods for the class.
4. **Convenience with Constructors**: Lombok provides annotations like `@AllArgsConstructor` and `@NoArgsConstructor` to automatically generate constructors. This can be handy for ensuring that a class constructor always accepts values for each of the class fields.
5. **Resource Management**: Lombok provides the `@Cleanup` annotation, which helps manage resources by automatically closing them. This is useful for preventing resource leaks that can occur if you forget to close a resource.
6. **Thread Safety**: Lombok offers the `@Synchronized` annotation, which provides a safer way to ensure that only one thread can access a method at a time. This is a safer alternative to using the `synchronized` keyword, as it allows you to lock on an instance field rather than on `this.`

These features can significantly simplify your code, reduce the likelihood of bugs, and speed up development time. However, it's crucial to remember that Lombok should be used judiciously, and developers should remain aware of its limitations and potential impact on code readability and maintainability.

<br/>

### Arguments Against the Use of Lombok
Despite the advantages that come with using Lombok, there are still some concerns that some developers have and based on these concerns they tend to advise against the use of it. Below are the arguments against the use of Lombok.

{{< tweet user="codaholichq" id="1747279361512738892" >}}

**IDEs can do the basic thing Lombok does:**
All of the big three Java IDEs produce getters, setters, equals, hashCode and toString with a couple of mouse clicks. With this you don't have to worry about whatever black magic Lombok is doing at runtime. Hence, no extra step to the build process.
It just takes 10 seconds to tell your IDE to produce all of that code. Adding another dependency, and especially a build time dependency, isn't worth it.

- **Response:**
    
    With Lombok you are not going to waste time regenerating code every time you want to do a small refactor like renaming a field. Using `@Getter` and `@Setter` has the benefit of forcing you not to put any kind of ‘dirty’ logic into those methods. With the use of the `@Builder` annotation, Lombok enables developers to craft fluent and expressive code, which is a cornerstone of modern Java development.
    
    Lombok is by no means perfect, just because IntelliJ can auto-generate the code once doesn't mean it doesn't need to get updated in the future. Lombok automatically updates it if the class itself is changed.
    

**Lombok is an Alternative Language Pretending to be a Library**
Technically, Lombok is an alternative language for the [JVM](https://en.wikipedia.org/wiki/Java_virtual_machine), just like Kotlin, Groovy, and Scala. However, What sets Lombok apart from those other languages is that it's a superset of the Java language. The compiler for the Lombok language is a fork of `javac` with some extra stuff hooked in required to compile Lombok sources. Note that there's nothing wrong with this: the Java Platform spec allows for alternative languages, and javac is open source.

The issue is that Lombok and its compiler don't want to appear as a language and as a compiler. The "magic" is completely not needed to perform Lombok's function; it is used only to masquerade Lombok's nature. It's presented as a library or as a compiler plugin, even though the API offered to Java compiler plugins is carefully designed to ensure that the result still complies with the Java language spec, whereas the Lombok compiler compiles programs written in the Lombok language which very much does not comply with the JLS. Furthermore, Lombok pretends that it's Java when it is not, and the spec forbids this kind of misrepresentation.

To do that, Lombok uses various unsafe mechanisms to fork and modify `javac` *as it runs*, to make it compile Lombok source code rather than Java source code. The internals Lombok reaches into may change at any time, hence, making it risky and the techniques to break into the JDK will soon be removed.

- **Response**
    
    Yes absolutely it isn't an annotation processor but the fact that it pretends to be, rather than introducing new syntax, for example, is a deliberate choice. The result makes Lombok not feel like an alternative language to Java though it is.
    
    All of these facts are not true for any other language and I am not arguing that this amounts to making Lombok the same as Java. But tricking developers into believing that it is Java is absolutely the point.
    
    Companies are happy to adopt Lombok because "it is just a library" or "it's just an annotation processor". Those same companies would raise the barrier to entry significantly if Lombok comes off as another language.
    
    Lombok may not survive anything that undermines this pretense. Lombok may be an alternative language, but it can not admit that and survive.
    
<br/>

***Also see:**[Spring Boot in 20 Minutes](/spring-boot-in-20-minutes)*

<br/>

### Try not to use the @Data Annotation

Use `@Data` cautiously. `@Setter` and `@Getter` are not likely to cause problems but `@Data` can cause serious serialization issues with ORMs like Hibernate and cfg engines like gson. `@Value` can cause issues too.

<br/>

### Conclusion

Lombok is often met with skepticism due to its novel approach to solving common Java development challenges. This skepticism is not surprising, given Java's widespread use and the emotional attachment many developers have to the language. However, Lombok's unique solution is often seen as a threat to the status quo, particularly among those who are invested in Java and believe it addresses problems better than other languages.

The defensiveness displayed by these individuals is understandable, given Java's long history and ubiquity in the industry. Java has been the go-to language for many developers for decades, and as a result, it has become a kind of "Aristotle" of programming languages - the benchmark against which other languages are measured. With so many languages emerging in response to Java, it's natural for some developers to feel defensive about their preferred language and to resist changes that could potentially disrupt the status quo.

Ultimately, the reaction to Lombok is a testament to the passion and dedication of developers to their chosen languages and the challenges they face in the industry. While Lombok may not be the right solution for everyone, its very existence highlights the ongoing quest for better tools and techniques in software development.

<br/>

{{< bmc-button slug="codaholic" >}}

<br/>

### Frequently Asked Questions
**Is Lombok a hack?**
Lombok's method of modifying the compiler's Abstract Syntax Tree (AST) to generate bytecode directly affects the final bytecode generation process, making it an unconventional and invasive approach. This unusual technique has contributed to Lombok being perceived as a hack.

**How does Lombok inject code?**
Lombok does not generate code in the classical sense but rather leverages internal compiler implementation APIs to modify the program's abstract syntax tree (AST) during the compilation process. This means that the source code is not transformed into machine code directly, but rather the AST is manipulated through undocumented and unspecified API calls to produce the compiled bytecode.

**What is the difference between @data and @value in Lombok?**
In essence, `@Value` represents the immutable version of `@Data`. By default, all fields in a `@Value` class are private, final, and non-generable setters. Additionally, the class itself is made final, as immutability cannot be enforced on a subclass. This means that once an instance of a `@Value` class is created, its state cannot be modified.
