## Назначение

Отделить реализацию абстракции от ее интерфейса, позволяя им развиваться независимо друг от друга. Этот паттерн дает возможность варьировать реализацию абстракции без изменения кода ее клиентов.

Проще говоря, паттерн Bridge позволяет создать иерархию классов, в которой один набор классов (абстракция) определяет интерфейс, а другой набор классов (реализация) обеспечивает реализацию. Паттерн "Мост" позволяет изменять реализацию независимо от интерфейса, что делает его мощным инструментом для управления сложностью и повышения гибкости кода.

## Принцип работы

???

## Пример

```java
public interface TV {
    void on();
    void off();
    void tuneChannel(int channel);
}

public interface RemoteControl {
    void power();
    void volumeUp();
    void volumeDown();
    void channelUp();
    void channelDown();
}

public class BaseTV implements TV {
    private RemoteControl remoteControl;

    public BaseTV(RemoteControl remoteControl) {
        this.remoteControl = remoteControl;
    }

    public void on() {
        System.out.println("TV is on");
        remoteControl.power();
    }

    public void off() {
        System.out.println("TV is off");
        remoteControl.power();
    }

    public void tuneChannel(int channel) {
        System.out.println("Tuning to channel " + channel);
        if (channel > 0) {
            remoteControl.channelUp();
        } else {
            remoteControl.channelDown();
        }
    }
}

public class RemoteControlImpl implements RemoteControl {
    private boolean on;
    private int volume;
    private int channel;

    public void power() {
        on = !on;
        System.out.println("TV is " + (on ? "on" : "off"));
    }

    public void volumeUp() {
        if (on) {
            volume++;
            System.out.println("Volume is now " + volume);
        }
    }

    public void volumeDown() {
        if (on) {
            volume--;
            System.out.println("Volume is now " + volume);
        }
    }

    public void channelUp() {
        if (on) {
            channel++;
            System.out.println("Channel is now " + channel);
        }
    }

    public void channelDown() {
        if (on) {
            channel--;
            System.out.println("Channel is now " + channel);
        }
    }
}

public class BridgePatternDemo {
    public static void main(String[] args) {
        RemoteControl remoteControl = new RemoteControlImpl();
        TV tv = new BaseTV(remoteControl);
        tv.on();
        tv.tuneChannel(5);
        tv.off();

        TV smartTv = new SmartTV(remoteControl);
        smartTv.on();
        smartTv.tuneChannel(5);
        ((SmartTV) smartTv).browseInternet();
        smartTv.off();
    }
}
```
