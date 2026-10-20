```cpp                    
                    Level 3 – Object Copy, Pointer và Memory
1. Object Assignment (Gán object)
2. Copy Constructor
3. Copy by Value (Truyền và trả về bằng giá trị)
4. Copy by Reference (Truyền bằng tham chiếu)
5. Shallow Copy
6. Deep Copy
7. Dynamic Memory (Bộ nhớ động)
8. Pointer Ownership (Quyền sở hữu tài nguyên)
9. Rule of Three
10. Object Slicing
--------------------------------------------------------------------- 
1. Object Assignment (Gán object)
    1.1. Phân biệt khởi tạo và gán
        #include <iostream>
        using namespace std;

        class Data {
        public:
            int value;

            Data(int v) : value(v) {}
        };

        Data a(10);
            // Khởi tạo object mới, b và c là những object mới, được khởi tạo từ a. Với các câu lệnh này, Copy Constructor được sử dụng.
        Data b = a;
        Data c(a);

            // Gán cho object đã tồn tại, d đã được tạo trước đó. Câu lệnh d = a gán dữ liệu từ a sang d, sử dụng Copy Assignment Operator.
        Data d(20);
        d = a;

    1.2. Copy Assignment Operator (operator=).
        Data a(10);
        Data b = a;  // Copy Constructor
        Data c(a);   // Copy Constructor

        Data d;
        d = a;       // Copy Assignment Operator

        - có thể tự định nghĩa:
            class Data 
            {
            public:
                int value;

                Data(int v) : value(v) {}

                Data& operator=(const Data& other) {
                    value = other.value;
                    return *this;
                }
            };
            + Giải thích:
                * operator= là hàm nạp chồng toán tử gán.
                * const Data& other nhận object nguồn bằng const reference, tránh tạo bản sao không cần thiết.
                * value = other.value sao chép giá trị member.
                * return *this trả về reference đến object đích, cho phép dùng phép gán nối tiếp như a = b = c.
            + Trong thực tế, với class chỉ chứa các member đơn giản như int, thường không cần tự viết operator=.

2. Copy Constructor
3. Copy by Value (Truyền và trả về bằng giá trị)
    3.1. Truyền object bằng value
        void change(Data d) {
            d.value = 200;
        }

        int main() {
            Data a(10);

            change(a);

            cout << a.value;
        }
        --> Output: 10

    3.2. Trả về object bằng value
        Data createData() {
            Data d(500);
            return d;
        }

        int main() {
            Data result = createData();

            cout << result.value;
        }
        --> Output: 500

4. Copy by Reference (Truyền bằng tham chiếu)
    4.1. Reference thông thường
        void updateData(Data& d)
        {
            d.value = 20;
        }

    4.2. Const Reference
        void readData(const Data& d)
        {
            // Chỉ đọc, không copy object
            d.value = 20; // Lỗi

        }


5. Shallow Copy (Sao chép nông)
    - Shallow Copy sao chép giá trị của từng member trong object gốc sang object mới.
    - Nếu member là biến thông thường (int, float...), giá trị của nó được sao chép.
    - Nếu member là con trỏ (int*), giá trị được sao chép chính là địa chỉ bộ nhớ mà con trỏ đang lưu.
    - Vì vậy, con trỏ trong object gốc và object mới cùng trỏ đến một vùng nhớ.

    - VD:
        class Data
        {
        public:
            int* value;

            Data(int v)
            {
                value = new int(v);
            }

            ~Data()
            {
                delete value;
            }
        };
        Data a(10);
        Data b = a;  // Shallow Copy

        --> Trong trường hợp này, a.value và b.value cùng lưu một địa chỉ. Khi cả hai object bị hủy, chương trình có nguy cơ giải phóng cùng một vùng nhớ hai lần, dẫn đến undefined behavior.


    - Khi viết: Data a(10);
                Data b = a;
        --> Compiler sẽ tự sinh một Copy Constructor tương đương với:
            Data(const Data& other) : value(other.value)
            {
            }
        --> nên không cần tự viết hàm:
            Data(const Data& other)
            {
                value = other.value;
            }

    - Các cách viết:
            + Data b = a;
            + Data b(a);
            + Data b{a};
            + Data b = {a};
            +
                void func(Data d)
                {
                }

                Data a(10);
                func(a);
                --> d là object mới được khởi tạo từ a. Với ví dụ này, Copy Constructor được gọi.

            + Data a(10);
              Data* p = new Data(a);
                --> new Data(a) tạo object mới trên vùng nhớ động, được khởi tạo từ a.

    - Trường hợp không gọi Copy Constructor:
                Data a(10);
                Data b(20);

                b = a;
            --> Đây là Copy Assignment Operator, không phải Copy Constructor, vì b đã tồn tại trước khi được gán.
            --> Copy Constructor được gọi khi một object mới được khởi tạo từ object cùng kiểu; còn b = a trên object đã tồn tại là phép gán, không phải khởi tạo.


6. Deep Copy (Sao chép sâu)
    - Cấp phát vùng nhớ:
            Data(const Data& other)
            {
                value = new int(*other.value);  // cấp phát vùng nhớ mới
            }
    - VD:
            class Data
            {
            public:
                int* value;

                Data(int v)
                {
                    value = new int(v);
                }

                Data(const Data& other)
                {
                    value = new int(*other.value);
                }

                ~Data()
                {
                    delete value;
                }
            };  


    - Khi cấp phát bộ nhớ động bằng new trong C++, bạn cần một pointer để lưu địa chỉ vùng nhớ được cấp phát nếu muốn truy cập vùng nhớ đó sau này.
            VD: int* p = new int(10);

            p
            │
            │ giá trị của p = địa chỉ vùng nhớ heap
            ↓
            ┌──────┐
            │  10  │
            └──────┘
                int* arr = new int[10]; //→ cấp phát động 10 ô int trên heap.

    - Không phải cứ bộ nhớ động là bản thân khái niệm "bắt buộc phải có pointer" theo nghĩa tuyệt đối. Trong C++ hiện đại, bạn thường dùng smart pointer hoặc container để quản lý memory:
            std::unique_ptr<int> p = std::make_unique<int>(10);
                unique_ptr vẫn là một smart pointer quản lý vùng heap cho bạn.
            Và với: std::vector<int> arr(10);
                bạn cũng có bộ nhớ được quản lý động bên dưới, nhưng không cần tự khai báo int*.

    - Hiện tại:
            new
            ↓
            cấp phát heap
            ↓
            trả về địa chỉ
            ↓
            pointer lưu địa chỉ
            ↓
            *p truy cập dữ liệu


7. Dynamic Memory (Bộ nhớ động)
    7.1. new và delete
    - new: cấp phát bộ nhớ và khởi tạo object.
    - delete: hủy object và giải phóng bộ nhớ.
    - VD:
        #include <iostream>
        using namespace std;

        int main() {
            int* p = new int(10);
            cout << *p << '\n';  // 10

            *p = 20;
            cout << *p << '\n';  // 20

            delete p;
            p = nullptr;        // giúp tránh vô tình sử dụng lại địa chỉ cũ thông qua p.
        }
    - Quan trọng: delete p không tự động gán p = nullptr. Nếu không tự gán lại, p vẫn giữ địa chỉ cũ nhưng địa chỉ đó không còn hợp lệ để truy cập object.

    7.2. new[] và delete[]
    - new[]: cấp phát mảng động.
    - delete[]: hủy các phần tử và giải phóng mảng động.
    - VD:
        int* arr = new int[5];

        arr[0] = 10;
        arr[1] = 20;
        arr[2] = 30;

        cout << arr[0] << '\n';
        cout << arr[1] << '\n';

        delete[] arr;
        arr = nullptr;
        --> Output: 10
                    20
          
    7.3. Nếu quên delete          
        void test() {
            int* p = new int(10);
        }         
    - Khi hàm test() kết thúc, biến pointer p cục bộ biến mất. Nhưng vùng nhớ động được cấp phát không tự giải phóng chỉ vì pointer biến mất.
    - Đây là một trường hợp có thể gây memory leak — rò rỉ bộ nhớ.
    - Với chương trình dài hạn, chẳng hạn ứng dụng chạy liên tục hoặc hệ thống nhúng, memory leak có thể làm lượng bộ nhớ khả dụng giảm dần.
    - Trong C++ hiện đại, nên ưu tiên những công cụ quản lý tài nguyên tự động như std::unique_ptr và std::vector thay vì tự quản lý new/delete khi không cần thiết. 
 
8. Pointer Ownership (Quyền sở hữu tài nguyên)
    8.1. Ownership nghĩa là gì?
    - Ownership trả lời câu hỏi: Ai chịu trách nhiệm giải phóng tài nguyên?
        int* p = new int(10);
        int* q = p;
        --> q cũng lưu cùng địa chỉ với p.

    - Nếu cả hai pointer đều được xem là chủ sở hữu và cả hai đều thực hiện delete, chương trình có nguy cơ giải phóng cùng vùng nhớ hai lần.
    - Thực tế, int* không tự thể hiện ownership. Việc p và q cùng lưu một địa chỉ không có nghĩa chúng đều có quyền sở hữu vùng nhớ.

    8.2. Dùng std::unique_ptr để thể hiện ownership
  
9. Rule of Three
    - Nếu một class tự quản lý tài nguyên bằng raw pointer và tự định nghĩa destructor để giải phóng tài nguyên, bạn cần xem xét cả:
        + Destructor.
        + Copy Constructor.
        + Copy Assignment Operator.
        --> Rule of Three.
    - VD:
        class Data {
        public:
            int* value;

            Data(int v) : value(new int(v)) {}

            Data(const Data& other)
                : value(new int(*other.value)) {}

            Data& operator=(const Data& other) {
                if (this != &other) {
                    *value = *other.value;
                }
                return *this;
            }

            ~Data() {
                delete value;
            }
        };
        + Trong thực tế, tự quản lý tài nguyên phức tạp hơn ví dụ trên vì còn phải xử lý an toàn ngoại lệ, tài nguyên cũ và nhiều tình huống khác. 
        Khi có thể, hãy ưu tiên RAII và các kiểu quản lý tài nguyên như std::unique_ptr, std::vector thay vì tự viết cả ba hàm.


10. Object Slicing
    10.1. Object Slicing là gì?
    - Object Slicing xảy ra khi bạn copy một object của class con sang một object của class cha bằng value, khiến phần dữ liệu riêng của class con bị mất khỏi object đích.
    - VD:
        #include <iostream>
        using namespace std;

        class Animal {
        public:
            int age = 5;
        };

        class Dog : public Animal {
        public:
            int legs = 4;
        };

        int main() {
            Dog d;
            Animal a = d;  // Object Slicing
            cout << a.age << '\n';
        }
        --> Output: 5

    - Giải thích:
        + Object d — kiểu Dog:
            age = 5
            legs = 4
        + Object a — kiểu Animal
            age = 5
            Object đích chỉ chứa phần Animal; phần riêng của Dog không được giữ lại.
        + a thực sự là một object kiểu Animal, không phải object Dog. Dữ liệu legs không được giữ trong a.

    10.2. Truyền class con vào hàm nhận class cha bằng value
        void showAnimal(Animal a) {
            cout << a.age;
        }

        Dog d;
        showAnimal(d);

    - Hàm nhận Animal a theo value. Khi truyền Dog vào, một object Animal được tạo từ phần Animal của Dog, nên xảy ra slicing.
    - Nếu muốn giữ tính đa hình, thường dùng reference hoặc pointer.    

    10.3. Cách tránh Object Slicing
    - Cách 1: Dùng reference
        void showAnimal(const Animal& a) {
            cout << a.age;
        }
        Dog d;
        showAnimal(d);
        --> Ở đây a tham chiếu đến object Dog ban đầu, không tạo object Animal riêng, nên không xảy ra slicing.

    - Cách 2: Dùng pointer
        void showAnimal(const Animal* a) {
            cout << a->age;
        }
        Dog d;
        showAnimal(&d);
        --> Pointer trỏ đến object Dog, nên cũng không xảy ra slicing.
        --> Nếu mục tiêu là gọi đúng hàm ảo của class con, class cha cần khai báo hàm đó là virtual.

    - Chốt lại: Chỉ cần nhớ quy tắc: khi truyền hoặc khởi tạo object class cha bằng value từ object class con, có thể xảy ra slicing. Nếu muốn giữ nguyên object class con và tận dụng polymorphism, hãy dùng reference hoặc pointer.



