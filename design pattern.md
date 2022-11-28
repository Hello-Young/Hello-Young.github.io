# 24种设计模式  

设计模式 (Design pattern) 代表了最佳的实践, 通常被有经验的面向对象的软件开发人员所采用. 设计模式是软件开发人员在软件开发过程中面临的一般问题的解决方案. 这些解决方案是众多软件开发人员经过相当长的一段时间的试验和错误总结出来的.  

设计模式可以分为**3**大类:  
+ **创建型模式** (***Creational Patterns***)  
+ **结构型模式** (***Structural Patterns***)  
+ **行为型模式** (***Behavioral Patterns***)

## 创建型模式  
创建型模式提供了一种**在创建对象的同时隐藏创建逻辑的方式**, 而不是使用 **new** 运算符直接实例化对象, 这使得程序在判断针对某个给定实例需要创建哪些对象时更加灵活,创建型模式包括以下**6**种: 
+ **简单工厂模式** (***Simple Factory Pattern***) 
+ **工厂模式** (***Factory Pattern***)  
+ **抽象工厂模式** (***Abstract Factory Pattern***)  
+ **建造者模式** (***Builder Pattern***)  
+ **单例模式** (***Singleton Pattern***)  
+ **原型模式** (***Prototype Pattern***)  

### 简单工厂模式  
#### 简介
简单工厂模式 又叫 静态方法模式, **实质是由一个工厂类根据传入的参数, 动态决定应该创建哪一个产品类的实例**, 这些产品类继承自一个父类或接口. 简单工厂模式的创建目标, 所有创建的对象都是充当这个角色的某个具体类的实例.  

其实就是**将一个具体类的实例化交给一个静态工厂方法来执行**, 现实中经常会用到, 思想非常简单.  
#### 结构  
简单工厂模式包含如下角色:  
+ **工厂角色(Factory)**  
+ **抽象产品角色(Product)**  
+ **具体产品角色(ConcreteProduct)**  

##### 工厂角色(Creator)  
工厂角色 是简单工厂模式的核心, 它负责实现创建所有具体产品类的实例. 工厂类可以被外界直接调用, 创建所需的产品对象.  
##### 抽象产品角色(Product)  
抽象产品角色 是所有具体产品角色的父类, 它负责描述所有实例所共有的公共接口.  
##### 具体产品角色(Concrete Product)  
具体产品角色 继承自 抽象产品角色, 一般为多个, 是简单工厂模式的创建目标, 工厂类返回的都是该角色的某一具体产品.  

#### 应用场景  
+ 客户如果只知道传入工厂类的参数, 对于如何创建对象的逻辑不关心时;  
+ 当工厂类负责创建的对象(具体产品)比较少时.  

#### 实例  
以水果加工厂的产线为背景, 具体思路如下:  
**建一家水果工厂 -> 建造各种水果加工产线 -> 产线生产**  
只需要切换需要加工水果的产线来加工水果即可.  

**工厂类定义** (***SimpleFactory.h***)  
```  
#ifndef _SIMPLEFACTORY_H_
#define _SIMPLEFACTORY_H_

#include <bits/stdc++.h>
#include "ProductionLine.h"
using namespace std;

class SimpleFactory {

public:

  enum TYPE {
    
    APPLE,
    BANANA,
    ORANGE,
    PEAR

  };

  static SimpleFactory* GetInstance();
  ProductionLine* CreateProLine(TYPE type);

private:

  SimpleFactory();

};

#endif  
```  

**工厂类实现** (***SimpleFactory.cpp***)  
```  
#include "SimpleFactory.h"

SimpleFactory::SimpleFactory() {
	
}

SimpleFactory* SimpleFactory::GetInstance() {

  static SimpleFactory factory;

  return &factory;

}

ProductionLine* SimpleFactory::CreateProLine(TYPE type) {

  ProductionLine* proLine;

  switch (type) {

    case APPLE: {

      proLine = new AppleLine();

    }break;
    case BANANA: {

      proLine = new BananaLine();

    }break;
    case ORANGE: {

      proLine = new OrangeLine();

    }break;
    case PEAR: {

       proLine = new PearLine();

    }

  }

  return proLine;

}  
```  
**产线类定义** (***ProductionLine.h***) 
```  
#ifndef _PRODUCTIONLINE_H_
#define _PRODUCTIONLINE_H_

#include <bits/stdc++.h>
using namespace std;

class ProductionLine {

public:

  ProductionLine();
  virtual ~ProductionLine();
  virtual void Product() = 0;

};

class AppleLine: public ProductionLine {

public:

  AppleLine();
  ~AppleLine();

  virtual void Product();

};

class BananaLine: public ProductionLine {

public:

  BananaLine();
  ~BananaLine();

  virtual void Product();

};

class OrangeLine: public ProductionLine {

public:

  OrangeLine();
  ~OrangeLine();

  virtual void Product();

};

class PearLine: public ProductionLine {

public:

  PearLine();
  ~PearLine();

  virtual void Product();

};

#endif  
```  
**产线类实现** (***ProductionLine.cpp***)  
```  
#include "ProductionLine.h"

ProductionLine::ProductionLine() {

}

ProductionLine::~ProductionLine() {
  
}

AppleLine::AppleLine() {

  cout << "Built AppleLine" << endl;

}

AppleLine::~AppleLine() {

  cout << "Destory AppleLine" << endl;

}

void AppleLine::Product() {

  cout << "Apple, Apple, Apple!!" << endl;

}

BananaLine::BananaLine() {

  cout << "Built BananaLine" << endl;

}

BananaLine::~BananaLine() {

  cout << "Destory BananaLine" << endl;

}

void BananaLine::Product() {

  cout << "Banana, Banana, Banana!!" << endl;

}

OrangeLine::OrangeLine() {

  cout << "Built OrangeLine" << endl;

}

OrangeLine::~OrangeLine() {

  cout << "Destory OrangeLine" << endl;

}

void OrangeLine::Product() {

  cout << "Orange, Orange, Orange!!" << endl;

}

PearLine::PearLine() {

  cout << "Built PearLine" << endl;

}

PearLine::~PearLine() {

  cout << "Destory PearLine" << endl;

}

void PearLine::Product() {

  cout << "Pear, Pear, Pear!!" << endl;

}  
```  
**应用** (***main.cpp***)  
```  
#include <bits/stdc++.h>
#include "ProductionLine.h"
#include "SimpleFactory.h"

using namespace std;

void Producting(ProductionLine* line) {

  if (line != nullptr) {
      line->Product();
  }

}

int main() {

  SimpleFactory* factory  = SimpleFactory::GetInstance();

  ProductionLine* line1 = factory->CreateProLine(SimpleFactory::APPLE);
  ProductionLine* line2 = factory->CreateProLine(SimpleFactory::BANANA);
  ProductionLine* line3 = factory->CreateProLine(SimpleFactory::ORANGE);
  ProductionLine* line4 = factory->CreateProLine(SimpleFactory::PEAR);

  Producting(line1);
  Producting(line2);
  Producting(line3);
  Producting(line4);
  
  return 0;

}  
```  
#### 优缺点  
##### 优点  
+ 将创建实例的工作与使用实例的工作分开, 使用者不必关心类对象如何创建, 实现了解耦.  
+ 把初始化实例时的工作放到工厂里进行, 使代码更容易维护. 更符合面向对象的原则 & 面向接口编程, 而不是面向实现编程.  

##### 缺点  
+ 工厂类集中了所有实例(产品)的创建逻辑, 一旦这个工厂不能正常工作, 整个系统都会受到影响.  
+ 违背 "开放-关闭原则", 一旦添加新产品就不得不修改工厂类的逻辑, 这样就会造成工厂逻辑过于复杂.  
+ 简单工厂模式由于使用了静态工厂方法, 静态方法不能被继承和重写, 会造成工厂角色无法形成基于继承的等级结构.  

***

### 工厂模式  
#### 简介  
简单工厂模式有一个问题就是: **类的创建依赖工厂类**, 也就是说, 如果想要拓展程序, 必须对工厂类进行修改, 这违背了**闭包原则**. 所以从设计角度考虑, 有一定的问题, 就需要用到 工厂模式 来解决. 创建一个 **工厂接口** 和 创建多个 **工厂实现类**, 这样一旦需要增加新的功能, 直接增加新的工厂类就可以了, 不需要修改之前的代码.  

工厂模式 也叫 **虚拟构造器模式** 或者 **多态工厂模式**, 它属于类创建型模式. 在工厂模式中, 工厂父类负责定义创建产品对象的公共接口, 而工厂子类则负责生成具体的产品对象, 这样做的目的是**将产品类的实例化操作延迟到工厂子类中完成, 即通过工厂子类来确定究竟应该实例化哪一个具体产品类.**  

#### 结构  
工厂模式 是 简单工厂模式 的进一步抽象和推广. 由于使用了面向对象的多态性, 工厂模式保持了简单工厂模式的优点, 而且克服了它的缺点. 在工厂模式中, 核心的工厂类不再负责所有产品的创建, 而是将具体创建工作交给了子类去做. 这个核心类仅仅负责给出具体工厂必须实现的接口, 而不负责哪一个产品类被实例化这种细节, 这使得工厂方法模式可以允许系统在不修改工厂角色的情况下引进新产品.  
工厂模式包含如下角色:  
+ **抽象产品角色(Product)**  
+ **具体产品角色(Concrete Product)**  
+ **抽象工厂角色(Creator)**  
+ **具体的工厂(Concrete Creator)**  

##### 抽象产品角色  
定义产品的接口. 是工厂方法模式所创建对象的超类型, 即产品对象的共同父类或接口   
##### 具体产品角色  
具体产品实现了抽象产品接口. 某种类型的具体产品由专门的具体工厂创建, 它们之间往往一一对应.   
##### 抽象工厂角色  
声明 工厂方法, 用于返回一个产品, 它是工厂方法模式的核心, 任何在模式中创建对象的工厂类都必须实现该接口.  
##### 具体的工厂  
具体工厂是抽象工厂类的子类, 实现了抽象工厂中定义的工厂方法, 并可由客户调用, 返回一个具体产品类的实例.  

#### 应用场景  
在以下情况下可以使用工厂模式:  
+ **一个类不知道它所需要的对象的类**, 在工厂方法模式中, 客户端不需要知道具体产品类的类名, 只需要知道所对应的工厂即可, 具体的产品对象由具体工厂类创建; 客户端需要知道创建具体产品的工厂类.  
+ **一个类通过其子类来指定创建哪个对象**, 在工厂方法模式中, 对于抽象工厂类只需要提供一个创建产品的接口, 而由其子类来确定具体要创建的对象, 利用 **面向对象的多态性** 和 **里氏代换原则**, 在程序运行时, 子类对象将覆盖父类对象, 从而使得系统更容易扩展. 将创建对象的任务委托给多个工厂子类中的某一个, 客户端在使用时可以无须关心是哪一个工厂子类创建产品子类, 需要时再动态指定, 可将具体工厂类的类名存储在配置文件或数据库中.

#### 实例  
还是以水果厂为背景, 具体思路如下:  
**创建父类工厂 -> 建造相应的水果工厂 -> 生产**  
只需要切换不同的水果工厂即可.  

**工厂类定义** (***Factory.h***)  
```  
#ifndef _FACTORY_H_  
#define _FACTORY_H_

#include <bits/stdc++.h>
#include "Fruit.h"
using namespace std;

class BaseFactory {

public:

  BaseFactory();
  virtual ~BaseFactory();

  virtual Fruit* Produce() = 0;

protected:

  Fruit* m_Fruit;

};

class AppleFactory: public BaseFactory {

public:

  AppleFactory();
  virtual ~AppleFactory() override;

  virtual Fruit* Produce() override;

};

class BananaFactory: public BaseFactory {

public:

  BananaFactory();
  virtual ~BananaFactory() override;

  virtual Fruit* Produce() override;

};

#endif
```  
**工厂类实现** (***Factory.cpp***)  
```  
#include "Factory.h"

using namespace std;

BaseFactory::BaseFactory() {

}

BaseFactory::~BaseFactory() {

  if (m_Fruit != nullptr) {
      delete m_Fruit;
      m_Fruit = nullptr;
  }

}

AppleFactory::AppleFactory() {

  cout << "Building AppleFactory" << endl;

}

AppleFactory::~AppleFactory() {

  cout << "Destory AppleFactory" << endl;

}

Fruit* AppleFactory::Produce() {

  m_Fruit = new Apple();

  return m_Fruit;

}

BananaFactory::BananaFactory() {

  cout << "Building BananaFactory" << endl;

}

BananaFactory::~BananaFactory() {

  cout << "Destory BananaFactory" << endl;

}

Fruit* BananaFactory::Produce() {

  m_Fruit = new Banana();

  return m_Fruit;

}  
```  
**水果类定义** (***Fruit.h***)  
```  
#ifndef _FRUIT_H_
#define _FRUIT_H_

#include <bits/stdc++.h>
using namespace std;

class Fruit {

public:

  Fruit();
  virtual ~Fruit();
  virtual void FruitType() = 0;

};

class Apple: public Fruit {

public:

  Apple();
  virtual ~Apple() override;
  virtual void FruitType() override;

};

class Banana: public Fruit {

public:

  Banana();
  virtual ~Banana() override;
  virtual void FruitType() override;

};

#endif  
```  
**水果类实现** (***Fruit.cpp***)  
```  
#include "Fruit.h"

Fruit::Fruit() {
	
}

Fruit::~Fruit() {
	
}

Apple::Apple() {
	
}

Apple::~Apple() {
	
}

Banana::Banana() {
	
}

Banana::~Banana() {
	
}

void Apple::FruitType() {

  cout << "Apple, Apple, Apple!" << endl;

}

void Banana::FruitType() {

  cout << "Banana, Banana, Banana!" << endl;

}  
```  
**应用** (***main.cpp***) 
```  
#include "Factory.h"
using namespace std;

int main() {

  BaseFactory* factory = new AppleFactory();
  factory->Produce()->FruitType();

  factory = new BananaFactory();
  factory->Produce()->FruitType();
  
  return 0;

}  
```  

#### 优缺点  
##### 优点  
在 工厂模式 中, 工厂方法用来创建客户所需要的产品, 同时还向客户隐藏了哪种具体产品类将被实例化这一细节, 用户只需要关心所需产品对应的工厂, 无须关心创建细节, 甚至无须知道具体产品类的类名.  

工厂模式之所以又被称为**多态工厂模式**, 是因为所有的具体工厂类都具有同一抽象父类, **基于工厂角色和产品角色的多态性设计是工厂模式的关键**. 它能够使工厂可以自主确定创建何种产品对象, 而如何创建这个对象的细节则完全封装在具体工厂内部.  

使用工厂模式的另一个优点是**在系统中加入新产品时, 无须修改抽象工厂和抽象产品提供的接口, 无须修改客户端, 也无须修改其他的具体工厂和具体产品, 而只要添加一个具体工厂和具体产品就可以了**. 这样, 系统的可扩展性也就变得非常好, 完全符合 "开闭原则".  

##### 缺点  
在添加新产品时, 需要编写新的具体产品类, 而且还要提供与之对应的具体工厂类, 系统中类的个数将成对增加, 在一定程度上增加了系统的复杂度, 有更多的类需要编译和运行, 会给系统带来一些额外的开销.  

由于考虑到系统的可扩展性, 需要引入抽象层, 在客户端代码中均使用抽象层进行定义, 增加了系统的抽象性和理解难度, 且在实现时可能需要用到DOM, 反射等技术, 增加了系统的实现难度.  

#### 工厂模式 和 简单工厂模式 的区别  
##### 简单工厂模式  
+ 工厂类负责创建的对象比较少, 由于创建的对象较少, 不会造成工厂方法中的业务逻辑太过复杂.  
+ 客户端只知道传入工厂类的参数, 对于如何创建对象并不关心.  

##### 工厂模式  
+ 客户端不知道它所需要的对象的类.  
+ 抽象工厂类通过其子类来指定创建哪个对象.  

***

### 抽象工厂模式  
#### 简介  
对于上面的水果工厂, 如果想再实现加工 橘子, 只需要把橘子加到你的水果工厂就足够了, 因为它们都是属于同一个品种, 都是水果, 但如果你想拓展业务, 需要生产蔬菜加工, 这时候工厂模式明显就不适合了, 因为工厂模式是对象都实现了同一接口, 这时候就需要使用抽象工厂模式.   

**抽象工厂模式 是所有形态的工厂模式中最为抽象和最具一般性的一种形态. 抽象工厂模式是指当有多个抽象角色时, 使用的一种工厂模式**.  

抽象工厂模式可以向客户端提供一个接口, 使客户端在不必指定产品的具体的情况下, 创建多个产品族中的产品对象.  

根据里氏替换原则, 任何接受父类型的地方, 都应当能够接受子类型. 因此, 实际上系统所需要的, 仅仅是类型与这些抽象产品角色相同的一些实例, 而不是这些抽象产品的实例. 换言之, 也就是这些抽象产品的具体子类的实例. 工厂类负责创建抽象产品的具体子类的实例.  

#### 结构  
抽象工厂模式的主要角色如下:  
+ **抽象工厂(Abstract Factory)**  
+ **具体工厂(Concrete Factory)**  
+ **抽象产品(Abstract Product)**  
+ **具体产品(Concrete Product)**  

##### 抽象工厂  
提供了创建产品的接口, 它包含多个创建产品的方法, 可以创建多个不同等级的产品.  
##### 具体工厂  
主要是实现抽象工厂中的多个抽象方法, 完成具体产品的创建.  
##### 抽象产品  
定义了产品的规范, 描述了产品的主要特性和功能, 抽象工厂模式有多个抽象产品.  
##### 具体产品  
实现了抽象产品角色所定义的接口, 由具体工厂来创建, 它同具体工厂之间是**多对一**的关系

#### 应用场景  
当一个对象都有相同的约束时, 可以使用抽象工厂模式. 比如: 这个工厂的几个产品都需要经过某些共同的步骤和打上相同的商标, 这一组产品可以在一个工厂里面生产, 减少很多重复的代码在不同的地方都出现多次.  

#### 实例  
**工厂类定义** (***AbstractFactory.h***)  
```  
#ifndef _ABSTRACTFACTORY_H_  
#define _ABSTRACTFACTORY_H_

#include "AbstractProduct.h"

class AbstractFactory {

public:

  AbstractFactory();
  virtual ~AbstractFactory();

  virtual AbstracProductA* CreateProductA() = 0;
  virtual AbstracProductB* CreateProductB() = 0;

};

class ConcreteFactory1: public AbstractFactory {

public:

  ConcreteFactory1();

  virtual AbstracProductA* CreateProductA() override;
  virtual AbstracProductB* CreateProductB() override;

};

class ConcreteFactory2: public AbstractFactory {

public:

  ConcreteFactory2();

  virtual AbstracProductA* CreateProductA() override;
  virtual AbstracProductB* CreateProductB() override;

};  

#endif  
```  

**工厂类实现** (***AbstractFactory.cpp***)  
```  
#include "AbstractFactory.h"  

AbstractFactory::AbstractFactory() {

}  

AbstractFactory::~AbstractFactory() {

}

ConcreteFactory1::ConcreteFactory1() {

    std::cout << "Create ConcreteFactory1 !" << std::endl;

}

AbstracProductA* ConcreteFactory1::CreateProductA() {

    std::cout << "Create ConcreteFactory1 -> CreateProductA" << std::endl;
    
    ConcreteProductA1* A = new ConcreteProductA1();

    return A;

}

AbstracProductB* ConcreteFactory1::CreateProductB() {

    std::cout << "Create ConcreteFactory1 -> CreateProductB" << std::endl;

    ConcreteProductB1* B = new ConcreteProductB1();

    return B;

}

ConcreteFactory2::ConcreteFactory2() {

    std::cout << "Create ConcreteFactory2 !" << std::endl;

}

AbstracProductA* ConcreteFactory2::CreateProductA() {

    std::cout << "Create ConcreteFactory2 -> CreateProductA" << std::endl;

    ConcreteProductA2* A = new ConcreteProductA2();

    return A;

}

AbstracProductB* ConcreteFactory2::CreateProductB() {

    std::cout << "Create ConcreteFactory2 -> CreateProductB" << std::endl;

    ConcreteProductB2* B = new ConcreteProductB2();

    return B;

}  
```  
**产品类定义** (***AbstractProduct.h***)  
```  
#ifndef _ABSTRACTPRODUCT_H_  
#define _ABSTRACTPRODUCT_H_

#include <bits/stdc++.h>
using namespace std;

class AbstracProductA {

public:

  AbstracProductA();
  virtual ~AbstracProductA();

  void Show();
  virtual string Name();

};

class ConcreteProductA1: public AbstracProductA {

public:

  ConcreteProductA1();
  string Name() override;

};

class ConcreteProductA2: public AbstracProductA {

public:

  ConcreteProductA2();
  string Name() override;

};

class AbstracProductB {

public:

  AbstracProductB();
  virtual ~AbstracProductB();

  void Show();
  virtual string Name();

};

class ConcreteProductB1: public AbstracProductB {

public:

  ConcreteProductB1();
  string Name() override;

};

class ConcreteProductB2: public AbstracProductB {

public:

  ConcreteProductB2();
  string Name() override;

};

#endif  
```  
**产品类实现** (***AbstractProduct.cpp***) 
```  
#include "AbstractProduct.h"  

AbstracProductA::AbstracProductA() {

}  

AbstracProductA::~AbstracProductA() {

}

string AbstracProductA::Name() {

    return "AbstracProductA";

}

void AbstracProductA::Show() {

    std::cout << Name() << std::endl;

}

ConcreteProductA1::ConcreteProductA1() {

}

string ConcreteProductA1::Name() {

    return "ProductA1";

}

ConcreteProductA2::ConcreteProductA2() {

}

string ConcreteProductA2::Name() {

    return "ProductA2";

}

AbstracProductB::AbstracProductB() {

}  

AbstracProductB::~AbstracProductB() {

}

string AbstracProductB::Name() {

    return "AbstracProductB";

}

void AbstracProductB::Show() {

    std::cout << Name() << std::endl;

}

ConcreteProductB1::ConcreteProductB1() {

}

string ConcreteProductB1::Name() {

    return "ProductB1";

}

ConcreteProductB2::ConcreteProductB2() {

}

string ConcreteProductB2::Name() {

    return "ProductB2";

}  
```  
**应用** (***main.cpp***) 
```  
#include "AbstractFactory.h"
#include "AbstractProduct.h"

using namespace std;

int main() {

  AbstractFactory* factory1 = new ConcreteFactory1();
  AbstracProductA* productA1 = factory1->CreateProductA();
  AbstracProductB* productB1 = factory1->CreateProductB();

  productA1->Show();
  productB1->Show();


  AbstractFactory* factory2 = new ConcreteFactory2();
  AbstracProductA* productA2 = factory2->CreateProductA();
  AbstracProductB* productB2 = factory2->CreateProductB();

  productA2->Show();
  productB2->Show();

  return 0;

}
```

#### 优缺点  
##### 优点  
+ 分离了具体的类  
+ 使得易于交换产品系列  
+ 有利于产品的一致性  

##### 缺点  
难以支持新种类的产品, **抽象方法模式的最大缺点就是产品族本身的扩展非常困难**, 如果在产品族中增加一个新的产品类型, 则需要修改多个接口, 并影响现已有的工厂类. 比如在上面的代码中, 你要在这个工厂创建第三个产品, 原本只是创建两个对象的, 哪么你就要在抽象方法中添加一个创建对象的方法, 那么所有实现了这个接口的类都要重新添加这个创建对象的方法, 这就是对之前的工厂有影响的原因.  

#### 抽象工厂模式, 工厂模式 和 简单工厂模式 的对比  
简单工厂模式无非是所有的东西都写在一个类里面, 要什么就调用什么, 如果要添加新的方法也是到这类里面添加, 代码很多, 看起来也是很乱, 就像一个大工厂, 什么都在里面, 扩展性很低.  

而工厂方法模式, 把说明的理论和生产的东西就分开一点; 抽象工厂模式是工厂方法模式的升级.  

+ **抽象工厂模式的定义**: 为创建一组相关或相互依赖的对象提供一个接口, 而且无需指定它们的具体类.  
+ **工厂模式的定义**: 为某个对象提供一个接口, 而且无需指定它们的具体类.  

两者都是**子类实现接口的方法, 并在子类写具体的代码**. 工厂模式中也是可以由多个抽象产品, 多个具体工厂, 具体产品类. **区别是在抽象工厂接口类中, 能创建几个产品对象, 抽象工厂模式的工厂能创建多个相关的产品对象, 而工厂方法模式的工厂只创建一个产品对象**.  

总结: **在工厂模式中弥补了简单工厂模式的缺陷(不符合开闭原则), 而在抽象工厂模式中弥补了工厂模式的不足(一个工厂只能生产一种产品类)**.  

***

### 建造者模式  
#### 简介  
建造者模式 也称 **生成器模式**, 将一个复杂对象的构建与它的表示分离, 使得同样的构建过程可以创建不同的表示(依赖倒转). 使用多个简单的对象一步一步构成一个复杂的对象, 这种类型的设计模式属于创建型模式, 它提供了一种创建对象的最佳方式.  

#### 结构  
建造者模式 主要有以下角色:  
+ **抽象建造者(Builder)**  
+ **具体建造者(Concrete Builder)**  
+ **指挥类(Director)**  
+ **产品类(Product)**  

##### 抽象建造者  
引入抽象建造者的目的**是为了将建造的具体过程交与它的子类来实现**, 这样更容易扩展. 一般至少会有两个抽象方法, 一个用来建造产品, 一个是用来返回产品.  
##### 具体建造者  
实现抽象类的所有未实现的方法, 具体来说一般是两项任务:  
+ **组建产品**  
+ **返回组建好的产品**  
##### 指挥类  
负责调用适当的建造者来组建产品, 指挥类一般不与产品类发生依赖关系, 与指挥类直接交互的是建造者类. 一般来说, 指挥类被用来封装程序中易变的部分.

#### 应用场景  
+ 创建复杂对象的算法独立于组成对象的部件  
+ 同一个创建过程需要有不同的内部表象的产品对象  

例如: 麦当劳的汉堡, 可乐, 薯条, 炸鸡翅等是不变的, 而其组合是经常变化的, 生成出所谓的"套餐".  

#### 实例  
**抽象建造者定义** (***Builder.h***)  
```  
#ifndef _BUILDER_H_
#define _BUILDER_H_

#include "product.h"

class Builder {

public:

  Builder();
  virtual ~Builder();

  virtual void AddPartA();
  virtual void AddPartB();

  Product* GetProduct();

protected:

  Product* m_Product;

};

#endif  
```  
**抽象建造者实现** (***Builder.cpp***)  
```  
#include "Builder.h"

Builder::Builder() {

  m_Product = new Product();

}

Builder::~Builder() {
  
}

void Builder::AddPartA() {

  m_Product->AddPart("PartA");

}

void Builder::AddPartB() {

  m_Product->AddPart("PartB");

}

Product* Builder::GetProduct() {

  if (m_Product->PartsCount() > 0){

    return m_Product;

  }else {

    cout << "No Proudct" <<endl;

  }

}
```  
**具体建造者定义** (***CocncreteBuilder.h***)  
```  
#ifndef _COCNCRETEBUILDER_H_
#define _COCNCRETEBUILDER_H_

#include "Builder.h"

class ConcreteBuilder1: public Builder {

public:

  ConcreteBuilder1();
  void AddPartA() override;
  void AddPartB() override;

};

class ConcreteBuilder2: public Builder {

public:

  ConcreteBuilder2();
  void AddPartA() override;
  void AddPartB() override;

};

#endif  
```  
**具体建造者实现** (***CocncreteBuilder.cpp***)  
```  
#include "CocncreteBuilder.h"

ConcreteBuilder1::ConcreteBuilder1() {

}

void ConcreteBuilder1::AddPartA() {

  m_Product->AddPart("Builder1 PartA");

}

void ConcreteBuilder1::AddPartB() {

  m_Product->AddPart("Builder1 PartB");

}

ConcreteBuilder2::ConcreteBuilder2() {

}

void ConcreteBuilder2::AddPartA() {

  m_Product->AddPart("Builder2 PartA");

}

void ConcreteBuilder2::AddPartB() {

  m_Product->AddPart("Builder2 PartB");

}  
```  
**指挥类定义** (***Director.h***)  
```  
#ifndef _DIRECTOR_H_
#define _DIRECTOR_H_

#include "Builder.h"

class Director {

public:

  Director();
  void Construct(Builder* builder);

};

#endif  
```  
**指挥类实现** (***Director.cpp***)  
```  
#include "Director.h"

Director::Director() {

}

void Director::Construct(Builder* builder) {

  builder->AddPartA();
  builder->AddPartB();

}  
```  
**产品类定义** (***Product.h***)  
```  
#ifndef _PRODUCT_H_
#define _PRODUCT_H_

#include <bits/stdc++.h>

using namespace std;

class Product {

public:

  Product();
  void AddPart(string part);
  void Show();

  int PartsCount();

private:

  list<string> m_Parts;

};

#endif
```  
**产品类实现** (***Product.cpp***)  
```  
#include "Product.h"

Product::Product() {

}

void Product::AddPart(string part) {

  m_Parts.push_back(part);

}

void Product::Show() {

  cout << "组成部分:" << endl;

  for (auto &i : m_Parts) {

    cout << i << endl;

  } 

}

int Product::PartsCount() {

  return m_Parts.size();

}  
```  
**应用** (***main.cpp***)  
```  
#include "CocncreteBuilder.h"
#include "Director.h"

int main() {

  shared_ptr<Builder> builder1 = make_shared<ConcreteBuilder1>();
  shared_ptr<Builder> builder2 = make_shared<ConcreteBuilder2>();

  Director director;
  director.Construct(builder1.get());

  Product* product1 = builder1->GetProduct();
  product1->Show();

  director.Construct(builder2.get());

  Product* product2 = builder2->GetProduct();
  product2->Show();

}  
```

#### 优缺点  
##### 优点  
+ **客户端不必知道产品内部组成的细节, 将产品本身与产品的创建过程解耦, 使得相同的创建过程可以创建不同的产品对象.**  
+ 每一个具体建造者都独立, 因此可以方便地替换具体建造者或增加新的具体建造者, **用户使用不同的具体建造者即可得到不同的产品对象**.  
+ 可以更加精细地控制产品的创建过程, 将复杂产品的创建步骤分解在不同的方法中, 使得创建过程更加清晰, 也更方便使用程序来控制创建过程.  
+ **增加新的具体建造者无须修改原有类库的代码, 指挥者类针对抽象建造者类编程, 系统扩展方便, 符合"开闭原则".**  

##### 缺点  
+ 当建造者过多时, 会产生很多类, 难以维护.  
+ 建造者模式所创建的产品一般具有较多的共同点, 其组成部分相似, 若产品之间的差异性很大, 则不适合使用该模式, 因此其使用范围受到一定限制.  
+ 若产品的内部变化复杂, 可能会导致需要定义很多具体建造者类来实现这种变化, 导致系统变得很庞大.  

#### 建造者模式 与 工厂模式 对比  
工厂模式 用于处理 "如何**获取**实例对象" 的问题; 
建造者模式 用于处理 "如何**建造**实例对象" 的问题.  

建造者模式 与 工厂模式 是极为相似的, 总体上 建造者模式 仅仅只比 工厂模式 多了一个"指挥类"的角色. 在建造者模式的类图中, 假如把这个指挥类看做是最终调用的客户端, 那么剩余的部分就可以看作是一个简单的工厂模式了.  

与工厂模式相比, **建造者模式一般用来创建更为复杂的对象**, 因为对象的创建过程更为复杂, 因此将对象的创建过程独立出来组成一个新的类 -- 指挥类. 也就是说, 工厂模式是将对象的全部创建过程封装在工厂类中, 由工厂类向客户端提供最终的产品. 而建造者模式中, 建造者类一般只提供产品类中各个组件的建造, 而将具体建造过程交付给指挥类. 由指挥类负责将各个组件按照特定的规则组建为产品, 然后将组建好的产品交付给客户端.  

***  

### 单例模式
#### 简介    
单例模式 是一种常用的软件设计模式, 其定义是单例对象的类只能允许一个实例存在. 许多时候整个系统只需要拥有一个的全局对象, 这样有利于我们协调系统整体的行为. 比如 Windows 是多进程多线程的, 在操作一个文件的时候, 就不可避免地出现多个进程或线程同时操作一个文件的现象, 所以所有文件的处理必须通过唯一的实例来进行.

#### 结构  
单例模式主要的角色:  
+ **单例类(Singleton)**  

##### 单例类  
单例类, 定义了一个 ***GetInstance*** 操作, 允许客户访问它的唯一实例. ***GetInstance*** 是一个静态方法, 主要负责创建自己的唯一实例.  

单例的实现主要是通过以下两个步骤:  
+ **将该类的构造方法定义为私有方法**, 这样其他处的代码就无法通过调用该类的构造方法来实例化该类的对象, 只有通过该类提供的静态方法来得到该类的唯一实例.  
+ 在该类内提供一个静态方法, 当我们调用这个方法时, 如果类持有的引用不为空就返回这个引用, 如果类保持的引用为空就创建该类的实例并将实例的引用赋予该类保持的引用.  

#### 应用场景  
在我们的 windows 桌面上, 我们打开一个回收站, 当我们试图再次打开一个新的回收站时, Windows 系统并不会为你弹出一个新的回收站窗口, 也就是说在整个系统运行的过程中, 系统只维护一个回收站的实例, 这就是一个典型的单例模式运用.  
适用场景:  
+ 需要生成唯一序列的环境  
+ 需要频繁实例化然后销毁的对象  
+ 创建对象时耗时过多或者耗资源过多, 但又经常用到的对象.  
+ 方便资源相互通信的环境  

#### 实例  
windows 的画图工具,画布可以多个, 画笔是单例, 我们可以改变画笔的颜色和粗细, 但是我们用来画出图案的画笔实例永远是同一个, 如下我们可以设计一个简单的画笔单例.  
**画笔单例类定义** (***Singleton.h***)  
```  
#ifndef _SINGLETON_H_  
#define _SINGLETON_H_

#include "Properties.h"

class Pen {

public:

  static Pen* getInstance();

  void setProperty(const Properties& proty);
  string draw();

private:

  Pen();

  Properties m_Property;

};

#endif  
```  
**画笔单例类实现** (***Singleton.cpp***)
```  
#include "Singleton.h"

Pen* Pen::getInstance() {

  static Pen sig;
  return &sig;

}

void Pen::setProperty(const Properties &proty) {

  m_Property = proty;

}

string Pen::draw() {

  return m_Property.p_Shape;

}

Pen::Pen() {
  
}  
```  
**画笔属性类定义** (***Properties.h***)  
```  
#ifndef _PROPERTIES_H_  
#define _PROPERTIES_H_

#include <bits/stdc++.h>
using namespace std;

struct Properties {

  string p_Color;
  int    p_Width;
  string p_Shape;

};

#endif
```  
**画布类定义** (***Draw.h***)  
```  
#ifndef _DRAW_H_  
#define _DRAW_H_

#include <bits/stdc++.h>
using namespace std;

class Draw {

public:

  void show();
  Draw(string name);
  void setPan();

private:

  string m_Name;

};

#endif  
```  
**画布类实现** (***Draw.cpp***)  
```  
#include "Draw.h"

Draw::Draw(string name) {

  m_Name = name;

}

void Draw::show() {

  cout << m_Name << ":" << endl;

  for (int n = 0; n < 20; n++) {

    for (int m = 0; m < n; m++) {

      cout << "* ";

    }

    cout << endl;

  }

}  
```  
**应用** (***main.cpp***) 
```  
#include "Singleton.h"
#include "Properties.h"
#include "Draw.h"

int main() {

    Pen* sig = Pen::getInstance();

    Properties proty;
    proty.p_Color = "red";
    proty.p_Width = 65;
    proty.p_Shape = "*";

    sig->setProperty(proty);

    thread t1([]() {

      Draw d1("Draw1");
      d1.show();

    });

    thread t2([]() {

      Draw d2("Draw2");
      d2.show();

    });

    thread t3([]() {

      Draw d3("Draw3");
      d3.show();

    });

    t1.join();
    t2.join();
    t3.join();

}  
```

#### 优缺点  
##### 优点  
+ 在内存中只有一个对象, 节省内存空间.  
+ 避免频繁的创建销毁对象, 可以提高性能.  
+ 避免对共享资源的多重占用, 简化访问.  
+ 为整个系统提供一个全局访问点.  

##### 缺点  
+ 不适用于变化频繁的对象.  
+ 滥用单例将带来一些负面问题, 如为了节省资源将数据库连接池对象设计为的单例类, 可能会导致共享连接池对象的程序过多而出现连接池溢出.  
+ 如果实例化的对象长时间不被利用, 系统会认为该对象是垃圾而被回收, 这可能会导致对象状态的丢失.

#### 总结  
单例模式有四种比较经典的实现方法:  
+ **饿汉式**  
+ **懒汉式**  
+ **双重加锁机制**  
+ **静态初始化**  

##### 饿汉式  
**是否 Lazy 初始化:** 否  
**是否多线程安全:** 是
**实现难度:** 易  
**描述:** 这种方式比较常用, 但容易产生垃圾对象.  
**优点:** 没有加锁, 执行效率会提高.  
**缺点:** 类加载时就初始化, 如果从始至终从未使用过这个实例, 浪费内存.  
```  
class Singleton {

private:

    Singleton() {

        singleton = new Singleton();

    };

    static Singleton* singleton;

public:

    static Singleton* getSingleton() {

        return singleton;

    }

};  
```  
##### 懒汉式  
**是否 Lazy 初始化:** 是  
**是否多线程安全:** 是
**实现难度:** 易  
**描述:** 这种方式具备很好的 lazy loading, 能够在多线程中很好的工作. 但是效率低, 99%情况下不需要同步.  
**优点:** 第一次调用才初始化, 避免内存浪费.  
**缺点:** 必须加锁 synchronized 才能保证单例, 但加锁会影响效率.  
```  
class Singleton {

private:

    Singleton() {};

    static Singleton* singleton;

public:

    static Singleton* getSingleton() {

        if (singleton == nullptr) {
            singleton = new Singleton();
        }

        return singleton;

    }

};  
```  
##### 双重加锁机制  
**是否 Lazy 初始化:** 是  
**是否多线程安全:** 是
**实现难度:** 较复杂  
**描述:** 这种方式采用双锁机制, 安全且在多线程情况下能保持高性能.
**优点:** 线程安全, 延迟加载, 效率较高.  
```  
std::mutex mlock;

class Singleton {

private:

    Singleton() {};
    static Singleton* singleton;

public:

    static Singleton* getSingleton() {

        if (singleton == nullptr) {

            mlock.lock();

            if (singleton == nullptr) {
                singleton = new Singleton();
                mlock.unlock();
                return singleton;
            }

            mlock.unlock();
            return singleton;

        }

        return singleton;

    }

};  
```  
##### 静态初始化  
**是否 Lazy 初始化:** 是  
**是否多线程安全:** 是
**实现难度:** 一般  
**描述:** 这种方法不需要开发人员显式地编写线程安全代码, 即可解决多线程环境下它是不安全的问题.  
```  
class Singleton {

private:

    Singleton() {};

public:

    static Singleton* getSingleton() {

        static Singleton singleton2;
        return &singleton2;

    }

};  
```

单例模式的实现方法还有很多, 这四种是比较经典的实现, 从这四种实现中, 可以总结出, 要想实现效率高的线程安全的单例, 必须注意以下两点:  
+ 尽量减少同步块的作用域  
+ 尽量使用细粒度的锁  

***

### 原型模式
#### 简介  
原型模式 是用于创建重复的对象, 同时又能保证性能, 它提供了一种创建对象的最佳方式. 这种模式是**实现了一个原型接口, 该接口用于创建当前对象的克隆. 当直接创建对象的代价比较大时, 则采用这种模式**. 例如, 一个对象需要在一个高代价的数据库操作之后被创建, 我们可以缓存该对象, 在下一个请求时返回它的克隆, 在需要的时候更新数据库, 以此来减少数据库调用. 由于它直接操作内存中的**二进制流**, 当大量操作或操作复杂对象时, 性能优势将会很明显.  

#### 结构  
原型模式 主要有以下这几个角色:  
+ **原型类(Prototype)**  
+ **具体原型类(Concrete Prototye)**  

##### 原型类  
声明一个克隆自身的接口, 里面的关键方法就是 ***clone***() 方法  
##### 具体原型类  
实现一个克隆自身的操作  

#### 应用场景  
多用于创建大对象, 或初始化繁琐的对象. 如游戏中的背景, 地图; Web中的画布等等. 以下场景适用:  
+ **资源优化场景**
  类初始化需要消耗非常多的资源, 这个资源包括数据, 硬件资源等;  
+ **性能和安全要求的场景**
  通过 **new** 产生一个对象需要非常繁琐的数据准备或访问权限, 则可以使用原型模式;  
+ **一个对象多个修改者的场景**
  一个对象需要提供给其他对象访问, 而且各个调用者可能都需要修改其值时, 可以考虑使用原型模式拷贝多个对象供调用者使用.  

#### 实例  
以用户数据复制来举例, 代码如下:  

**原型类和具体原型类定义** (***Prototype.h***)
```  
#ifndef _PROTOTYPE_H_
#define _PROTOTYPE_H_

#include <bits/stdc++.h>
using namespace std;

class Prototype {

public:

  Prototype();
  virtual ~Prototype();
  virtual Prototype* clone();

  string GetName();
  void SetName(string name);

  int GetAge();
  void SetAge(int* age);

protected:

  string  m_Name;
  int* m_Age;

};

class ConcretePrototype: public Prototype {

public:

  ConcretePrototype();
  virtual ~ConcretePrototype();
  virtual Prototype* clone() override;

};

#endif  
```  
**原型类和具体原型类实现** (***Prototype.cpp***)  
```  
#include "Prototype.h"

Prototype::Prototype() {

}

Prototype::~Prototype() {

}

Prototype* Prototype::clone() {

  return new Prototype(*this);

}

string Prototype::GetName() {

  return m_Name;

}

void Prototype::SetName(string name) {

  m_Name = name;

}

int Prototype::GetAge() {

  return *m_Age;

}

void Prototype::SetAge(int* age) {

  m_Age = age;

}

ConcretePrototype::ConcretePrototype() {

}

ConcretePrototype::~ConcretePrototype() {

}

Prototype* ConcretePrototype::clone() {

  return new ConcretePrototype(*this);

}
```
**应用** (***main.cpp***)  
```  
#include "Prototype.h"

#define DELETEOBJECT(x) if (x != nullptr) { delete x; x = nullptr;}

int main() {

  int age1 = 25;
  Prototype* proto1 = new Prototype();
  proto1->SetAge(&age1);
  proto1->SetName("小明");

  cout << proto1->GetName() << endl;
  cout << proto1->GetAge() << endl;

  int age2 = 35;
  Prototype* proto2 = proto1->clone();
  proto2->SetAge(&age2);
  proto2->SetName("小红");

  cout << proto2->GetName() << endl;
  cout << proto2->GetAge() << endl; 

  DELETEOBJECT(proto1);
  DELETEOBJECT(proto2);

}  
```  
#### 优缺点  
##### 优点  
+ **相对于 new 创建新对象**
  首先, 用 **new** 新建对象不能获取当前对象运行时的状态, 其次就算 **new** 了新对象, 在将当前对象的值复制给新对象, 效率也不如原型模式高.
+ **相对于拷贝构造函数**  
  原型模式与拷贝构造函数是不同的概念, 拷贝构造函数涉及的类是已知的, 原型模式涉及的类可以是未知的. 原型模式生成的新对象可能是一个派生类, 拷贝构造函数生成的新对象只能是类本身. 原型模式是描述了一个通用方法(或概念), 它不管是如何实现的, 而拷贝构造则是描述了一个具体实现方法.  

##### 缺点  
+ 配备克隆方法需要对类的功能进行通盘考虑, 这对于全新的类不是很难, 但对于已有的类不一定很容易, 特别当一个类引用不支持串行化的间接对象, 或者引用含有循环结构的时候.  
+ 实现原型模式每个派生类都必须实现 ***Clone*** 接口.  
+ 逃避构造函数的约束.  

#### 总结  
原型模式通过 **Object** 的 ***Clone*** 方法实现, 由于是内存操作, 无视构造方法和访问权限, 直接获取新的对象. 但对于引用类型, 需使用深拷贝, 其它浅拷贝即可.

***

## 结构型模式  
结构型模式 关注类和对象的组合. 继承的概念被用来组合接口和定义组合对象获得新功能的方式. 结构型模式包括以下**7**种:  
+ **组合模式** (***Composite Pattern***)  
+ **适配器模式** (***Adapter Pattern***)  
+ **装饰模式** (***Decorator Pattern***)  
+ **外观模式** (***Facade Pattern***)  
+ **享元模式** (***Flyweight Pattern***)  
+ **代理模式** (***Proxy Pattern***)  
+ **桥接模式** (***Bridge Pattern***)  

### 组合模式 
#### 简介   
组合模式, 又叫 **部分整体模式**, 是用于把一组相似的对象当作一个单一的对象. 组合模式依据树形结构来组合对象, 用来表示部分以及整体层次. 这种类型的设计模式属于结构型模式, 它创建了对象组的树形结构.  

这种模式创建了一个包含自己对象组的类, 该类提供了修改相同对象组的方式.
#### 结构  
组合模式 主要有以下角色:  
+ **Component**  
+ **Leaf**  
+ **Composite**  
+ **Client**  

##### Component  
组合中的对象声明接口, 在适当情况下实现所有类共有的默认行为, **声明一个接口用于访问和管理Component的子组件**. 在递归结构中定义一个接口, 用于访问一个父部件, 并在合适的情况下实现它.  
##### Leaf  
在组合中表示叶节点, 叶节点没有子节点, 定义对象的基本行为.  
##### Composite  
定义有子部件的那些部件的行为, 存储子部件并在Component接口实现与子部件有关的操作.  
##### Client  
通过Component接口操作组合部件的对象  

#### 应用场景  
+ 想表示对象的部分-整体层次结构(树形结构)  
+ 希望用户忽略组合对象与单个对象的不同, 用户将统一地使用组合结构中的所有对象.  

比如 树形菜单, 文件, 文件夹的管理

#### 实例  
以一个大型公司为需求背景, 组织我们的代码. 一个北京总公司, 下面有深圳和上海两个分公司, 然后每个公司不管是总公司还是分公司都有自己的人力资源部, IT部和市场部.  
**Component类定义** (***Component.h***)  
```  
#ifndef _COMPONENT_H_
#define _COMPONENT_H_

#include <bits/stdc++.h>
using namespace std;

class Component {

public:

  Component(string name);
  virtual ~Component();

  virtual void add(Component* com, int depth = 0) = 0;
  virtual void remove(Component* com) = 0;
  virtual void display() = 0;
  virtual void LineOfDuty() = 0;

  string name();
  void setDepth(int depth);
  int depth();

private:

  string m_Name;
  int m_Depth;

};


class ConcreteCompany: public Component {

public:

  ConcreteCompany(string name);
  virtual ~ConcreteCompany();

  virtual void add(Component* com, int depth = 0);
  virtual void remove(Component* com);
  virtual void display();
  virtual void LineOfDuty();

private:

  list<Component*> m_ChildCom;

};

class HumanResoure: public Component {

public:

  HumanResoure(string name);
  virtual ~HumanResoure();

  virtual void add(Component* com, int depth = 0);
  virtual void remove(Component* com);
  virtual void display();
  virtual void LineOfDuty();

};

class IT: public Component {

public:

  IT(string name);
  virtual ~IT();

  virtual void add(Component* com, int depth = 0);
  virtual void remove(Component* com);
  virtual void display();
  virtual void LineOfDuty();

};

class Marketing: public Component {

public:

  Marketing(string name);
  virtual ~Marketing();

  virtual void add(Component* com, int depth = 0);
  virtual void remove(Component* com);
  virtual void display();
  virtual void LineOfDuty();

};

#endif  
```  
**Component类实现** (***Component.cpp***)  
```  
#include "Component.h"

Component::Component(string name) 
         : m_Name(name) {

}

Component::~Component() {

}

string Component::name() {

    return m_Name;

}

void Component::setDepth(int depth) {

    m_Depth = depth;

}

int Component::depth() {

    return m_Depth;

}

ConcreteCompany::ConcreteCompany(string name)
               : Component(name) {

}

ConcreteCompany::~ConcreteCompany() {

    for (auto &com : m_ChildCom) {

        if (com != nullptr) {
            com = nullptr;
        }

    }

    m_ChildCom.clear();

}

void ConcreteCompany::add(Component* com, int depth) {

    com->setDepth(depth);
    m_ChildCom.push_back(com);

}

void ConcreteCompany::remove(Component* com) {

    m_ChildCom.remove(com);

}

void ConcreteCompany::display() {

    string str;

    for (int n = 0; n < depth(); n++) {

        str += "--";

    }

    cout << str << " " << name() << endl;

    for (auto &com : m_ChildCom) {

        com->display();

    }

}

void ConcreteCompany::LineOfDuty() {

}

HumanResoure::HumanResoure(string name)
            : Component(name) {

}

HumanResoure::~HumanResoure() {

}

void HumanResoure::add(Component* com, int depth) {

    com->setDepth(depth);

}

void HumanResoure::remove(Component* com) {

}

void HumanResoure::display() {

    string str;

    for (int n = 0; n < depth(); n++) {

        str += "--";

    }

    cout << str << " " << name() << endl;

}

void HumanResoure::LineOfDuty() {

    cout << "人力资源部, 负责招聘" << endl;

}

IT::IT(string name)
  : Component(name) {

}

IT::~IT() {

}

void IT::add(Component* com, int depth) {

    com->setDepth(depth);

}

void IT::remove(Component* com) {

}

void IT::display() {

    string str;

    for (int n = 0; n < depth(); n++) {

        str += "--";

    }

    cout << str << " " << name() << endl;

}

void IT::LineOfDuty() {

    cout << "IT部门, 负责写代码" << endl;

}

Marketing::Marketing(string name)
         : Component(name) {

}

Marketing::~Marketing() {

}

void Marketing::add(Component* com, int depth) {

    com->setDepth(depth);

}

void Marketing::remove(Component* com) {

}

void Marketing::display() {

    string str;

    for (int n = 0; n < depth(); n++) {

        str += "--";

    }

    cout << str << " " << name() << endl;

}

void Marketing::LineOfDuty() {

    cout << "市场部门, 负责招聘" << endl;

}  
```  
**应用** (***main.cpp***)  
```  
#include "Component.h"

int main() {

    Component* root = new ConcreteCompany("北京总部");
    root->setDepth(0);
    root->add(new HumanResoure("人力资源部门"), 1);
    root->add(new IT("IT部门"), 1);
    root->add(new Marketing("市场部门"), 1);

    Component* sz = new ConcreteCompany("深圳子公司");
    sz->add(new HumanResoure("人力资源部门"), 2);
    sz->add(new IT("IT部门"), 2);
    sz->add(new Marketing("市场部门"), 2);
    root->add(sz, 1);

    Component* sh = new ConcreteCompany("上海子公司");
    sh->add(new HumanResoure("人力资源部门"), 2);
    sh->add(new IT("IT部门"), 2);
    sh->add(new Marketing("市场部门"), 2);
    root->add(sh, 1);

    root->display();

}  
```  
#### 优缺点  
##### 优点  
+ 使客户端调用简单, 客户端可以一致的使用组合结构或其中单个对象, 用户就不必关系自己处理的是单个对象还是整个组合结构, 这就简化了客户端代码.  
+ 更容易在组合体内加入对象部件, 客户端不必因为加入了新的对象部件而更改代码.  

##### 缺点  
+ 在使用组合模式时, 其叶子和树枝的声明都是实现类, 而不是接口, 违反了依赖倒置原则.

***  

### 适配器模式
#### 简介  
适配器模式 是作为两个不兼容的接口之间的桥梁. 这种类型的设计模式属于结构型模式, 它结合了两个独立接口的功能. 这种模式涉及到一个单一的类, 该类负责加入独立的或不兼容的接口功能. 比如 读卡器是作为内存卡和笔记本之间的适配器, 将内存卡插入读卡器, 再将读卡器插入笔记本, 这样就可以通过笔记本来读取内存卡.  

#### 结构  
适配器模式 主要有以下角色:  
+ **初始角色(Adaptee)**  
+ **目标角色(Target)**  
+ **适配器角色(Adapter)**  

##### 初始角色  
实现了我们想要使用的功能, 但是接口不匹配  
##### 目标角色  
定义了客户端期望的接口  
##### 适配器角色  
实现了目标接口, 实现目标接口的方法是: **内部包含一个Adaptee的对象, 通过这个对象调用Adaptee的原有方法实现目标接口**.  

#### 应用场景  
+ 系统需要复用现有类, 而该类的接口不符合系统的需求.  
+ 想要建立一个可重复使用的类, 用于与一些彼此之间没有太大关联的一些类, 包括一些可能在将来引进的类一起工作.  
+ 对于对象适配器模式, 在设计里需要改变多个已有子类的接口, 如果使用类的适配器模式, 就要针对每一个子类做一个适配器, 而这不太实际.  

#### 实例  
**初始角色类定义** (***Adaptee.h***)  
```  
#ifndef _ADAPTEE_H_  
#define _ADAPTEE_H_

#include <bits/stdc++.h>
using namespace std;

class Adaptee {

public:

  Adaptee();
  void SpecialRequest();

};

#endif  
```  
**初始角色类实现** (***Adaptee.cpp***)  
```  
#include "Adaptee.h"

Adaptee::Adaptee() {

}

void Adaptee::SpecialRequest() {

  cout << __FUNCTION__ << endl;

}  
```  
**适配器类定义** (***Adapter.h***)  
```  
#ifndef _ADAPTER_H_
#define _ADAPTER_H_

#include <bits/stdc++.h>
#include "Adaptee.h"
using namespace std;

class Target {

public:

  Target();
  virtual ~Target();

  virtual void Request();

};

class Target1: public Target {

public:

  Target1();
  virtual ~Target1() override;
  virtual void Request() override;

};

class Adapter: public Target {

public:

  Adapter(Adaptee* adaptee);
  ~Adapter();

  void Request();
  Adaptee* m_Adaptee;

};

#endif
```  
**适配器类实现** (***Adapter.cpp***)  
```  
#include "Adapter.h"

Adapter::Adapter(Adaptee* adaptee) {

    m_Adaptee = adaptee;

}

Adapter::~Adapter() {

  if (m_Adaptee != nullptr) {
      delete m_Adaptee;
      m_Adaptee = nullptr;
  }

}

void Adapter::Request() {

    m_Adaptee->SpecialRequest();

}

void Target::Request() {

}

void Target1::Request() {

    cout << "普通请求" <<endl;

}

Target::Target() {

}

Target::~Target() {
    
}

Target1::Target1() {

}

Target1::~Target1() {

}  
```  
**应用** (***main.cpp***)  
```  
#include "Adapter.h"

int main() {

    Target* target = new Target1();
    target->Request();

    Adaptee* adaptee = new Adaptee();
    Target* target1 = new Adapter(adaptee);
    target1->Request();

}  
```  

#### 优缺点  
##### 优点  
+ 可以让任何两个没有关联的类一起运行.  
+ 可以在不修改原有代码的基础上来复用现有类, 很好地符合"开闭原则".   
+ 增加了类的透明度和更好的灵活性.  

##### 缺点  
+ 采用了类和接口的“双继承”实现方式, 带来了不良的高耦合.  

***  

### 装饰模式  
#### 简介  
装饰器模式, 又名**包装模式(Wrapper)**, 装饰器模式以对客户端透明的方式拓展对象的功能, 是继承关系的一种替代方案. 允许向一个现有的对象添加新的功能, 同时又不改变其结构. 这种类型的设计模式属于结构型模式, 它是作为现有的类的一个包装. 这种模式创建了一个装饰类, 用来包装原有的类, 并在保持类方法签名完整性的前提下, 提供了额外的功能. 装饰模式实际上是一直提倡的**组合代替继承**的实践方式, 面向对象的特性有继承与封装, 但两者却又有一点矛盾, 继承意味子类依赖了父类中的实现, 一旦父类中改变实现则会对子类造成影响, 这是打破了封装性的一种表现, 而组合就是巧用封装性来实现继承功能的代码复用.  

#### 结构  
装饰模式 包括以下主要角色:  
+ **抽象构件角色(Component)**  
+ **具体构件角色(Concrete Component)**  
+ **抽象装饰角色(Decorator)**  
+ **具体装饰角色(Concrete Decorator)**  

##### 抽象构件角色  
给出一个抽象接口, 已规范准备接收附加责任的对象.  
##### 具体构件角色  
定义一个将要接收附加责任的类  
##### 抽象装饰角色  
持有一个构件对象的实例, 并定义一个与抽象构件接口一致的接口.  
##### 具体装饰角色  
负责给构件对象 "贴上" 附加的责任  

#### 应用场景  
+ 需要扩展一个类的功能或给一个类增加附加责任.  
+ 需要动态地给一个对象增加功能, 这些功能可以再动态地撤销.  
+ 需要增加由一些基本功能的排列组合而产生的非常大量的功能.  

#### 实例  
以王者荣耀的星元皮肤为例, 在皮肤模块里面我们可以自定义英雄的皮肤组合方式, 从而装饰出来独一无二的的装扮.  
**抽象构件角色/具体构件角色类定义** (***DecoratorSkin.h***)  
```  
#ifndef _HERO_H_
#define _HERO_H_

#include <bits/stdc++.h>
using namespace std;

class Hero {

public:

  Hero() {}
  virtual ~Hero() {}

  //造型
  virtual void Modeling() = 0;

};

class Xiangxiang: public Hero {

public:

  Xiangxiang();
  virtual ~Xiangxiang() override;
  virtual void Modeling() override;

};

#endif
```  
**具体构件角色类实现** (***Hero.cpp***)  
```  
#include "Hero.h"

Xiangxiang::Xiangxiang() {

  cout << "大小姐驾到，通通闪开~" << endl;

}

Xiangxiang::~Xiangxiang() {

}

void Xiangxiang::Modeling() {

  cout << "默认经典皮肤" << endl;

}  
```  
**抽象装饰角色/具体装饰角色类定义** (***Adapter.h***)  
```  
#ifndef _DECORATORSKIN_H_  
#define _DECORATORSKIN_H_

#include "Hero.h"

class DecoratorSkin: public Hero {

public:

  DecoratorSkin(Hero* hero = nullptr);
  virtual ~DecoratorSkin() override;
  virtual void Modeling() override;

  virtual void UpdateModeling();
  void SetHero(Hero* hero);

protected:

  Hero* m_Hero;

};

class DecoratorGuns: public DecoratorSkin {

public:

  DecoratorGuns(Hero* hero = nullptr);
  virtual ~DecoratorGuns() override;
  virtual void Modeling() override;

  void UpdateGuns1();
  void UpdateGuns2();

};

class DecoratorHairstyle: public DecoratorSkin {

public:

  DecoratorHairstyle(Hero* hero = nullptr);
  virtual ~DecoratorHairstyle() override;
  virtual void Modeling() override;

  void UpdateHairstyle1();
  void UpdateHairstyle2();

};

class DecoratorWindbreaker: public DecoratorSkin {

public:

  DecoratorWindbreaker(Hero* hero = nullptr);
  virtual ~DecoratorWindbreaker() override;
  virtual void Modeling() override;

  void UpdateWindbreaker1();
  void UpdateWindbreaker2();

};

class DecoratorLongBoots: public DecoratorSkin {

public:

  DecoratorLongBoots(Hero* hero = nullptr);
  virtual ~DecoratorLongBoots() override;
  virtual void Modeling() override;

  void UpdateLongBoots1();
  void UpdateLongBoots2();

};

#endif  
```
**抽象装饰角色/具体装饰角色类实现** (***DecoratorSkin.h***)  
```
#include "DecoratorSkin.h"

DecoratorSkin::DecoratorSkin(Hero* hero)
             : m_Hero(hero) {

}

DecoratorSkin::~DecoratorSkin() {

}

void DecoratorSkin::Modeling() {

  if (m_Hero) {
      m_Hero->Modeling();
  }

}

void DecoratorSkin::UpdateModeling() {

}

void DecoratorSkin::SetHero(Hero* hero) {

  if (m_Hero == hero) {
      return;
  }

  m_Hero = hero;

}

DecoratorGuns::DecoratorGuns(Hero* hero) 
             : DecoratorSkin(hero) {

}

DecoratorGuns::~DecoratorGuns() {

}

void DecoratorGuns::Modeling() {

  m_Hero->Modeling();
  UpdateGuns1();

}

void DecoratorGuns::UpdateGuns1() {

  cout << "换上了大炮1, 更酷了" << endl;

}

void DecoratorGuns::UpdateGuns2() {

  cout << "换上了大炮2, 更酷了" << endl;

}

DecoratorWindbreaker::DecoratorWindbreaker(Hero* hero) {

}

DecoratorWindbreaker::~DecoratorWindbreaker() {

}

void DecoratorWindbreaker::Modeling() {

  m_Hero->Modeling();
  UpdateWindbreaker1();

}

void DecoratorWindbreaker::UpdateWindbreaker1() {

  cout << "换上了风衣1, 更拉风了" << endl;

}

void DecoratorWindbreaker::UpdateWindbreaker2() {

  cout << "换上了风衣2, 更拉风了" << endl;

}

DecoratorLongBoots::DecoratorLongBoots(Hero* hero) {

}

DecoratorLongBoots::~DecoratorLongBoots() {

}

void DecoratorLongBoots::Modeling() {

  m_Hero->Modeling();
  UpdateLongBoots1();

}

void DecoratorLongBoots::UpdateLongBoots1() {

  cout << "换了长筒靴1, 更高了" << endl;

}

void DecoratorLongBoots::UpdateLongBoots2() {

  cout << "换了长筒靴2, 更高了" << endl;

}

DecoratorHairstyle::DecoratorHairstyle(Hero* hero) {

}

DecoratorHairstyle::~DecoratorHairstyle() {

}

void DecoratorHairstyle::Modeling() {

  m_Hero->Modeling();
  UpdateHairstyle1();

}

void DecoratorHairstyle::UpdateHairstyle1() {

  cout << "换了发型1, 更好看了" << endl;

}

void DecoratorHairstyle::UpdateHairstyle2() {

  cout << "换了发型2, 更好看了" << endl;

}  
```
**应用** (***main.cpp***) 
```  
#include "Hero.h"
#include "DecoratorSkin.h"

int main() {

  Hero* xiangxiang = new Xiangxiang();

  DecoratorSkin* dec1 = new DecoratorHairstyle();
  DecoratorSkin* dec2 = new DecoratorGuns();
  DecoratorSkin* dec3 = new DecoratorWindbreaker();
  DecoratorSkin* dec4 = new DecoratorLongBoots();

  dec1->SetHero(xiangxiang);
  dec2->SetHero(dec1);
  dec3->SetHero(dec2);
  dec4->SetHero(dec3);

  dec4->Modeling();

}  
```

#### 优缺点  
##### 优点  
+ 装饰这模式和继承的目的都是扩展对象的功能, 但装饰者模式比继承更灵活.  
+ 通过使用不同的具体装饰类以及这些类的排列组合, 设计师可以创造出很多不同行为的组合.  
+ 装饰者模式有很好地可扩展性.  

##### 缺点  
+ 装饰者模式会导致设计中出现许多小对象, 如果过度使用, 会让程序变的更复杂. 并且更多的对象会是的差错变得困难, 特别是这些对象看上去都很像.  

#### 总结  
装饰者模式本质上来说是AOP思想的一种实现方式, 其持有被装饰者, 因此可以控制被装饰者的行为从而达到了AOP的效果.  
**要点:**  
+ 继承属于扩展形式一种, 但不见的是达到弹性设计的最佳方式, 组合优于继承.  
+ 应该允许行为可以被拓展, 而无需修改现有的代码.  
+ 装饰者模式意味着一群装饰者类, 这些类用来包装具体组件.  
+ 装饰者类反映出被装饰组件类型.  
+ 可以使用无数个装饰者包装一个组件.  
+ 装饰者会导致设计中出现许多小对象, 如果过度使用, 会让程序变得很复杂.  

***  

### 外观模式 
#### 简介 
外观模式 隐藏系统的复杂性, 并向客户端提供了一个客户端可以访问系统的接口. 这种类型的设计模式属于结构型模式, 它向现有的系统添加一个接口, 来隐藏系统的复杂性. 这种模式涉及到一个单一的类, 该类提供了客户端请求的简化方法和对现有系统类方法的委托调用.  
#### 结构  
外观模式 主要有以下主要角色:  
+ **Facade**  
+ **Subsystem Classes**  
##### Facade  
负责子系统的的封装调用, 知道哪些子系统类负责处理请求, 将客户的请求代理给适当的子系统对象.  
##### Subsystem Classes  
具体的子系统, 子系统类集合, 实现子系统的功能, 处理 Facade 对象指派的任务. **子类中没有 Facade 的任何信息, 即没有对 Facade 对象的引用.**  

#### 应用场景  
+ 设计初期阶段, 应该有意识的将不同层分离, 层与层之间建立外观模式.  
+ 开发阶段, 子系统越来越复杂, 增加外观模式提供一个简单的调用接口.  
+ 维护一个大型遗留系统的时候, 可能这个系统已经非常难以维护和扩展, 但又包含非常重要的功能, 为其开发一个外观类, 以便新系统与其交互.  

#### 实例  
以王者荣耀系统胜率安排系统为例, 
外观类: 游戏不同的阵容组合  
英雄类: 不同的英雄, 可以打不同的位置  
外观类组合不同的英雄, 得到一个胜率值.  
**Facade类定义** (***Facade.h***)  
```  
#ifndef _FACADE_H_
#define _FACADE_H_

#include "Hero.h"

class Facade {

public:

  Facade();
  ~Facade();

  double LineUpA();
  double LineUpB();
  double LineUpC();

private:

  Xiangxiang* m_XX;
  ZhaoYun*    m_ZY;
  LvBu*       m_LB;
  LianPo*     m_LP;
  ZhenJi*     m_ZJ;

};

#endif  
```  
**Facade类实现** (***Facade.cpp***)  
```  
#include "Facade.h"

#define DELEOBJECT(x) if (x == nullptr) { delete x; x = nullptr; }

Facade::Facade() {

  m_XX = new Xiangxiang();
  m_ZY = new ZhaoYun();
  m_LB = new LvBu();
  m_LP = new LianPo();
  m_ZJ = new ZhenJi();

}

Facade::~Facade() {

  DELEOBJECT(m_XX);
  DELEOBJECT(m_ZY);
  DELEOBJECT(m_LB);
  DELEOBJECT(m_LP);
  DELEOBJECT(m_ZJ);

}

double Facade::LineUpA() {

  m_XX->FaYu();
  m_ZY->DaYe();
  m_LB->KangYa();
  m_LP->FuZhu();
  m_ZJ->ZhangDan();

  return 0.88;

}

double Facade::LineUpB() {

  m_XX->FaYu();
  m_ZY->KangYa();
  m_LB->DaYe();
  m_LP->FuZhu();
  m_ZJ->ZhangDan();

  return 0.40;

}

double Facade::LineUpC() {

  m_XX->FaYu();
  m_ZY->DaYe();
  m_LB->ZhangDan();
  m_LP->KangYa();
  m_ZJ->FuZhu();

  return 0.60;

}  
```  
**英雄类定义** (***Hero.h***)  
```  
#ifndef _HERO_H_
#define _HERO_H_

#include <bits/stdc++.h>
using namespace std;

class Hero {

public:

  Hero(string name): m_Name(name) {}
  ~Hero() {}

  inline void  ZhangDan() {cout << m_Name << "中单" << endl;}
  inline void  DaYe() {cout << m_Name << "打野" << endl;}
  inline void  FuZhu() {cout << m_Name << "辅助" << endl;}
  inline void  FaYu() {cout << m_Name << "发育" << endl;}
  inline void  KangYa() {cout << m_Name << "抗压" << endl;}

protected:

  string m_Name;

};

class ZhenJi: public Hero {

public:

  ZhenJi():Hero("甄姬") {}
  ~ZhenJi() {}

};

class ZhaoYun: public Hero {

public:

  ZhaoYun():Hero("赵云") {}
  ~ZhaoYun() {}

};

class Xiangxiang: public Hero {

public:

  Xiangxiang():Hero("香香") {}
  ~Xiangxiang() {}

};

class LianPo: public Hero {

public:

  LianPo():Hero("廉颇") {}
  ~LianPo() {}

};

class LvBu: public Hero {

public:

  LvBu():Hero("吕布") {}
  ~LvBu() {}

};

#endif  
```
**应用** (***main.cpp***) 
```  
#include "Facade.h"

int main() {

  Facade facade;

  cout << "胜率: \n" << facade.LineUpA() * 100 << "%" << endl;
  cout << "胜率: \n" << facade.LineUpB() * 100 << "%" << endl;
  cout << "胜率: \n" << facade.LineUpC() * 100 << "%" << endl;

}  
```

#### 优缺点  
##### 优点  
+ 实现了子系统与客户端之间的松耦合关系.  
+ 客户端屏蔽了子系统组件, 减少了客户端所需处理的对象数目, 并使得子系统使用起来更加容易.  

##### 缺点  
+ 不符合开闭原则, 如果要修改某一个子系统的功能, 通常外观类也要一起修改.  
+ 没有办法直接阻止外部不通过外观类访问子系统的功能, 因为子系统类中的功能必须是公开的(根据需要决定是否使用internal访问级别可解决这个缺点, 但外观类需要和子系统类在同一个程序集内).  

***  

### 享元模式  
#### 简介  
享元模式, 主要用于减少创建对象的数量, 以减少内存占用和提高性能. 这种类型的设计模式属于结构型模式, 它提供了减少对象数量从而改善应用所需的对象结构的方式.  

在软件开发过程, 如果我们**需要重复使用某个对象的时候, 如果我们重复地使用new创建这个对象的话, 这样我们在内存就需要多次地去申请内存空间了, 这样可能会出现内存使用越来越多的情况**, 这样的问题是非常严重, 然而享元模式可以解决这个问题.  

共享元对象, 运用共享技术有效地支持大量细粒度对象的复用. 如果在一个系统中存在多个相同的对象, 那么只需要共享一份对象的拷贝, 而不必为每一次使用创建新的对象. **享元模式是为数不多只为提升系统性能而生的设计模式, 主要作用就是复用大对象(重量级对象), 以节省内存空间和对象创建时间.**  

面向对象可以非常方便的解决一些扩展性的问题, 但是在这个过程中系统务必会产生一些类或者对象, 如果系统中存在对象的个数过多时, 将会导致系统的性能下降. 对于这样的问题解决最简单直接的办法就是减少系统中对象的个数. 享元模式提供了一种解决方案, 使用共享技术实现相同或者相似对象的重用. 也就是说实现相同或者相似对象的代码共享.  

**所谓享元模式就是运行共享技术有效地支持大量细粒度对象的复用. 系统使用少量对象, 而且这些都比较相似, 状态变化小, 可以实现对象的多次复用.**  

共享模式是支持大量细粒度对象的复用, 所以享元模式要求能够共享的对象必须是细粒度对象.  

首先了解两个概念: **内部状态, 外部状态**.  
内部状态: 在享元对象内部不随外界环境改变而改变的共享部分.  
外部状态: 随着环境的改变而改变, 不能够共享的状态就是外部状态. 

由于享元模式区分了内部状态和外部状态, 所以我们可以通过设置不同的外部状态使得相同的对象可以具备一些不同的特性, 而内部状态设置为相同部分.  

在我们的程序设计过程中, 我们可能会需要大量的细粒度对象来表示对象, 如果这些对象除了几个参数不同外其他部分都相同, 这个时候我们就可以利用享元模式来大大减少应用程序当中的对象.  

#### 结构  
享元模式 主要有以下主要角色:  
+ **享元接口(Flyweight)**  
+ **具体的享元实现对象(Concrete Flyweight)**  
+ **非共享的享元实现对象(Unshare Concrete Flyweight)**  
+ **享元工厂类(Flyweight Factoty)**  
+ **享元客户端(Client)**  

##### 享元接口  
所有具体享元类的超类或接口, 通过该接口 Flyweight 可以接受并作用于外部状态. 通过该接口可以传入外部的状态, 在享元对象的方法处理中可能会使用这些外部的数据.  
##### 具体的享元实现对象  
指定内部状态, 必须是共享的, 需要封装Flyweight的内部状态.  
##### 非共享的享元实现对象  
非共享的享元实现对象, 并不是所有的Flyweight实现对象都需要共享. 非共享的享元实现对象通常是对享元对象的组合对象.  
##### 享元工厂类  
主要用来创建并管理共享的享元对象, 并对外提供访问共享享元的接口. 当用户请求一个 Flyweight 时, FlyweightFactory 就会提供一个已经创建的 Flyweight 对象或者新建一个(如果不存在).  
##### 享元客户端  
主要的工作就是维持一个对 Flyweight 的引用, 计算或存储享元的外部状态, 当然这里可访问共享和不共享的 Flyweight 对象.  

**享元模式的核心在于享元工厂类**, 享元工厂类的作用在于提供一个用于存储享元对象的享元池, 用户需要对象时, 首先从享元池中获取, 如果享元池中不存在, 则创建一个新的享元对象返回给用户, 并在享元池中保存该新增对象.  

#### 应用场景  
+ 一个系统有大量相同或者相似的对象, 造成内存的大量耗费.  
+ 对象的大部分状态都可以外部化, 可以将这些外部状态传入对象中.  
+ 在使用享元模式时需要维护一个存储享元对象的享元池, 而这需要耗费一定的系统资源, 因此, 应当在需要多次重复使用享元对象时才值得使用享元模式.

#### 实例  
创建一个20*20的画布, 上面初始都是'o'图案表示无颜色填充的画点, 再在上面使用黑色(用'+'图案表示), 白色(用'-'图案表示)的画点随机填充.  

可以看出里面的 颜色画点 就是多个相同的对象, 而且它们频繁地被调用.  
**享元工厂类定义** (***PieceFactory.h***)  
```  
#ifndef _PIECEFACTORY_H_
#define _PIECEFACTORY_H_

#include "Piece.h"

class PieceFactory{

public:

  PieceFactory();
  Piece* find(Color color);

private:

  map<Color, Piece*> m_PiecesMap;

};

#endif  
```  
**享元工厂类实现** (***PieceFactory.cpp***)  
```  
#include "PieceFactory.h"

PieceFactory::PieceFactory() {

}

Piece* PieceFactory::find(Color color) {

  auto piece = m_PiecesMap.find(color);

  if (piece != m_PiecesMap.end()) {

      return piece->second;

  }else {

      Piece* p = new Piece(color);
      m_PiecesMap[color] = p;
      
      return p;

  }

}  
```  
**享元接口类定义** (***Piece.h***)  
```  
#ifndef _PIECE_H_  
#define _PIECE_H_

#include <bits/stdc++.h>
using namespace std;

enum Color {

  white,
  black

};

class Piece {

public:

  Piece(Color color);
  void setPoint(int x, int y);
  Color GetColor() const;

public:

  int m_X;
  int m_Y;

private:

  Color m_Color;

};

#endif
```  
**享元接口类实现** (***Piece.cpp***)  
```  
#include "Piece.h"

Piece::Piece(Color color) 
     : m_Color(color) {

}

void Piece::setPoint(int x, int y) {

  m_X = x;
  m_Y = y;

}

Color Piece::GetColor() const {

  return m_Color;

}
```  
**非共享的享元实现对象类定义** (***CheckerBoard.h***) 
```  
#ifndef _CHECKERBOARD_H_
#define _CHECKERBOARD_H_

#include "Piece.h"

class CheckerBoard {

public:

  CheckerBoard();
  void Draw();
  void refresh();
  void GetPiece(const Piece &piece);

private:

  string m_Checker;

};

#endif
```  
**非共享的享元实现对象类实现** (***CheckerBoard.cpp***) 
```
#include "CheckerBoard.h"

CheckerBoard::CheckerBoard() {

  for (int m = 0; m < 20; m++) {

      for (int n = 0; n < 20; n++) {

          m_Checker += "o";

      }

      m_Checker += '\n';

  }

}

void CheckerBoard::Draw() {

    cout << m_Checker << endl;

}

void CheckerBoard::refresh() {

    system("cls");
    cout << m_Checker << endl;

}

void CheckerBoard::GetPiece(const Piece &piece) {

    int pos;
    pos = (piece.m_Y * 21) + piece.m_X;

    if (piece.GetColor() == Color::white) {

        m_Checker.replace(pos, 1, "-");

    }else if (piece.GetColor() == Color::black) {

        m_Checker.replace(pos, 1, "+");

    }

}  
```  
**应用** (***main.cpp***) 
```  
#include "CheckerBoard.h"
#include "Piece.h"
#include "PieceFactory.h"

void GeneratePoint(Piece* p) {

    p->m_X = rand() % 20;
    p->m_Y = rand() % 20;

}

int main() {

    CheckerBoard board;
    board.Draw();

    PieceFactory factory;

    for (int n = 0; n < 20; n++) {

        Piece* p1 = factory.find(Color::black);
        GeneratePoint(p1);

        _sleep(1);

        Piece* p2 = factory.find(Color::white);
        GeneratePoint(p2);

        board.GetPiece(*p2);
        board.GetPiece(*p1);
        board.refresh();

    }

    return 0;

}  
```

#### 优缺点  
##### 优点  
+ 可以极大减少内存中对象的数量, 使得相同或相似对象在内存中只保存一份, 从而可以节约系统资源, 提高系统性能.  
+ 享元模式的外部状态相对独立, 而且不会影响其内部状态, 从而使得享元对象可以在不同的环境中被共享.  

##### 缺点  
+ 享元模式使得系统变得复杂, 需要分离出内部状态和外部状态, 这使得程序的逻辑复杂化.  
+ 为了使对象可以共享, 享元模式需要将享元对象的部分状态外部化, 而读取外部状态将使得运行时间变长.  

#### 享元模式 和 单例模式 的异同  
享元模式可以再次创建对象, 也可以取缓存对象.  
单例模式则是严格控制单个进程中只有一个实例对象.  

享元模式可以通过自己实现对外部的单例 也可以在需要的使用创建更多的对象.  
单例模式是自身控制, 需要增加不属于该对象本身的逻辑.

两者都可以实现节省对象创建的时间.  

***  

### 代理模式  
#### 简介  
代理模式, **就是给某一个对象提供一个代理, 并由代理对象控制对原对象的引用.** 在一些情况下, 一个客户不想或者不能直接引用一个对象, 而代理对象可以在客户端和目标对象之间起到中介的作用. 例如电脑桌面的快捷方式就是一个代理对象, 快捷方式是它所引用的程序的一个代理.  
#### 结构  
代理模式 主要有以下角色:  
+ **抽象主题角色(Subject)**  
+ **代理主题角色(Proxy)**  
+ **真实主题角色(Real Subject)**  

##### 抽象主题角色  
它声明了 真实主题 和 代理主题 的共同接口, 这样一来在任何使用真实主题的地方都可以使用代理主题, 客户端通常需要针对抽象主题角色进行编程.  
##### 代理主题角色  
它包含了对真实主题的引用, 从而可以在任何时候操作真实主题对象; 在代理主题角色中提供一个与真实主题角色相同的接口, 以便在任何时候都可以替代真实主题; 代理主题角色还可以控制对真实主题的使用, 负责在需要的时候创建和删除真实主题对象, 并对真实主题对象的使用加以约束. 通常, 在代理主题角色中, 客户端在调用所引用的真实主题操作之前或之后还需要执行其他操作, 而不仅仅是单纯调用真实主题对象中的操作.  
##### 真实主题角色  
它定义了代理角色所代表的真实对象, 在真实主题角色中实现了真实的业务操作, 客户端可以通过代理主题角色间接调用真实主题角色中定义的操作.  

#### 代理模式的种类  
代理模式根据其目的和实现方式不同可分为很多种类, 其中常用的几种代理模式:  
+ **远程代理 (Remote Proxy) :** 为一个位于不同的地址空间的对象提供一个本地的代理对象, 这个不同的地址空间可以是在同一台主机中, 也可是在另一台主机中, 远程代理又称为**大使(Ambassador)**.  
+ **虚拟代理 (Virtual Proxy) :** 如果需要创建一个资源消耗较大的对象, 先创建一个消耗相对较小的对象来表示, 真实对象只在需要时才会被真正创建.  
+ **保护代理 (Protect Proxy) :** 控制对一个对象的访问, 可以给不同的用户提供不同级别的使用权限.  
+ **缓冲代理 (Cache Proxy) :** 为某一个目标操作的结果提供临时的存储空间, 以便多个客户端可以共享这些结果.  
+ **智能引用代理 (Smart Reference Proxy) :** 当一个对象被引用时, 提供一些额外的操作, 例如将对象被调用的次数记录下来等.  

#### 应用场景  
代理模式的类型较多, 不同类型的代理模式有不同的优缺点, 它们应用于不同的场合:  
+ 当客户端对象需要访问远程主机中的对象时可以使用**远程代理**.  
+ 当需要用一个消耗资源较少的对象来代表一个消耗资源较多的对象, 从而降低系统开销, 缩短运行时间时可以使用**虚拟代理**, 例如一个对象需要很长时间才能完成加载时.  
+ 当需要为某一个被频繁访问的操作结果提供一个临时存储空间, 以供多个客户端共享访问这些结果时可以使用**缓冲代理**. 通过使用缓冲代理, 系统无须在客户端每一次访问时都重新执行操作, 只需直接从临时缓冲区获取操作结果即可.  
+ 当需要控制对一个对象的访问, 为不同用户提供不同级别的访问权限时可以使用**保护代理**.  
+ 当需要为一个对象的访问(引用), 提供一些额外的操作时可以使用**智能引用代理**.  

#### 实例  
比如, 腼腆的男生可以创建一个代理, 让代理给女生送礼物, 这样就可以解决问题了:  
**代理主题角色类定义** (***Proxy.h***)  
```  
#ifndef _PROXY_H_  
#define _PROXY_H_

#include "GiveGift.h"
#include "ConcreteGirl.h"
#include "ConcretePeople.h"

class Proxy: public GiveGift {

public:

  Proxy(ConcreteGirl* girl);
  void GiveFlow() override;
  void GiveDolls() override;
  void GiveChocolate() override;

private:

  ConcretePeople* m_People;

};

#endif  
```  
**代理主题角色类实现** (***Proxy.cpp***)  
```  
#include "Proxy.h"
#include "ConcreteGirl.h"
#include "ConcretePeople.h"

Proxy::Proxy(ConcreteGirl* girl) {

  m_People = new ConcretePeople("小明");
  m_People->ChasingGirls(girl);

}

void Proxy::GiveFlow() {

  if (m_People) {
      m_People->GiveFlow();
  }

}

void Proxy::GiveDolls() {

  if (m_People) {
      m_People->GiveDolls();
  }

}

void Proxy::GiveChocolate() {

  if (m_People) {
      m_People->GiveChocolate();
  }

}  
```
**真实主题角色类定义** (***ConcretePeople.h***)  
```  
#ifndef _CONCRETEPEOPLE_H_  
#define _CONCRETEPEOPLE_H_

#include "GiveGift.h"
#include "ConcreteGirl.h"

class ConcretePeople: public GiveGift {

public:

  ConcretePeople(string name);
  void GiveFlow() override;
  void GiveDolls() override;
  void GiveChocolate() override;

  void ChasingGirls(ConcreteGirl* girl);

private:

  string        m_Name;
  ConcreteGirl* m_Girl;

};

#endif  
```  
**真实主题角色类实现** (***ConcretePeople.h***) 
```  
#include "ConcretePeople.h"

ConcretePeople::ConcretePeople(string name)
              : m_Name(name) {

}

void ConcretePeople::GiveFlow() {

    if (m_Girl) {
        cout << m_Girl->Name() << " 这是送你的花花" << endl;
    }

}

void ConcretePeople::GiveDolls() {

    if (m_Girl) {
        cout << m_Girl->Name() << " 这是送你的洋娃娃" << endl;
    }

}

void ConcretePeople::GiveChocolate() {

    if (m_Girl) {
        cout << m_Girl->Name() << " 这是送你的巧克力" << endl;
    }

}

void ConcretePeople::ChasingGirls(ConcreteGirl* girl) {

    m_Girl = girl;

}  
``` 
 **真实主题角色类定义** (***ConcreteGirl.h***)  
```  
#ifndef _CONCRETEGIRL_H_  
#define _CONCRETEGIRL_H_

#include <bits/stdc++.h>
using namespace std;

class ConcreteGirl {

public:

  ConcreteGirl(string name);
  string Name();

private:

  string m_Name;

};

#endif  
```
**真实主题角色类实现** (***ConcreteGirl.cpp***)  
```  
#include "ConcreteGirl.h"  

ConcreteGirl::ConcreteGirl(string name)
            : m_Name(name) {

}

string ConcreteGirl::Name() {

    return m_Name;

}  
```  
**抽象主题角色类定义** (***GiveGift.h***)
```  
#ifndef _GIVEGIFT_H_  
#define _GIVEGIFT_H_

#include <bits/stdc++.h>
using namespace std;

class GiveGift {

public:

  GiveGift();
  virtual ~GiveGift();
  virtual void GiveFlow() = 0;
  virtual void GiveChocolate() = 0;
  virtual void GiveDolls() = 0;

};

#endif
```  
**抽象主题角色类实现** (***GiveGift.cpp***)  
```    
#include "GiveGift.h"

GiveGift::GiveGift() {

}

GiveGift::~GiveGift() {
  
}  
``` 
**应用** (***main.cpp***) 
```  
#include "Proxy.h"
#include "ConcreteGirl.h"

int main() {

    shared_ptr<ConcreteGirl> girl = make_shared<ConcreteGirl>("Kiana");
    Proxy* proxy = new Proxy(girl.get());

    proxy->GiveFlow();
    proxy->GiveDolls();
    proxy->GiveChocolate();

    return 0;

}  
```

#### 优缺点  
##### 优点  
+ 能够协调调用者和被调用者, 在一定程度上降低了系统的耦合度.  
+ 客户端可以针对抽象主题角色进行编程, 增加和更换代理类无须修改源代码, 符合开闭原则, 系统具有较好的灵活性和可扩展性.  

##### 缺点  
+ 由于在客户端和真实主题之间增加了代理对象, 因此有些类型的代理模式可能会造成请求的处理速度变慢, 例如保护代理.  
+ 实现代理模式需要额外的工作, 而且有些代理模式的实现过程较为复杂, 例如远程代理.  

#### 代理模式 和 装饰模式 的异同  
代理模式和装饰模式的代码实现方式很相同, **主要不同点是代理模式关注与被代理对象行为的控制, 然而装饰模式关注于在一个对象上动态的添加方法**.  

代理模式可以对客户端隐藏被代理对象的具体实现, 代理模式的时候常常是在一个代理类中创建一个对象的实例, 当使用装饰模式的时候, 将原始对象转为一个参数传递给装饰者的构造器中.  

**代理模式强调的是限制, 装饰模式强调的是增强.**  

***  

### 桥接模式  
#### 简介  
桥接模式, 将抽象部分与它的实现部分分离解耦, 使得二者可以独立变化而互不影响. 它是一种对象结构型模式, 又称 **柄体模式** 和 **接口模式**.  
#### 结构  
桥接模式一般有以下主要角色:  
+ **抽象类(Abstraction)**  
+ **扩充抽象类(RefinedAbstraction)**  
+ **实现类接口(Implementor)**  
+ **具体实现类(Concrete Implementor)**  

##### 抽象类  
用于定义抽象类的接口, 它一般是抽象类而不是接口, 其中定义了一个 Implementor（实现类接口）类型的对象并可以维护该对象, 它与 Implementor 之间具有关联关系, 它既可以包含抽象业务方法, 也可以包含具体业务方法.  
##### 扩充抽象类  
扩充由 Abstraction 定义的接口, 通常情况下它不再是抽象类而是具体类, 它实现了在 Abstraction 中声明的抽象业务方法, 在RefinedAbstraction 中可以调用在 Implementor 中定义的业务方法.  
##### 实现类接口  
定义实现类的接口, 这个接口不一定要与 Abstraction 的接口完全一致, 事实上这两个接口可以完全不同, 一般而言, Implementor 接口仅提供基本操作, 而 Abstraction 定义的接口可能会做更多更复杂的操作. Implementor 接口对这些基本操作进行了声明, 而具体实现交给其子类. 通过关联关系, 在 Abstraction 中不仅拥有自己的方法, 还可以调用到 Implementor 中定义的方法, 使用关联关系来替代继承关系.  
##### 具体实现类  
具体实现 Implemento 接口, 在不同的 Concrete Implementor 中提供基本操作的不同实现, 在程序运行时, Concrete Implementor对象将替换其父类对象, 提供给抽象类具体的业务操作方法.  

#### 应用场景  
+ 当对象存在多种变化的因素时, 考虑对其变化的因素和场景进行抽象, 然后进行桥接; 如笔拥有不同的功能.  
+ 当多个对象存在多种变化的因素时, 考虑将这部分变化的部分抽象出来再聚合进来; 比如不同品牌的电脑安装不同的系统, 使用不同的软件等, 相当于将第一条进行横向扩展, 增加桥接的数量.  

#### 实例  
设想如果要绘制矩形, 三角形, 我们至少需要2个形状类, 如果绘制的图形还需要不同的颜色. 
第一种做法: 每一种形状都提供一套各种颜色的版本.
第二种做法: 根据实际需要对形状和颜色进行组合.  
明显第二种做法采用聚合的方式组织类结构能够使类的数量减少很多
**抽象类定义** (***Color.h***)  
```  
#ifndef _COLOR_H_  
#define _COLOR_H_

#include <bits/stdc++.h>
using namespace std;

class Color {

public:

  Color() {}
  virtual ~Color() {}

  virtual string Name() = 0;

protected:

  string m_Name;

};

class Black: public Color {

public:

  Black();
  virtual ~Black();

  virtual string Name();

};

class Red: public Color {

public:

  Red();
  virtual ~Red();
  virtual string Name();

};

#endif  
```  
**抽象类定义** (***Color.cpp***) 
```  
#include "Color.h"

Black::Black() {

  m_Name = "Black";

}

Black::~Black() {

}

string Black::Name() {

  return m_Name;

}

Red::Red() {

  m_Name = "Red";

}

Red::~Red() {

}

string Red::Name() {

  return m_Name;

}
```  
**实现类接口/具体实现类定义** (***Shape.h***) 
```  
#ifndef _SHAPE_H_
#define _SHAPE_H_

#include "Color.h"

class Shape {

public:

  Shape() {}
  virtual ~Shape() {}

  void SetColor(Color* color);
  virtual void MyShape() = 0;

protected:

  Color* m_Color;

};

class Rectangle: public Shape {

public:

  Rectangle();
  ~Rectangle();

  virtual void MyShape();

};

class Circle: public Shape {

public:

  Circle();
  ~Circle();

  virtual void MyShape();

};

#endif
```  
**实现类接口/具体实现类定义** (***Shape.cpp***)  
```  
#include "Shape.h"

void Shape::SetColor(Color* color) {

  m_Color = color;

}

Rectangle::Rectangle() {

}

Rectangle::~Rectangle() {

}

void Rectangle::MyShape() {

  cout << "Rectangle And " + m_Color->Name() << endl; 

}

Circle::Circle() {

}

Circle::~Circle() {

}

void Circle::MyShape() {

  cout << "Circle And " + m_Color->Name() << endl;

}
```
**应用** (***main.cpp***)  
```
#include "Shape.h"
#include "Color.h"

#define DELETEOBJECT(x) if (x != nullptr) {delete x; x = nullptr;}

int main() {

  Color* red = new Red();
  Color* black = new Black();

  Shape* rectangle = new Rectangle();
  rectangle->SetColor(red);

  Shape* circle = new Circle();
  circle->SetColor(black);

  rectangle->MyShape();
  circle->MyShape();

  DELETEOBJECT(red);
  DELETEOBJECT(black);
  DELETEOBJECT(rectangle);
  DELETEOBJECT(circle);

  return 0;

}  
```

#### 优缺点  
##### 优点  
+ 分离抽象接口及其实现部分. 桥接模式使用 "对象间的关联关系" 解耦了抽象和实现之间固有的绑定关系, 使得抽象和实现可以沿着各自的维度来变化. 所谓抽象和实现沿着各自维度的变化, 也就是说抽象和实现不再在同一个继承层次结构中, 而是 "子类化" 它们, 使它们各自都具有自己的子类, 以便任何组合子类, 从而获得多维度组合对象.  
+ 在很多情况下, 桥接模式可以取代多层继承方案, 多层继承方案违背 "单一职责原则", 复用性较差, 且类的个数非常多, 桥接模式是比多层继承方案更好的解决方法, 它极大减少了子类的个数.  
+ 桥接模式提高了系统的可扩展性, 在两个变化维度中任意扩展一个维度, 都不需要修改原有系统, 符合 "开闭原则".

##### 缺点  
+ 桥接模式的使用会增加系统的理解与设计难度, 由于关联关系建立在抽象层, 要求开发者一开始就针对抽象层进行设计与编程.  
+ 桥接模式要求正确识别出系统中两个独立变化的维度, 因此其使用范围具有一定的局限性, 如何正确识别两个独立维度也需要一定的经验积累.  

#### 装饰, 桥接 和 适配器模式 的异同  
三者都是结构型的设计模式, 而且都存在依赖抽象的情况.  

**适配器模式:**  
重点强调的是**适配的功能**. (适配器依赖抽象)  
关键点是:  
+ 主体类和适配器类实现相同的接口A  
+ 主体类依赖适配器类  
+ 适配器类依赖抽象接口B  
+ 被适配的类实现抽象接口B  
  
最终的效果就是, 主体类可以使用之前不相关的被适配类中的某些功能.  

**桥接模式:**  
重点强调的是**多维度的变化**. (主体类直接依赖抽象)  
关键点:  
+ 主体类依赖抽象A  
+ 主体类具有多个不同的实现类  
+ 抽象A具有多个不同的实现类  

最终的效果就是, 主体类的实现类和抽象的实现类分别可以在两个维度上进行各自的变化. 如果主体类依赖多个抽象, 则维度进行增加, 方便扩展.  

**装饰器模式:**  
重点强调的是装饰功能. (主体类不仅依赖抽象, 而且实现该抽象接口)  
关键点:
+ 抽象A具有多个具体子类  
+ 装饰器类依赖抽象A  
+ 装饰器类实现抽象A  
+ 装饰器类存在不同子类  

最终的效果就是, 装饰器实现类 对 原抽象的子类 进行某些方法的功能加强.  

#### 总结  
桥接模式是一个非常有用的模式, 在桥接模式中体现了很多面向对象设计原则的思想, 包括 "单一职责原则", "开闭原则", "合成复用原则", "里氏代换原则", "依赖倒转原则"等. 熟悉桥接模式有助于我们深入理解这些设计原则, 也有助于我们形成正确的设计思想和培养良好的设计风格.  

***  

## 行为型模式  
行为型模式特别关注对象之间的通信, 行为型模式包括以下**11**种:
+ **迭代器模式** (***Iterator Pattern***)  
+ **模板方法模式** (***Template Pattern***)  
+ **策略模式** (***Strategy Pattern***)  
+ **访问者模式** (***Visitor Pattern***)  
+ **职责链模式** (***Chain of Responsibility Pattern***)  
+ **中介者模式** (***Mediator Pattern***)  
+ **观察者模式** (***Observer Pattern***)  
+ **备忘录模式** (***Memento Pattern***)  
+ **状态模式** (***State Pattern***)  
+ **命令模式** (***Command Pattern***) 
+ **解释器模式** (***Interpreter Pattern***) 

### 迭代器模式  
#### 简介  
迭代器模式是针对集合对象而生的, 对于集合对象而言, 肯定会涉及到对集合的添加和删除操作, 同时也肯定支持遍历集合元素的操作, 我们此时可以把遍历操作放在集合对象中, 但这样的话, 集合对象既承担太多的责任了, 面向对象设计原则中有一条就是单一职责原则, 所有我们要尽可能地分离这些职责, 用不同的类取承担不同的责任, 迭代器模式就是用迭代器类来承担遍历集合的职责.  

迭代器模式提供了一种方法顺序访问一个聚合对象中的各个元素, 而又无需暴露该对象的内部实现, 这样既可以做到不暴露集合的内部结构, 又可让外部代码透明地访问集合内部的数据.  

#### 结构  
迭代器模式 主要有以下角色:  
+ **抽象容器角色(Agregate)**  
+ **具体容器角色(Concrete Aggregate)**  
+ **抽象迭代器角色(Iterator)**  
+ **具体迭代器角色(Concrete Iterator)**  

##### 抽象容器角色  
负责提供创建具体迭代器角色的接口, 一般是一个接口, 提供一个 ***iterator*** ()方法.  
##### 具体容器角色  
就是实现抽象容器的具体实现类. 
##### 抽象迭代器角色  
负责定义访问和遍历元素的接口.  
##### 具体迭代器角色  
实现迭代器接口, 并要记录遍历中的当前位置.  

#### 应用场景  
+ 访问一个集合对象的内容而无需暴露它的内部表示.  
+ 为遍历不同的集合结构提供一个统一的接口.  

#### 实例  
**抽象容器角色类定义** (***Aggregate.h***)  
```  
#ifndef _AGGREGATE_H_
#define _AGGREGATE_H_

#include <bits/stdc++.h>
using namespace std;

class Aggregate {

public:

  Aggregate();
  virtual ~Aggregate();

  virtual int Count() = 0;
  virtual string& operator [](int i) = 0;

  void insert(string obj);

protected:

  vector<string> m_Vec;

};

#endif  
```  
**抽象容器角色类实现** (***Aggregate.cpp***)  
```  
#include "Aggregate.h"

Aggregate::Aggregate() {

}

Aggregate::~Aggregate() {

}

void Aggregate::insert(string obj) {

  m_Vec.push_back(obj);

}  
```  
**具体容器角色类定义** (***ConcreteAggregate.h***)  
```  
#ifndef _CONCRETEAGGREGATE_H_
#define _CONCRETEAGGREGATE_H_

#include "Iterator.h"

class ConcreteAggregate: public Aggregate {

public:

  ConcreteAggregate();
  int Count() override;
  string &operator[](int i) override;

private:

  Iterator* m_Iterator;

};

#endif  
```  
**具体容器角色类实现** (***ConcreteAggregate.cpp***)  
```  
#include "ConcreteAggregate.h"

ConcreteAggregate::ConcreteAggregate() {

}

int ConcreteAggregate::Count() {

  return static_cast<int>(m_Vec.size());

}

string& ConcreteAggregate::operator[](int i) {

  return m_Vec[size_t(i)];

}  
```  
**抽象迭代器角色类定义** (***Iterator.h***)  
```  
#include "ConcreteAggregate.h"

ConcreteAggregate::ConcreteAggregate() {

}

int ConcreteAggregate::Count() {

  return static_cast<int>(m_Vec.size());

}

string& ConcreteAggregate::operator[](int i) {

  return m_Vec[size_t(i)];

}  
```  
**抽象迭代器角色类实现** (***Iterator.cpp***)  
```  
#include "Iterator.h"

Iterator::Iterator(Aggregate* aggregate)
        : m_Aggregate(aggregate) {

}

Iterator::~Iterator() {
  
}  
```  
**具体迭代器角色类定义** (***ConcreteIterator.h***)  
```  
#ifndef _CONCRETEITERATOR_H_
#define _CONCRETEITERATOR_H_

#include "Iterator.h"
#include "Aggregate.h"

class ConcreteIterator: public Iterator {

public:

  ConcreteIterator(Aggregate* aggregate);
  string First() override;
  string Next() override;
  bool IsDone() override;
  string CurrentItem() override;

private:

  int m_CurrentIndex;

};

#endif  
```  
**具体迭代器角色类实现** (***ConcreteIterator.cpp***)  
```  
#include "ConcreteIterator.h"

ConcreteIterator::ConcreteIterator(Aggregate* aggregate)
                : Iterator(aggregate) {

  m_CurrentIndex = 0;

}

string ConcreteIterator::First() {

  return (*m_Aggregate)[0];

}

string ConcreteIterator::Next() {

  m_CurrentIndex++;

  if (m_CurrentIndex < m_Aggregate->Count()) {

      return (*m_Aggregate)[m_CurrentIndex];

  }else {

      return "";

  }

}

bool ConcreteIterator::IsDone() {

  return (m_CurrentIndex >= m_Aggregate->Count())? true: false;

}

string ConcreteIterator::CurrentItem() {

  return (*m_Aggregate)[m_CurrentIndex];

}  
```  
**应用** (***main.cpp***)  
```  
#include "ConcreteAggregate.h"
#include "ConcreteIterator.h"

int main() {

  Aggregate* agt = new ConcreteAggregate();
  agt->insert("Kiana");
  agt->insert("Asuna");
  agt->insert("Orange");

  Iterator* i = new ConcreteIterator(agt);

  while (!i->IsDone()) {

    cout << i->CurrentItem() << " 请出示您的车票!" << endl;
    i->Next();

  }

  return 0;
  
}  
```

#### 优缺点  
##### 优点  
+ 迭代器模式使得访问一个聚合对象的内容而无需暴露它的内部表示, 即迭代抽象.  
+ 迭代器模式为遍历不同的集合结构提供了一个统一的接口, 从而支持同样的算法在不同的集合结构上进行操作.  

##### 缺点  
+ 迭代器模式在遍历的同时更改迭代器所在的集合结构会导致出现异常. 所以使用foreach语句只能在对集合进行遍历, 不能在遍历的同时更改集合中的元素.  

#### 总结  
迭代器模式 就是抽象一个迭代器类来分离了集合对象的遍历行为. 这样既可以做到不暴露集合的内部结构, 又可让外部代码透明地访问集合内部的数据.  

***  

### 模板方法模式  
#### 简介  
模板方法模式, 在一个方法中定义一个算法的骨架, 而将一些步骤的实现延迟到子类中. 模板方法使得子类可以在不改变算法结构的情况下, 重新定义算法中某些步骤的具体实现.  

看到 "设计模式" 这四个字我们往往会觉得高深莫测, 但是模板方法模式却是一个例外, 模板方法模式确实非常简单, **仅仅使用继承机制**, 但是它是一个应用非常广泛的模式.  

#### 结构  
模板方法模式 主要有以下角色:  
+ **抽象模板类(Abstract Class)**  
+ **具体模板类(Concrete Class)**  

##### 抽象模板类  
定义并实现了一个模板方法, 这个模板一般是一个具体方法, 它给出了一个顶级逻辑的骨架, 而逻辑的组成步骤在相应的抽象操作中, 推迟到子类实现.  
##### 具体模板类  
实现父类所定义的一个或者多个抽象方法, 每一个 AbstractClass 都可以有任意多个 ConcreteClass 与之对应, 而每一个 ConcreteClass 都可以给出这些抽象方法的不同实现, 从而使得顶级逻辑的实现各不相同.  

#### 应用场景  
当系统中算法的骨架是固定的时候, 而算法的实现可能有很多种的时候, 就需要使用模板方法模式.  
+ 多个子类有共有的方法, 并且逻辑基本相同.  
+ 重要, 复杂的算法, 可以把核心算法设计为模板方法, 周边的相关细节功能则由各个子类实现.  
+ 重构时, 模板方法是一个经常使用的方法, 把相同的代码抽取到父类中, 然后通过构造函数约束其行为.  

比如: 需要做一个报表打印程序, 用户规定需要表头, 正文, 表尾. 但是客户的需求会变化, 一会希望这样显示表头, 一会希望那样显示. 这时候采用模板方式就合适.  

#### 实例  
**抽象模板类定义** (***AbstractClass.h***)  
```  
#ifndef _ABSTRACTCLASS_H_
#define _ABSTRACTCLASS_H_

#include <bits/stdc++.h>

class AbstractClass {

public:

  AbstractClass();
  virtual ~AbstractClass();

  void MethodA();
  virtual int MethodB(int a = 0, int b = 0);

};

#endif  
```  
**抽象模板类实现** (***AbstractClass.cpp***)  
```  
#include "AbstractClass.h"

AbstractClass::AbstractClass() {

}

AbstractClass::~AbstractClass() {

}

void AbstractClass::MethodA() {

    std::cout << __FUNCTION__ << " Call method b : " << this->MethodB(5, 4) << std::endl;

}

int AbstractClass::MethodB(int a, int b) {

    return a + b;

}  
```  
**具体模板类定义** (***ConcreteClassA.h***)  
```  
#ifndef _ABSTRACTCLASSA_H_
#define _ABSTRACTCLASSA_H_

#include "AbstractClass.h"

class ConcreteClassA: public AbstractClass {

public:

  ConcreteClassA();
  int MethodB(int a = 0, int b = 0) override;

};

#endif  
```  
**具体模板类定义** (***ConcreteClassB.h***)
```  
#ifndef _ABSTRACTCLASSB_H_
#define _ABSTRACTCLASSB_H_

#include "AbstractClass.h"

class ConcreteClassB: public AbstractClass {

public:

  ConcreteClassB();
  int MethodB(int a = 0, int b = 0) override;


};

#endif  
```
**具体模板类实现** (***ConcreteClassA.cpp***)
```  
#include "ConcreteClassA.h"

ConcreteClassA::ConcreteClassA() {

}

int ConcreteClassA::MethodB(int a, int b) {

    return a * b;

}  
```
**具体模板类实现** (***ConcreteClassB.cpp***)  
```  
#include "ConcreteClassB.h"

ConcreteClassB::ConcreteClassB() {

}

int ConcreteClassB::MethodB(int a, int b) {

    return a - b;

}  
```
**应用** (***main.cpp***) 
```  
#include "ConcreteClassA.h"
#include "ConcreteClassB.h"

int main() {

    ConcreteClassA* classA = new ConcreteClassA();
    ConcreteClassB* classB = new ConcreteClassB();

    classA->MethodA();
    classB->MethodA();

    return 0;

}  
```
#### 优缺点  
##### 优点  
+ 封装不变部分, 扩展可变部分. 把认为不变部分的算法封装到父类中实现, 而可变部分的则可以通过继承来继续扩展.  
+ 提取公共部分代码, 便于维护.  
+ 行为由父类控制, 子类实现.  

##### 缺点  
+ 算法骨架需要改变时需要修改抽象类.  
+ 按照设计习惯, 抽象类负责声明最抽象, 最一般的事物属性和方法, 实现类负责完成具体的事务属性和方法, 但是模板方式正好相反, 子类执行的结果影响了父类的结果, 会增加代码阅读的难度.  

#### 总结  
**重复 = 易错 + 难改**, 模板方法模式是 **通过父类建立框架, 子类在重写了父类部分方法之后, 在调用从父类继承的方法, 产生不同的效果, 通过修改子类, 影响父类行为的结果**. 模板方法在一些开源框架中应用非常多, 它提供了一个抽象类, 然后开源框架写了一堆子类, 如果需要扩展功能, 可以继承此抽象类, 然后覆写 protected 基本方法, 然后在调用一个类似 ***TemplateMethod***() 的模板方法, 完成扩展开发.  

***  

### 策略模式  
#### 简介  
策略模式, **定义了一系列算法, 并将每个算法封装起来, 使他们可以相互替换, 且算法的变化不会影响到使用算法的客户.** 需要设计一个接口, 为一系列实现类提供统一的方法, 多个实现类实现该接口, 设计一个抽象类(可有可无, 属于辅助类), 提供辅助函数.  

策略模式定义和封装了一系列的算法, 它们是可以相互替换的, 也就是说它们具有共性, 而它们的共性就体现在策略接口的行为上, 另外为了达到最后一句话的目的, 也就是说让算法独立于使用它的客户而独立变化, 我们需要让客户端依赖于策略接口.  

一种很简单的解释, 在我们的开发过程中, 经常会遇到大量的 **if - else** 或是 **switch - case** 语句, 当这些语句在开发中只是为了起到分流作用, 这些分流和业务逻辑无关, 那么这个时候就可以考虑用策略模式.  

#### 结构  
策略模式主要有以下角色:  
+ **上下文环境角色(Context)**  
+ **抽象策略角色(Strategy)**  
+ **具体策略角色(Context Strategy)**  

##### 上下文环境角色  
Context 上下文, 用一个 Context Strategy 来配置, 维护一个对 Strategy 对象的引用.  
##### 抽象策略角色  
策略类, 定义所有支持的算法的公共接口.  
##### 具体策略模式角色  
封装了具体的算法或行为, 继承于 Strategy  

#### 应用场景  
+ 多个类只区别在表现行为不同, 可以使用Strategy模式, 在运行时动态选择具体要执行的行为.  
+ 需要在不同情况下使用不同的策略(算法), 或者策略还可能在未来用其它方式来实现.  
+ 对客户隐藏具体策略(算法)的实现细节, 彼此完全独立.  

举一个例子, 商场搞促销: 打8折, 满200送50, 满1000送礼物, 这种促销就是策略.  

#### 实例  
商场促销策略. 
**头文件** (***Common.h***) 
```  
#ifndef _COMMON_H_  
#define _COMMON_H_

#include "Strategy.h"
#include "CashNormal.h"
#include "CashRebate.h"
#include "CashReturn.h"

#endif  
```
**上下文环境类定义** (***Context.h***)  
```  
#ifndef _CONTEXT_H_  
#define _CONTEXT_H_


#include "Strategy.h"

class Context {

public:

  Context(std::string strName);

  double getResult(double cash);

private:

  Strategy* m_Strategy;

};

#endif  
```  
**上下文环境类实现** (***Context.cpp***)  
```  
#include "Context.h"
#include "Common.h"

Context::Context(std::string strName) {

    if (strName == "dis") {

        m_Strategy = new CashRebate(0.8);

    }else if (strName == "return") {

        m_Strategy = new CashReturn(200, 100);

    }else if (strName == "nor") {

        m_Strategy = new CashNormal();

    }

}

double Context::getResult(double cash) {

    if (m_Strategy) {
        return m_Strategy->getResult(cash);
    }

}  
```  
**抽象策略类定义** (***Strategy.h***)  
```  
#ifndef _STRATEGY_H_  
#define _STRATEGY_H_

#include <bits/stdc++.h>

class Strategy {

public:

  Strategy();
  virtual ~Strategy();

  virtual double getResult(double price) { return 0; };

};

#endif  
```  
**抽象策略类实现** (***Strategy.cpp***)  
```  
#include "Strategy.h"  

Strategy::Strategy() {

}

Strategy::~Strategy() {

}  
```
**具体策略类定义** (***CashNormal.h***) 
```  
#ifndef _CASHNORMAL_H_  
#define _CASHNORMAL_H_

#include "Strategy.h"

class CashNormal: public Strategy {

public:

  CashNormal();
  double getResult(double price) override;

};

#endif
```  
**具体策略类实现** (***CashNormal.cpp***) 
```  
#include "CashNormal.h"

CashNormal::CashNormal() {

}

double CashNormal::getResult(double price) {

    assert(price > 0);
    return price;

}  
```  
**具体策略类定义** (***CashRebate.h***)  
```  
#ifndef _CASHREBATE_H_  
#define _CASHREBATE_H_

#include "Strategy.h"

class CashRebate: public Strategy {

public:

  CashRebate(double rebate);
  double getResult(double price) override;

private:

  double m_Rebate;

};

#endif  
```  
**具体策略类实现** (***CashRebate.cpp***)  
```  
#include "CashRebate.h"

CashRebate::CashRebate(double rebate)
          : m_Rebate(rebate) {
  
}

double CashRebate::getResult(double price) {

    assert(price > 0);
    return price * m_Rebate;

}  
```  
**具体策略类定义** (***CashReturn.h***)  
```  
#ifndef _CASHRETURN_H_
#define _CASHRETURN_H_

#include "Strategy.h"

class CashReturn: public Strategy {

public:

  CashReturn(double condition, double returnNum);
  double getResult(double price) override;

private:

  double m_Condition;
  double m_ReturnNum;

};

#endif  
```  
**具体策略类定义** (***CashReturn.cpp***)  
``` 
#include "CashReturn.h"

CashReturn::CashReturn(double condition, double returnNum) 
          : m_Condition(condition),
            m_ReturnNum(returnNum) {

}

double CashReturn::getResult(double price) {

    if (price >= m_Condition) {

        return price - m_ReturnNum;

    }else {

        return price;

    }

}  
```
**应用** (***main.cpp***)  
```  
#include "Context.h"

int main() {

    std::string input;

    std::cout << "select promotions:" << std::endl;
    std::cin >> input;

    Context cn(input);
    double ret = cn.getResult(1000);
    std::cout << "Result is :" << ret << std::endl;

    return 0;

}  
```

#### 优缺点  
##### 优点  
+ 结构清晰, 把策略分离成一个个单独的类, 替换了传统的 **if - else**  
+ 代码耦合度降低, 各个策略的细节被屏蔽, 安全性提高.  

##### 缺点  
+ 客户端必须要知道所有的策略类, 否则你不知道该使用那个策略, 所以策略模式适用于提前知道所有策略的情况下.  
+ 每一个策略类复用性很小, 如果需要增加算法, 就只能新增类, 策略类数量增多.  

#### 策略模式 和 简单工厂模式 的异同  
策略模式 和 简单工厂模式 看起来非常相似, 都是通过多态来实现不同子类的选取, 这种思想应该是从程序的整体来看得出的.  

如果从使用这两种模式的角度来看的话, 我们会发现在简单工厂模式中我们只需要传递相应的条件就能得到想要的一个对象, 然后通过这个对象实现算法的操作. 而策略模式, 使用时必须首先创建一个想使用的类对象, 然后将该对象最为参数传递进去, 通过该对象调用不同的算法.  

**在简单工厂模式中实现了通过条件选取一个类去实例化对象, 策略模式则将选取相应对象的工作交给模式的使用者, 它本身不去做选取工作.**  

#### 总结  
策略模式, **实质就是封装了一些算法, 让算法可以互相替换**, 用户可以自由选择这些算法进行操作, 在实际应用中其本身主要结合工厂模式一起使用.  

***  

### 访问者模式  
#### 简介  
访问者模式, 表示一个作用于其对象结构中的各元素的操作, 它使你可以在不改变各元素类的前提下定义作用于这些元素的新操作. 可以这么理解: 有这么一个操作, 它是作用于一些元素之上的, 而这些元素属于某一个对象结构. 同时这个操作是在不改变各元素类的前提下, 在这个前提下定义新操作是访问者模式精髓中的精髓.  

访问者模式 主要解决**稳定的数据结构和易变的操作耦合问题. 就是把数据结构和作用于结构上的操作解耦合, 使得操作集合可相对自由地演化.**  

访问者模式的实现主要就是**通过预先定义好调用的通路,  在被访问的对象上定义** ***accept*** **方法, 在访问者的对象上定义** ***visit*** **方法, 然后在调用真正发生的时候, 通过两次分发的技术, 利用预先定义好的通路, 回调到访问者具体的实现上.**  

#### 结构  
访问者模式主要有以下角色:  
+ **抽象访问者接口(Vistor)**  
+ **具体访问者接口(Concrete Vistor)**  
+ **抽象元素角色(Element)**  
+ **具体元素角色(Concrete Element)**  
+ **结构对象角色(Object Structure)**  

##### 抽象访问者接口  
它定义了对每一个元素访问的行为, 它的参数就是可以访问的元素, 它的方法个数理论上来讲与元素个数是一样的, 从这点不难看出, 访问者模式要求元素类的个数不能改变, **不能改变的意思是说, 如果元素类的个数经常改变, 则说明不适合使用访问者模式**  
##### 具体访问者接口  
它需要给出对每一个元素类访问时所产生的具体行为.  
##### 抽象元素角色  
它定义了一个接受访问者 *accept* 的方法, 其意义是指: 每一个元素都要可以被访问者访问.  
##### 具体元素角色  
它提供接受访问方法的具体实现, 而这个具体的实现, 通常情况下是使用访问者提供的访问该元素类的方法.  
##### 结构对象角色  
这个便是定义当中所提到的对象结构, 对象结构是一个抽象表述, 具体点可以理解为一个具有容器性质或者复合对象特性的类, 它会含有一组元素, 并且可以迭代这些元素, 供访问者访问.  

#### 应用场景  
+ 对象结构比较稳定, 但经常需要在此对象结构上定义新的操作.  
+ 需要对一个对象结构中的对象进行很多不同的且不相关的操作, 而需要避免这些操作"污染"这些对象的类, 也不希望在增加新操作时修改这些类.  

#### 实例  
**抽象访问者类定义** (***Action.h***) 
```  
#ifndef _ACTION_H_
#define _ACTION_H_

#include "Man.h"
#include "Woman.h"

class Action {

public:

  Action();
  ~Action();

  virtual void PerformanceMan(Man* man) = 0;
  virtual void PerformanceWoman(Woman* woman) = 0;

};

#endif  
```  
**抽象访问者类实现** (***Action.cpp***)  
```  
#include "Action.h"  

Action::Action() {

}

Action::~Action() {
  
}  
```  
**具体访问者类定义** (***Success.h***)  
```  
#ifndef _SUCCESS_H_  
#define _SUCCESS_H_

#include "Action.h"

class Success: public Action {

public:

  Success();

  void PerformanceMan(Man* man) override;
  void PerformanceWoman(Woman* woman) override;

private:

  std::string m_Type;

};

#endif
```  
**具体访问者类实现** (***Success.cpp***)   
```  
#include "Success.h"

Success::Success()
       : m_Type("成功") {

}

void Success::PerformanceMan(Man* man) {

    std::cout << man->Type() << this->m_Type << "时候的表现" << std::endl;

}

void Success::PerformanceWoman(Woman* woman) {

    std::cout << woman->Type() << this->m_Type << "时候的表现" << std::endl;

}  
```
**抽象元素类定义** (***Person.h***)  
```  
#ifndef _PERSON_H_  
#define _PERSON_H_

#include <bits/stdc++.h>

class Action;

class Person {

public:

  Person();
  virtual void Accept(Action* action);

};

#endif  
```  
**抽象元素类实现** (***Person.cpp***)  
```  
#include "Person.h"

Person::Person() {
  
}

void Person::Accept(Action* action) {
  
}  
```
**具体元素类定义** (***Man.h***) 
```  
#ifndef _MAN_H_
#define _MAN_H_

#include "Person.h"

class Man: public Person {

public:

  Man();
  void Accept(Action* action) override;
  std::string Type();

private:

  std::string m_Type;

};

#endif  
```
**具体元素类实现** (***Man.cpp***) 
```  
#include "Man.h"
#include "Action.h"  

Man::Man()
   : m_Type("男人") {

}

void Man::Accept(Action* action) {

    action->PerformanceMan(this);

}

std::string Man::Type() {

    return m_Type;

}  
```
**具体元素类定义** (***Woman.h***)   
```  
#ifndef _WOMAN_H_
#define _WOMAN_H_

#include "Person.h" 

class Woman: public Person {

public:

  Woman();
  void Accept(Action* action) override;
  std::string Type();

private:

  std::string m_Type;

};

#endif  
```
**具体元素类实现** (***Woman.cpp***)  
```  
#include "Woman.h"
#include "Action.h"  

Woman::Woman()
     : m_Type("女人") {

}

void Woman::Accept(Action* action) {

    action->PerformanceWoman(this);

}

std::string Woman::Type() {

    return m_Type;

}  
```
**结构对象类定义** (***ObjectStruct.h***) 
```  
#ifndef _OBJECTSTRUCT_H_
#define _OBJECTSTRUCT_H_

#include "Person.h"
#include "Action.h"

class ObjectStruct {

public:

  ObjectStruct();

  void Display(Action* action);

  void Attach(Person* person);
  void Detach(Person* person);

  std::list<Person*> m_Person;

};

#endif
```  
**结构对象类实现** (***ObjectStruct.cpp***)  
```  
#include "ObjectStruct.h"

ObjectStruct::ObjectStruct() {

}

void ObjectStruct::Display(Action* action) {

    for (auto person : m_Person) {

        person->Accept(action);

    }

}

void ObjectStruct::Attach(Person* person) {

    m_Person.push_back(person);

}

void ObjectStruct::Detach(Person* person) {

    m_Person.remove(person);

}  
```
**应用** (***main.cpp***)  
```  
#include "ObjectStruct.h"
#include "Man.h"
#include "Woman.h"
#include "Success.h"

int main() {

    ObjectStruct obj;

    Man* man = new Man();
    obj.Attach(man);

    Woman* woman = new Woman();
    obj.Attach(woman);

    Success* success = new Success();
    obj.Display(success);

    return 0;

}  
```

#### 优缺点  
##### 优点  
+ 访问者模式使得易于增加新的操作, 访问者使得增加依赖于复杂对象结构的构件的操作变得容易了. 仅需增加一个新的访问者即可在一个对象结构上定义一个新的操作. 相反, 如果每个功能都分散在多个类之上的话, 定义新的操作时必须修改每一类.  
+ 访问者集中相关的操作而分离无关的操作. 相关的行为不是分布在定义该对象结构的各个类上, 而是集中在一个访问者中. 无关行为却被分别放在它们各自的访问者子类中, 这就既简化了这些元素的类, 也简化了在这些访问者中定义的算法. 所有与它的算法相关的数据结构都可以被隐藏在访问者中.  

##### 缺点  
+ **增加新的 ConcreteElement 类很困难**. 
  Visitor模式 使得难以增加新的 Element 的子类. 每添加一个新的 ConcreteElement 都要在 Vistor 中添加一个新的抽象操作, 并在每一个 ConcretVisitor 类中实现相应的操作. 有时可以在 Visitor 中提供一个缺省的实现, 这一实现可以被大多数的 ConcreteVisitor 继承, 但这与其说是一个规律还不如说是一种例外.  

  所以在应用访问者模式时考虑关键的问题是系统的哪个部分会经常变化, 
  是作用于对象结构上的算法呢还是构成该结构的各个对象的类. 如果老是有新的 ConcretElement 类加入进来的话, Vistor 类层次将变得难以维护. 在这种情况下,直接在构成该结构的类中定义这些操作可能更容易一些. 如果 Element 类层次是稳定的, 而你不断地增加操作获修改算法, 访问者模式可以帮助你管理这些改动.

+  **破坏封装**  
  访问者方法假定 Concrete Element 接口的功能足够强, 足以让访问者进行它们的工作. 结果是该模式常常迫使你提供访问元素内部状态的公共操作, 这可能会破坏它的封装性.  

***  

### 职责链模式  
##### 简介  
职责链模式, 使多个对象都有机会处理请求, 从而避免了请求的发送者和接受者之间的耦合关系. 为请求创建了一个接收者对象的链, 并沿着这条链传递该请求, 直到有对象处理它为止, 其过程实际上是一个递归调用.  

客户端发出一个请求, 链上的对象都有机会来处理这一请求, 而客户端不需要知道谁是具体的处理对象, 并且在客户端可以实现动态的组合职责链, 使编程更有灵活性.  

要点主要是:  
+ 有多个对象共同对一个任务进行处理  
+ 这些对象使用链式存储结构, 形成一个链, 每个对象知道自己的下一个对象.  
+ 一个对象对任务进行处理, 可以添加一些操作后将对象传递个下一个任务, 也可以在此对象上结束任务的处理, 并结束任务.  
+ 客户端负责组装链式结构, 但是客户端不需要关心最终是谁来处理了任务.   

##### 结构  
职责链模式主要有以下角色:  
+ **抽象处理者角色(Handler)**  
+ **具体处理者角色(Concrete Handler)**  

###### 抽象处理者角色  
定义出一个处理请求的接口, 如果需要, 接口可以定义出一个方法以设定和返回对下家的引用.  
###### 具体处理者角色  
具体处理者接到请求后, 可以选择将请求处理掉, 或者将请求传给下家. 由于具体处理者持有对下家的引用, 因此如果需要, 具体处理者可以访问下家.  

#### 应用场景  
+ 如果有多个对象可以处理同一个请求, 但是具体由哪个对象处理是由运行时刻动态决定的, 这种对象就可以使用职责链模式, 把处理请求的对象实现成职责对象, 然后构造链, 当请求在这个链中传递的时候, 会根据运行状态判断.  
+ 在请求处理者不明确的情况下向多个对象中的一个提交请求.  
+ 需要动态指定处理一个请求的对象集合.  

#### 实例  
以 公司审批流程 为例子.  
**抽象处理者类定义** (***Manager.h***) 
```  
#ifndef _MANAGER_H_  
#define _MANAGER_H_

#include "Request.h"

class Manager{

public:

  Manager();
  virtual ~Manager();

  virtual void DoRequest(Request &Request) = 0;

  void SetLastLevel(Manager* manager);

protected:

  Manager* m_Last;

};

#endif  
```
**抽象处理者类实现** (***Manager.cpp***)  
```  
#include "Manager.h"  

Manager::Manager() {

}

Manager::~Manager() {

}

void Manager::SetLastLevel(Manager* manager) {

    m_Last = manager;

}
```
**具体处理者类定义** (***ManagerP.h***)  
```  
#ifndef _MANAGERP_H_  
#define _MANAGERP_H_

#include "Manager.h"

class ManagerP: public Manager {

public:

  ManagerP();
  void DoRequest(Request &request) override;

};

#endif  
```
**具体处理者类实现** (***ManagerP.cpp***)  
``` 
#include "ManagerP.h"

ManagerP::ManagerP() {

}

void ManagerP::DoRequest(Request &request) {

    if (request.Type() == "请假" && 
        request.Count() < 6) {

        std::cout << "经理批准请假" << std::endl;

    }else if (request.Type() == "加薪" &&
              request.Count() < 200) {

        std::cout << "经理批准加薪" << std::endl;

    }else {

        if (m_Last) {
            m_Last->DoRequest(request);
        }

    }

}
```
**具体处理者类定义** (***Director.h***)  
```  
#ifndef _DIRECTOR_H_
#define _DIRECTOR_H_

#include "Manager.h"

class Director: public Manager {

public:

  Director();
  void DoRequest(Request &request) override;

};

#endif
```  
**具体处理者类实现** (***Director.cpp***)  
```  
#include "Director.h"

Director::Director() {

}

void Director::DoRequest(Request &request) {

    if (request.Type() == "请假" &&
        request.Count() < 11) {

        std::cout << "总监批准请假" << std::endl;

    }else if (request.Type() == "加薪" &&
              request.Count() < 1000) {

        std::cout << "总监批准加薪" << std::endl;

    }else {

        if (m_Last) {
            m_Last->DoRequest(request);
        }

    }

}  
```
**具体处理者类定义** (***GeneralManager.h***)  
```  
#ifndef _GENERALMANAGER_H_
#define _GENERALMANAGER_H_

#include "Manager.h"

class GeneralManager: public Manager {

public:

  GeneralManager();
  void DoRequest(Request &request) override;

};

#endif  
```
**具体处理者类实现** (***GeneralManager.cpp***)  
```  
#include "GeneralManager.h"

GeneralManager::GeneralManager() {

}

void GeneralManager::DoRequest(Request &request) {

    if (request.Type() == "请假" &&
        request.Count() < 8) {

        std::cout << "总经理批准请假" << std::endl;

    }else if (request.Type() == "加薪" &&
              request.Count() < 500) {

        std::cout << "总经理批准加薪" << std::endl;

    }

}  
```
**请求类定义** (***Request.h***)  
```  
#ifndef _REQUEST_H_  
#define _REQUEST_H_

#include <bits/stdc++.h>

class Request {

public:

  Request(std::string type, std::string content, int count);

  std::string Type();
  std::string Content();

  int Count();

private:

  std::string   m_Type;
  std::string   m_Content;
  int           m_Count;

};

#endif
```
**请求类实现** (***Request.cpp***)  
```  
#include "Request.h"

Request::Request(std::string type, std::string content, int count)
       : m_Type(type),
         m_Content(content),
         m_Count(count) {

}

std::string Request::Type() {

    return m_Type;

}

std::string Request::Content() {

    return m_Content;

}

int Request::Count() {

    return m_Count;

}  
```

#### 优缺点  
##### 优点  
职责链模式的最主要功能就是: **动态组合, 请求者 和 接受者解耦.**  
+ **请求者和接受者松散耦合**  
  请求者不需要知道接受者, 也不需要知道如何处理. 每个职责对象只负责自己的职责范围, 其他的交给后继者, 各个组件间完全解耦.  

+ **动态组合职责**  
  职责链模式会把功能分散到单独的职责对象中, 然后在使用时动态的组合形成链, 从而可以灵活的分配职责对象, 也可以灵活的添加改变对象职责.  

##### 缺点  
+ **产生很多细粒度的对象**  
  因为功能处理都分散到了单独的职责对象中, 每个对象功能单一, 要把整个流程处理完, 需要很多的职责对象, 会产生大量的细粒度职责对象.  

+ **不一定能处理**  
  每个职责对象都只负责自己的部分, 这样就可以出现某个请求, 即使把整个链走完, 都没有职责对象处理它. 这就需要提供默认处理, 并且注意构造链的有效性.  

#### 总结  
对于责任链中的一个处理者对象, 有两个行为: **一是处理请求, 二是将请求传递到下一节点.**, 不允许某个处理者对象在处理了请求后又将请求传送给上一个节点的情况.   

对于一条责任链来说, 一个请求最终只有两种情况: **一是被某个处理对象所处理, 另一个是所有对象均未对其处理**, 对于前一种情况我们称为**纯的责任链模式**, 后一种为**不纯的责任链**. 实际中大多为不纯的责任链.  

***  

### 中介者模式  
#### 简介  
中介者模式, 是用来降低多个对象和类之间的通信复杂性. **用一个中介对象来封装一系列的对象交互, 中介者使各对象不需要显式地相互引用, 从而使其耦合松散, 而且可以独立地改变它们之间的交互.**  

中介者作为一种行为设计模式, 它公开一个统一的接口, 系统的不同对象或组件可以通过该接口进行通信. 增加一个中介者对象后, 所有的相关对象通过中介者对象来通信, 而不是互相引用, 所以当一个对象发生改变时, 只需要通知中介者对象即可.  

#### 结构  
中介者模式, 主要有以下角色:  
+ **中介者接口(Mediator)**  
+ **具体中介者实现对象(Concrete Mediator)**  
+ **同事类(Colleague)**  
+ **具体的同事类(Concrete Colleague)**  

##### 中介者接口  
在里面定义各个同事之间交互需要的方法, 可以是公共的通讯方法, 也可以是小范围的交互方式.  
##### 具体中介者实现对象  
它需要了解并维护各个同事对象, 并负责具体的协调各同事对象的交互关系.  
##### 同事类  
主要负责约束同事对象的类型, 并实现一些具体同事类之间的公共功能.  
##### 具体的同事类  
实现自己的业务, 在需要与其它同事通讯的时候, 就与持有的中介者通信, 中介者会负责与其它的同事交互.  

#### 应用场景  
+ 一组定义良好的对象, 现在要进行复杂的通信.  
+ 定制一个分布在多个类中的行为, 而又不想生成太多的子类.  

**中介对象主要是用来封装行为的, 行为的参与者就是那些对象, 但是通过中介者, 这些对象不用相互知道.**  

#### 实例  
建立一个中介媒体, 为两位同事传话.  
**具体中介者实现对象类定义** (***Mediator.h***) 
```  
#ifndef _MEDIATOR_H_
#define _MEDIATOR_H_

#include <bits/stdc++.h>

class Colleague;
class Mediator {

public:

  Mediator();
  void Send(std::string message, Colleague* colleague);

  void SetColleague1(Colleague* colleague);
  void SetColleague2(Colleague* colleague);

private:

  Colleague* m_Colleague1;
  Colleague* m_Colleague2;

};

#endif  
```
**具体中介者实现对象类实现** (***Mediator.cpp***) 
```  
#include "Mediator.h"
#include "Colleague.h"
#include "ConcreteColleague.h"

Mediator::Mediator() {

}

void Mediator::Send(std::string message, Colleague* colleague) {

    if (colleague == m_Colleague1) {

        m_Colleague2->Notify(message);

    }else {

        m_Colleague1->Notify(message);

    }

}

void Mediator::SetColleague1(Colleague* colleague) {

    m_Colleague1 = colleague;

}

void Mediator::SetColleague2(Colleague* colleague) {

    m_Colleague2 = colleague;

}  
```
**同事类定义** (***Colleague.h***)  
```  
#ifndef _COLLEAGUE_H_  
#define _COLLEAGUE_H_

#include <bits/stdc++.h>
#include "Mediator.h"

//class Mediator;
class Colleague {

public:

  Colleague();
  Colleague(Mediator* mediator);
  virtual ~Colleague();

  virtual void Notify(std::string message) = 0;
  virtual void Send(std::string message) = 0;

private:

  Mediator*  m_Mediator;

};

#endif  
```
**同事类实现** (***Colleague.cpp***)  
```  
#include "Colleague.h"
#include "Mediator.h"

Colleague::Colleague() {

}


Colleague::Colleague(Mediator* mediator) {

    m_Mediator = mediator;

}

Colleague::~Colleague() {

}
```  
**具体的同事类定义** (***ConcreteColleague.h***)  
```  
#ifndef _CONCRETECOLLEAGUE_H_  
#define _CONCRETECOLLEAGUE_H_

#include "Colleague.h"

class ConcreteColleague1: public Colleague {

public:

  ConcreteColleague1(Mediator* mediator);
  void Send(std::string message) override;
  void Notify(std::string message) override;

private:

  Mediator*  m_Mediator;

};

class ConcreteColleague2: public Colleague {

public:

  ConcreteColleague2(Mediator* mediator);
  void Send(std::string message) override;
  void Notify(std::string message) override;

private:

  Mediator*  m_Mediator;

};

#endif  
```
**具体的同事类实现** (***ConcreteColleague.cpp***)  
```  
#include "ConcreteColleague.h"

ConcreteColleague1::ConcreteColleague1(Mediator* mediator) {

    m_Mediator = mediator;

}

void ConcreteColleague1::Send(std::string message) {

    m_Mediator->Send(message, this);

}

void ConcreteColleague1::Notify(std::string message) {

    std::cout << "同事1得到消息: " << message << std::endl;

}

ConcreteColleague2::ConcreteColleague2(Mediator* mediator) {

    m_Mediator = mediator;

}

void ConcreteColleague2::Send(std::string message) {

    m_Mediator->Send(message, this);

}

void ConcreteColleague2::Notify(std::string message) {

    std::cout << "同事2得到消息: " << message << std::endl;

}  
```
**应用** (***main.cpp***)  
```  
#include "ConcreteColleague.h"
#include "Mediator.h"

int main() {

    Mediator* mediator = new Mediator();

    Colleague* c1 = new ConcreteColleague1(mediator);
    Colleague* c2 = new ConcreteColleague2(mediator);

    mediator->SetColleague1(c1);
    mediator->SetColleague2(c2);

    c1->Send("吃了吗?");
    c2->Send("我吃过了, 你呢?");

    return 0;

}  
```

#### 优缺点  
##### 优点  
+ 减少各个 Colleague 之间的耦合, 使得可以独立地改变和复用各个 Colleague类 和 Mediator.  

##### 缺点  
+ 中介者模式的缺点是显而易见的, 因为这个 "中介" 承担了较多的责任, 所以一旦这个中介对象出现了问题, 那么整个系统就会受到重大的影响.  

***  

### 观察者模式  
#### 简介  
观察者模式 又叫做 **发布-订阅模式, 模型-视图模式, 源-监听器模式, 从属者模式**. 在许多设计中, 经常涉及多个对象都对一个特殊对象中的数据变化感兴趣, 而且这多个对象都希望跟踪那个特殊对象中的数据变化, 也就是说当对象间存在一对多关系时, 在这样的情况下就可以使用 观察者模式. **当一个对象被修改时, 则会自动通知它的依赖对象.**  

观察者模式, 是关于多个对象想知道一个对象中数据变化情况的一种成熟的模式. 观察者模式中有一个称作 "主题" 的对象和若干个称作 "观察者" 的对象, "主题" 和 "观察者" 间是一种一对多的依赖关系, 当 "主题" 的状态发生变化时, 所有 "观察者" 都得到通知.  

**主要解决: 一个对象状态改变给其他对象通知的问题, 而且要考虑到易用和低耦合, 保证高度的协作.**  

#### 结构  
观察者模式的结构中包含四种角色:  
+ **主题(Subject)**  
+ **观察者(Observer)**  
+ **具体主题(Concrete Subject)**  
+ **具体观察者(Concrete Observer)**  

##### 主题  
主题是一个接口, 该接口规定了具体主题需要实现的方法, 比如: 添加, 删除观察者以及通知观察者更新数据的方法.  
##### 观察者  
观察者是一个接口, 该接口规定了具体观察者用来更新数据的方法.  
##### 具体主题  
具体主题是实现主题接口类的一个实例, 该实例包含有可以经常发生变化的数据, 具体主题需使用一个集合, 比如 ArrayList, 存放观察者的引用, 以便数据变化时通知具体观察者.  
##### 具体观察者  
具体观察者是实现观察者接口类的一个实例. 具体观察者包含有可以存放具体主题引用的主题接口变量, 以便具体观察者让具体主题将自己的引用添加到具体主题的集合中, 使自己成为它的观察者, 或让这个具体主题将自己从具体主题的集合中删除, 使自己不再是它的观察者.  

#### 应用场景  
+ 当一个对象的数据更新时需要通知其他对象, 但这个对象又不希望和被通知的那些对象形成紧耦合.  
+ 当一个对象的数据更新时, 这个对象需要让其他对象也各自更新自己的数据, 但这个对象不知道具体有多少对象需要更新数据.  

观察者模式在实际项目的应用中非常常见, 比如: 你到 ATM 机器上取钱, 多次输错密码, 卡就会被 ATM 吞掉, 吞卡动作发生的时候会触发哪些事件呢?   

第一, 摄像头连续快拍;  
第二, 通知监控系统, 吞卡发生;  
第三, 初始化 ATM 机屏幕, 返回最初状态.  

一般前两个动作都是通过观察者模式来完成的, 观察者可以实现消息的广播, **一个消息可以触发多个事件**, 这是观察者模式非常重要的功能.  

使用观察者模式也有两个重点问题要解决:  
+ **广播链的问题** 在一个观察者模式中最多出现一个对象既是观察者也是被观察者, 也就是说消息最多转发一次.  
+ **异步处理问题** 被观察者发生动作了, 观察者要做出回应, 如果观察者比较多, 就要用异步处理, 异步处理就要考虑线程安全和队列的问题.  

#### 实例  
王者荣耀中, 牛魔 使用了大闪, 李白 和 香香 分别用了自己的位移技能进行了躲避.  
**主题类/具体主题类定义** (***Subject.h***)  
```  
#ifndef _SUBJECT_H_  
#define _SUBJECT_H_

#include <bits/stdc++.h>
#include "ObServer.h"

class Subject {

public:

    Subject(std::string name);
    virtual ~Subject();

    virtual void Attach(ObServer* obs);
    virtual void Detach(ObServer* obs);
    virtual void Notify();

    virtual void SetAction(std::string act);
    virtual std::string GetAction();

private:

    std::string m_Name;
    std::string m_Action;
    std::vector<ObServer*> m_ObServerVec;

};

class ConcretSubject: public Subject {

public:  

    ConcretSubject(std::string name);
    virtual ~ConcretSubject();

};

#endif  
```  
**主题类/具体主题类实现** (***Subject.cpp***)  
```  
#include "Subject.h"
#include "ObServer.h"

Subject::Subject(std::string name) {

    m_Name = name;

}

Subject::~Subject() {

}

void Subject::Attach(ObServer* obs) {

    m_ObServerVec.push_back(obs);

}

void Subject::Detach(ObServer* obs) {

    for (auto it = m_ObServerVec.begin(); it != m_ObServerVec.end(); it++) {

        if (*it == obs) {
            m_ObServerVec.erase(it);
        }

    }

}

void Subject::Notify() {

    for (ObServer* obs : m_ObServerVec) {

        obs->Update(this);

    }

}

void Subject::SetAction(std::string act) {

    m_Action = act;

}

std::string Subject::GetAction() {

    return m_Action;

}

ConcretSubject::ConcretSubject(std::string name) 
              : Subject(name) {

}

ConcretSubject::~ConcretSubject() {
  
}  
```  
**观察者类/具体观察者类定义** (***ObServer.h***)  
```  
#ifndef _OBSERVER_H_  
#define _OBSERVER_H_

#include <bits/stdc++.h>

class Subject;
class ObServer {

public:

  ObServer(std::string name);
  virtual ~ObServer();

  virtual void Update(Subject* obj);

protected:

  std::string m_Name;

};

class ConcretObServer: public ObServer {

public:

  ConcretObServer(std::string name);
  virtual ~ConcretObServer();

};

#endif  
```  
**观察者类/具体观察者类实现** (***ObServer.cpp***)  
```  
#include "ObServer.h"
#include "Subject.h"

ObServer::ObServer(std::string name) {

    m_Name = name;

}

ObServer::~ObServer() {

}

void ObServer::Update(Subject* obj) {

    std::string action;

    if (m_Name == "李白") {

        action = "使用灵动的一技能快速避开";

    }else if (m_Name == "香香") {

        action = "使用翻滚，并走位找机会打出翻滚之后的加成伤害";

    }

    std::cout << m_Name << " " << action << std::endl;

}

ConcretObServer::ConcretObServer(std::string name) 
               : ObServer(name) {

}

ConcretObServer::~ConcretObServer() {
  
}  
```  
**应用** (***main.cpp***)  
```  
#include "ObServer.h"
#include "Subject.h"

int main() {

    Subject* sub = new ConcretSubject("敌方牛魔");

    ObServer* obj1 = new ConcretObServer("李白");
    ObServer* obj2 = new ConcretObServer("香香");

    sub->Attach(obj1);
    sub->Attach(obj2);

    sub->SetAction("使用了大闪");
    sub->Notify();

    return 0;

}  
```  

#### 优缺点  
##### 优点  
+ 具体主题和具体观察者是松耦合关系. 由于主题接口仅仅依赖于观察者接口, 因此具体主题只是知道它的观察者是实现观察者接口的某个类的实例, 但不需要知道具体是哪个类. 由于观察者仅仅依赖于主题接口, 因此具体观察者只是知道它依赖的主题是实现主题接口的某个类的实例, 但不需要知道具体是哪个类.  
+ 观察者模式满足 "开闭原则", 主题接口仅仅依赖于观察者接口, 这样就可以让创建具体主题的类也仅仅是依赖于观察者接口. 因此, 如果增加新的实现观察者接口的类, 不必修改创建具体主题的类的代码. 同样, 创建具体观察者的类仅仅依赖于主题接口, 如果增加新的实现主题接口的类, 也不必修改创建具体观察者类的代码.   

##### 缺点  
+ 如果一个被观察者对象有很多的直接和间接的观察者的话, 将所有的观察者都通知到会花费很多时间.  
+ 如果在观察者和观察目标之间有循环依赖的话, 观察目标会触发它们之间进行循环调用, 可能导致系统崩溃.  
+ 观察者模式没有相应的机制让观察者知道所观察的目标对象是怎么发生变化的, 而仅仅只是知道观察目标发生了变化.  

#### 观察者模式 与 中介模式 的异同  
+ 中介者模式与业务相关, 观察者模式与业务无关.  
+ 两个模式都有集中调度效果, 对象之间不直接参与通信.  

#### 总结  
实现观察者模式的时候要注意, **观察者和被观察对象之间的互动关系不能体现成类之间的直接调用, 否则就将使观察者和被观察对象之间紧密的耦合起来, 从根本上违反面向对象的设计的原则**. 无论是 观察者 "观察" 观察对象, 还是被 观察者 将自己的改变 "通知" 观察者, 都不应该直接调用.

***  

### 备忘录模式  
#### 简介  
备忘录模式, 又叫做 **快照模式** 或 **Token模式**, 是 GOF 的23种设计模式之一, 属于行为模式. 意图是在不破坏封闭的前提下, 捕获一个对象的内部状态, 并在该对象之外保存这个状态. 这样以后就可将该对象恢复到原先保存的状态. 该模式用于保存对象当前状态, 并且在之后可以再次恢复到此状态. 备忘录模式实现的方式**需要保证被保存的对象状态不能被对象从外部访问, 目的是为了保护被保存的这些对象状态的完整性以及内部实现不向外暴露**.  

#### 结构  
备忘录模式 主要有以下主要角色:  
+ **发起人(Originator)**  
+ **备忘录(Memento)**  
+ **管理者(Caretaker)**  

##### 发起人  
负责创建一个备忘录 Memento, 用以记录当前时刻自身的内部状态, 并可使用备忘录恢复内部状态. Originator 可以根据需要决定 Memento 存储自己的哪些内部状态.  
##### 备忘录  
负责存储 Originator 对象的内部状态, 并可以防止 Originator 以外的其他对象访问备忘录. 备忘录有两个接口: **Caretaker 只能看到备忘录的窄接口, 他只能将备忘录传递给其他对象; Originator 却可看到备忘录的宽接口, 允许它访问返回到先前状态所需要的所有数据.**  
##### 管理者  
负责备忘录 Memento, 不能对 Memento 的内容进行访问或者操作.  

#### 应用场景  
+ 需要保存和恢复数据的相关状态场景.  
+ 提供一个可回滚(rollback)的操作.  
+ 数据库连接的事务管理就是用的备忘录模式.  

#### 实例  
模拟象棋悔棋的操作, 支持多次悔棋以及撤销悔棋, 支持悔棋后重新落子, 采用两个栈存储备忘录.  
**发起人类定义** (***Originator.h***)  
```  
#ifndef _ORIGINATOR_H_
#define _ORIGINATOR_H_

#include <bits/stdc++.h>
#include "Memento.h"

class ChessMemento;
class ChessMan {

public:  

    ChessMan(const std::string &type, int x, int y);
    ~ChessMan();

    void moveX(int i);
    void moveY(int i);

    int getX();
    int getY();

    std::string getType();

    ChessMemento* save();

    void restore(ChessMemento* memento);

private:

    std::string type;
    int x;
    int y;

};

#endif
```  
**发起人类实现** (***Originator.cpp***)  
```  
#include "Originator.h"  

ChessMan::ChessMan(const std::string &type, int x, int y) {

    this->type = type;
    this->x = x;
    this->y = y;

    std::cout << "ChessMan Hello, Type = " 
    << this->type << " X = " << this->x << " Y = " << this->y << std::endl;

}

ChessMan::~ChessMan() {

}

void ChessMan::moveX(int i) {

    this->x = i;

}

void ChessMan::moveY(int i) {

    this->y = i;

}

int ChessMan::getX() {

    return x;

}

int ChessMan::getY() {

    return y;

}

std::string ChessMan::getType() {

    return type;

}

ChessMemento* ChessMan::save() {


    return new ChessMemento(type, x, y);

}

void ChessMan::restore(ChessMemento* memento) {

    type = memento->getType();
    x = memento->getX();
    y = memento->getY();

}  
```
**备忘录类定义** (***Memento.h***)   
```  
#ifndef _MEMENTO_H_
#define _MEMENTO_H_

#include <bits/stdc++.h>

class ChessMemento {

public:  

    ChessMemento(const std::string &type, int x, int y);
    ~ChessMemento();

    int getX();
    int getY();

    std::string getType();

private:  

    std::string type;  
    int x;
    int y;

};

#endif  
```
**备忘录类实现** (***Memento.cpp***) 
```  
#include "Memento.h"

ChessMemento::ChessMemento(const std::string &type, int x, int y) {

    this->type = type;
    this->x = x;
    this->y = y;
    std::cout << "ChessMemento Hello, Type = " << this->type << " X = " << this->x << " Y = "
    << this->y << std::endl;

}

ChessMemento::~ChessMemento() {

    std::cout << "ChessMemento Bye, Type = " << this->type << " X = " << this->x << " Y = "
    << this->y << std::endl;

}

int ChessMemento::getX() {

    return x;

}

int ChessMemento::getY() {

    return y;

}

std::string ChessMemento::getType() {

    return type;

}  
```
**管理者类定义** (***Caretaker.h***) 
```  
#ifndef _CARETAKER_H_  
#define _CARETAKER_H_  

#include <bits/stdc++.h>
#include "Memento.h"

class ChessCaretaker {

public:  

    ChessCaretaker();
    ~ChessCaretaker();

    void saveMemento(ChessMemento* memento);
    ChessMemento* undoMemento();
    ChessMemento* redoMemento();

    ChessMemento* showCurrent();

private:  

    static void releaseStack(std::stack<ChessMemento*> &stack) {

        while (!stack.empty()) {

            delete stack.top();
            stack.pop();

        }

    };

    std::stack<ChessMemento*> undoStack;
    std::stack<ChessMemento*> redoStack;

};

#endif
```
**管理者类实现** (***Caretaker.cpp***)  
```  
#include "Caretaker.h"  

ChessCaretaker::ChessCaretaker() {

    std::cout << "ChessCaretaker Hello" << std::endl;

}

ChessCaretaker::~ChessCaretaker() {

   std::cout << "ChessCaretaker Bye" << std::endl;
   releaseStack(undoStack);
   releaseStack(redoStack); 

}

void ChessCaretaker::saveMemento(ChessMemento* memento) {

    undoStack.push(memento);
    releaseStack(redoStack);

}

ChessMemento* ChessCaretaker::undoMemento() {

    ChessMemento* memento = undoStack.top();
    undoStack.pop();
    redoStack.push(memento);
    return memento;

}

ChessMemento* ChessCaretaker::redoMemento() {

    ChessMemento* memento = redoStack.top();
    redoStack.pop();
    undoStack.push(memento);
    return memento;

}

ChessMemento* ChessCaretaker::showCurrent() {

    ChessMemento* memento = undoStack.top();
    return memento;

}
```
**棋手类定义** (***ChessPlayer.h***)  
```  
#ifndef _CHESSPLAYER_H_  
#define _CHESSPLAYER_H_  

#include "Originator.h"  
#include "Caretaker.h"

class ChessPlayer {

public:  

    ChessPlayer();
    ~ChessPlayer();

    void play(ChessMan* chess);
    void undo();
    void redo();  

private:  

    static void printChess(ChessMemento* memento) {

        std::cout << "棋子" << memento->getType() << "当前位置为: " << " X = "
        << memento->getX() << " X = " << memento->getY() << std::endl;

    }

    ChessCaretaker chessCaretaker;

};

#endif  
```
**棋手类实现** (***ChessPlayer.cpp***)  
```  
#include "ChessPlayer.h"  

ChessPlayer::ChessPlayer() {

    std::cout << "ChessPlayer Hello" << std::endl;

}

ChessPlayer::~ChessPlayer() {

    std::cout << "ChessPlayer Bye" << std::endl;

}

void ChessPlayer::play(ChessMan* chess) {

    std::cout << "棋子落子" << std::endl;
    ChessMemento* chessMemento = chess->save();
    chessCaretaker.saveMemento(chessMemento);

}

void ChessPlayer::undo() {

    std::cout << "棋手悔棋" << std::endl;
    ChessMemento* chessMemento = chessCaretaker.undoMemento();
    printChess(chessMemento);

}

void ChessPlayer::redo() {

    std::cout << "棋手重做" << std::endl;
    ChessMemento* chessMemento = chessCaretaker.redoMemento();
    printChess(chessMemento);

}  
```
**应用** (***main.cpp***)  
```  
#include "Caretaker.h" 
#include "ChessPlayer.h"    
#include "Memento.h"
#include "Originator.h"  

int main() {

    std::cout << "=====Memento Pattern Start=======" << std::endl;

    ChessPlayer player;

    ChessMan chessMan1("车", 0, 0);
    ChessMan chessMan2("马", 0, 1);

    chessMan2.moveX(2);
    chessMan2.moveY(2);
    player.play(&chessMan2);

    chessMan1.moveY(2);
    player.play(&chessMan1);

    chessMan2.moveX(3);
    chessMan2.moveY(4);
    player.play(&chessMan2);

    chessMan1.moveX(1);
    chessMan1.moveY(2);
    player.play(&chessMan1);

    std::cout << "=================================" << std::endl;

    player.undo();
    player.undo();
    player.redo();

    std::cout << "=================================" << std::endl;

    chessMan1.moveX(3);
    chessMan1.moveY(2);
    player.play(&chessMan1);

    std::cout << "======Memento Pattern End========" << std::endl;

    return 0;

}  
```

#### 优缺点  
##### 优点  
+ 有时一些发起人对象的内部信息必须保存在发起人对象以外的地方, 但是必须要由发起人对象自己读取, 这时使用备忘录模式可以把复杂的发起人内部信息对其他的对象屏蔽起来, 从而可以恰当地保持封装的边界.  
+ 本模式简化了 发起人 类, 发起人不再需要管理和保存其内部状态的一个个版本, 客户端可以自行管理他们所需要的这些状态的版本.  
+ 当发起人角色的状态改变的时候, 有可能这个状态无效, 这时候就可以使用暂时存储起来的备忘录将状态复原.  

##### 缺点  
+ 如果发起人角色的状态需要完整地存储到备忘录对象中, 那么在资源消耗上面备忘录对象会很昂贵.  
+ 当负责人角色将一个备忘录 存储起来的时候, 负责人可能并不知道这个状态会占用多大的存储空间, 从而无法提醒用户一个操作是否很昂贵.  
+ 当发起人角色的状态改变的时候, 有可能这个协议无效. 如果状态改变的成功率不高的话, 不如采取 "假如" 协议模式.  

*** 

### 状态模式  
#### 简介  
状态模式, 当一个对象的内在状态改变时允许改变其行为, 这个对象看起来像是改变了其类. 主要解决**当控制一个对象状态的条件表达式过于复杂时的情况**. 把状态的判断逻辑转移到表示不同状态的一系列类中, 可以把复杂的判断逻辑简化. 使用状态模式, 可以很好地避免过多的 **if - else** 分支, 状态模式将每一个状态分支放入一个独立的类中, 每一个状态对象都可以独立存在, 程序根据不同的状态使用不同的状态对象来实现功能.      
#### 结构  
状态模式, 主要有以下角色:  
+ **环境类角色**  
+ **抽象状态类/状态接口**  
+ **具体状态类**  

##### 环境类角色  
定义客户感兴趣的接口, 维护一个 State子类 的实例, 这个实例对应的是对象当前的状态.   
##### 抽象状态类/状态接口  
定义一个或者一组行为接口, 表示该状态下的行为动作.  
##### 具体状态类  
实现 State抽象类 中定义的接口方法, 从而达到不同状态下的不同行为.  

#### 应用场景  
+ 一个对象的行为取决于它的状态, 并且它必须在运行时刻根据状态改变它的行为.  
+ 一个操作中含有庞大的多分支结构, 并且这些分支决定于对象的状态.  

比如, 我们在微博上看到一篇文章, 觉得还不错, 于是想评论或者转发, 但如果用户没有登录, 这个时候就会先自动跳转到登录注册界面; 如果已经登录, 当然就可以直接评论或者转发了. 这里我们可以看到, 用户的行为是由当前是否登录这个状态来决定的, 这就是典型的状态模式情景.  

#### 实例  
一个成年人, 他每天都有 工作状态 或是 生活状态.  
**环境类定义** (***People.h***)  
```  
#ifndef _PEOPLE_H_
#define _PEOPLE_H_

#include "State.h"

class People {

public:

  People(State* state);
  void ChangeState(State* state);
  void Show();
  void SetTime(int time);
  int Time();

private:

  int m_Now;
  State* m_State;

};

#endif  
```
**环境类实现** (***People.cpp***)  
```  
#include "People.h"

People::People(State* state)
       :m_State(state) {

}

void People::ChangeState(State* state) {

    m_State = state;

}

void People::Show() {

    std::cout << "来吧, 展示!" << std::endl;
    std::cout << "当前状态:" << m_State->Content() << std::endl;

}

void People::SetTime(int time) {

    m_Now = time;
    m_State->Handle(this);

}

int People::Time() {

    return m_Now;

}  
```
**抽象状态类/状态接口类定义** (***State.h***)  
```  
#ifndef _STATE_H_
#define _STATE_H_


#include <bits/stdc++.h>

class People;

class State {

public:

  State();
  State(std::string context);
  virtual ~State();
  virtual void Handle(People* people) = 0;

  std::string Content();

protected:

  std::string m_Content;

private:

  int m_Time;

};

#endif  
```
**抽象状态类/状态接口类定义** (***State.cpp***)  
```  
#include "State.h"  

State::State() {

}

State::State(std::string content) {

    m_Content = content;

}

State::~State() {

}

std::string State::Content() {

    return m_Content;

}  
```
**具体状态类定义** (***WorkState.h***)   
```  
#ifndef _WORKSTATE_H_ 
#define _WORKSTATE_H_ 


#include "State.h"

class WorkState: public State {

public:

  WorkState(std::string content = "工作状态");
  void Handle(People* people) override;

};

#endif  
```
**具体状态类定义** (***WorkState.cpp***)  
```  
#include "lifestate.h"
#include "workstate.h"
#include "people.h"

WorkState::WorkState(std::string content)
          :State(content) {

}

void WorkState::Handle(People* people) {

    int time = people->Time();

    if (time < 9 || time > 18) {
        people->ChangeState(new LifeState());
    }

}  
```
**具体状态类定义** (***LifeState.h***)  
```  
#ifndef _LIFESTATE_H_  
#define _LIFESTATE_H_ 

#include "State.h"

class LifeState: public State {

public:

  LifeState(std::string content = "生活状态");
  void Handle(People* people) override;

};

#endif  
```  
**具体状态类定义** (***LifeState.cpp***)  
```  
#include "LifeState.h"
#include "WorkState.h"
#include "People.h"

LifeState::LifeState(std::string content)
          :State(content) {

}

void LifeState::Handle(People* people) {

    int time = people->Time();

    if (time > 9 && time < 18) {
        people->ChangeState(new WorkState());
    }

}  
```
**应用** (***main.cpp***)  
```  
#include "People.h"
#include "WorkState.h"
#include "LifeState.h"

using namespace std;

int main() {

    std::shared_ptr<State> state1 = std::make_shared<WorkState>();
    People p(state1.get());

    p.Show();
    p.SetTime(9);
    p.Show();
    p.SetTime(11);
    p.Show();
    p.SetTime(20);
    p.Show();

    return 0;

}  
```

#### 优缺点  
##### 优点  
+ 封装了转换规则.  
+ 枚举可能的状态, 在枚举状态之前需要确定状态种类.  
+ 将所有与某个状态有关的行为放到一个类中, 并且可以方便地增加新的状态, 只需要改变对象状态即可改变对象的行为.  
+ 允许状态转换逻辑与状态对象合成一体, 而不是某一个巨大的条件语句块.  
+ 可以让多个环境对象共享一个状态对象, 从而减少系统中对象的个数.  

##### 缺点  
+ 状态模式的使用必然会增加系统类和对象的个数.  
+ 状态模式的结构与实现都较为复杂, 如果使用不当将导致程序结构和代码的混乱.  
+ 状态模式对 "开闭原则" 的支持并不太好, 对于可以切换状态的状态模式, 增加新的状态类需要修改那些负责状态转换的源代码, 否则无法切换到新增状态; 而且修改某个状态类的行为也需修改对应类的源代码.  

##### 状态模式 和 策略模式 对比  
如果我们在编写代码的时候, 遇到大量的条件判断的时候, 可能会采用策略模式来优化结构, 因为这时涉及到策略的选择, 但有时候仔细查看下, 就会发现这些所谓的策略其实是对象的不同状态, 更加明显的是对象的某种状态也成为判断的条件.  

我们**把 Strategy接口 改个名字为 State, 这就是状态模式了**, 同样 Context 也有一个 State类型 的引用, 也将自己的部门功能委托给 State 来完成.  

要使用状态模式, 我们必须明确两个东西: **状态和每个状态下执行的动作**.  

在状态模式中, 因为所有的状态都要执行相应的动作, 所以我们可以考虑将状态抽象出来. 状态的抽象一般有两种形式: **接口和抽象类**. 如果所有的状态都有共同的数据域, 可以使用抽象类, 但如果只是单纯的执行动作, 就可以使用接口.  

他们之间真正的区别: **在 策略模式 对 Strategy 的具体实现类有绝对的控制权, 即 Context 要感知 Strategy 具体类型**. 而 **状态模式, Context 不感知 State 的具体实现, Context 只需调用自己的方法, 这个调用的方法会委托给 State 来完成, State 会在相应的方法调用时, 自动为 Context 设置状态, 而这个过程对 Context 来说是透明的, 不被感知的***.  

#### 总结  
在对象的行动取决于本身的状态时, 可以适用于状态模式, 免去了过多的 **if – else** 判断, 这对于一些复杂的和繁琐的判断逻辑有很好的帮助. **但是使用状态模式, 势必会造成更多的接口和类, 对于非常简单的状态判断, 可以不使用**.  

***  

### 命令模式  
#### 简介  
命令模式是一个**高内聚**的模式, 其定义为: **将一个请求封装成一个对象, 从而让你使用不同的请求把客户端参数化, 对请求排队或者记录请求日志, 可以提供命令的撤销和恢复功能**.  

#### 结构  
命令模式 主要有以下主要角色:  
+ **接受者角色(Receiver)**  
+ **命令角色(Command)**  
+ **调用者角色(Invoker)**  

##### 接受者角色  
该角色就是干活的角色, 命令传递到这里是应该被执行的.  
##### 命令角色  
需要执行的所有命令都在这里声明.  
##### 调用者角色  
接收到命令, 并执行命令.  

#### 应用场景  
当需要先将一个函数登记上, 然后再以后调用此函数时, 就需要使用命令模式, **其实这就是回调函数**.  

有时候需要向某些对象发送请求, 但是并不知道请求的接收者是谁, 也不知道被请求的操作是什么. 此时希望用一种松耦合的方式来设计程序, 使得请求发送者和请求接收者能够消除彼此之间的耦合关系.  

比如拿订餐来说, 客人需要向厨师发送请求, 但是完全不知道这些厨师炒菜的方式和步骤. 命令模式把客人订餐的请求封装成 command 对象, 也就是订餐中的订单对象. 这个对象可以在程序中被四处传递, 就像订单可以从服务员手中传到厨师的手中. 这样一来, 从而解开了请求调用者和请求接收者之间的耦合关系.  

#### 实例  
以客户订餐为例子.  
**抽象命令角色类定义** (***Command.h***)  
```  
#ifndef _COMMAND_H_
#define _COMMAND_H_

#include "Chef.h"

class Command {

public:

  Command(Chef* Chef);

  virtual ~Command();
  virtual void ExcuteCmd() = 0;

protected:

  Chef* m_Chef;

};

#endif  
```  
**抽象命令角色类实现** (***Command.cpp***)  
```  
#include "Command.h"

Command::Command(Chef* chef) {

    m_Chef = chef;

}

Command::~Command() {
  
}  
```  
**具体命令角色类定义** (***KungPaoChickenCmd.h***)  
```  
#ifndef _KUNGPAOCHICKENCMD_H_  
#define _KUNGPAOCHICKENCMD_H_

#include "Command.h"
#include "Chef.h"

class KungPaoChickenCmd: public Command {

public: 

    KungPaoChickenCmd(Chef* chef);
    void ExcuteCmd() override;

};

#endif  
```
**具体命令角色类实现** (***KungPaoChickenCmd.cpp***)  
```  
#include "KungPaoChickenCmd.h"  

KungPaoChickenCmd::KungPaoChickenCmd(Chef* chef) 
                 : Command(chef) {

}

void KungPaoChickenCmd::ExcuteCmd() {

    m_Chef->KungPaoChicken();

}  
```
**具体命令角色类定义** (***FishFlavoredShreddedPorkCmd.h***)  
```  
#ifndef _FISHFLAVOREDSHREDDEDPORKCMD_H_  
#define _FISHFLAVOREDSHREDDEDPORKCMD_H_  

#include "Command.h"
#include "Chef.h"

class FishFlavoreShreddedPorkCmd: public Command {

public:

    FishFlavoreShreddedPorkCmd(Chef* Chef);
    void ExcuteCmd() override;

};

#endif  
```
**具体命令角色类实现** (***FishFlavoredShreddedPorkCmd.cpp***)  
```  
#include "FishFlavoredShreddedPorkCmd.h"  

FishFlavoreShreddedPorkCmd::FishFlavoreShreddedPorkCmd(Chef* chef) 
                          : Command(chef) {

}

void FishFlavoreShreddedPorkCmd::ExcuteCmd() {

    m_Chef->FishFlavoredShreddedPork();

}  
```
**具体命令角色类定义** (***BigPlateChickenCmd.h***) 
```  
#ifndef _BIGPLATECHICKENCMD_H_  
#define _BIGPLATECHICKENCMD_H_ 

#include "Command.h"
#include "Chef.h"

class BigPlateChickenCmd: public Command {

public:

    BigPlateChickenCmd(Chef* chef);
    void ExcuteCmd() override;


};

#endif  
```
**具体命令角色类实现** (***BigPlateChickenCmd.cpp***)  
```  
#include "BigPlateChickenCmd.h"

BigPlateChickenCmd::BigPlateChickenCmd(Chef* chef)
                  : Command(chef) {

}

void BigPlateChickenCmd::ExcuteCmd() {

    m_Chef->BigPlateChicken();

}  
```
**调用者角色类定义** (***Waiter.h***)  
```  
#ifndef _WAITER_H_  
#define _WAITER_H_   

#include <bits/stdc++.h>
#include "Command.h"

class Waiter {

public:  

    Waiter();
    void AddCmd(Command* cmd);
    void DelCmd(Command* cmd);
    void Nodify();

private:

    std::list<Command*> m_CmdList;

};

#endif  
```
**调用者角色类实现** (***Waiter.cpp***)  
```  
#include "Waiter.h"  

Waiter::Waiter() {

}

void Waiter::AddCmd(Command* cmd) {

    m_CmdList.push_back(cmd);

}

void Waiter::DelCmd(Command* cmd) {

    m_CmdList.remove(cmd);

}

void Waiter::Nodify() {

    for (auto &cmd : m_CmdList) {

        if (cmd) {
            cmd->ExcuteCmd();
        }

    }

}  
```
**接受者角色类定义** (***Chef.h***)  
```  
#ifndef _CHEF_H_  
#define _CHEF_H_

#include <bits/stdc++.h>

class Chef {

public:

  Chef();

  void KungPaoChicken();
  void FishFlavoredShreddedPork();
  void BigPlateChicken();

};

#endif
```  
**接受者角色类实现** (***Chef.cpp***)  
```  
#include "Chef.h"

Chef::Chef() {

}

void Chef::KungPaoChicken() {

    std::cout << "宫保鸡丁" << std::endl;

}

void Chef::FishFlavoredShreddedPork() {

    std::cout << "鱼香肉丝" << std::endl;

}

void Chef::BigPlateChicken() {

    std::cout << "大盘鸡" << std::endl;

}  
```
**应用** (***main.cpp***)  
```  
#include "Chef.h"
#include "Command.h"
#include "KungPaoChickenCmd.h"
#include "FishFlavoredShreddedPorkCmd.h"
#include "BigPlateChickenCmd.h"
#include "Waiter.h"

#include <bits/stdc++.h>

int main() {

    std::shared_ptr<Chef> chef = std::make_shared<Chef>();

    Waiter waiter;  

    std::shared_ptr<KungPaoChickenCmd> kpcc;
    std::shared_ptr<FishFlavoreShreddedPorkCmd> ffspc;
    std::shared_ptr<BigPlateChickenCmd> bpcc;

    kpcc = std::make_shared<KungPaoChickenCmd>(chef.get());
    ffspc = std::make_shared<FishFlavoreShreddedPorkCmd>(chef.get());
    bpcc = std::make_shared<BigPlateChickenCmd>(chef.get());
    
    waiter.AddCmd(ffspc.get());
    waiter.AddCmd(kpcc.get());
    waiter.AddCmd(bpcc.get());

    waiter.Nodify();

    return 0;

}
```

#### 优缺点  
##### 优点  
+ 类间解耦, 调用者角色与接收者角色之间没有任何依赖关系, 调用者实现功能时只需调用 **Command** 抽象类的 ***execute*** 方法就可以, 不需要了解到底是哪个接收者执行.  
+ 可扩展性, Command 的子类可以非常容易地扩展, 而调用者Invoker 和高层次的模块 Client 不产生严重的代码耦合.  
+ 命令模式结合其他模式会更优秀, 命令模式可以结合责任链模式, 实现命令族解析任务; 结合模板方法模式, 则可以减少 Command 子类的膨胀问题.  

##### 缺点  
命令模式也是有缺点的, 请看 Command 的子类: 如果有N个命令, Command 的子类就是N个, 这个类膨胀得非常大, 这个就需要读者在项目中慎重考虑使用.  

#### 总结  
命令模式的意图是将一个请求封装成一个对象, 从而可以用不同的请求对客户进行参数化. 命令模式主要解决的问题是:**在软件系统中, 行为请求者与行为实现者通常是一种紧耦合的关系**, 但某些场合, 比如需要对行为进行记录, 撤销或重做, 事务等处理时, 这种无法抵御变化的紧耦合的设计就不太合适.  

系统需要支持命令的撤销(Undo)操作和恢复(Redo)操作, 也可以考虑使用命令模式, 命令模式的实现过程通过调用者调用接受者执行命令, 顺序: **调用者 -> 接受者 -> 命令**  

***  

### 解释器模式  
#### 简介  
解释器模式, 提供了评估语言的语法或表达式的方式, 它属于行为型模式. 这种模式实现了一个表达式接口, 该接口解释一个特定的上下文. 这种模式被用在 SQL 解析, 符号处理引擎等.  

#### 结构  
解释器模式主要有以下角色:  
+ **抽象表达式角色(Expression)**  
+ **终结符表达式角色(Terminal Expression)**  
+ **非终结符表达式角色(Nonterminal Expression)**  
+ **环境角色(Context)**  

##### 抽象表达式角色  
声明一个所有的具体表达式角色都需要实现的抽象接口, 这个接口主要是一个 ***interpret***() 方法, 称做解释操作.  
##### 终结符表达式角色  
实现了抽象表达式角色所要求的接口, 主要是一个 ***interpret***() 方法; 文法中的每一个终结符都有一个具体终结表达式与之相对应. 比如有一个简单的公式 **R = R1 + R2**, 在里面 R1 和 R2 就是终结符, 对应的解析 R1 和 R2 的解释器就是终结符表达式.  
##### 非终结符表达式角色  
文法中的每一条规则都需要一个具体的非终结符表达式, 非终结符表达式一般是文法中的运算符或者其他关键字, 比如公式 **R = R1 + R2** 中, **+** 就是非终结符, 解析 **+** 的解释器就是一个非终结符表达式.  
##### 环境角色  
这个角色的任务一般是用来存放文法中各个终结符所对应的具体值, 比如公式 **R = R1 + R2** 中, 我们给 R1 赋值 100, 给 R2 赋值 200. 这些信息需要存放到环境角色中, 很多情况下我们使用Map来充当环境角色就足够了.  

#### 应用场景  
+ 当有一个语言需要解释执行, 并且你可将该语言中的句子表示为一个抽象语法树, 可以使用解释器模式. 而当存在以下情况时该模式效果最好.  
+ 该文法的类层次结构变得庞大而无法管理, 此时语法分析程序生成器这样的工具是最好的选择, 他们无需构建抽象语法树即可解释表达式, 这样可以节省空间而且还可能节省时间.   
+ 效率不是一个关键问题, 最高效的解释器通常不是通过直接解释语法分析树实现的, 而是首先将他们装换成另一种形式, 例如正则表达式通常被装换成状态机, 即使在这种情况下, 转换器仍可用解释器模式实现, 该模式仍是有用的.  

#### 实例  
音乐解释器  
**环境角色类定义** (***Context.h***)  
```  
#ifndef _CONTEXT_H_  
#define _CONTEXT_H_ 

#include <bits/stdc++.h>

class PlayContext {

public:

    void setPlayText(std::string text);
    std::string getPlayText();

private:

    std::string m_Text;

};

#endif  
```
**环境角色类实现** (***Context.cpp***)  
```  
#include "Context.h"

void PlayContext::setPlayText(std::string text) {

    m_Text = text;

}

std::string PlayContext::getPlayText() {

    return m_Text;

}  
```  
**抽象表达式角色类定义** (***AbstractExpression.h***)  
```  
#ifndef _ABSTRACTEXPRESSION_H_  
#define _ABSTRACTEXPRESSION_H_

#include "Context.h"

class Expression {

public:  

    void interpret(PlayContext* context);

    virtual void excute(std::string key, double value) = 0;

};

#endif  
```  
**抽象表达式角色类实现** (***AbstractExpression.cpp***)  
```  
#include "AbstractExpression.h"
#include <bits/stdc++.h>
using namespace std;

void Expression::interpret(PlayContext* context) {

    std::string str = context->getPlayText();

    if (str.length() == 0) {
        return;
    }

    std::string playKey = str.substr(0, 1);
    context->setPlayText(str.substr(2));


    std::string str2 = context->getPlayText();

    if (str2.length() == 0) {
        return;
    }

    double playValue = std::stod(str2.substr(0, str2.find(' ')));

    if (str2.find(' ') == -1) {

        context->setPlayText("");

    }else {

        context->setPlayText(str2.substr(str2.find(' ') + 1));

    }

    std::cout << playKey << " " << playValue << " : ";
    excute(playKey, playValue);

}  
```
**终结符表达式角色类定义** (***TerminaExperssion.h***)  
```  
#ifndef _TERMINAEXPRESSION_H_  
#define _TERMINAEXPRESSION_H_ 

#include "AbstractExpression.h"  
#include <bits/stdc++.h>

class Note: public Expression {

public:

    void excute(std::string key, double value) override;

};

class Scale: public Expression {

public:

    void excute(std::string key, double value) override;

};

#endif  
```
**终结符表达式角色类实现** (***TerminaExperssion.cpp***)
```  
#include "TerminaExperssion.h"

void Note::excute(std::string key, double value) {

    std::string note = "";

    if (key == "C") {

        note = "1";

    }else if (key == "D") {

        note = "2";

    }else if (key == "E") {

        note = "3";

    }else if (key == "F") {

        note = "4";

    }else if (key == "G") {

        note = "5";

    }else if (key == "A") {

        note = "6";

    }else if (key == "B") {

        note = "7";

    }

    std::cout << note << std::endl;

}

void Scale::excute(std::string key, double value) {

    std::string scale = "";

    int v = (value + 0.5);

    if (v == 1) {

        scale = "低音";

    }else if (v == 2) {

        scale = "中音";

    }else if (v == 3) {

        scale = "高音";

    }

    std::cout << scale << std::endl;

}  
```
**应用** (***main.cpp***)  
```  
#include "TerminaExperssion.h"
#include "Context.h"

int main() {

    PlayContext* context = new PlayContext();

    context->setPlayText("O 2 E 0.5 G 0.5 A 3 E 0.5");

    Expression* expression = nullptr;

    while ((context->getPlayText()).length() > 0) {

        std::string str = (context->getPlayText()).substr(0, 1);

        if (str == "O") {

            expression = new Scale();

        }else if (str == "P") {

            expression = new Note();

        }

        expression->interpret(context);

    }

    return 0;

}  
```

#### 优缺点  
##### 优点  
+  可以很容易地改变和扩展方法, 因为该模式使用类来表示方法规则, 你可以使用继承来改变或扩展该方法.  
+  也比较容易实现方法, 因为定义抽象语法树总各个节点的类的实现大体类似, 这些类都易于直接编写.  
+  解释器模式就是将一句话, 转变为实际的命令程序执行而已. 而不用解释器模式本身也可以分析, 但通过继承抽象表达式的方式, 由于依赖转置原则, 使得文法的扩展和维护都带来的方便.  

##### 缺点  
解释器模式为方法中的每一条规则至少定义了一个类, 因此包含许多规则的方法可能难以管理和维护. 因此当方法非常复杂时, 使用其他的技术如 语法分析程序 或 编译器生成器 来处理.  

***  

## 常用七大设计原则  
面向对象设计的目标之一在于支持可维护性复用, 一方面需要实现设计方案或者源码的重用; 另一方面要确保系统能够易于扩展和修改, 具有较好的灵活性. 常用的设计原则有**7**个原则:  
+ **单一职责原则** (***single responsibility principle***, **SPR**)  
+ **开闭原则** (***Open-Close Principe***, **OCP**)  
+ **里氏替换原则** (***Liskov Substitution Principe***, **LPS**)  
+ **依赖倒转原则** (***Dependence Inversion Principle***, **DIP**)  
+ **接口隔离原则** (***Interface  Segregation Principle***, **ISP**)  
+ **合成复用原则** (***Composite Reuse Principe***, **CRP**)  
+ **迪米特法则** (***Law of Demeter***, **LoD**)  

### 单一职责原则 
#### 简介 
当一个类承担的职责过多, 就等于把这些职责耦合在一起, 一个职责的变化可能会削弱或者一直这类完成其他职责的能力. 所以为了减轻这个类的负担, 就要进行职责分离, 将不同的支付封装在不同的类中.  

#### 原则  
**就一个类而言, 应该仅有一个引起它变化的原因**.  

单一职责原则是实现高内聚, 低耦合的指导方针, 它是最简单但又最难运用的原则, 需要设计人员发现类的不同职责并将其分离, 而发现类的多重职责需要设计人员具有较强的分析设计能力和相关实践经验.  

#### 例子  
手机与照相机, 它们都有照相的功能. 但是手机的功能不仅仅只有照相还可以打电话, 听音乐; 而但是照相机就只负责照相. 因此, 这里面的手机职能就过多, 在维护等方面就会比单一职责的困难很多.  

***  

### 开闭原则  
#### 简介  
没有十全十美的软件, 随着时间的推移, 软件也是需要更新换代的. 当软件需要增加新的需求的时候, 程序员应该尽量保证系统设计框架的稳定. 如果一个软件符合 开放—封闭 原则, 那么就可以很方便的进行扩展, 无需修改其他的代码.  

#### 原则  
**软件实体(类, 模块, 函数等等) 应该可以可扩展, 但是不可以修改**.  

此原则是面向对象设计的核心, 遵循这个原则可以带来很多好处, 当出现频繁变化的那部分的时候, 就可以将其抽象出来. 但是抽象的时候一定要注意: **拒绝不成熟的抽象**.  

#### 例子  
代工厂加工. 如果一个代工厂只是一心一意加工一类产品, 断绝其他产品的业务, 这样如果当此产品滞销时, 代工厂也会面临危机. 如果代工厂的业务是可拓展的, 不需要去触动原来产品的业务, 可以加工各种其他的产品, 增加工厂的其他业务收入, 这样工厂的抗风险能力就会大大提高. 这就是对扩展的开放, 对修改的关闭的意义.  

***  

### 里氏替换原则  
#### 简介  
一个软件实体如果使用的是一个父类的话, 那么一定适用于其他子类, 二且他察觉不出父类对象和子类对象的区别. 换言之, 在软件里面, 把父类都替换成他的子类, 程序的行为没有发生变化.  

如果对于每一个类型为**A**的对象**a**, 都有类型为**B**的对象**b**, 使得**A**定义的所有程序**P**在所有对象**a**都替代成**b**时, 程序**P**没有变化, 那么**B**是**A**的子类型. 

#### 原则  
**子类型必须能够替换掉它们的父类型**  

#### 例子  
企鹅类是鸟类的子类, 但企鹅不会飞. 如果造物者在鸟类中添加 *飞*这个行为, 那么企鹅类将不能继承鸟类这个类了.  

***  

### 依赖倒转原则  
#### 简介  
高层模块不应该依赖与底层模块, 二者都应该依赖于抽象, **抽象不应该依赖于细节, 细节应该依赖于抽象; 针对接口而非实现编程**.  

#### 原则  
+ **高层模块不应该依赖底层模块, 两个都应该依赖抽象**.  
+ **抽象不应该依赖细节, 细节应该依赖抽象**.  

尽量引用高层抽象的类, 即 使用接口和抽象类进行变量类型声明, 方法返回类声明的转换等, 而不要用具体的类来做这些事情. 为了确保该原则的应用, 一个具体类应当只实现接口或抽象类中声明过的方法, 而不要给出多余的方法.  

#### 例子  
台式电脑的 CPU, 内存, 硬盘等都是分开的, 当有某个零件出问题的时候, 只需要将出问题的零件替换掉. 而如果把全部零件全部集成在一起, 每个零件之间相互依赖错综复杂, 如果一个零件出了问题就会导致全部都有问题了.  

***  

### 接口隔离原则  
#### 简介  
只做自己应该做的事情, 而不干不该干的事情, 每个接口应该承担一种独立的角色.  

#### 原则  
**使用多个专门的接口, 而不使用单一的总接口, 即客户端不应该依赖那些它不需要的接口**.  

在使用接口隔离原则时, 我们需要注意控制接口的粒度, 接口不能太小, 如果太小会导致系统中接口泛滥, 不利于维护; 接口也不能太大, 太大的接口将违背接口隔离原则, 灵活性较差, 使用起来很不方便. 这个原则可以和单一职责原则配合使用.  

从接口隔离原则的定义可以看出, 跟 **SRP** 有许多相似之处, 都是强调**职责的单一性**. 接口隔离原则告诉我们在定义接口的时候要根据职责定义 "较小" 的接口, 不要定义 "高大全" 的接口, 也就是说接口要尽可能的职责单一, 这样更容易复用, 暴露给客户端的方法更具有 "针对性". 如果把一摊子风牛马不相及的方法封装到一个接口里面, 很显然的构成了接口污染.  

#### 例子  
现在的智能手机简直是太强大了, 若是每个接口不好好承担自己应该承担的, 去做了其他的接口的事情那么智能手机就乱了, 放歌的时候开相机, 打电话的时候发短信.  

***  

### 合成复用原则
#### 简介  
这个原则就是在一个新的对象中通过关联关系(组合关系和聚合关系) 来使用一些已有的对象, 使之成为新对象的一部分, 尽量使用合成/聚合, 尽量少用继承.  

#### 原则  
**尽量使用对象的组合, 而不是继承来达到复用的目的**.  

***  

### 迪米特法则  
#### 简介  
迪米特法则, 也称为**最少知识原则**, 清掉了类之间的松耦合, 降低了系统的耦合度. 对象间尽量最少了解, 彻底将API接口和具体实现相分离, 模块间仅仅通过API进行通信.  

#### 原则  
**如果两个类不必彼此直接通信, 那么这两个类就不应该发生直接的相互作用. 如果其中一个类需要调用另一个类的某个方法的话, 可以通过第三者转发这个调用.**  

在类的划分上, 应当尽量创建松耦合的类, 类之间的耦合度越低, 就越有利于复用, 一个处在松耦合中的类一旦被修改, 不会对关联的类造成太大波及; 在类的结构设计上, 每一个类都应当尽量降低其成员变量和成员函数的访问权限; 在类的设计上, 只要有可能, 一个类型应当设计成不变类; 在对其他类的引用上, 一个对象对其他对象的引用应当降到最低.  

#### 例子  
小时候父母经常教我们 "不和陌生人说话", 也就是说你只能和你的朋友发生直接的交流, 而不能和陌生人发生直接的交流, 以免你出现危险, 也就是带来不必要的影响.  

***  
