---
title: "Decorator"
date: 2023-03-06T01:00:37+02:00
draft: false
---

## Назначение

Decorator позволяет динамически добавлять поведение к объекту во время выполнения. Это может быть полезно, когда у вас есть объект, который должен иметь различное поведение или функциональность в зависимости от ситуации.

## Принцип работы

Основная идея заключается в том, чтобы иметь базовый объект (часто интерфейс или абстрактный класс), определяющий базовую функциональность, к которой мы хотим добавить поведение. Затем мы создаем один или несколько классов-декораторов, которые реализуют тот же интерфейс или абстрактный класс, что и базовый объект, а также хранят ссылку на базовый объект.

Каждый декоратор добавляет новое поведение к базовому объекту, оборачивая его новой функциональностью. Когда клиентский код вызывает метод декорированного объекта, вызов передается через декоратор базовому объекту, который затем выполняет метод как обычно. Однако декоратор может также добавить новое поведение до или после вызова базового объекта.

## Пример

```java
public interface Car {
    void drive();
}

public class SimpleCar implements Car {
    @Override
    public void drive() {
        System.out.println("Driving a simple car.");
    }
}

public class NavigationDecorator implements Car {
    private Car car;

    public NavigationDecorator(Car car) {
        this.car = car;
    }

    @Override
    public void drive() {
        car.drive();
        System.out.println("Using navigation.");
    }
}

public class Main {
    public static void main(String[] args) {
        Car car = new SimpleCar();
        car = new NavigationDecorator(car);
        car.drive();
    }
}
```
