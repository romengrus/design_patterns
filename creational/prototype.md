## Назначение

Шаблон Prototype используется, когда создание объекта требует много времени и ресурсов, а аналогичный объект уже существует. Модель прототипа предоставляет механизм для копирования исходного объекта в новый объект и последующей его модификации в соответствии с нашими потребностями. Шаблон проектирования Prototype использует клонирование java для копирования объекта.

## Принцип работы

1. Добавить метод clone() к существующей иерархии классов
2. Создать "реестр", который будет поддерживать кэш прототипных объектов. Реестр может находиться в классе Factory или в базовом классе иерархии.
3. Создать метод фабрики, который находит нужный объект - прототип, вызывает clone() для этого объекта и возвращает результат.
4. Клиент заменяет все ссылки на оператор new вызовами метода фабрики.

## Пример

```java
public class Employees implements Cloneable{
	private List<String> empList;

	public Employees(){
		empList = new ArrayList<String>();
	}

	public Employees(List<String> list){
		this.empList=list;
	}
	public void loadData(){
		//read all employees from database and put into the list
		empList.add("Sally");
		empList.add("Roman");
		empList.add("David");
		empList.add("Lisa");
	}

	public List<String> getEmpList() {
		return empList;
	}

	@Override
	public Object clone() throws CloneNotSupportedException{
        List<String> temp = new ArrayList<String>();
        for(String s : this.getEmpList()){
            temp.add(s);
        }
        return new Employees(temp);
	}
}

public class PrototypePatternTest {
	public static void main(String[] args) throws CloneNotSupportedException {
		Employees emps = new Employees();
		emps.loadData();

		//Use the clone method to get the Employee object
		Employees empsNew = (Employees) emps.clone();
		Employees empsNew1 = (Employees) emps.clone();
		List<String> list = empsNew.getEmpList();
		list.add("John");
		List<String> list1 = empsNew1.getEmpList();
		list1.remove("Sally");

		System.out.println("emps List: "+emps.getEmpList());
		System.out.println("empsNew List: "+list);
		System.out.println("empsNew1 List: "+list1);
	}
}

```
