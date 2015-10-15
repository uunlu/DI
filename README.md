# Dependency Injection

Understanding what Dependency Injection is took me a lot of time. Giving big names to little concepts to scare junior developers must be a tradition of software industry megaminds. 

In this article, I will try to explain:

- A Real Life Analogy about Dependency Injection ( This part is not ready yet!)
- What is Dependency Injection?
- Why to use Dependency Injection?
- How to implement Dependency Injection with two examples. (Currently, there is only one example code)

# - What is Dependency Injection?
Instead of giving a class as a paremeter to another class, implement an interface on any class and pass that interface to the target class.

```
public class PadController
{
	public void RightMoveButtonClick()
	{
		Console.Log("Move Right");
	}
	public void LeftMoveButtonClick()
	{
		Console.Log("Move Left");
	}
}


public class Game
{
	private PadController _controller { get; set; // We are tigth coupling Game class with PadController }

	// We are tigth coupling Game class with PadController 
	public Game(PadController GameController)
	{
		_controller = GameController;
	}
	if(Key.Left)
	   _controller.LeftButtonClick();
}

```

# - Why tight coupling is evil?

```
public class PadController
{
    // we renamed the method RightMoveButtonClick as:
	public void RightMoveButton()
	{
		Console.Log("Move Right");
	}
	// we renamed the method LeftMoveButtonClick as:
	public void LeftMoveButton()
	{
		Console.Log("Move Left");
	}
}
```
Now, the code will not compile since there are no more RightMoveButtonClick and LeftMoveButtonClick methods to be called in the Game class. 

The other problem is, if you want to use a JoyStickController instead of PadController, you need to implement Joystick methods in your Game class as well. That is really confusing stuff!

# - How to implement Dependency Injection?

This is IGameController Interface.
```
public interface IGameController
{
	public void MoveLeft;
	public void MoveRight;
}
```

Let's PadController inherit from IGameController. Now, PadController class has to implement IGameController methods. There are two method signatures in IGameController: MoveLeft, MoveRight. PadController class needs to write its own implementation for these methods.
```
public PadController : IGameController
{
	public void MoveLeft()
	{
		Console.Log("Push Left Button");
	}
	public void MoveRight()
	{
		Console.Log("Push Right Button");
	}
}
```
Now, we inject IGameController in the PadController constructor in the code below. Hoarray! We are loosely coupled!
```
public class Game
{
	private PadController _controller { get; set; }

	// Contructor Injection 
	public Game(IGameController GameController)
	{
		_controller = GameController;
	}
	if(Key.Left)
	   _controller.MoveLeft();
}
```
In the future, if we ever want to use a joystick instead of a game pad, all we need to do is writing a Joystick class that inherits from IGameController interface. 
```
public class Joystick : IGameController
{
	public void MoveRight()
	{
		Console.Log("Move Right from Joystick");
	}
	public void MoveLeft()
	{
		Console.Log("Move Left from Joystick");
	}
}
```
Our Game class does not need to know anything about the implementation. Because injected parameters in the contructors will always provide those two methods used in Game class: MoveLeft and MoveRight. 
