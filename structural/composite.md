## Назначение

Позволяет обращаться с группой объектов так же, как и с отдельным объектом. Этот паттерн используется, когда у вас есть иерархическая структура объектов, и мы хотим иметь возможность обрабатывать отдельные объекты и группы объектов одинаковым образом.

Основная идея шаблона Composite заключается в представлении древовидной структуры объектов, где каждый объект может быть либо листом (отдельный объект), либо составным объектом (группа объектов). Составной объект содержит список дочерних объектов, которые могут быть как отдельными объектами, так и другими составными объектами.

## Принцип работы

Состоит из следующих компонентов:

1. Компонент: абстрактный класс или интерфейс, определяющий общие операции, которые могут быть выполнены как над отдельными объектами, так и над группами объектов.

2. Лист: класс, представляющий отдельный объект, который не может содержать другие объекты.

3. Композит: класс, который представляет группу объектов и может содержать как другие композитные объекты, так и отдельные объекты.

Реализация:

1. Создать абстрактный класс или интерфейс под названием Component, определяющий общие операции, которые можно выполнять как над отдельными объектами, так и над группами объектов. Этот класс должен иметь такие методы, как add(), remove() и getChild(), которые используются для управления дочерними объектами.

2. Создать класс Leaf, представляющий отдельный объект, который не может содержать другие объекты. Этот класс должен расширять класс Component и предоставлять реализацию метода operation().

3. Создайте класс Composite, который представляет группу объектов и может содержать как другие составные объекты, так и отдельные объекты. Этот класс также должен расширять класс Component и реализовывать методы add(), remove(), getChild() и operation().

4. Создайте клиентский класс, который использует объекты Component для выполнения некоторой операции.

## Пример

```java
public abstract class Component {
    public abstract void operation();
    public void add(Component c) {
        throw new UnsupportedOperationException();
    }
    public void remove(Component c) {
        throw new UnsupportedOperationException();
    }
    public Component getChild(int i) {
        throw new UnsupportedOperationException();
    }
}

public class Leaf extends Component {
    public void operation() {
        // Do something for leaf node
    }
}

public class Composite extends Component {
    private List<Component> childComponents = new ArrayList<Component>();
    public void operation() {
        // Do something for composite node
        for (Component component : childComponents) {
            component.operation();
        }
    }
    public void add(Component c) {
        childComponents.add(c);
    }
    public void remove(Component c) {
        childComponents.remove(c);
    }
    public Component getChild(int i) {
        return childComponents.get(i);
    }
}

public class Client {
    public static void main(String[] args) {
        Component leaf1 = new Leaf();
        Component leaf2 = new Leaf();
        Component composite1 = new Composite();
        Component composite2 = new Composite();
        composite1.add(leaf1);
        composite1.add(composite2);
        composite2.add(leaf2);
        composite1.operation();
    }
}
```
