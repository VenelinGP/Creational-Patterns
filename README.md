Singleton Pattern

Singleton Pattern е object creational шаблон. Той е creational, защото има отношение към създаването на обекти. По-специално, той цели да ограничи създаването на повече от един обект от конкретен клас. Singleton Pattern е object шаблон, защото е ангажиран с връзките между обектите (т.е. уникалната инстанция към обект, която даден клас държи и достъпа на други обекти до него).

Необходимостта от прилагане на Singleton Pattern възниква при моделиране на обекти, които имат само една инстанция в реалния свят. Така например, в клас Computer може да има само едно устройство за възпроизвоеждане на звук. В случай, че се допусне създаването на няколко такива устройства и едновременното извеждане на различен звук, крайният резултат ще се различава от очаквания.

Целите, които се преследват при прилагането на Singleton Pattern са две:

подсигуряване създаването на само една инстанция на даден клас;
осигуряване на достъп до тази инстанция.
Singleton Pattern е приложим при наличие на конкурентни заявки за достъп до (общи) ресурси, което води до необходимост от централизирано осигуряване на достъпа до тях.

В софтуерния дизайн, употребата на Singleton Pattern се е наложила в следните случаи:

при дизайн на Logger класове;
при дизайн на Configuration класове;
при достъп до споделени ресурси (напр. сериен порт);
в комбинация с Abstract Factory или Factory Method шаблоните, при тяхното използване в многонишкова среда.
Singleton Pattern се имплементира чрез static поле в Singleton класа, private конструктор и static public метод, който връща референция към static полето.

using System.Runtime.CompilerServices;

internal class Singleton
{
    private static Singleton instance;

    private int exampleField;

    private Singleton()
    {
    }

    [MethodImpl(MethodImplOptions.Synchronized)]
    public static Singleton GetInstance()
    {
        if (instance == null)
        {
            return new Singleton();
        }
        else
        {
            return instance;
        }
    }

    public void ExampleMethod()
    {
    }
}
При имплементацията на Singleton Pattern участват Singleton класа и клиента.

Singleton класа дефинира метод, който дава възможност за достъп до неговата уникална инстанция. Инстанцирането се реализира като static метод, която отговаря за създаването на единствена инстанция на класа.
Клиентът получава достъп до Singleton класа, единствено чрез неговия static метод.
Следствията от използването на Singleton Pattern са:

осигуряване на контролиран достъп на клиентите до единствената инстанция на Singleton класа;
разширяване функционалността в сравнение с глобалните променливи;
възможност за наследяване и разширяване функционалността на Singleton класа;
гъвкавост в сравнение с опредациите върху клас

# Builder Pattern

## Мотивация

 * Служи за  създаване на обекти, при които е важна последователността на инициализиране на различните компоненти на обекта. 
 * В общия случай различните компоненти са взаимно зависими, което налага определена последователност при създаването им. 
 * Създаването на различните компоненти се осъществява чрез методи, които са дефинирани в интерфейс. Това позволява на всеки наследник на съответния интерфейс да имплементира по свой начин създаването на компонентите. 
 * Следващата стъпка е създаването на клас, който определя необходимите компоненти и последователността на създаването им. 
 * Това означава, че както можем да имаме различни имплементации на методологиите за създаване на компоненти, така можем да имаме и различни имплементации за композирането им.
 

## Цел

 * Създаване на обекти, при които е важна последователността на инициализиране на различните компоненти на обекта.

## Приложение

* Пример
 
 	Искаме да произвеждаме коли. Създаваме интерфейс, който дефинира всички методи, които ще ни създават съставните части на колата. Следващата стъпка е да създадем класове производители на колите - Мерцедес, БМВ(класове, които имплементират интерфейса). Всеки производител, ще произведе компонентите на колите по свой собствен начин. Съответно накрая ще имаме една машина, която сглобава елементите в определен ред(клас CarConstructor. Евентуално може и всеки производител да има свой собствен CarConstructor). Примерът не е напълно адекватен, защото със сигурност БМВ и Мерцедес използват различни части за конструирането на колите си и съответно ще ползват различни Builder интерфейси, но с учебна цел забравяме за това :)
    
## Известни употреби
* конструирането на HTML докимент

## Имплментация 

```
using System;
using System.Collections;

  public class MainApp
  {
    public static void Main()
    { 
      // Create director and builders 
      Director director = new Director();

      Builder b1 = new ConcreteBuilder1();
      Builder b2 = new ConcreteBuilder2();

      // Construct two products 
      director.Construct(b1);
      Product p1 = b1.GetResult();
      p1.Show();

      director.Construct(b2);
      Product p2 = b2.GetResult();
      p2.Show();

      // Wait for user 
      Console.Read();
    }
  }

  // "Director" 
  class Director
  {
    // Builder uses a complex series of steps 
    public void Construct(Builder builder)
    {
      builder.BuildPartA();
      builder.BuildPartB();
    }
  }

  // "Builder" 
  abstract class Builder
  {
    public abstract void BuildPartA();
    public abstract void BuildPartB();
    public abstract Product GetResult();
  }

  // "ConcreteBuilder1" 
  class ConcreteBuilder1 : Builder
  {
    private Product product = new Product();

    public override void BuildPartA()
    {
      product.Add("PartA");
    }

    public override void BuildPartB()
    {
      product.Add("PartB");
    }

    public override Product GetResult()
    {
      return product;
    }
  }

  // "ConcreteBuilder2" 
  class ConcreteBuilder2 : Builder
  {
    private Product product = new Product();

    public override void BuildPartA()
    {
      product.Add("PartX");
    }

    public override void BuildPartB()
    {
      product.Add("PartY");
    }

    public override Product GetResult()
    {
      return product;
    }
  }

  // "Product" 
  class Product
  {
    ArrayList parts = new ArrayList();

    public void Add(string part)
    {
      parts.Add(part);
    }

    public void Show()
    {
      Console.WriteLine("\nProduct Parts -------");
      foreach (string part in parts)
        Console.WriteLine(part);
    }
  }
  ```

## Последствия
* Дава ясна представа какво трябва да се имплементира от клиента 
* Осигурява цялостния модел на обекта

## Сродни модели
* Factory method
* Abstract Factory

## Проблеми
* Създава сложен обект, който ако не удовлетворява изискванията на повече от един ползватели, е безпредметно да се използва. С други думи, ако кодът, който ни създава обект няма никаква перспектива да се използва за създаване на други обекти, съставени от същите компоненти, нямаме нужда от него. По-просто би било да създадем логиката за създаване на обекта в самия клас, който го представлява.

# Object pool pattern

##Мотивация

Object pool може да предложи значително увеличение на производителността; тя е най-ефективнa в ситуации, в които разходите за инициализиране клас са високи, скоростта на инстанция на класа е висока, а броят на инстанцирания в употреба във всеки един момент е ниска.
##Цел

Object pool са използвани за управление на обект които са запазени. Всеки клиент, с достъп до Object pool обект може да се избегне създаването на нови обекти, като просто пита Object pool за обект, вместо инстанция. Object pool ще бъде все по-голям, т.е. Object pool ще създаде нови обекти, ако пулът е празен, или можем да имаме Object pool, който да ограничава броя на обектите, които могат да бъдат създадени.


```
namespace IVSR.DesignPatern.Objectpool 
 {
 public class PooledObject
 {
    DateTime _createdAt = DateTime.Now;
 
    public DateTime CreatedAt
    {
        get { return _createdAt; }
    }
 
    public string TempData { get; set; }
}

public static class Pool
{
    private static List<PooledObject> _available = new List<PooledObject>();
    private static List<PooledObject> _inUse = new List<PooledObject>();
 
    public static PooledObject GetObject()
    {
        lock(_available)
        {
            if (_available.Count != 0)
            {
                PooledObject po = _available[0];
                _inUse.Add(po);
                _available.RemoveAt(0);
                return po;
            }
            else
            {
                PooledObject po = new PooledObject();
                _inUse.Add(po);
                return po;
            }
        }
    }
 
    public static void ReleaseObject(PooledObject po)
    {
        CleanUp(po);
 
        lock (_available)
        {
            _available.Add(po);
            _inUse.Remove(po);
        }
    }
 
    private static void CleanUp(PooledObject po)
    {
        po.TempData = null;
    }
  }
}
```


## https://github.com/VenelinGP/Creational-Patterns