# Лабораторная работа №3
## Цель работы
1. Научиться синтаксису и принципам перегрузки операторов языка C#.


### Задание №1
Создайте структуру Vector с тремя полями x, y и z. 
Для созданной структуры переопределите операторы сложения векторов, умножения векторов, умножения вектора на число, а также логические операторы. Для логических операторов используйте сравнение по длине от начала координат.


### Решение №1
    using System;

    public abstract class Currency
    {
    protected double value;
    }
    class CurrencyUSD : Currency
    {
    public CurrencyUSD(double value)
    {
        this.value = value;
    }
    public static implicit operator CurrencyEUR(CurrencyUSD val) //public static implicit operator Тип_в_который_надо_преобразовать(исходный_тип param)
    {
        return new CurrencyEUR(val.value / CurrencyEUR.ExChange);
    }
    public static implicit operator CurrencyRUB(CurrencyUSD val)
    {
        return new CurrencyRUB(val.value / CurrencyRUB.ExChange);
    }
    public double Value
    {
        get { return this.value; }
    }
    }
    class CurrencyEUR : Currency
    {
    public static double ExChange { get; set; }

    public CurrencyEUR(double value)
    {
        this.value = value;
    }
    public static implicit operator CurrencyRUB(CurrencyEUR val)
    {
        return new CurrencyRUB(val.value * CurrencyEUR.ExChange / CurrencyRUB.ExChange);
    }
    public static implicit operator CurrencyUSD(CurrencyEUR val)
    {
        return new CurrencyUSD(val.value * CurrencyEUR.ExChange);
    }
    public double Value
    {
        get { return this.value; }
    }
    }
    class CurrencyRUB : Currency
    {
    public static double ExChange { get; set; }
    public CurrencyRUB(double value)
    {
        this.value = value;
    }
    public static implicit operator CurrencyUSD(CurrencyRUB val)
    {
        return new CurrencyUSD(val.value * CurrencyRUB.ExChange);
    }
    public static implicit operator CurrencyEUR(CurrencyRUB val)
    {
        return new CurrencyEUR(val.value * CurrencyRUB.ExChange / CurrencyEUR.ExChange);
    }
    public double Value
    {
        get { return this.value; }
    }
    }

    public class Program
    {
    public static void Main(string[] args)
    {
        CurrencyEUR.ExChange = 1.06;//обменный курс между долларом США (USD) и евро (Euro)
        CurrencyRUB.ExChange = 0.0102;//(рубль в доларах)

        CurrencyUSD cur = new CurrencyUSD(100);
        CurrencyEUR eur = cur;
        Console.WriteLine($"It's {eur.Value} EUR");
        CurrencyRUB rub = eur;
        Console.WriteLine($"It's {rub.Value} RUB");
    }
    }


### Задание №2
Создайте класс Car со свойствами Name, Engine, MaxSpeed. Переопределите оператор ToString() таким образом, чтобы он возвращал название машины(Name). Реализуйте возможность сравнения объектов Car, реализовав интерфейс IEquatable<Car>. 
Создайте класс CarsCatalog, содержащий коллекцию машин – элементов типа Car и переопределите для него индексатор таким образом, чтобы он возвращал строку с названием машины и типом двигателя.



### Решение №2
    using System;
    using System.Collections.Generic;

    public class Car : IEquatable<Car>
    {
    public string Name { get; set; }
    public string Engine { get; set; }
    public int MaxSpeed { get; set; }

    public override string ToString()
    {
        return Name;
    }

    public bool Equals(Car other)
    {
        if (other == null)
            return false;

        return Name == other.Name && Engine == other.Engine && MaxSpeed == other.MaxSpeed;
    }
    }

    public class CarsCatalog
    {
    private List<Car> cars = new List<Car>();

    public string this[int index]
    {
        get
        {
            if (index >= 0 && index < cars.Count)
                return $"{cars[index].Name} - {cars[index].Engine}";
            else
                throw new IndexOutOfRangeException();
        }
    }

    public void AddCar(Car car)
    {
        cars.Add(car);
    }
    }

    class Program
    {
    static void Main(string[] args)
    {
        CarsCatalog catalog = new CarsCatalog();

        Car car1 = new Car { Name = "Car A", Engine = "Petrol", MaxSpeed = 200 };
        Car car2 = new Car { Name = "Car B", Engine = "Diesel", MaxSpeed = 180 };
        Car car3 = new Car { Name = "Car C", Engine = "Electric", MaxSpeed = 220 };

        catalog.AddCar(car1);
        catalog.AddCar(car2);
        catalog.AddCar(car3);

        Console.WriteLine(catalog[0]);
        Console.WriteLine(catalog[1]);
        Console.WriteLine(catalog[2]);
    }
    }

### Задание №3
Создайте базовый класс Currency со свойством Value. Создайте 3 производных от Currency класса – CurrencyUSD, CurrencyEUR и CurrencyRUB со свойствами, соответствующими обменному курсу. В каждом из производных классов переопределите операторы преобразования типов таким образом, чтобы можно было явно или неявно преобразовать одну валюту в другую по курсу, заданному пользователем при запуске программы.


### Решение №3
    using System;

    public abstract class Currency
    {
    protected double value;
    }
    class CurrencyUSD : Currency
    {
    public CurrencyUSD(double value)
    {
        this.value = value;
    }
    public static implicit operator CurrencyEUR(CurrencyUSD val) //public static implicit operator Тип_в_который_надо_преобразовать(исходный_тип param)
    {
        return new CurrencyEUR(val.value / CurrencyEUR.ExChange);
    }
    public static implicit operator CurrencyRUB(CurrencyUSD val)
    {
        return new CurrencyRUB(val.value / CurrencyRUB.ExChange);
    }
    public double Value
    {
        get { return this.value; }
    }
    }
    class CurrencyEUR : Currency
    {
    public static double ExChange { get; set; }

    public CurrencyEUR(double value)
    {
        this.value = value;
    }
    public static implicit operator CurrencyRUB(CurrencyEUR val)
    {
        return new CurrencyRUB(val.value * CurrencyEUR.ExChange / CurrencyRUB.ExChange);
    }
    public static implicit operator CurrencyUSD(CurrencyEUR val)
    {
        return new CurrencyUSD(val.value * CurrencyEUR.ExChange);
    }
    public double Value
    {
        get { return this.value; }
    }
    }
    class CurrencyRUB : Currency
    {
    public static double ExChange { get; set; }
    public CurrencyRUB(double value)
    {
        this.value = value;
    }
    public static implicit operator CurrencyUSD(CurrencyRUB val)
    {
        return new CurrencyUSD(val.value * CurrencyRUB.ExChange);
    }
    public static implicit operator CurrencyEUR(CurrencyRUB val)
    {
        return new CurrencyEUR(val.value * CurrencyRUB.ExChange / CurrencyEUR.ExChange);
    }
    public double Value
    {
        get { return this.value; }
    }
    }

    public class Program
    {
    public static void Main(string[] args)
    {
        CurrencyEUR.ExChange = 1.06;//обменный курс между долларом США (USD) и евро (Euro)
        CurrencyRUB.ExChange = 0.0102;//(рубль в доларах)

        CurrencyUSD cur = new CurrencyUSD(100);
        CurrencyEUR eur = cur;
        Console.WriteLine($"It's {eur.Value} EUR");
        CurrencyRUB rub = eur;
        Console.WriteLine($"It's {rub.Value} RUB");
    }
    }

