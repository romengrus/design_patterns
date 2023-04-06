---
title: "Template_method"
date: 2023-03-04T17:37:29+02:00
draft: false
---

## Назначение

Определить скелет алгоритма в операции, отложив некоторые шаги для клиентских подклассов. Template Method позволяет подклассам переопределять определенные шаги алгоритма без изменения структуры алгоритма.

## Принцип работы

- Определить какие шаги в алгоритме являются стандартными, а какие - характерными для каждого из существующих классов.
- Определить новый абстрактный базовый класс.
- Перенести оболочку алгоритма (теперь она называется "шаблонный метод") и определить все стандартные шаги в новый базовый класс.
- Определить в базовом классе метод, который требует множества различных реализаций. Этот метод может содержать реализацию по умолчанию - или - он может быть определен как абстрактный.
- Вызвать этот метод из метода шаблона.
- Каждый из существующих классов объявляет связь "is-a" с новым абстрактным базовым классом.
- Удалить из существующих классов все детали реализации, которые были перенесены в базовый класс.
- Единственными деталями, которые останутся в существующих классах, будут детали реализации, свойственные каждому производному классу.

## Пример

```java
public abstract class HouseTemplate {
	//template method, final so subclasses can't override
	public final void buildHouse(){
		buildFoundation();
		buildPillars();
		buildWalls();
		buildWindows();
		System.out.println("House is built.");
	}

	//default implementation
	private void buildWindows() {
		System.out.println("Building Glass Windows");
	}

	//methods to be implemented by subclasses
	public abstract void buildWalls();
	public abstract void buildPillars();

	private void buildFoundation() {
		System.out.println("Building foundation with cement,iron rods and sand");
	}
}

public class WoodenHouse extends HouseTemplate {
	@Override
	public void buildWalls() {
		System.out.println("Building Wooden Walls");
	}

	@Override
	public void buildPillars() {
		System.out.println("Building Pillars with Wood coating");
	}
}

public class GlassHouse extends HouseTemplate {
	@Override
	public void buildWalls() {
		System.out.println("Building Glass Walls");
	}

	@Override
	public void buildPillars() {
		System.out.println("Building Pillars with glass coating");
	}
}

public class HousingClient {
	public static void main(String[] args) {

		HouseTemplate houseType = new WoodenHouse();

		//using template method
		houseType.buildHouse();
		System.out.println("************");

		houseType = new GlassHouse();

		houseType.buildHouse();
	}
}
```
