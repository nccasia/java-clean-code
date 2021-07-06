# java-clean-code
[1. Khái niệm](#1-khái-niệm)

[2. Tại sao cần Clean Code?](#2-tại-sao-cần-clean-code)

[3. Đặc điểm](#3-đặc-điểm)

[4. Clean Coding trong Java](#4-clean-coding-trong-java)
  - [4.1. Cấu trúc của Project](#41-cấu-trúc-của-project)
  - [4.2. Quy tắc đặt tên](#42-quy-tắc-đặt-tên)
  - [4.3. Hàm](#43-hàm)
  - [4.4. Chú thích](#44-chú-thích)
  - [4.5. Đối tượng và Cấu trúc dữ liệu](#45-đối-tượng-và-cấu-trúc-dữ-liệu)
  - [4.6. SOLID](#46-solid)
  - [4.7. Xử lý ngoại lệ](#47-xử-lý-ngoại-lệ)
  
[5. Tham khảo](#tham-khảo)
## 1. Khái niệm
**Clean Code** hay **Code sạch**, là code thể hiện rõ ý đồ của lập trình viên, đồng thời khiến người khác có thể hiểu một dễ dàng
## 2. Tại sao cần Clean Code?
- Dễ dàng trong việc bảo trì và phát triển
- Giúp người khác dễ đọc hiểu code hơn
- Thể hiện trình độ của lập trình viên
## 3. Đặc điểm
- **Focus**: Code được viết để giải quyết một vấn đề cụ thể
- **Simple**: Càng đơn giản càng tốt
- **Testable**: Trực quan và dễ dàng cho việc test
## 4. Clean Coding trong Java
### 4.1. Cấu trúc của Project
- *src/main/java*: source files
- *src/main/resources*: resource files, ví dụ như properties, temple
- *src/test/java*: test source files
- *src/test/resources*: test resource files, ví dụ như properties, temple
### 4.2. Quy tắc đặt tên
#### Tên thể hiện mục đích
>Không tốt
```java
  int d; // elapsed time in days
```
>Tốt
```java
  int elapsedTimeInDays;
  int daysSinceCreation;
```
#### Tên có ý nghĩa và dễ phát âm
>Không tốt
```java
  String yyyymmdstr = new SimpleDateFormat("YYYY/MM/DD").format(new Date());
```
>Tốt
```java
  String currentDate = new SimpleDateFormat("YYYY/MM/DD").format(new Date());
```
#### Sử dụng những tên có thể tìm kiếm được
>Không tốt
```java
  // what is 86400000?
  setTimeout(blastOff, 86400000);
```
>Tốt
```java
  public static final int MILLISECONDS_IN_A_DAY = 86400000;
  setTimeout(blastOff, MILLISECONDS_IN_A_DAY);
```
#### Các tên phải có ý nghĩa khác biệt
>Không tốt
```java
  getUserInfo();
  getClientData();
  getCustomerRecord();
```
>Tốt
```java
  getUser();
```
#### Tên lớp
Tên lớp và các đối tượng nên sử dụng danh từ hoặc cụm danh từ, như **Customer**, **WikiPage**,
**Account**, và **AddressParser**
#### Dùng thuật ngữ
Hãy sử dụng các thuật ngữ khoa học, design pattern,... cho việc đặt tên
```java
  public class userService{
    
  }
  public class userRepository{

  }
```
#### Tên phương thức
Tên các phương thức nên có động từ hoặc cụm động từ như `postMessage`, `deleteMessage`,
hoặc save. Các phương thức truy cập, chỉnh sửa thuộc tính phải được đặt tên cùng với `get`, `set` và
`is` theo tiêu chuẩn của **Javabean**.

### 4.3. Hàm
#### Đối số của hàm (lý tưởng là ít hơn hoặc bằng 2)
Giới hạn số lượng param của hàm là một điều cực kì quan trọng bởi vì nó làm cho hàm của bạn trở nên dễ test hơn. Trường hợp có nhiều hơn 3 params có thể dẫn đến việc phải test hàng tấn test case khác nhau với những đối số riêng biệt.
#### Hàm chỉ nên giải quyết một vấn đề
Đây là quy định quan trọng nhất của kỹ thuật phần mềm. Khi một hàm thực hiện nhiều hơn 1 việc, chúng sẽ trở nên khó khăn hơn để viết code, test, và suy luận. Khi có thể tách biệt một hàm để chỉ thực hiện một hành động, thì sẽ dễ dàng hơn để tái cấu trúc và code của bạn sẽ dễ đọc hơn nhiều. 
>Không tốt:

```java
  public void emailClients(List<Client> clients) {
      for (Client client : clients) {
          Client clientRecord = repository.findOne(client.getId());
          if (clientRecord.isActive()){
              email(client);
          }
      }
  }
```
>Tốt:

```java
  public void emailClients(List<Client> clients) {
      for (Client client : clients) {
          if (isActiveClient(client)) {
              email(client);
          }
      }
  }

  private boolean isActiveClient(Client client) {
      Client clientRecord = repository.findOne(client.getId());
      return clientRecord.isActive();
  }
```
#### Tên hàm phải nói ra được những gì chúng làm
>Không tốt:
```JAVA
    private void addToDate(Date date, int month){
    //..
    }
    
    Date date = new Date();
    // Khó để biết được hàm này thêm gì thông qua tên hàm.
    addToDate(date, 1);
```
>Tốt:
```java
    private void addMonthToDate(Date date, int month){
    //..
    }
    
    Date date = new Date();
    addMonthToDate(1, date);
```
#### Xoá những đoạn code trùng lặp
Tuyệt đối tránh những dòng code trùng lặp. Code trùng lặp thì không tốt bởi vì nếu cần thay đổi cùng một logic, chúng ta phải sửa ở nhiều hơn một nơi.

### 4.4. Chú thích
#### Giải thích ý nghĩa ngay trong code
>Không tốt
```java
  // kiểm tra nhân viên có đủ điều kiện để nhận các quyền lợi không
  if ((employee.flags & HOURLY_FLAG) && 
  (employee.age > 65))
```
>Tốt
```java
  if (employee.isEligibleForFullBenefits())
```
#### Chỉ nên viết chú thích cho những đoạn code có logic phức tạp
>Không tốt:
```java
// Tạo list customerNames
  List<String> customerNames = Arrays.asList('Bob', 'Linda', 'Steve', 'Mary'); 

  // Sử dụng stream dể tìm người đầu tiên
  Optional<String> firstCustomer = customerNames.stream().findFirst(); 

  // Nếu stream rỗng thì in ra no value
  if (firstCustomer.isPresent()) { 
      System.out.println(firstCustomer.get()); 
  } else { 
      System.out.println("no value"); 
  }
``` 
>Không tốt:
```java
  List<String> customerNames = Arrays.asList('Bob', 'Linda', 'Steve', 'Mary'); 

  Optional<String> firstCustomer = customerNames.stream().findFirst(); 

  if (firstCustomer.isPresent()) { 
      System.out.println(firstCustomer.get()); 
  } else { 
      System.out.println("no value"); 
  } 
```
#### Không giữ lại những đoạn code bị chú thích
>Không tốt:
```java
  doStuff();
  // doOtherStuff();
  // doSomeMoreStuff();
  // doSoMuchStuff();
```
>Tốt:
```java
  doStuff();
```
#### Không viết chú thích nhật kí
Thay vì sử dụng những chú thích nhật kí, chúng ta có thể sử dụng công cụ như Git. Và `git log` có thể xem lại lịch sử của Soure code.
>Không tốt:
```java
  /**
  * 2016-12-20: Removed monads, didn't understand them (RM)
  * 2016-10-01: Improved using special monads (JP)
  * 2016-02-03: Removed type-checking (LI)
  * 2015-03-14: Added combine with type-checking (JR)
  */
  function combine(a, b) {
    return a + b;
  }
```
>Tốt:
```java
  function combine(a, b) {
    return a + b;
  }
```
#### Không dùng các chú thích để "đánh dấu lãnh thổ"
>Không tốt
```java
  /////////////////////////////////////////////
  //Khai báo
  /////////////////////////////////////////////
  int dayOfMonth;

  /////////////////////////////////////////////
  //Các phương thức
  /////////////////////////////////////////////
  public int getDayOfMonth(){
    return dayOfMonth;
  }
```
>Tốt
```java
  int dayOfMonth;

  public int getDayOfMonth(){
    return dayOfMonth;
  }
```

### 4.5. Đối tượng và Cấu trúc dữ liệu
#### Sử dụng getter và setter
- Khi bạn muốn thực hiện nhiều hơn việc lấy một thuộc tính của đối tượng, bạn không cần phải tìm kiếm và thay đổi mỗi accessor trong codebase của bạn.
- Làm cho việc thêm các validation đơn giản khi thực hiện trên một tập hợp.
- Đóng gói các biểu diễn nội bộ.
- Dễ dàng thêm log và xử lí lỗi khi getting và setting.
- Kế thừa lớp này, bạn có thể override những hàm mặc định.
- Bạn có thể lazy load các thuộc tính của một đối tượng, lấy nó từ server.
#### Sử dụng hàm khởi tạo (constructor) quá nhiều biến hoặc gọi nhiều hàm set liên tiếp nhau
Tránh sử dụng hàm constructor với nhiều hơn 3 đối số truyền vào thay vì vậy hãy áp dụng Builder Pattern

### 4.6. SOLID
**SOLID** là viết tắt của 5 chữ cái đầu trong 5 nguyên tắc thiết kế hướng đối tượng. Giúp cho lập trình viên viết ra những đoạn code dễ đọc, dễ hiểu, dễ maintain. Nó được đưa ra bởi **Robert C. Martin** và **Michael Feathers**. 5 nguyên tắc đó bao gồm:

- **S**ingle responsibility priciple (SRP)
- **O**pen/Closed principle (OCP)
- **L**iskov substitution principe (LSP)
- **I**nterface segregation principle (ISP)
- **D**ependency inversion principle (DIP)

#### Nguyên lí đơn trách nhiệm (Single Responsibility Principle)
Mỗi lớp chỉ nên chịu trách nhiệm về một nhiệm vụ cụ thể nào đó mà thôi.
> Ví dụ
```java
    public class DBHelper {

        public Connection openConnection() {};

        public List<User> getUserByUsername(String user) {};

        public List<Role> getRole() {};

        public void closeConnection() {};
    }
```

```java
    public class DBConnection{

        public Connection openConnection() {};

        public void closeConnection() {};
    }

    public class UserRepository{
        public List<User> getUserByUsername(String user) {};
    }

    public class RoleRepository{
        public List<Role> getRole() {};
    }
```

#### Nguyên lí đóng mở (Open/Closed Principle)
Không được sửa đổi một Class có sẵn, nhưng có thể mở rộng bằng kế thừa.
> Ví dụ
 
Trước đây, logic xử lý tính phí vận chuyển của một đơn hàng được đặt luôn bên trong class Order.
```java
    public class Order {

        public long calculateShipping(ShippingMethod shippingMethod) {
            if (shippingMethod == GROUND) {
                // Calculate for ground shipping
            } else if (shippingMethod == AIR) {
                // Calculate for air shipping
            } else {
                // Default
            }
        }
    }
```
Giả sử hệ thống cần bổ sung thêm một phương thức vận chuyển mới, chúng ta lại phải bổ sung một case nữa trong method `calculateShipping`. Điều này sẽ làm code trở nên rất khó quản lý.

Thay vào đó, chúng ta nên tách rời logic xử lý tính phí vận chuyển vào một interface `Shipping` chẳng hạn. Interface `Shipping` sẽ có nhiều implementation ứng với từng hình thức vận chuyển: `GroundShipping`, `AirShipping`, ...

```java
    public interface Shipping {

        long calculate();
    }

    public class GroundShipping implements Shipping {

        @Override
        public long calculate() {
            // Calculate for ground shipping
        }
    }

    public class AirShipping implements Shipping {

        @Override
        public long calculate() {
            // Calculate for air shipping
        }
    }

    public class SeaShipping implements Shipping {

        @Override
        public long calculate() {
            // Calculate for Sea shipping
        }
    }

    public class Order {

        private Shipping shipping;

        public long calculateShipping(ShippingMethod shippingMethod) {
            // Find relevant Shipping implementation then call calculate() method
        }
    }
```
Như ví dụ trên, khi bổ sung dịch vụ vận chuyển bằng đường biển, ta chỉ cần tạo thêm 1 class `SeaShipping` và implements interface `Shipping`.


#### Nguyên lí thay thế Liskov (Liskov Substitution Principle)
Các đối tượng (instance) kiểu class con có thể thay thế các đối tượng kiểu class cha mà không gây ra lỗi.
>Ví dụ:
```java
    public interface Animal{
        void fly();
    }

    public class Bird implements Animal(){
        @Override
        void fly();
    }

    public class Dog implements Animal(){
        @Override
        void fly(); //?? Dog cannot fly
    }
```
Như ta biết, chó không thể bay được. Vậy như ví dụ trên đã vi phạm Nguyên lí thay thế Liskov.

#### Nguyên lí phân tách interface (Interface Segregation Principle)
Thay vì dùng 1 interface lớn, ta nên tách thành nhiều interface nhỏ, với nhiều mục đích cụ thể.

> Ví dụ:
```java
  interface Animal {

    void eat();

    void run();

    void fly();
}
```

Chúng ta có 2 class `Dog` và `Snake` implement interface `Animal`. Nhưng thật vô lý, `Dog` thì làm sao có thể `fly()`, cũng như Snake không thể nào `run()` được? Thay vào đó, chúng ta nên tách thành 3 interface như thế này:

```java
  interface Animal {
      void eat();
  }

  interface RunnableAnimal extends Animal {
      void run();
  }

  interface FlyableAnimal extends Animal {
      void fly();
  }
```
#### Nguyên lí đảo ngược dependency (Dependency Inversion Principle)
Các module cấp cao không nên phụ thuộc vào các modules cấp thấp. Cả 2 nên phụ thuộc vào abstraction.

Interface (abstraction) không nên phụ thuộc vào chi tiết, mà ngược lại (Các class giao tiếp với nhau thông qua interface (abstraction), không phải thông qua implementation.)
>Ví dụ
```java
    // Interface
    public interface IDatabase {
        void Save(int orderId);
    }

    public interface ILogger {
        void LogInfo(string info);
    }

    public interface IEmailSender {
        void SendEmail(int userId);
    }

    // Các Module implement các Interface
    public class Logger implements ILogger{
        public void LogInfo(string info) {
        }
    }

    public class Database implements IDatabase{
        public void Save(int orderId) {
        }
    }

    public class EmailSender implements IEmailSender{
        public void SendEmail(int userId) {}
    }

    public void Checkout(int orderId, int userId){
        IDatabase db = new Database();
        db.Save(orderId);
    
        ILogger log = new Logger();
        log.LogInfo("Order has been checkout");
    
        IEmailSender es = new EmailSender();
        es.SendEmail(userId);
    }
``` 
### 4.7. Xử lý ngoại lệ
Đừng bỏ qua những ngoại lệ mà có thể bắt được
> Không tốt
```java
  try{
    inputAge();

  } catch (Exception e){

  }
```
> Tốt
```java
  try{
    inputAge();

  } catch (InputMissMatchException e){

  } catch (AgeMustBePositive e){

  } catch (Exception e){
    
  }
```
## Tham khảo

[Clean Code: A Handbook of Agile Software Craftsmanship](https://www.amazon.com/Clean-Code-Handbook-Software-Craftsmanship/dp/0132350882) (Robert C. Martin)

https://github.com/leonardolemie/clean-code-java

https://topdev.vn/blog/solid-la-gi

https://kipalog.com/posts/Tim-hieu-nhanh-SOLID-than-thanh
