---
layout: resource-post
title: "Java MacOS Dev Setup"
slug: java-macos
description: "A beginner's Java development environment setup for MacOS"
date: 2020-06-11
author: Franco Posa
order_number: 3
---

## 1. Install Java with Homebrew

### Install OpenJDK

We are going to use OpenJDK and not Oracle's JDK. You could write a very long book on why this is correct and morally good choice, and I am sure someone has. The short version is "FOSS good, Oracle bad", even though Oracle contributes to the OpenJDK project. Like I said, it would be a long book. 

Anyway:

```
% brew install openjdk
```

#### Make sure Java from the OpenJDK is first in your `PATH`

Add the following to your ~/.zshrc:
```
export PATH="/usr/local/opt/openjdk/bin:$PATH"
```

#### Confirm your `java` and `javac` commands map to the OpenJDK

```
% which java
/usr/local/opt/openjdk/bin/java
% which javac
/usr/local/opt/openjdk/bin/javac
```

## 2. Hello Java

Create `HelloWorld.java` in your preferred editor:

```
public class HelloWorld {
    public static void main(String[] args) { 
        System.out.println("Hello, world");
    }
}
```

#### Compile your code:

This will create the `HelloWorld.class` file containing the Java bytecode compiled for the JVM.
```
% javac HelloWorld.java
```

Bonus: the Java bytecode in the `HelloWorld.class` file is in a binary format that will not be readable with most text editors, but, you can use the `javap` [class file disassembler command](https://docs.oracle.com/en/java/javase/11/tools/javap.html). This is not much use to us at this point, but it is pretty interesting.

```
% javap -c HelloWorld
Compiled from "HelloWorld.java"
public class HelloWorld {
  public HelloWorld();
    Code:
       0: aload_0
       1: invokespecial #1                  // Method java/lang/Object."<init>":()V
       4: return

  public static void main(java.lang.String[]);
    Code:
       0: getstatic     #7                  // Field java/lang/System.out:Ljava/io/PrintStream;
       3: ldc           #13                 // String Hello, World
       5: invokevirtual #15                 // Method java/io/PrintStream.println:(Ljava/lang/String;)V
       8: return
}
```

#### Run your code

Using `java` command, point the `-classpath/-cp` argument to the location of the class file and provide the name of the class whose `main` method should be run.

```
% java -classpath . HelloWorld
Hello, World
```

#### Run your code without the .class file

More recent Java versions support running single Java files without the intermediate `javac` command for compiling to a class file. The compilation step still occurs, but the JVM bytecode is held in memory instead of written to the disk.

The limitation to a single file will not make this very useful for running an application but it is nice to speed up running scratch files or single-file programs while leaving fewer unwanted class files in your working directory.

```
% java HelloWorld.java 
Hello, World
```


