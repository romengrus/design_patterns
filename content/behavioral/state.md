---
title: "State"
date: 2023-03-03T23:26:13+02:00
draft: false
---

## Назначение

Если нам нужно изменить поведение объекта в зависимости от его состояния, мы можем иметь переменную состояния в объекте. Затем использовать блок условий if-else для выполнения различных действий в зависимости от состояния. Паттерн проектирования State используется для обеспечения систематического способа достижения этой цели. Контекст паттерна State - это класс, который имеет ссылку на одну из конкретных реализаций State. Контекст передает запрос объекту State для обработки.

## Принцип работы

Паттерн State - это решение проблемы, как сделать так, чтобы поведение зависело от состояния.

- Определить класс Context для представления единого интерфейса внешнему миру.
- Определить абстрактный базовый класс State.
- Представьте различные "состояния" как производные классы от базового класса State.
- Определить поведение, специфичное для конкретного состояния, в соответствующих производных классах State.
- Сохранить указатель на текущей State в классе Context.
- Чтобы изменить состояние - нужно изменить указатель на текущее "состояние".
- Шаблон State не указывает, где будут определены переходы состояния.
  Вариантов два: объект Context или каждый отдельный производный класс State. Преимуществом последнего варианта является простота добавления новых производных классов State. Недостатком является то, что каждый производный класс State имеет знания о своих братьях и сестрах (связь с ними), что вводит зависимости между подклассами.

## Пример

```java
public interface State {
	public void doAction();
}

public class TVStartState implements State {
	@Override
	public void doAction() {
		System.out.println("TV is turned ON");
	}
}

public class TVStopState implements State {
	@Override
	public void doAction() {
		System.out.println("TV is turned OFF");
	}
}

public class TVContext {
	private State tvState;

	public void setState(State state) {
		this.tvState=state;
	}

	public State getState() {
		return this.tvState;
	}

	public void doAction() {
		this.tvState.doAction();
	}
}

public class TVRemote {
	public static void main(String[] args) {
		TVContext context = new TVContext();
		State tvStartState = new TVStartState();
		State tvStopState = new TVStopState();

		context.setState(tvStartState);
		context.doAction();

		context.setState(tvStopState);
		context.doAction();
	}
}
```
