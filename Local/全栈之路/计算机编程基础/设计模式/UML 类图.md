# UML 类图

UML——Unified modeling language UML (统一建模语言)，是一种用于软件系统分析和设计的语言工具，它用于帮助软件开发人员进行思考和记录思路的结果。

<img src="../../../../Image/UML 类图/UML-符号.svg" alt="UML-符号" style="object-fit: cover; border-radius: 10px; width: 80%;" />

## UML图

UML 图分类：

1. 用例图(use case)
2. 静态结构图：类图、对象图、包图、组件图、部署图
3. 动态行为图：交互图（时序图与协作图）、状态图、活动图

## UML 类图

1. 用于描述系统中的类**(**对象**)**本身的组成和类**(**对象**)**之间的各种静态关系。
2. 类之间的关系：依赖、泛化（继承）、实现、关联、聚合与组合。

举例

```java
public class Person{
    private Integer id;
    private String name;
    public void setName(String name){ 
        this.name=name;
    }
    public String getName(){ 
        return	name;
    }
}
```

<img src="../../../../Image/UML 类图/UML-类示例.svg" alt="UML-类示例" style="object-fit: cover; border-radius: 10px; width: 60%;" />

### 依赖关系(Dependence)

只要是在<font color="#faa755">类中用到了对方</font>，那么他们之间就存在依赖关系。如果没有对方，连编绎都通过不了。

```java
public class PersonServiceBean {
	private PersonDao personDao;// 类

	public void save(Person person) {
	}

	public IDCard getIDCard(Integer personid) {
		return null;
	}

	public void modify() {
		Department department = new Department();
	}

}

public class PersonDao {}
public class Person {}
public class IDCard {}
public class Department {}
```

<img src="../../../../Image/UML 类图/UML-依赖关系.svg" alt="UML-依赖关系" style="object-fit: cover; border-radius: 10px; width: 100%;" />

小结：

1. 类中用到了对方
2. 如果是类的成员属性
3. 如果是方法的返回类型
4. 是方法接收的参数类型
5. 方法中使用到

### 泛化关系(generalization）

1. 泛化关系实际上就是继承关系
2. 如果 A 类继承了 B 类，我们就说 A 和 B 存在泛化关系

```java
public abstract class DaoSupport{
	public void save(Object entity){
	}
	public void delete(Object id){
	}
}

public class PersonServiceBean extends DaoSupport {}
```

<img src="../../../../Image/UML 类图/UML-泛化关系.svg" alt="UML-泛化关系" style="object-fit: cover; border-radius: 10px; width: 100%;" />

 小结:

1. 泛化关系实际上就是继承关系
2. 如果 A 类继承了 B 类，我们就说 A 和 B 存在泛化关系

### 实现关系(Implementation)

实现关系实际上就是 **A** 类实现 **B** 接口，他是依赖关系的特例

```java
public interface PersonService {
	public void delete(Integer id);
}
public class PersonServiceBean implements PersonService{
	@Override
	public void delete(Integer id) {
		System.out.println("delete..");
	}
}
```

<img src="../../../../Image/UML 类图/UML-实现关系.svg" alt="UML-实现关系" style="object-fit: cover; border-radius: 10px; width: 100%;" />

### 关联关系(Association)

关联关系实际上就是类与类之间的联系，他是依赖关系的特例

关联具有<font color="#faa755">导航性</font>：即双向关系或单向关系

关系具有多重性：如“1”（表示有且仅有一个），“0…”（表示0个或者多个），
“0,1”（表示0个或者一个），“n...m（表示n到m个都可以），“m.*”（表示至少m个）

单项一对一关系

```java
class Person {
    private IDCard card;
}

class IDCard {}
```

双向一对一关系

```java
class Person {
    private IDCard card;
}

class IDCard {
    private Person person;
}
```

<img src="../../../../Image/UML 类图/UML-关联关系.svg" alt="UML-关联关系" style="object-fit: cover; border-radius: 10px; width: 80%;" />

### 聚合关系(Aggregation)

聚合关系（Aggregation）表示的是整体和部分的关系，整体与部分可以分开。聚合关系是<font color="#faa755">关联关系</font>的特例，所以他具有关联的导航性与多重性。

如：一台电脑由键盘(keyboard)、显示器(monitor)，鼠标等组成；组成电脑的各个配件是<font color="#faa755">可以从电脑上分离</font>出来的，使用带空心菱形的实线来表示

```java
class Computer {
	private Mouse mouse; //鼠标可以和computer分离
	private Monitor monitor;//显示器可以和Computer分离
	public void setMouse(Mouse mouse) {
		this.mouse = mouse;
	}
	public void setMonitor(Monitor monitor) {
		this.monitor = monitor;
	}
}

class Monitor {}
class Mouse {}
```

<img src="../../../../Image/UML 类图/UML- 聚合关系.svg" alt="UML- 聚合关系" style="object-fit: cover; border-radius: 10px; width: 100%;" />

### 组合关系(Composition)

组合关系：也是整体与部分的关系，但是<font color="#faa755">整体与部分不可以分开</font>。

```java
class Computer {
    private Mouse mouse = new Mouse(); //鼠标可以和computer不能分离
    private Monitor monitor = new Monitor();//显示器可以和Computer不能分离

    public void setMouse(Mouse mouse) {this.mouse = mouse;}

    public void setMonitor(Monitor monitor) {this.monitor = monitor;}

}
class Monitor {}
class Mouse {}
```

<img src="../../../../Image/UML 类图/UML-组合关系.svg" alt="UML-组合关系" style="object-fit: cover; border-radius: 10px; width: 100%;" />

在程序中我们定义实体：Person 与 IDCard、Head, 那么 Head 和 Person 就是 组合，IDCard 和 Person 就是聚合，但是如果在程序中 Person 实体中定义了对 IDCard 进行级联删除，即删除 Person 时连同 IDCard 一起删除，那么 IDCard 和 Person 就是组合了。

