## Назначение

Шаблон Builder был введен для решения некоторых проблем с шаблонами проектирования Factory и Abstract Factory, когда объект содержит много атрибутов. Есть три основные проблемы с паттернами проектирования Factory и Abstract Factory, когда объект содержит много атрибутов.

- Слишком много аргументов для передачи от клиентской программы к классу Factory, что может привести к ошибкам, поскольку в большинстве случаев тип аргументов одинаков, и со стороны клиента трудно сохранить порядок аргументов.
- Некоторые из параметров могут быть необязательными, но в паттерне Factory мы вынуждены передавать все параметры, а необязательные параметры должны передаваться как NULL.
- Если объект тяжелый и его создание сложное, то вся эта сложность будет частью классов Factory, что сбивает с толку.

Мы можем решить проблемы с большим количеством параметров, предоставив конструктор с обязательными параметрами, а затем различные методы setter для установки необязательных параметров. Проблема такого подхода заключается в том, что состояние объекта будет несогласованным, пока все атрибуты не будут установлены явно. Шаблон Builder решает проблему с большим количеством необязательных параметров и несогласованным состоянием, предоставляя способ создания объекта шаг за шагом и предоставляя метод, который фактически возвращает конечный объект.

## Принцип работы

1. Создать статический вложенный класс Builder, а затем скопировать все аргументы из внешнего класса в класс Builder. Мы должны следовать соглашению об именовании, и если имя класса - Computer, то класс Builder должен быть назван ComputerBuilder.
2. Класс Builder должен иметь публичный конструктор со всеми необходимыми атрибутами в качестве параметров.
3. Класс Builder должен иметь методы для установки необязательных параметров и должен возвращать тот же объект Builder после установки необязательного атрибута.
4. Последним шагом является предоставление метода build() в классе конструктора, который будет возвращать объект, необходимый клиентской программе. Для этого нам необходимо иметь приватный конструктор в классе с классом Builder в качестве аргумента.

## Пример

```java
public class Computer {
	//required parameters
	private String HDD;
	private String RAM;

	//optional parameters
	private boolean isGraphicsCardEnabled;
	private boolean isBluetoothEnabled;


	public String getHDD() {
		return HDD;
	}

	public String getRAM() {
		return RAM;
	}

	public boolean isGraphicsCardEnabled() {
		return isGraphicsCardEnabled;
	}

	public boolean isBluetoothEnabled() {
		return isBluetoothEnabled;
	}

	private Computer(ComputerBuilder builder) {
		this.HDD=builder.HDD;
		this.RAM=builder.RAM;
		this.isGraphicsCardEnabled=builder.isGraphicsCardEnabled;
		this.isBluetoothEnabled=builder.isBluetoothEnabled;
	}

	//Builder Class
	public static class ComputerBuilder{

		// required parameters
		private String HDD;
		private String RAM;

		// optional parameters
		private boolean isGraphicsCardEnabled;
		private boolean isBluetoothEnabled;

		public ComputerBuilder(String hdd, String ram){
			this.HDD=hdd;
			this.RAM=ram;
		}

		public ComputerBuilder setGraphicsCardEnabled(boolean isGraphicsCardEnabled) {
			this.isGraphicsCardEnabled = isGraphicsCardEnabled;
			return this;
		}

		public ComputerBuilder setBluetoothEnabled(boolean isBluetoothEnabled) {
			this.isBluetoothEnabled = isBluetoothEnabled;
			return this;
		}

		public Computer build(){
			return new Computer(this);
		}
	}
}

public class TestBuilderPattern {
	public static void main(String[] args) {
		//Using builder to get the object in a single line of code and
        //without any inconsistent state or arguments management issues
		Computer comp = new Computer
            .ComputerBuilder("500 GB", "2 GB")
            .setBluetoothEnabled(true)
            .setGraphicsCardEnabled(true)
            .build();
	}
}
```
