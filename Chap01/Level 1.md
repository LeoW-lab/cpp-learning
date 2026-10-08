```cpp 
                    Level 1 – C++ cơ bản và kiểu dữ liệu
Class
    class Motor {
    public:
        int speed = 100;

        void showSpeed() {
            std::cout << this->speed;
        }
    };
    Motor m1;
    Motor m2;

    - Theo mặc định trong C++, biến thành viên là riêng cho từng object, còn hàm thành viên thông thường dùng chung.
    m1.speed và m2.speed là hai vùng dữ liệu riêng biệt.
    showSpeed() chỉ có một phần mã hàm dùng chung cho cả hai object.
    this giúp hàm biết object nào đang gọi nó.
    Biến static là ngoại lệ: các object dùng chung một biến. Hàm static cũng thuộc về class và không có this.

    m1.showSpeed(); C++ ngầm truyền địa chỉ của m1 vào hàm thành viên. Có thể hình dung nó hoạt động gần giống như: showSpeed(&m1); (minh họa)
    Trong hàm, this nhận địa chỉ của m1, nên: this->speed tương đương với m1.speed


    - Class tổng quát:
    #include <iostream>
    class Motor {
    private:
        int speed;                         // Member variable
        static int count;                  // Static member variable

    public:
        using Speed = int;                 // Type alias
        enum class State { ON, OFF };      // Nested type
        Motor(int s) : speed(s) {          // Constructor
            count++;
        }
        ~Motor() {                         // Destructor
            count--;
        }
        void showSpeed() const {           // Member function
            std::cout << speed << '\n';
        }
        static int getCount() {            // Static member function
            return count;
        }
        friend void showMotor(const Motor& m); // Friend function
        Motor& operator=(const Motor&) = default; // Operator overloading
    };

    int Motor::count = 0;
    void showMotor(const Motor& m) {
        std::cout << m.speed << '\n';
    }



const
    - const variable: biến không được phép thay đổi sau khi khởi tạo.
    - const phải được khởi tạo khi khai báo đối với biến thông thường.
    - const với con trỏ:
        + const int* p: không thể sửa giá trị thông qua p.
        + int* const p: không thể thay đổi địa chỉ mà p lưu giữ.
        + const int* const p: không thể thay đổi cả địa chỉ lẫn giá trị thông qua p.
    - const với tham số hàm.
    - const với giá trị trả về, ở mức nhận biết.
    - Phân biệt const và constexpr ở mức cơ bản.

Reference
    Khái niệm Reference: bí danh (alias) của một biến hoặc object.
    Khai báo Lvalue Reference: int& ref = a;
    Reference phải được khởi tạo khi khai báo.
    Reference không thể đổi sang tham chiếu một biến khác sau khi khởi tạo.
    Reference không phải là một object độc lập.
    Reference với biến và với object.
    Pass by reference.
    Const Lvalue Reference: const int& ref = a;
    Pass by const reference.
    Phân biệt Reference và Pointer.
    Rvalue Reference: T&& (chỉ học cú pháp và nhận biết, phần Move Semantics học ở Level 5).
        int a = 10;
        int& ref = a;

        ref = 20;   // a = 20

        const int& cref = a;    // cref = 30; // Lỗi

        ref và a cùng tham chiếu đến một đối tượng. Thay đổi thông qua ref cũng làm thay đổi a.

            REFERENCE
            │
            ├── Lvalue reference
            │   └── T&
            │       ├── reference tới variable
            │       ├── reference tới object
            │       └── dùng làm function parameter
            │
            ├── const lvalue reference
            │   └── const T&
            │       └── reference chỉ đọc
            │
            └── Rvalue reference
                └── T&&
                    └── move semantics

Pointer





