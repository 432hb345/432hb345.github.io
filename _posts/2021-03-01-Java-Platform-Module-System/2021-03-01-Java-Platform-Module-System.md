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

Moduł jest agregatorem z unikalną nazwą, obejmuje reużywalną grupę powiązanych packages, resources i module descriptor.
Modułu dostarczane są w plikach JAR z packages i module descriptorem w postaci pliku module-info.java.
Module-info.java zawiera: nazwę, dependencies, paczki publiczne, serwisy konsumowane i oferowane, uprawnienia refleksji.

Przykład module descriptora:

`module java.sql {
    requires transitive java.logging;
    requires transitive java.transaction.xa;
    requires transitive java.xml;

    exports java.sql;
    exports javax.sql;

    uses java.sql.Driver;
}`

