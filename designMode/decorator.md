#装饰器模式
##概述
>继承，是所有面向对象语言的一个基本特征。如果已经存在的一个类缺少某些方法，或者需要给方法添加更多功能，你可以通过继承这个类来产生一个新类-这建立在额外的代码上。通过继承一个现有类可以使的子类在拥有自身方法的同时还拥有 父类的方法。但是这种方法是静态的，用户不能控制增加行为的方式和时机。
##问题
>你如何组织你的代码使其可以容易的添加基本的或者一些很少用到的特性，而不是直接写额外的代码在你的类内部。
##解决方案
>装饰器模式：动态地给一个对象添加一些额外的职责或者行为。就增加功能来说，Decorator模式相比生成子类更为灵活。  
>装饰器模式提供了改变子类的灵活方案。装饰器模式在不必改变原类文件和使用继承的情况下，动态的扩展一个对象的功能。它是通过创建一个包装对象，也就是装饰来包裹真实的对象。  
>当用于一组子类时，装饰器模式更加游泳。如果你拥有一族子类(从一个父类派生而来)，你需要在与子类独立使用情况下添加额外的特性，你可以使用装饰器模式，以避免代码重复和具体子类数量的增加。
##适用性
>以下情况下使用Decorator模式
>1. 在不影响其他对象的情况下，以动态、透明的方式给单个对象添加职责。
>2. 处理那些可以撤销的职责。
>3. 当不能采用生成子类方法进行扩充时。一种情况是，可能有大量独立的扩展，为支持每一种组合将产生大量的子类，使得子类数目呈爆炸性增长。另一种情况可能是因为类定义被隐藏，或类定义不能用于生成子类。

**eg.**
```java
public interface IWork{
	public void doWork();
}
public class Coder implements IWork{
	//程序员编码工作
	public void doWork(){}
}
public class Manager implements IWork{
	private IWork iwork;
	public void wrapToManager(IWork iwork){
		this.iwork = iwork;
		return this;
	}
	public void doWork(){
		doEarlyWork();
		iwork.doWork();
		doEndWork();
	}
	//前期工作
	public void doEarlyWork(){}
	//收尾工作
	public void doEndWork(){}
}
public class ManagerWrapper extends Manager{
	public void doEarlyWork(){
		System.out.println("earlyWork");
	}
}
public class Client{
	public static void main(String args[]){
		IWork coder = new Coder();
		Manager manager = new ManagerWrapper();
		manager.wrapToManager(coder).doWork();
	}
}
```