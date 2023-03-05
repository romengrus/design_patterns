---
title: "Singleton"
date: 2023-03-05T17:58:37+02:00
draft: true
---

## Назначение

Kласс имеет только один экземпляр, и доступ к нему обеспечен через единую глобальную точку входа.

## Принцип работы

1. Определить приватный статический атрибут в классе Singleton.
2. Определить публичную статическую функцию доступа в классе.
3. Выполнить "ленивую инициализацию" (создание при первом использовании) в функции доступа.
4. Определить все конструкторы как защищенные или приватные.
5. Клиенты могут использовать функцию доступа только для манипулирования синглтоном.

## Пример

```java
public class LazyInitializedSingleton {
    private static LazyInitializedSingleton instance;

    private LazyInitializedSingleton(){}

    public static LazyInitializedSingleton getInstance() {
        if (instance == null) {
            instance = new LazyInitializedSingleton();
        }
        return instance;
    }
}

```
