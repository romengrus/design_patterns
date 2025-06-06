## Назначение

Преобразовывает интерфейс класса в другой интерфейс, который ожидают клиенты. Адаптер позволяет работать вместе классам, которые иначе не могли бы работать из-за несовместимости интерфейсов.

## Принцип работы

1. Определить участников: компонент(ы), которые хотят приспособиться (т.е. клиент), и компонент, который должен адаптироваться (т.е. адаптант).
2. Определить интерфейс, который требуется клиенту.
3. Создать класс-"обертку", который может согласовать интерфейс адаптируемого компонента с клиентом.
4. Клиент использует новый интерфейс

## Пример

```java
public interface USB {
   void connectWithUsbCable();
}

public class MemoryCard {
   public void insert() {
       System.out.println("Карта памяти успешно вставлена!");
   }

   public void copyData() {
       System.out.println("Данные скопированы на компьютер!");
   }
}

public class CardReader implements USB {
   private MemoryCard memoryCard;

   public CardReader(MemoryCard memoryCard) {
       this.memoryCard = memoryCard;
   }

   @Override
   public void connectWithUsbCable() {
       this.memoryCard.insert();
       this.memoryCard.copyData();
   }
}

public class Main {
   public static void main(String[] args) {
       USB cardReader = new CardReader(new MemoryCard());
       cardReader.connectWithUsbCable();
   }
}

```
