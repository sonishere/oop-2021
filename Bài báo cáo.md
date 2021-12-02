# Nhóm gồm 3 thành viên:
- Lê Trần Lâm Bình - 19020226
- Nguyễn Duy Đường - 19020266
- Hoàng Văn Đô - 19020251

>## Link
>* Link Repo: https://github.com/abishekaditya/DesignPatterns
>* Link mẫu chuẩn: https://refactoring.guru/design-patterns/java
# 1. Creational Patterns
## 1.1 Abstract Factory
- Là một mẫu thiết kế sáng tạo cho phép bạn tạo ra các họ các đối tượng liên quan mà không cần chỉ định các lớp cụ thể của chúng.
- Code: [Abstract Factory](https://github.com/abishekaditya/DesignPatterns/tree/master/FactoryPattern/Abstract%20Factory)
- Ở đây, cung cấp một interface cho việc tạo lập các đối tượng (có liên hệ với nhau) mà không cần qui định lớp khi hay xác định lớp cụ thể (concrete) tạo mỗi đối tượng, 2 class được khai báo theo mẫu chuẩn.
```
internal class ChicagoIngredientsFactory : IIngredientsFactory
internal class NyIngredientsFactory : IIngredientsFactory
```

## 1.2 Factory Method
- Là một mẫu thiết kế sáng tạo cung cấp một giao diện để tạo các đối tượng trong lớp cha, nhưng cho phép các lớp con thay đổi loại đối tượng sẽ được tạo.
- Code: [Factory Method](https://github.com/abishekaditya/DesignPatterns/tree/master/FactoryPattern/Factory%20Method)
- Lớp tạo khai báo phương thức gốc phải trả về một đối tượng của một lớp sản phẩm. Các lớp con ChicagoPizzaFactory, NyPizzaFactory cung cấp việc triển khai phương thức này.
```
abstract class PizzaFactory
    {
        public Pizza Order(string type) {}
        protected abstract Pizza Create(string type);
    }
class ChicagoPizzaFactory : PizzaFactory
    {
        protected override Pizza Create(string type) {}
    }
class NyPizzaFactory : PizzaFactory
    {
        protected override Pizza Create(string type) {}
    }
```
## 1.3 Builder
- Builder là một mẫu thiết kế sáng tạo cho phép bạn xây dựng các đối tượng phức tạp theo từng bước. Mẫu cho phép bạn tạo ra các kiểu và hình ảnh đại diện khác nhau của một đối tượng bằng cách sử dụng cùng một mã xây dựng.
- Code: [Builder](https://github.com/abishekaditya/DesignPatterns/tree/master/BuilderPattern)
- Khởi tạo interface IBuilder:
```
public interface IBuilder
    {
        void AddIngredients();
        void AddShape();
        void AddSize();
        void Reset();
        Hamburger Build();
    }
```
- Các lớp MyHamburgerBuilder, WifesHamburgerBuilder kế thừa các thuộc tính trong lớp IBuilder. Tách tiến trình xây dựng 1 đối tượng phức tạp sao cho một tiến trình tạo được các biểu diễn khác nhau 
=> Builder Design Pattern
## 1.4 Prototype Pattern
- Prototype là một mẫu thiết kế sáng tạo cho phép bạn sao chép các đối tượng hiện có mà không làm cho mã của bạn phụ thuộc vào các lớp của chúng.
- Code: [Prototype Pattern](https://github.com/abishekaditya/DesignPatterns/tree/master/PrototypePattern)
- Khởi tạo `interface IFigure : ICloneable` dùng làm đối tượng mẫu để quy định các loại đối tượng của lớp Circle và Rectangle, hai lớp đó kế thừa lớp IFigure. Tất cả các lớp sau tuân theo cùng một interface, cung cấp một phương thức clone().
```
interface IFigure : ICloneable
    {}
class Circle : IFigure
    {
        public object Clone()
        {
            return new Circle(_radius);
        }
    }
class Rectangle : IFigure
    {
        public object Clone()
        {
            return new Rectangle(_width, _height);
        }
    }
 ```
 - Giống với mẫu chuẩn trong [Prototype Pattern](https://refactoring.guru/design-patterns/prototype)
 
 ## 1.5 Singleton
 - Singleton cho phép đảm bảo rằng một lớp chỉ có một thể hiện, đồng thời cung cấp một điểm truy cập toàn cục cho thể hiện này.
 - Code: [Singleton](https://github.com/abishekaditya/DesignPatterns/tree/master/SingletonPattern)
 - Class `ChocolateBoiler` được khởi tạo là `internal partial class ChocolateBoiler`, bên trong là phương thức khởi tạo tĩnh và phương thức khởi tạo private.
 ```
 internal partial class ChocolateBoiler
    {
        private static readonly Lazy<ChocolateBoiler> _singleton = new Lazy<ChocolateBoiler>(() => new ChocolateBoiler());

        public static ChocolateBoiler GetInstance() => _singleton.Value;

        private Status _boiler;

        private ChocolateBoiler()
        {
            Console.WriteLine("Starting");
            _boiler = Status.Empty;
        }

        public void Fill() {}

        public void Drain() {}

        public void Boil() {}

        private bool IsEmpty => (_boiler == Status.Empty);

        private bool IsBoiled => (_boiler == Status.Boiled);
    }
 ```
 - Giống với mẫu chuẩn trong [Singleton](https://refactoring.guru/design-patterns/singleton)
 
 # 2. Behavioral Patterns
 ## 2.1 Chain of Responsibility
 - Cho phép chuyển các yêu cầu dọc theo một chuỗi các trình xử lý. Khi nhận được yêu cầu, mỗi trình xử lý sẽ quyết định xử lý yêu cầu hoặc chuyển nó cho trình xử lý tiếp theo trong chuỗi.
 - Code: [Chain of Responsibility](https://github.com/abishekaditya/DesignPatterns/tree/master/ChainOfResponsibilityPattern)
 ```
 // Giao diện trình xử lý khai báo một phương pháp để xây dựng một chuỗi trình xử lý. Nó cũng khai báo phương thức để thực hiện một yêu cầu.
public interface IHandler {
        void AddChain(IHandler handler);
        double Handle(double[] values, string action);
    }
    
// The base class for simple components.
public abstract class BaseHandler : IHandler {
        public void AddChain(IHandler handler) {
            _nextInLine = handler;    
        }

        public abstract double Handle(double[] values, string action);
    }
    
// Các mối quan hệ dây chuyền được thành lập. Class kế thừa override Handler từ cha nó.
public class AdditionHandler : BaseHandler {
        public override double Handle(double[] values, string action) {}
public class MultiplicationHandler : BaseHandler {
        public override double Handle(double[] values, string action) {}
```
- Không có sự thay đổi so với mẫu chuẩn [Chain of Responsibility](https://refactoring.guru/design-patterns/chain-of-responsibility)

## 2.2 Command
- Command biến một yêu cầu thành một đối tượng độc lập chứa tất cả thông tin về yêu cầu. Sự chuyển đổi này cho phép bạn chuyển các yêu cầu dưới dạng đối số của phương thức, trì hoãn hoặc xếp hàng đợi việc thực hiện một yêu cầu và hỗ trợ các hoạt động hoàn tác.
- Code: [Command](https://github.com/abishekaditya/DesignPatterns/tree/master/CommandPattern)
- The base command class xác định giao diện chung cho các concrete commands.
```
internal interface ICommand
    {
        void Execute();
        void Undo();
    }
```
- The concrete commands:
```
internal class GarageDoorCloseCommand : ICommand
    {
        public GarageDoorCloseCommand(Garage g) {...}
        public void Execute() {}
        public void Undo() {}
    }
internal class GarageDoorOpenCommand : ICommand
    {
        public GarageDoorOpenCommand(Garage g) {...}
        public void Execute() {}
        public void Undo() {}
    }
v.v.....
```
- Tất cả các class GarageDoorCloseCommand, GarageDoorOpenCommand, LightOffCommand, LightOnCommand, MacroCommand, NoCommand, .... đều đưuọc xác định từ ICommand nhưng là những đối tượng độc lập, chứa các yêu cầu Execute(), Undo().
- Không có thay đổi so với mẫu chuẩn [Command](https://refactoring.guru/design-patterns/command)

## 2.3 **Iterator** 
- Là một mẫu thiết kế hành vi cho phép duyệt tuần tự thông qua một cấu trúc dữ liệu phức tạp mà không để lộ các chi tiết bên trong của nó. Ý tưởng chính của mẫu Iterator là trích xuất hành vi truyền tải của một tập hợp thành một đối tượng riêng biệt được gọi là trình vòng lặp.
- Ví dụ như trình vòng lặp hồ sơ mạng xã hội,  mẫu Iterator được sử dụng để duyệt qua các hồ sơ có trong một bộ sưu tập mạng xã hội từ xa mà không để lộ bất kỳ chi tiết giao tiếp nào với phía client.
- VD trong project [Iterator](https://github.com/abishekaditya/DesignPatterns/blob/master/IteratorPattern/Client.cs)
 ```
 public class Client
    {
        private IEnumerable _breakfast;
        private IEnumerable _dinner;

        public Client(BreakfastMenu breakfast, DinnerMenu dinner)
        {
            this._breakfast = breakfast.Items;
            this._dinner = dinner.Items;
        }

        public void PrintMenu()
        {
            var breakfast = _breakfast;
            PrintMenu(breakfast);
            var dinner = _dinner;
            PrintMenu(dinner);
        }

        private void PrintMenu(IEnumerable iter)
        {
            foreach (var item in iter)
            {
                var i = (Menu)item;
                Console.WriteLine($"{i.Name}  Rs. {i.Price} {  (i.Vegetarian ? "*" : "x") } \n {i.Description} ");

            }
        }
    }
```
- So sánh: khá giống nhau vì nó không để lộ các thuộc tính có bên trong class.
    
 ## 2.4 **Mediator**
  - Làm giảm sự ghép nối giữa các thành phần của chương trình bằng cách làm cho chúng giao tiếp gián tiếp, thông qua một đối tượng trung gian đặc biệt.
  - Ví dụ rất nhiều phần tử GUI hợp tác với sự trợ giúp của người trung gian nhưng không phụ thuộc vào nhau.
  - VD trong project [Mediator](https://github.com/abishekaditya/DesignPatterns/blob/master/MediatorPattern/Customer.cs)
 ```
   class Customer : Colleague
    {
        public Customer(Mediator mediator) : base(mediator) {}

        public override void Notify(string message)
        {
            Console.WriteLine($"Message to customer: {message}");
        }
    }
```
- So sánh: về cơ bản thì giống với cách sử dụng trong phần lý thuyết nhưng khác nhau ở chỗ trong phần này thì lớp Customer không kế thừa lớp Mediator mà sử dụng một đối tượng Mediator. 
    
 ## 2.5 **Memento**
- Cho phép tạo ảnh chụp nhanh trạng thái của một đối tượng và khôi phục nó trong tương lai.
- Không làm ảnh hưởng đến cấu trúc bên trong của đối tượng mà nó làm việc cùng, cũng như dữ liệu được lưu giữ bên trong ảnh chụp nhanh.
- Trong project không sử dụng mẫu này.
    
 ## 2.6 **Observer**
- Cho phép một số đối tượng thông báo cho các đối tượng khác về những thay đổi trong trạng thái của chúng.
- Cung cấp một cách để đăng ký và hủy đăng ký cho bất kỳ đối tượng nào triển khai giao diện người đăng ký.
- VD trong project [Observer](https://github.com/abishekaditya/DesignPatterns/blob/master/ObserverPattern/WeatherSupplier.cs)
- so sánh: cơ bản thì giống nhau, chỉ khác ở cách xử lý sự kiện
    
## 2.7 **State**
- Là một mẫu thiết kế hành vi cho phép một đối tượng thay đổi hành vi khi trạng thái bên trong của nó thay đổi.
- State pattern gợi ý nên tạo các lớp mới cho tất cả các trạng thái có thể có của một đối tượng và trích xuất tất cả các hành vi dành riêng cho trạng thái vào các lớp này.
- VD trong project [State](https://github.com/abishekaditya/DesignPatterns/blob/master/StatePattern/GumballMachine.cs)
```
public void InsertQuarter()
        {
            State.InsertQuarter();
        }

        public void EjectQuarter()
        {
            State.EjectQuarter();
        }

        public void TurnCrank()
        {
            State.TurnCrank();
            State.Dispense();
        }

        public void ReleaseBall()
        {
            Console.WriteLine("A ball comes rolling down");
            if (Count == 0) return;
            Count--;
        }
    }
```
- So sánh: giống với cách sử dụng trong lý thuyết.
    
## 2.8 **Strategy**
- Là một mẫu thiết kế biến một tập hợp các hành vi thành các đối tượng và làm cho chúng có thể hoán đổi cho nhau bên trong đối tượng ngữ cảnh ban đầu.
- Strategy pattern gợi ý rằng nên chọn một lớp thực hiện điều gì đó cụ thể theo nhiều cách khác nhau và trích xuất tất cả các thuật toán này thành các lớp riêng biệt.
- VD trong project [Strategy](https://github.com/abishekaditya/DesignPatterns/blob/master/StrategyPattern/QuackSqueak.cs)
```
class QuackSqueak : IQuackBehaviour
     {
        public void Quack()
        {
            Console.WriteLine("Squeeeak");
        }
     }
```
- So sánh: giống với cách sử dụng trong lý thuyết.
    
## 2.9 **Template Method**
- Là một mẫu thiết kế cho phép bạn xác định khung của một thuật toán trong một lớp cơ sở và để các lớp con ghi đè các bước mà không thay đổi cấu trúc của thuật toán tổng thể.
- Template Method khá phổ biến trong các khung công tác Java. Các nhà phát triển thường sử dụng nó để cung cấp cho người dùng khung một phương tiện đơn giản để mở rộng chức năng tiêu chuẩn bằng cách sử dụng tính năng kế thừa.
- VD trong project [Template Method](https://github.com/abishekaditya/DesignPatterns/blob/master/TemplatePattern/Comparable/Person.cs)
- So sánh: giống với cách sử dụng trong lý thuyết do thuật toán trong lớp Person không làm thay đổi cấu trúc của thuật toán tổng thể mà chỉ đối chiếu đối tượng đầu vào với đối tượng Person.
## 2.10 **Visitor**
- Là một mẫu thiết kế hành vi cho phép thêm các hành vi mới vào hệ thống phân cấp lớp hiện có mà không thay đổi bất kỳ mã hiện có nào.
- Visitor pattern không phải là một mẫu quá phổ biến vì tính phức tạp và khả năng áp dụng hẹp của nó.
- VD trong project [Visitor](https://github.com/abishekaditya/DesignPatterns/blob/master/VisitorPattern/LivingRoomVisitor.cs)
```
public class LivingRoomVisitor : IUnitVisitor
    {
        public void VisitApartment(Apartment apartment)
        {
        }

        public void VisitStudio(Studio studio)
        {
        }

        public void VisitBedroom(Bedroom bedroom)
        {
        }

        public void VisitLivingRoom(LivingRoom livingRoom)
        {
            Console.WriteLine("This is the living room");
        }
    }
```
- So sánh: khá giống nhau về cách sử dụng, vì các phương thức được thêm vào trong class không làm thay đổi hành vi của các đoạn code trước đó 

# 3. Structural Patterns
 ## 3.1 Adapter
 
Được sử dụng trong [AdaptePattern.](https://github.com/abishekaditya/DesignPatterns/tree/master/AdapterPattern)

Các client interface:
IDuck.cs:
```
	public interface IDuck
    {
        void Quack();
        void Fly();
    }
```

ITurkey.cs:
```
public interface ITurkey
    {
        void Gobble();
        void Fly();
    }
```
Method Tester trong class Program.cs tương ứng là methodService.
```
private static void Tester(IDuck duck)
    {
        duck.Fly();
        duck.Quack();
    }
```

Môt instance của ITukey muốn sử dụng Tester phải chuyển đổi sang IDuck thông qua class TurkeyAdapter:
```
    public class TurkeyAdapter : IDuck
    {
    ...
        public void Quack()
        {
             ...
        }

        public void Fly()
        {
            ...
        }
    }
```
Ví dụ với 2 client WildTurkey và TurkeyAdapter

```
    private static void Main()
    {
        var turkey = new WildTurkey();
        var adapter = new TurkeyAdapter(turkey);

        Tester(adapter);
    }
```

Kết luận: hoàn toàn tương đồng so với mẫu thiết kế Adapte, sử dụng adapter để chuyển đổi một instance để phù hợp với service;

## 3.2 Bridge

Được sử dụng trong [BridgePattern.](https://github.com/abishekaditya/DesignPatterns/tree/master/BridgePattern)

Các implementation gồm IEnchantment và IWeapon:
```
    public interface IEnchantment
    {
        void OnActivate();
        void Apply();
        void OnDeactivate();
    }
  
    public interface IWeapon
    {
        void Wield();
        void Swing();
        void Unwield();
        IEnchantment GetEnchantment();
    }
```
Các instance của IEnchantment: SoulEatingEnchantment FlyingEnchantment.
Các instance của IWeapon: Sword, Hammer.

Author đã sử dụng 2 bridge để quán lý 2 thể loại vũ khí là bùa mê (Enchantment) và vũ khí vật lý (Weapon). Các intance sẽ cho biết cách sử dụng và hiệu ứng của từng loại. Giống với mẫu Brigde

Điểm khác: Author có kết hợp sử dụng thêm mẫu Adapter để chuyển đổi IWeapon sang IEnchantment.
```
    public interface IWeapon
    {
        ...
        IEnchantment GetEnchantment();
    }
```

## 3.3 Composite

Được sử dụng trong [CompositePattern.](https://github.com/abishekaditya/DesignPatterns/tree/master/CompositePattern)

Lớp cha MenuComponent gồm các leaf Menu, MenuIteam và Composite là Client.

Ví dụ trong program.cs, Client sử dụng các instance của MenuComponent để quản lý bữa ăn (sửa dụng Menu) và thực đơn trong bữa ăn (MenuIteam).

```
static class Program
    {
        public static void Main()
        {

            var breakfast = new Menu("Breakfast", "Pancake House");
            var lunch = new Menu("Lunch", "Deli Diner");
            var dinner = new Menu("Dinner", "Dinneroni");

            var dessert = new Menu("Dessert", "Ice Cream");

            var menu = new Menu("All", "McDonalds");

            breakfast.Add(new MenuItem("Waffles", "Butterscotch waffles", 140, false));
            breakfast.Add(new MenuItem("Corn Flakes", "Kellogs", 80, true));

            lunch.Add(new MenuItem("Burger", "Cheese and Onion Burger", 250, true));
            lunch.Add(new MenuItem("Sandwich", "Chicken Sandwich", 280, false));

            dinner.Add(new MenuItem("Pizza", "Cheese and Tomato Pizza", 210, true));
            dinner.Add(new MenuItem("Pasta", "Chicken Pasta", 280, false));

            dessert.Add(new MenuItem("Ice Cream", "Vanilla and Chocolate", 120, true));
            dessert.Add(new MenuItem("Cake", "Choclate Cake Slice", 180, false));

            dinner.Add(dessert);
            menu.Add(breakfast);
            menu.Add(lunch);
            menu.Add(dinner);

            menu.Print();
        }
    }
```

Kết luận: Sử dụng mẫu Composite để quản lý các loại Menu, cây tạo ra đơn giản dễ hiểu và hiệu quả.

## 3.4 Facade

Được dùng tại [FacadePattern.](https://github.com/abishekaditya/DesignPatterns/tree/master/FacadePattern)

**Facade** tương ứng với class *HomeTheatreFacade* được dùng để ... gồm các **Subsystem class** : *Dimmer*, *Dvd* và *DvdPlayer*. Với Dimmer quản lý độ sáng của màn hình, Dvd quản lý các moive, DvdPlayer quản lý các tác vụ điều khiển khi xem phim (ví dụ bật, dừng phim).

Kết luận: Author đã sử dụng mẫu Facade để thiết kế các class quản lý từng tác vụ được quản lý chung bới class *HomeTheatreFacade* khi đó Client chỉ cần thao tác với *HomeTheatreFacade* để sử dụng các chứ năng được cung cấp bởi tất cả các subsystem.
Ví dụ trong program.cs:
```
    internal static class Program
    {
        private static void Main()
        {
            var dimmer = new Dimmer();
            var dvdPlayer = new DvdPlayer();
            var dvd = new Dvd("Gone with the Wind 2 : Electric Bugaloo");
            var homeTheater = new HomeTheatreFacade(dimmer, dvd, dvdPlayer);

            homeTheater.WatchMovie();
            Console.WriteLine();
            homeTheater.Pause();
            Console.WriteLine();
            homeTheater.Resume();
            Console.WriteLine();
            homeTheater.Pause();
        }
    }
```

## 3.5 Decorator
Được sử dụng tại [DecoratorPattern](https://github.com/abishekaditya/DesignPatterns/tree/master/DecoratorPattern)

**Component** tương ứng là abstract class *Beverage* khai báo các method chung cho quá trình bao bọc (Mix đồ uống - Description và giá thành - Cost) và đối tượng là đồ uống:

```
    abstract class Beverage
    {
        protected string _description = "No Description";
        public abstract string Description { get; }
        public abstract double Cost();
    }
```
Các **Concrete Component** là các class Espresso, DrakRoast, HouseBlend. Là các đối tượng đồ uống xác định.
ví dụ Espresso.cs
```
class Espresso : Beverage
    {
        public Espresso()
        {
            _description = "Espresso";
        }

        public override string Description => _description;

        public override double Cost()
        {
            return 1.99;
        }
    }
```

**Base Decorator** tương ứng là *Condiment Decorator* có các **Concrete Decorator** tương ứng là  *MochaCondiment*  *WhipCondiment* quản lý việc bổ sung các loại condiment cho đồ uống. 

Điểm khác: Component được sử dụng là abstract class thay vì interface như trong mẫu.

## 3.6 Flyweight
Được sử dụng tại [FlyweightPattern](https://github.com/abishekaditya/DesignPatterns/tree/master/FlyweightPattern)

IBeverage  OolingMilkTea FoamMilkTea CoconutMilkTea BubbleMilkTea 

**Flyweight Factory** tương ứng là class *BubbleTeaShop* đại diện cho một cửa hàng đồ uống quản lý các loại đồ uống - Enumerate(), các hóa đơn - TakeOrders():
```
    private void TakeOrders()
        {
            var factory = new BeverageFlyweightFactory();

            takeAwayOrders.Add(factory.MakeBeverage(BeverageType.BubbleMilk));
            takeAwayOrders.Add(factory.MakeBeverage(BeverageType.BubbleMilk));
            takeAwayOrders.Add(factory.MakeBeverage(BeverageType.CoconutMilk));
            takeAwayOrders.Add(factory.MakeBeverage(BeverageType.FoamMilk));
            takeAwayOrders.Add(factory.MakeBeverage(BeverageType.OolongMilk));
            takeAwayOrders.Add(factory.MakeBeverage(BeverageType.OolongMilk));
        }

        public void Enumerate()
        {
            Console.WriteLine("Enumerating take away orders\n");
            foreach (var beverage in takeAwayOrders)
            {
                beverage.Drink();
            }
        }
```
Có 4 loại đồ uống có khác nhau tương ứng với 4 **Context** :  *BubbleMilk, FoamMilk, OolongMilk, CoconutMilk* có **Flyweight** là *IBeverage*.

Kết luận: Tương đồng với mẫu thiết ké nhưng chưa thể hiện rõ các repeatingState và uniquaState.

## 3.7 Proxy
Được sử dụng tại [ProxyPattern](https://github.com/abishekaditya/DesignPatterns/tree/master/ProxyPattern)

**Service** là RealImg
```
    public class RealImage : Image
    {

        private string _fileName;

        public RealImage(string fileName)
        {
            _fileName = fileName;
            loadFromDisk(_fileName);
        }

        public void display()
        {
            Console.WriteLine("Displaying " + _fileName);
        }

        private void loadFromDisk(string fileName)
        {
            Console.WriteLine("Loading " + fileName);
        }
    }
```
**Service Interface** tương ứng là class Image cung cấp service display() của class RealImg. Và **Proxy** tương ứng là ProxyImg tham chiếu đến display().
```
    public class ProxyImage : Image
    {
    ...
        public void display()
        {
            if (_realImage == null)
            {
                _realImage = new RealImage(_fileName);
            }
            _realImage.display();
        }
    }
```
Kết luận: hoàn toàn tương đồng với mẫu thiết kế, không có sự khác biệt.

# Kết Luận chung: **Tất cả các mẫu thiết kế trên đều giống, không có nhiều thay đổi so với 23 mẫu thiết kế chuẩn.**
