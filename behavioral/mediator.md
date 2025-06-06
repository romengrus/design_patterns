## Назначение

Паттерн Посредник позволяет обеспечить свободную связь между множеством объектов в приложении, обрабатывая взаимодействие между ними.

Хорошо спроектированные корпоративные приложения состоят из легких объектов с определенными обязанностями, распределенными между ними в соответствии с принципом единой ответственности, одним из принципов проектирования SOLID. Однако преимущества приложения, состоящего из большого количества небольших объектов, сопровождаются проблемой - коммуникацией между ними. Объектам необходимо взаимодействовать для выполнения бизнес-требований, и по мере роста числа объектов необходимые связи между ними могут быстро стать неуправляемыми. Кроме того, объекты, взаимодействующие между собой, должны знать друг о друге - все они тесно связаны, что нарушает основы принципов SOLID. Паттерн Посредник - это проверенный способ решения таких повторяющихся задач и возникающих из-за них проблем.

Если существующий объект обновляется новыми правилами взаимодействия или в систему добавляется новый объект, необходимо обновить только объект-посредник. В отсутствие посредника пришлось бы обновлять все соответствующие объекты, которые должны взаимодействовать. Благодаря использованию паттерна медиатора код становится более инкапсулированным, поэтому изменения не так масштабны.

## Принцип работы

Чтобы убрать множественные зависимости между классами - создается один класс, который собираетс на себя все зависимости и управляет работой других классов.

## Преимущества

- Развязывает классы компонентов. Компонент зависит только от посредника. Это превращает взаимодействие "многие-ко-многим" во взаимодействие "один-ко-многим".

- Позволяет переиспользовать компоненты.

- Полезен, когда логика связи между объектами сложна, мы можем иметь центральную точку, которая заботится о логике связи.

## Недостатки

- Получается слишком большой объект с большим списком зависимостей и знаний о других объектах.

- Мы не должны использовать паттерн медиатора только для уменьшения связности, потому что если количество медиаторов будет расти, то их будет трудно поддерживать.

## Пример

```java
public interface ChatMediator {
	public void sendMessage(String msg, User user);
	void addUser(User user);
}

public abstract class User {
	protected ChatMediator mediator;
	protected String name;

	public User(ChatMediator med, String name){
		this.mediator=med;
		this.name=name;
	}

	public abstract void send(String msg);

	public abstract void receive(String msg);
}

public class ChatMediatorImpl implements ChatMediator {
	private List<User> users;

	public ChatMediatorImpl(){
		this.users=new ArrayList<>();
	}

	@Override
	public void addUser(User user){
		this.users.add(user);
	}

	@Override
	public void sendMessage(String msg, User user) {
		for(User u : this.users){
			//message should not be received by the user sending it
			if(u != user){
				u.receive(msg);
			}
		}
	}
}

public class UserImpl extends User {
	public UserImpl(ChatMediator med, String name) {
		super(med, name);
	}

	@Override
	public void send(String msg){
		System.out.println(this.name+": Sending Message="+msg);
		mediator.sendMessage(msg, this);
	}
	@Override
	public void receive(String msg) {
		System.out.println(this.name+": Received Message:"+msg);
	}
}

public class ChatClient {
	public static void main(String[] args) {
		ChatMediator mediator = new ChatMediatorImpl();
		User user1 = new UserImpl(mediator, "Pankaj");
		User user2 = new UserImpl(mediator, "Lisa");
		User user3 = new UserImpl(mediator, "Saurabh");
		User user4 = new UserImpl(mediator, "David");
		mediator.addUser(user1);
		mediator.addUser(user2);
		mediator.addUser(user3);
		mediator.addUser(user4);

		user1.send("Hi All");
	}
}
```
