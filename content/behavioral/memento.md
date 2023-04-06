---
title: "Memento"
date: 2023-01-29T21:05:01+02:00
draft: false
---

## Назначение

Предназначен для сохранения и последующего восстановления состояния объекта.

## Принцип работы

В паттерне Memento 3 участника:

- Originator(Editor) - класс в который нуждается в сохранении и восстановлении состояния.

- Memento - класс который содержит все поля, которые нужно сохранять

- Caretaker(History) - класс в который содержит коллекцию состояний Memento.

## Преимущества

- Простой способ сохранения и восстановления состояния

- Сохраняет инкапсуляцию

## Недостатки

- Если сохраняемое состояние большое, то будет использовано много памяти.

## Пример

```java
public class EditorState {
    private final String content;

    public EditorState(String content) {
        this.content = content;
    }

    public String getContent() {
        return content;
    }
}

public class Editor {
    private String content;

    public String getContent() {
        return content;
    }

    public void setContent(String content) {
        this.content = content;
    }

    public EditorState createState() {
        return new EditorState(getContent());
    }

    public void restore(EditorState state) {
        content = state.getContent();
    }
}

public class History {
    Stack<EditorState> states = new Stack<>();

    public void save(EditorState state) {
        states.push(state);
    }

    public void revert(Editor editor) {
        if (!states.isEmpty())
            editor.restore(states.pop());
        else
            System.out.println("Cannot undo !");
    }
}

public class Application {
    public static void main(String[] args) {
        var editor = new Editor();
        var history = new History();
        editor.setContent("I");
        history.save(editor.createState());
        editor.setContent("I am");
        history.save(editor.createState());
        editor.setContent("I am Jalitha");

        System.out.println(editor.getContent());
        //output will be "I am Jalitha"
        history.revert(editor);
        System.out.println(editor.getContent());
        //output will be "I am"
        history.revert(editor);
        System.out.println(editor.getContent());
        //output will be "I"
        history.revert(editor);
         //output will be "Cannot undo !"
    }
}

```
