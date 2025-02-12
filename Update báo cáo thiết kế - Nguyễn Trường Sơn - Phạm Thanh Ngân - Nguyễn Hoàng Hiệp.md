# Bài báo cáo tìm hiểu về các mẫu thiết kế
## Môn lập trình hướng đối tượng
### Lớp INT_2204_3

#### Danh sách thành viên:
* Nguyễn Trường Sơn - MSV: 19020609
* Nguyễn Hoàng Hiệp - MSV: 19020541
* Phạm Thanh Ngân - MSV: 19020584

#### **Về việc làm bài báo cáo**: 
- Mỗi thành viên sẽ làm phần việc mình được giao và sẽ commit vào repo chung của nhóm khi đã hoàn tất (**[link repo của nhóm](https://github.com/sonishere/oop-2021)**)
- **[Link repo mẫu làm tài liệu tham khảo và phân tích](https://github.com/youlookwhat/DesignPattern)**
- **[Link tài liệu lí thuyết làm cơ sở tham khảo và phân tích](https://gpcoder.com/4164-gioi-thieu-design-patterns/)**

## Nội dung báo cáo:
### **A. Creational Pattern**
#### ***1. Abstract Factory***
- Giống nhau: 
  -  Pattern có cấu tạo như sau:
  1. AbstractFactory: interface *XianRoujiaMoYLFoctory* chứa công đoạn làm món ăn **Rou Jia Mo** (Bánh mì kẹp Tây An)
  2. Product: Các đối tượng Meet, Yuanlao
  3. ConcreteFactory: Các subclass của các Product như: ChangShaFreshMeet, ChangShaTeSeYuanLiao, XianTeSeYuanLiao...
  4. Client: **Rou Jia Mo**
- Khác nhau: 
  - Không tồn tại AbstractProduct, các Method (Meet, YuanLao) được khởi tạo chỉ dưới dạng class thường, không phải abstract hoặc interface
```java
                  package com.example.jingbin.designpattern.factory.cxgc;

                  /**
                   * Created by jingbin on 2016/10/26.
                   */

                  public class Meet {
                  }
```

#### ***2. Factory Method***
- Giống nhau:
  - Cấu tạo của Pattern:
  1. Super Class: Class RoujiaMoStore đại diện cho các cửa hàng bán **Rou Jia Mo**, chứa các công đoạn chuẩn bị và xuất kho
  2. Sub Class: Các chi nhánh: XianKuRoujiMo, XianSuanRoujiMo, XianlaRoujiMo, XianSuanRoujiMo
  3. Factory: Class XianSimpleRoujiaMoFactory xử lí input và đưa về các chi nhánh (sub class)
- Khác nhau:
  - Không có sự khác nhau, Pattern đang xét tuân thủ đúng theo cấu trúc mẫu
 
 #### ***3. Builder***
 - Giống nhau:
 
<p align="center">
 <img src="https://github.com/sonishere/sonishere/blob/main/UML.png" width="500">
</p>

 - Như ta có thể thấy theo hình trên, pattern builder của repo có cấu trúc tương tự như của Gang Of Four (GOF):
    - Builder: (Builder.java) là 1 abstract class và được kế thừa bởi 1 ConcreteBuilder (ConcreteBuider.java)
    - Director: (Director.java) để gọi tới Builder
    - Product: đại diện cho đối tượng cần tạo, đối tượng này phức tạp, có nhiều thuộc tính
- Khác nhau: Không có sự khác nhau, Pattern đang xét tuân thủ đúng theo cấu trúc mẫu

 #### ***4. Prototype***
 - Giống nhau:
    - Prototype: Class Shape đóng vai trò khởi tạo một hình nói chung
    - ConcretePrototype: Các class con đóng vai trò clone theo yêu cầu (Circle, Rectangle, Square)
    - Client: Class ShapeCache đóng vai trò yêu cầu Shape clone các hình (Circle, Rectangle, Square)
```java
              public class ShapeCache {

              private static Hashtable<String, Shape> shapeMap = new Hashtable<String, Shape>();

              public static Shape getShape(String shapeId) {
                  Shape shapeCache = shapeMap.get(shapeId);
                  return (Shape) shapeCache.clone();
              }

              public static void loadCache() {
                  Circle circle = new Circle();
                  circle.setId("1");
                  shapeMap.put(circle.getId(), circle);

                  Rectangle rectangle = new Rectangle();
                  rectangle.setId("2");
                  shapeMap.put(rectangle.getId(), rectangle);

                  Square square = new Square();
                  square.setId("3");
                  shapeMap.put(square.getId(), square);
              }
          }
```
- Khác nhau: Không có sự khác nhau, Pattern đang xét tuân thủ đúng theo cấu trúc mẫu

#### ***5. Singleton***
- Giống nhau: Đều tuân thủ các nguyên tắc cơ bản của Implement Singleton Pattern:
    - private constructor để hạn chế truy cập từ class bên ngoài.
    - Đặt private static final variable đảm bảo biến chỉ được khởi tạo trong class.
    - Có một method public static để return instance được khởi tạo ở trên.
- Repo có một số loại Implement khá hiệu quả như: 
  - Double Check Locking Singleton:
```java
        private static SingletonLanHan singletonLanHanFour;

        public static SingletonLanHan getSingletonLanHanFour() {
            if (singletonLanHanFour == null) {
                synchronized (SingletonLanHan.class) {
                    if (singletonLanHanFour == null) {
                        singletonLanHanFour = new SingletonLanHan();
                    }
                }
            }
            return singletonLanHanFour;
        }
```

   > Cách này đã khắc phục được nhược điểm của cách Thread Safe Singleton (một phương thức synchronized sẽ chạy rất chậm và tốn hiệu năng, bất kỳ Thread nào gọi đến đều phải chờ nếu có một Thread khác đang sử dụng. Có những tác vụ xử lý trước và sau khi tạo thể hiện không cần thiết phải block). Để implement theo cách này, chúng ta sẽ kiểm tra sự tồn tại thể hiện của lớp, với sự hổ trợ của đồng bộ hóa, hai lần trước khi khởi tạo. Phải khai báo volatile cho instance để tránh lớp làm việc không chính xác do quá trình tối ưu hóa của trình biên dịch.
    
   - Bill Pugh Singleton Implementation:
  
```java
      public class SingletonIn {

          private SingletonIn() {
          }

          private static class SingletonInHodler {
              private static SingletonIn singletonIn = new SingletonIn();
          }

          public static SingletonIn getSingletonIn() {
              return SingletonInHodler.singletonIn;
          }
      }
```

  >Với cách làm này bạn sẽ tạo ra static nested class với vai trò 1 Helper khi muốn tách biệt chức năng cho 1 class function rõ ràng hơn. Đây là cách thường hay được sử dụng và có hiệu suất tốt (theo các chuyên gia đánh giá). Khi Singleton được tải vào bộ nhớ thì SingletonInHodler chưa được tải vào. Nó chỉ được tải khi và chỉ khi phương thức getSingletonIn() được gọi. Với cách này tránh được lỗi cơ chế khởi tạo instance của Singleton trong Multi-Thread, performance cao do tách biệt được quá trình xử lý. Do đó, cách làm này được đánh giá là cách triển khai Singleton nhanh và hiệu quả nhất.

- Khác nhau: Không có quá nhiều sự khác biệt rõ rệt, cơ bản Pattern tuân thủ theo GOF

### **B. Structural Pattern**
#### ***1. Adapter***
  Adapter Pattern (Người chuyển đổi) là một trong những Pattern thuộc nhóm cấu trúc (Structural Pattern). Adapter Pattern cho phép các inteface (giao diện) không liên quan tới nhau có thể làm việc cùng nhau. Đối tượng giúp kết nối các interface gọi là Adapter.
- Giống nhau: Pattern bao gồm các thành phần cơ bản sau:
  1. Adaptee
  2. Adapter
  3. Target
  4. Client
  - Trong đó giao diện trên một chiếc điện thoại di động tốn 5V, điện áp gia đình là 220, vậy nên ta cần 1 Adapter (cụ thể là class V5PowerAdapter) để chuyển đổi điện áp 220V sang 5V:

```java
          public class V5PowerAdapter implements V5Power {

              private int v220power;

              public V5PowerAdapter(V220Power v220Power) {
                  v220power = v220Power.provideV220Power();
              }

              @Override
              public int provideV5Power() {

                  Log.e("---", "适配器: 经过复杂的操作,将" + v220power + "v电压转为5v");
                  return 5;
              }
          }
```

-Khác nhau: Về cơ bản, không có quá nhiều sự khác biệt rõ rệt, cơ bản Pattern tuân thủ theo GOF

#### ***2. Bridge***
- Bridge Pattern là một trong những Pattern thuộc nhóm cấu trúc (Structural Pattern). Ý tưởng của nó là tách tính trừu tượng (abstraction) ra khỏi tính hiện thực (implementation) của nó. Từ đó có thể dễ dàng chỉnh sửa hoặc thay thế mà không làm ảnh hưởng đến những nơi có sử dụng lớp ban đầu.
- Giống nhau:
   -  Cấu trúc Pattern Bridge:
   1. Client: đại diện cho khách hàng sử dụng các chức năng thông qua Abstraction.
   2. Abstraction : định ra một abstract interface quản lý việc tham chiếu đến đối tượng hiện thực cụ thể (Implementor).
   3. Refined Abstraction (AbstractionImpl) : hiện thực (implement) các phương thức đã được định ra trong Abstraction bằng cách sử dụng một tham chiếu đến một đối tượng của      Implementer.
   4. Implementor : định ra các interface cho các lớp hiện thực. Thông thường nó là interface định ra các tác vụ nào đó của Abstraction.
   5. ConcreteImplementor : hiện thực Implementor interface.
   - Trong đó repo đã sử dụng các ví dụ về vẽ vòng tròn và màu sắc gồm các bước:
      - Tạo class DrawAPI để thực hiện việc tạo giao diện
      - Tạo các class triển khai dao diện dựa trên DrawAPI
      - Tạo class Shape dựa trên DrawAPI
      - Tạo class Circle dựa trên Shape
      - Sử dụng Shape và DrawAPI để vẽ các hình màu khác nhau
```java
        binding.btRed.setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View v) {
                // 画红圆
                Circle circle = new Circle(10, 10, 100, new RedCircle());
                circle.draw();
            }
        });

        binding.btGreen.setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View v) {
                // 画绿圆
                Circle circle = new Circle(20, 20, 100, new GreenCircle());
                circle.draw();
            }
        });
```

-Khác nhau: Về cơ bản, không có quá nhiều sự khác biệt rõ rệt, cơ bản Pattern tuân thủ theo GOF

#### ***3. Composite***
- Bản chất: Composite là một mẫu thiết kế thuộc nhóm cấu trúc (Structural Pattern). Composite Pattern là một sự tổng hợp những thành phần có quan hệ với nhau để tạo ra thành phần lớn hơn. Nó cho phép thực hiện các tương tác với tất cả đối tượng trong mẫu tương tự nhau.
- Trong repo lần này tác giả đã không theo quy tắc chuẩn của GOF bởi không tồn tại FileComponent một cách cụ thể, chỉ tồn tại FolderComposite tên Employee (bản thân đã đóng vai trò là FileComponent và bao gồm các Leaf)
```java
    public class Employee {

    private String name;
    // 部门
    private String dept;
    // 工资
    private int salary;
    // 员工 list
    private List<Employee> subordinates;

    public Employee(String name, String dept, int salary) {
        this.name = name;
        this.dept = dept;
        this.salary = salary;
        this.subordinates = new ArrayList<Employee>();
    }

    public void add(Employee e) {
        subordinates.add(e);
    }

    public void remove(Employee e) {
        subordinates.remove(e);
    }

    public List<Employee> getSubordinates() {
        return subordinates;
    }

    @Override
    public String toString() {
        return "Employee{" +
                "name='" + name + '\'' +
                ", dept='" + dept + '\'' +
                ", salary=" + salary +
                ", subordinates=" + subordinates +
                '}';
    }
```

-  Và class **Client** (class CompositeActivity) để thao tác:

```java
        final Employee ceo = new Employee("John", "CEO", 30000);

        Employee headSales = new Employee("Robert", "Head sales", 20000);

        Employee headMarketing = new Employee("Michel", "Head Marketing", 20000);

        Employee clerk1 = new Employee("Laura", "Marketing", 10000);
        Employee clerk2 = new Employee("Bob", "Marketing", 10000);

        Employee salesExecutive1 = new Employee("Richard", "Sales", 10000);
        Employee salesExecutive2 = new Employee("Rob", "Sales", 10000);

        ceo.add(headSales);
        ceo.add(headMarketing);

        headSales.add(salesExecutive1);
        headSales.add(salesExecutive2);

        headMarketing.add(clerk1);
        headMarketing.add(clerk2);
```
#### ***4. Decorator***
- Bản chất: Decorator pattern là một trong những Pattern thuộc nhóm cấu trúc (Structural Pattern). Nó cho phép người dùng thêm chức năng mới vào đối tượng hiện tại mà không muốn ảnh hưởng đến các đối tượng khác. Kiểu thiết kế này có cấu trúc hoạt động như một lớp bao bọc (wrap) cho lớp hiện có. Mỗi khi cần thêm tính năng mới, đối tượng hiện có được wrap trong một đối tượng mới (decorator class).
- Repo đang xét đã lấy ví dụ về việc đính đá quý (GemDecorator) lên các thiết bị (Equips) trong trò chơi, trong đó:
  -  Lớp **Component** tên IEquips là một interface quy định các method chung cần phải có cho tất cả các thành phần tham gia vào mẫu này (ví dụ ở đây là calculateAttack và description)
```java
        public interface IEquip {

            /**
             * 计算攻击力
             */
            public int caculateAttack();

            /**
             * 装备的描述
             */
            public String description();
        }
```
  - Lớp **ConcreteComponent** là các lớp thực thể của Component (ArmEquips, RingEquips,...)
  - Lớp **Decorator** tên IEuipDecorator quản lý việc "đính" các đá quý lên trang bị (Concrete Component)
  - Lớp **Concrete Decorator** là lớp hiện thực cho Decorator, cụ thể ở đây là các loại đá quý (BlueGemDecorator, RedGemDecorator, GreenGemDecorator)
 - Tổng kết, không có quá nhiều sự khác biệt rõ rệt, cơ bản Pattern tuân thủ theo GOF
#### ***5. Facade***
- Bản chất: Facade Pattern là một trong những Pattern thuộc nhóm cấu trúc (Structural Pattern). Pattern này cung cấp một giao diện chung đơn giản thay cho một nhóm các giao diện có trong một hệ thống con (subsystem). Facade Pattern định nghĩa một giao diện ở một cấp độ cao hơn để giúp cho người dùng có thể dễ dàng sử dụng hệ thống con này.
- Giống nhau:
  - Repo đã dùng cấu trúc của việc chiếu phim tại nhà (HomeTheater) để làm ví dụ, trong đó: 
    - Khi xem phim tại nhà, ta cần chuẩn bị rất nhiều thứ như: bật máy tính, tắt đèn, bật máy lòng bỏng ngô, làm bỏng ngô, bật máy chiếu, bật âm thanh.... và sau khi xem xong lại phải lặp lại 1 loạt để thực hiện việc kết thúc. Nếu như không sắp xếp các công việc rõ ràng thì sẽ rất mệt và tốn thời gian!
    - Vì vậy class HomeTheaterFacade đã sắp xếp các công việc riêng lẻ vào từng hành động chính (xem phim và ngừng xem phim) một cách hợp lí. Các công việc này không cần biết class Facade và ko cần Implement từ nó.
```java
      public void watchMovie() {
        computer.on();
        light.down();
        popcornPopper.on();
        popcornPopper.makePopcorn();
        projector.on();
        projector.open();
        player.on();
        player.make3DListener();
    }

    public void stopMovie() {
        computer.off();
        light.up();
        player.off();
        popcornPopper.off();
        projector.close();
        projector.off();
    }
```
- Khác nhau: không có quá nhiều sự khác biệt rõ rệt, cơ bản Pattern tuân thủ theo GOF
#### ***6. Flyweight***
- Bản chất: Flyweight Pattern là một trong những Pattern thuộc nhóm cấu trúc (Structural Pattern). Nó cho phép tái sử dụng đối tượng tương tự đã tồn tại bằng cách lưu trữ chúng hoặc tạo đối tượng mới khi không tìm thấy đối tượng phù hợp.
- Giống nhau:
  - Repo đã sử dụng việc vẽ 20 hình tròn bằng 5 màu tại các vị trí random (ngẫu nhiên) để làm ví dụ, trong đó:
  -  Class FlyweightActivity đóng vai trò như FlyweightFactory quản lí việc vẽ hình tròn. Để tránh tình trạng tràn bộ nhớ hoặc quái tải bộ nhớ, Hàm ShapeFactory cố gắng sử dụng lại các đối tượng hiện có cùng loại và nếu không tìm thấy đối tượng phù hợp, một đối tượng mới sẽ được tạo, nếu hình tròn chuẩn bị vẽ cùng màu thì sẽ gọi sẵn những hình đã vẽ.
```java
    public static Shape getShape(String color) {
        Shape shape = circleMap.get(color);
        if (shape == null) {
            shape = new Circle(color);
            circleMap.put(color, shape);
            Log.e("getShape", "Creating circle of color : " + color);
        }
        return shape;
    }
```
- Khác nhau: không có quá nhiều sự khác biệt rõ rệt, cơ bản Pattern tuân thủ theo GOF

#### ***7. Proxy***

- Bản chất: Proxy Pattern là mẫu thiết kế mà ở đó tất cả các truy cập trực tiếp đến một đối tượng nào đó sẽ được chuyển hướng vào một đối tượng trung gian (Proxy Class). Mẫu Proxy (người đại diện) đại diện cho một đối tượng khác thực thi các phương thức, phương thức đó có thể được định nghĩa lại cho phù hợp với múc đích sử dụng.
- Giống nhau:
   - Repo đã lấy việc load ảnh từ disk làm ví dụ:
   1. Tạo giao diện (Image.java)
   2. Tạo một RealImage lớp thực thể triển khai giao diện. (Lớp proxy tương ứng: ProxyImage)
   3. Khi được yêu cầu, hãy sử dụng ProxyImage để lấy các đối tượng của lớp RealImage. 
```java
     public RealImage(String fileName) {
        this.fileName = fileName;
        loadFromDisk(fileName);
    }

    private void loadFromDisk(String fileName) {
        Log.e("RealImage", "loading " + fileName);
    }

    @Override
    public void display() {
        Log.e("RealImage", "Displaying " + fileName);
    }
```
- Khác nhau: không có quá nhiều sự khác biệt rõ rệt, cơ bản Pattern tuân thủ theo GOF

### **C. Behavioral Pattern**
#### ***1. Chain of Responsibility***
- Bản chất: Chain of Responsiblity cho phép một đối tượng gửi một yêu cầu nhưng không biết đối tượng nào sẽ nhận và xử lý nó. Điều này được thực hiện bằng cách kết nối các đối tượng nhận yêu cầu thành một chuỗi (chain) và gửi yêu cầu theo chuỗi đó cho đến khi có một đối tượng xử lý nó.

- Giống nhau: 
  - Trong cấu trúc của repo:
    - Class AbstractLogger đóng vai trò như một **Handler** là 1 class interface để xử lý các yêu cầu. Gán giá trị cho đối tượng successor
```java
        public abstract class AbstractLogger {

          public static int INFO = 1;
          public static int DEBUG = 2;
          public static int ERROR = 3;

          protected int level;

          protected AbstractLogger nextLogger;

          public void setNextLogger(AbstractLogger nextLogger) {
        this.nextLogger = nextLogger;
          }

          public void logMessage(int level, String message) {
        if (this.level <= level) {
            write(message);
        }

        if (nextLogger != null) {
            nextLogger.logMessage(level, message);
        }
          }

          protected abstract void write(String message);
      }
```
 - Các **ConcreteHandler** (ConsoleLogger, ErrorLogger, FileLogger) xử lý yêu cầu. Có thể truy cập đối tượng successor (thuộc class Handler). Nếu đối tượng ConcreateHandler không thể xử lý được yêu cầu, nó sẽ gởi lời yêu cầu cho successor của nó.

```java
      public class ConsoleLogger extends AbstractLogger {

           public ConsoleLogger(int level) {
         this.level = level;
           }

           @Override
           protected void write(String message) {
         Log.e("---", "Standard Console::Logger  " + message);
           }
       }
       public class FileLogger extends AbstractLogger {

           public FileLogger(int level) {
         this.level = level;
           }

           @Override
           protected void write(String message) {
         Log.e("---", "File::Logger  " + message);
           }
       }
       public class ErrorLogger extends AbstractLogger {

           public ErrorLogger(int level) {
         this.level = level;
           }

           @Override
           protected void write(String message) {
         Log.e("---", "Error Console::Logger  " + message);
           }
       }
```

- Khác nhau: không có quá nhiều sự khác biệt rõ rệt, cơ bản Pattern tuân thủ theo GOF

#### ***2. Command***
- Bản chất: Command Pattern là một trong những Pattern thuộc nhóm hành vi (Behavior Pattern). Nó cho phép chuyển yêu cầu thành đối tượng độc lập, có thể được sử dụng để tham số hóa các đối tượng với các yêu cầu khác nhau như log, queue (undo/redo), transtraction.

- Giống nhau: 
  - Repo bao gồm:
    - Class **Command** là một interface hoặc abstract class, chứa một phương thức trừu tượng thực thi (execute) một hành động (operation)

    - Class ControlPanel giúp điều khiển qua input và QuickCommand giúp thực hiện một lệnh có thể thực hiện nhiều hành động (multiple execute)

    - Các Object như Computer, Light, Door và các option thực hiện cho nó (On, Off)
  
```java
      public class QuickCommand implements Command {

      private Command[] commands;

      public QuickCommand(Command[] commands) {
          this.commands = commands;
      }

      @Override
      public void execute() {
          for (Command command : commands) {
              command.execute();
          }
      }
}
```
- Khác nhau: không có quá nhiều sự khác biệt rõ rệt, cơ bản Pattern tuân thủ theo GOF

#### ***3. Interpreter***
- Bản chất: Interpreter Pattern giúp người lập trình có thể “xây dựng” những đối tượng “động” bằng cách đọc mô tả về đối tượng rồi sau đó “xây dựng” đối tượng đúng theo mô tả đó.
- Giống nhau
  - Repo bao gồm:
    - Class interface **Expression** định nghĩa phương thức interpreter chung cho tất cả các node trong cấu trúc cây phân tích ngữ pháp   
```java
      public interface Expression {
        public boolean interpreter(String content);
      }
```
    - Class **NonTerminalExpression** (TerminalExpression, OrExpression, AndExpression) có chức năng cài đặt các phương thức của Expression và dùng các class có kiểu Expression để phân tích, mô tả đối tượng
    
```java
       public class OrExpression implements Expression {

          private Expression expression1;
          private Expression expression2;

          public OrExpression(Expression expression1, Expression expression2) {
            this.expression1 = expression1;
            this.expression2 = expression2;
          }

          @Override
          public boolean interpreter(String content) {
            return expression1.interpreter(content) || expression2.interpreter(content);
          }
      }
```
- Khác nhau:
  - Repo chỉ sử dụng **NonTerminalExpression** (biểu thức không đầu cuối) chứ không dùng **TerminalExpression** (biểu thức đầu cuối)
  - Không có quá nhiều sự khác biệt rõ rệt, cơ bản Pattern tuân thủ theo GOF
#### ***4. Iterator***
- Bản chất: Iterator Pattern là một trong những Pattern thuộc nhóm hành vi (Behavior Pattern). Nó được sử dụng để “Cung cấp một cách thức truy cập tuần tự tới các phần tử của một đối tượng tổng hợp, mà không cần phải tạo dựng riêng các phương pháp truy cập cho đối tượng tổng hợp này”.
- Giống nhau:
  - Trong repo gồm:
  - **Iterator**: là một interface hay abstract class, định nghĩa các phương thức để truy cập và duyệt qua các phần tử
```java
    public interface Iterator {

      public boolean hasNext();

      public Object next();
    }
```
    - **ConcreteIterator**: cài đặt các phương thức của Iterator, giữ index khi duyệt qua các phần tử.

```java
      public class NameRepository implements Container {

          private String names[] = {"John", "jingbin", "youlookwhat", "lookthis"};

          @Override
          public Iterator getIterator() {
              return new NameIterator();
          }

          private class NameIterator implements Iterator {

              int index;

              @Override
              public boolean hasNext() {
                  if (index < names.length) {
                      return true;
                  }
                  return false;
              }

              @Override
              public Object next() {
                  if (hasNext()) {
                      return names[index++];
                  }
                  return null;
              }
          }
      }
```
    
- Khác nhau:
   - Tác giả repo đã không sử dụng các thành phần **Aggregate** và **ConcreteAggregate**
 
#### ***5. Mediator***
- Bản chất: Mediator Pattern là một trong những Pattern thuộc nhóm hành vi (Behavior Pattern). Mediator có nghĩa là người trung gian. Pattern này nói rằng “Định nghĩa một đối tượng gói gọn cách một tập hợp các đối tượng tương tác. Mediator thúc đẩy sự khớp nối lỏng lẻo (loose coupling) bằng cách ngăn không cho các đối tượng đề cập đến nhau một cách rõ ràng và nó cho phép bạn thay đổi sự tương tác của họ một cách độc lập”.
- Giống nhau:
  - Trong repo gồm:
    - Colleague : là một abstract class, giữ tham chiếu đến Mediator object.
    - ConcreteColleague : cài đặt các phương thức của Colleague. Giao tiếp thông qua Mediator khi cần giao tiếp với Colleague khác.
    - Mediator : là một interface, định nghĩa các phương thức để giao tiếp với các Colleague object.
    - ConcreteMediator : cài đặt các phương thức của Mediator, biết và quản lý các Colleague object.
- Không có quá nhiều sự khác biệt rõ rệt, cơ bản Pattern tuân thủ theo GOF
#### ***6. Memento***
- Bản chất: Memento là một trong những Pattern thuộc nhóm hành vi (Behavior Pattern). Memento là mẫu thiết kế có thể lưu lại trạng thái của một đối tượng để khôi phục lại sau này mà không vi phạm nguyên tắc đóng gói.
- Giống nhau:
  - Trong repo gồm:
    - Originator : đại diện cho đối tượng mà chúng ta muốn lưu. Nó sử dụng memento để lưu và khôi phục trạng thái bên trong của nó.
    - Caretaker : Nó không bao giờ thực hiện các thao tác trên nội dung của memento và thậm chí nó không kiểm tra nội dung. Nó giữ đối tượng memento và chịu trách nhiệm bảo vệ an toàn cho các đối tượng. Để khôi phục trạng thái trước đó, nó trả về đối tượng memento cho Originator.
    - Memento : đại diện cho một đối tượng để lưu trữ trạng thái của Originator. Nó bảo vệ chống lại sự truy cập của các đối tượng khác ngoài Originator.
- Không có quá nhiều sự khác biệt rõ rệt, cơ bản Pattern tuân thủ theo GOF
#### ***7. Observer***
- Bản chất: Observer Pattern là một trong những Pattern thuộc nhóm hành vi (Behavior Pattern). Nó định nghĩa mối phụ thuộc một – nhiều giữa các đối tượng để khi mà một đối tượng có sự thay đổi trạng thái, tất các thành phần phụ thuộc của nó sẽ được thông báo và cập nhật một cách tự động.
- Giống nhau:
  - Trong repo gồm:
    - Subject : chứa danh sách các observer,  cung cấp phương thức để có thể thêm và loại bỏ observer.
    - Observer : định nghĩa một phương thức update() cho các đối tượng sẽ được subject thông báo đến khi có sự thay đổi trạng thái.
    - ConcreteSubject : cài đặt các phương thức của Subject, lưu trữ trạng thái danh sách các ConcreateObserver, gửi thông báo đến các observer của nó khi có sự thay đổi trạng thái.
    - ConcreteObserver : cài đặt các phương thức của Observer, lưu trữ trạng thái của subject, thực thi việc cập nhật để giữ cho trạng thái đồng nhất với subject gửi thông báo đến.
- Không có quá nhiều sự khác biệt rõ rệt, cơ bản Pattern tuân thủ theo GOF
#### ***8. State***
- Bản chất: State Pattern là một trong những Pattern thuộc nhóm hành vi (Behavior Pattern). Nó cho phép một đối tượng thay đổi hành vi của nó khi trạng thái nội bộ của nó thay đổi. Đối tượng sẽ xuất hiện để thay đổi lớp của nó.
- Giống nhau:
  - Trong repo gồm:
    - Context : được sử dụng bởi Client. Client không truy cập trực tiếp đến State của đối tượng. Lớp Context này chứa thông tin của ConcreteState object, cho hành vi nào tương ứng với trạng thái nào hiện đang được thực hiện.
    - State : là một interface hoặc abstract class xác định các đặc tính cơ bản của tất cả các đối tượng ConcreteState. Chúng sẽ được sử dụng bởi đối tượng Context để truy cập chức năng có thể thay đổi.
    - ConcreteState : cài đặt các phương thức của State. Mỗi ConcreteState có thể thực hiện logic và hành vi của riêng nó tùy thuộc vào Context.
- Không có quá nhiều sự khác biệt rõ rệt, cơ bản Pattern tuân thủ theo GOF
#### ***9. Strategy***
- Bản chất: Strategy Pattern là một trong những Pattern thuộc nhóm hành vi (Behavior Pattern). Nó cho phép định nghĩa tập hợp các thuật toán, đóng gói từng thuật toán lại, và dễ dàng thay đổi linh hoạt các thuật toán bên trong object. Strategy cho phép thuật toán biến đổi độc lập khi người dùng sử dụng chúng.
- Giống nhau:
   - Repo bao gồm:
    - Class **Strategy** tên Role định nghĩa các hành vi có thể có của một Strategy 
```java
      public abstract class Role {

        protected String name;

        // 着装
        protected abstract void display();

        // 逃跑
        protected abstract void run();

        // 攻击
        protected abstract void attack();

        // 防御
        protected abstract void defend();
    }
```
   - Các **ConcreteStrategy** class (IAttackBehavior, IDefendBehavior IRunBehavior) cài đặt các hành vi cụ thể của Strategy. 
- Khác nhau: Không tồn tại class **Context** đóng vai trò nhận các yêu cầu từ Client, các yêu cầu này sau đó được ủy quyền cho Strategy thực hiện.
#### ***10. Template Method***
- Bản chất: Template Method Pattern là một trong những Pattern thuộc nhóm hành vi (Behavior Pattern). Pattern này nói rằng “Định nghĩa một bộ khung của một thuật toán trong một chức năng, chuyển giao việc thực hiện nó cho các lớp con. Mẫu Template Method cho phép lớp con định nghĩa lại cách thực hiện của một thuật toán, mà không phải thay đổi cấu trúc thuật toán“.
- Giống nhau
  - Repo bao gồm:
   - Class **Abstract Class** tên Worker có chức năng định nghĩa các phương thức trừu tượng cho từng bước có thể được điều chỉnh bởi các lớp con, cài đặt một phương thức duy nhất điều khiển thuật toán và gọi các bước riêng lẻ đã được cài đặt ở các lớp con. 
```java 
      public abstract class Worker {

          protected String name;

          public Worker(String name) {
              this.name = name;
          }

          /**
           * 具体方法
           */
          public final void workOneDay() {
              Log.e("workOneDay", "-----------------work start----------------");

              enterCompany();
              computerOn();
              work();
              computerOff();
              exitCompany();

              Log.e("workOneDay", "-----------------work end----------------");
          }

          /**
           * 工作  抽象方法
           */
          public abstract void work();

          /**
           * 钩子方法
           */
          public boolean isNeedPrintDate() {
              return false;
          }

          private void exitCompany() {
              if (isNeedPrintDate()) {
                  Log.e("exitCompany", "---" + new Date().toLocaleString() + "--->");
              }
              Log.e("exitCompany", name + "---离开公司");
          }

      //    -----------------------------------

          private void computerOn() {
              Log.e("computerOn", "---打开电脑");
          }

          private void computerOff() {
              Log.e("computerOff", "---关闭电脑");
          }

          private void enterCompany() {
              Log.e("enterCompany", "---进入公司");
          }
      }

```
   - Class **ConcreteClass** (CTOWorker.java, HRWorker.java, ITWorker.java, OtherWorker.java, QAWorker.java) là các thuật toán cụ thể, cài đặt các phương thức của AbstractClass. Các thuật toán này ghi đè lên các phương thức trừu tượng để cung cấp các triển khai thực sự.
- Không có quá nhiều sự khác biệt rõ rệt, cơ bản Pattern tuân thủ theo GOF
#### ***11. Visitor***
- Bản chất: Visitor Pattern là một trong những Pattern thuộc nhóm hành vi (Behavior Pattern). Visitor cho phép định nghĩa các thao tác (operations) trên một tập hợp các đối tượng (objects) không đồng nhất (về kiểu) mà không làm thay đổi định nghĩa về lớp (classes) của các đối tượng đó. Để đạt được điều này, trong mẫu thiết kế visitor ta định nghĩa các thao tác trên các lớp tách biệt gọi các lớp visitors, các lớp này cho phép tách rời các thao tác với các đối tượng mà nó tác động đến. Với mỗi thao tác được thêm vào, một lớp visitor tương ứng được tạo ra.
- Giống nhau: 
  - Trong repo bao gồm:
   - Class **Visitor** tên ComputerPart là một interface hoặc một abstract class được sử dụng để khai báo các hành vi cho tất cả các loại visitor
```java
        public interface ComputerPartVisitor {

            public void visit(Computer computer);

            public void visit(Mouse mouse);

            public void visit(Keyboard keyboard);

            public void visit(Monitor monitor);
        }
```
  - Class **ConcreteVisitor** tên ComputerPartVisitorDisplay dành cho khách truy cập và hiển thị output

  - Class **Element** tên ComputerPart là một thành phần trừu tượng, nó khai báo phương thức accept() và chấp nhận đối số là Visitor
```java
        public interface ComputerPart {
            public void accept(ComputerPartVisitor computerPartVisitor);
        }
```
  - Class **ConcreteElement** (Computer, Keyboard,...) cài đặt phương thức đã được khai báo trong Element dựa vào đối số visitor được cung cấp
- Khác nhau: Không có quá nhiều sự khác biệt rõ rệt, cơ bản Pattern tuân thủ theo GOF


## Tổng Kết: Mặc dù có sự khác biệt ở một số Patterns, các mẫu thiết kế được sử dụng và phát triển đều khá giống và tương đồng với 23 mẫu có sẵn của Gang Of Four!
