---
title: "Flyweight"
date: 2023-03-06T01:24:33+02:00
draft: false
---

## Назначение

Целью паттерна Flyweight является минимизация использования памяти и повышение производительности за счет совместного использования как можно большего количества данных между похожими объектами. Модель Flyweight достигает этого путем создания пула общих объектов, которые могут быть использованы повторно при любой возможности.

Во многих приложениях может существовать большое количество объектов, которые очень похожи друг на друга. Например, в приложении для обработки текстов могут существовать тысячи экземпляров буквы "a", которые имеют одинаковые свойства и поведение. Создание отдельного объекта для каждого из этих экземпляров было бы неэффективным с точки зрения использования памяти и производительности.

## Принцип работы

1. Определить объекты, которые можно использовать совместно. Во многих случаях это будут объекты, имеющие большое количество экземпляров, которые очень похожи друг на друга.

2. Создать интерфейс Flyweight или абстрактный класс, определяющий свойства и поведение общих объектов.

3. Создать один или несколько классов ConcreteFlyweight, которые реализуют интерфейс Flyweight или абстрактный класс. Эти классы обеспечивают конкретную реализацию общих объектов.

4. Создать класс FlyweightFactory, который создает и управляет общими объектами flyweight. Этот класс поддерживает пул объектов flyweight и предоставляет клиентам методы для доступа и использования этих объектов.

5. Создать класс Client, который использует объекты Flyweight. Клиент запрашивает объект flyweight у FlyweightFactory и использует его для выполнения некоторой операции.

## Пример

Предположим, у нас есть игра, в которой участвует множество различных типов персонажей, таких как солдаты, лучники и маги. Каждый персонаж имеет некоторые общие свойства, такие как имя, значение здоровья и положение на игровом поле, а также некоторые уникальные свойства, такие как тип оружия, дальность атаки и специальная способность.

```java
public interface Character {
    void move(int x, int y);
    void attack();
    void useSpecialAbility();
}

public class Soldier implements Character {
    private String name;
    private int health;
    private int x;
    private int y;

    public Soldier(String name, int health, int x, int y) {
        this.name = name;
        this.health = health;
        this.x = x;
        this.y = y;
    }

    public void move(int x, int y) {
        // Move the soldier to the specified position
        this.x = x;
        this.y = y;
    }

    public void attack() {
        // Perform the soldier's attack
    }

    public void useSpecialAbility() {
        // Perform the soldier's special ability
    }
}

public class Archer implements Character {
    private String name;
    private int health;
    private int x;
    private int y;

    public Archer(String name, int health, int x, int y) {
        this.name = name;
        this.health = health;
        this.x = x;
        this.y = y;
    }

    public void move(int x, int y) {
        // Move the archer to the specified position
        this.x = x;
        this.y = y;
    }

    public void attack() {
        // Perform the archer's attack
    }

    public void useSpecialAbility() {
        // Perform the archer's special ability
    }
}

public class CharacterFactory {
    private Map<String, Character> characters = new HashMap<>();

    public Character getCharacter(String name, int health, int x, int y, String type) {
        String key = type + "_" + name;
        Character character = characters.get(key);
        if (character == null) {
            switch (type) {
                case "Soldier":
                    character = new Soldier(name, health, x, y);
                    break;
                case "Archer":
                    character = new Archer(name, health, x, y);
                    break;
                // Add more cases for other types of characters
            }
            characters.put(key, character);
        }
        return character;
    }
}

public class Game {
    private CharacterFactory factory = new CharacterFactory();

    public void addCharacter(String name, int health, int x, int y, String type) {
        Character character = factory.getCharacter(name, health, x, y, type);
        // Add the character to the game board
    }
}
```

В этом примере паттерн Flyweight используется для разделения общих свойств и поведения персонажей между несколькими экземплярами одного типа. CharacterFactory поддерживает кэш существующих объектов flyweight и возвращает существующий объект, если объект с такими же свойствами уже существует
