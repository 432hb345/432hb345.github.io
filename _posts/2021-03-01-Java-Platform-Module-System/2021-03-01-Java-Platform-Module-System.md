---
title: Java Platform Module System
date: '2021-03-02T14:20:56+0000'
modified: '2021-03-02T14:20:56+0000'
tags:
  - java
  - modules
description: java platform module system
image: /apa-itu-shell/shell_evolution.png
published: true
---
Do Javy 9, najwyższym elementem organizującym była paczka.
Wcześniej monolityczne JDK, zostało podzielone na 90 modułów, np Logging, Swing, Instrumentation.
Od Javy 9 nad paczką jest teraz moduł.

+ Moduł jest agregatorem z unikalną nazwą, obejmuje reużywalną grupę powiązanych packages, resources i module descriptor.
Modułu dostarczane są w plikach JAR z packages i module descriptorem w postaci pliku module-info.java.
+ Module-info.java zawiera: nazwę, dependencies, paczki publiczne, serwisy konsumowane i oferowane, uprawnienia refleksji.

Przykład module descriptora:

```java
module java.sql {
    requires transitive java.logging;
    requires transitive java.transaction.xa;
    requires transitive java.xml;
    exports java.sql;
    exports javax.sql;

    uses java.sql.Driver;
}
```
oraz:

```java
module jdk.javadoc {
   requires java.xml;
   
   requires transitive java.compiler;
   requires transitive jdk.compiler;
   
   exports jdk.javadoc.doclet;
   
   provides java.util.spi.ToolProvider with
       jdk.javadoc.internal.tool.JavadocToolProvider;
   
   provides javax.tools.DocumentationTool with
       jdk.javadoc.internal.api.JavadocTool;
   
   provides javax.tools.Tool with
      jdk.javadoc.internal.api.JavadocTool;   
}
```

+ _requires_ oznacza moduły, na których obecny moduł polega,
+ _requires transitive_ oznacza dependencję przechodnią: jeżeli moduł m1 tranzytywnie zależy od modułu m2, a posiadamy trzeci moduł mX który zależy na m1, wtedy moduł mX także będzie miał dostęp do modułu m2;
+ _requires static_ specyfikuje zależności compile-time-only,
+ _exports_ specyfikuje paczki modułu które powinny być dostępne dla innych modułów (nie uwzględniając sub-packages),
+ _exports…to…_ umożliwia ograniczenie dostępu: export *com.my.package.name* do *com.specific.package*;
