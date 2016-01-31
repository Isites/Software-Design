#访问者模式
##访问者模式的优点
>* **符合单一职责原则**: 凡是使用访问者模式的场景中，元素类中需要封装在访问者中的操作必定是与元素`类本身关系不大且易变的操作`，使用访问者模式一方面符合单一职责原则，另一方面，因为被封装的操作通常来说都是易变的，所以当发生变化时，就可以在不改变类本身的前提下，实现对变化部分的扩展。  
>* **扩展性好**:  元素类可以通过接受不同的访问者来实现对不同操作的扩展。  
##访问者模式适用的场景
>假如一个对象中存在一些与本对象不相干(或者相关较弱)的操作，为了避免这些操作污染这个对象，则可以使用访问者模式来吧这些操作封装到访问这种去。  
>假如`一组对象中，存在着相似的操作`，为了避免出现大量重复的代码，也可以将这些重复的操作封装到访问这种去。但是访问者模式并不是那么完美，它也有致命的缺陷: 增加新的元素比较困难。通过访问者模式的代码可以看到，在访问者类中，每一个元素类都有它对应的处理方法，也就是说，每增加一个元素类都需要修改访问者类(也包括访问者类的子类或者实现类)，修改起来相当麻烦。也就是说，在元素数目不确定的情况下，应该慎用访问者模式。所以，`访问者模式比较适用于对已有功能的重构`，比如说，一个项目的基本功能已经确定下来，元素的数据已经基本确定下来不会变了，会变的只是这些元素内的相关操作，这时候，我们可以使用访问者模式对原有代码进行重构一遍，这样一来，就可以在不修改各个元素类的情况下，对原有功能进行修改。  

##角色
* **抽象访问者角色**：声明了一个或者多个方法发操作，形成所有的具体访问者角色必须实现的接口  
*  **具体访问者角色**： 实现抽象访问者所声明的接口，也就是抽象访问者所声明的各个访问操作。  
*  **抽象这节点角色**：声明一个接受操作，接受一个访问者对象作为一个参数  
*  **具体节点角色**： 实现了抽象节点所规定的接受操作  
*  **结构对象角色**：可以遍历结构中的所有元素;如果需要，提供一个高层次的接口让访问者对象可以访问每一个元素；如果需要可以设计成一个复合对象或者一个聚集，如List或Set  


**eg.**
**访问者模式主要利用了函数的重载**  
```java
public interface Visitor{
	public void visit(NodeImplA node);
	public void visit(NodeImplB node);
}
public class VisitorImpl{
	public void visit(NodeImplA node){
		System.out.println("A");
	}
	public void visit(NodeImplB node){
		System.out.println("B");
	}
}
public abstract class Node{
	public abstract void accept(Visitor visitor);
}
public class NodeImplA extends Node{
	public void accept(Visitor visitor){
		//在这里和下面的方法，虽然代码一模一样，但是实际上this的类型不一样，充分利用了方法的重载
		visitor.visit(this);
	}
}
public class NodeImplB extends Node{
	public void accept(Visitor visitor){
		visitor.visit(this);
	}
}
public class Client{
	public static void main(String args[]){
		Node nodes[] = new Node[]{
			new NodeImplA(),
			new NodeImplB()
		};
		Visitor visit = new VisitorImpl();
		for(Node node : nodes){
			node.accept(visit);
		}
	}
}
```

**expand**
###分派的概念
>变量被声明时的类型叫做变量的静态类型(static type),有些人又把静态类型叫做明显类型(apparent type);而变量所引用的对象的真是类型又叫做变量的实际类型(actual type)。  
```java
List list = null;
list = new ArrayList();
```
声明了一个变量list，它的静态类型(也叫明显类型)是List，而它的实际类型是ArrayList。根据对象的类型而对方发进行选择，就是分派(Dispathc),分派分为两种，既静态分配和动态分派。  
**静态分派**发生在编译时期，分派根据类型信息发生。静态分派对我们来说并不陌生，方法重载就是静态分派。  
**动态分派**发生在运行时期，动态分派动态地置换掉某个方法发。  
java 编译器在编译时期并不总是知道那些代码会被执行，因为编译器仅仅知道对象的静态类型，而不知道对象的真是类型；而方法的调用那个则是根据对象的真实类型，而不是发静态类型。**[more](http://www.cnblogs.com/java-my-life/archive/2012/06/14/2545381.html)**
