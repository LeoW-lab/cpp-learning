```cpp                    
                    Level 2 – Class, Object và Object Lifetime
1. Class và object
2. Access Specifiers: public / protected / private
3. Member Function (Hàm thành viên)
4. this Pointer
5. Constructor
6. Constructor + destructor, object lifetime
7. Object Lifetime (Vòng đời object)
8. Const Member Function và Const Object
9. Static Member và Static Member Function
10. Pointer to Object
11. Reference to Object
12. Function nhận và trả về Object
-----------------------------------------------------------------
1. Class và object

2. Access Specifiers: public / protected / private

3. Member Function (Hàm thành viên)
    - Khai báo và định nghĩa member function.
    - Gọi member function thông qua object.
    - Định nghĩa hàm bên trong class và bên ngoài class bằng toán tử ::.
    - Member function nhận tham số.
    - Member function trả về giá trị.
    - Function overloading trong class.
    - Phân biệt member function và non-member function.
    -------------------------------------------------------------
    - Member Function là gì?
        - VD:
            class Motor
            {
            public:
                int speed;
                void start()
                {
                    speed = 100;
                }
            };

            int main()
            {
                Motor m;
                m.start();
            }
            Trong ví dụ này:
            - Motor là class.
            - speed là Data Member.
            - start() là Member Function.
            - m là object.
            - m.start() là cách gọi Member Function thông qua object.

        - VD:
            class Motor
            {
            private:
                int speed = 0;

            public:
                void start()
                {
                    speed = 100;
                }
            };
            + Dù speed là private, hàm start() vẫn có thể thay đổi nó vì start() là thành viên của Motor.
            + Ngược lại, nếu viết m.speed = 100; ở bên ngoài class thì sẽ bị lỗi truy cập.

    - Khai báo và định nghĩa Member Function
        + Cách 1: Định nghĩa bên trong class
            class Motor
            {
            public:
                void start()
                {
                    std::cout << "Motor started";
                }
            };
            * Khai báo và định nghĩa cùng một chỗ.
            * Phù hợp với những hàm ngắn, đơn giản.
            * Hàm được định nghĩa bên trong class thường được xem là inline theo quy tắc của C++.

        + Cách 2: Định nghĩa bên ngoài class
            class Motor
            {
            public:
                void start();
            };

            void Motor::start()
            {
                std::cout << "Motor started";
            }
            * Bên trong class chỉ khai báo hàm.
            * Bên ngoài class mới định nghĩa phần thân hàm.
            * Dùng toán tử :: (scope resolution operator) để xác định hàm thuộc class nào.
            * Phù hợp khi hàm dài hoặc muốn tách file .h và .cpp.

        + Hai cách trên có thể cho kết quả giống nhau. Điểm khác biệt chủ yếu là cách tổ chức mã nguồn.
        + Ví dụ khi tách file:
            * Motor.h
                class Motor
                {
                public:
                    void start();
                };

            * Motor.cpp
                #include "Motor.h"
                #include <iostream>

                void Motor::start()
                {
                    std::cout << "Motor started";
                }

    - Member Function có tham số và giá trị trả về
        + Member Function hoạt động tương tự hàm thông thường: có thể nhận tham số và trả về giá trị.
        + VD1: Hàm có tham số, không trả về giá trị
            class Motor
            {
            public:
                void setSpeed(int s)
                {
                    std::cout << s;
                }
            };

            int main()
            {
                Motor m;
                m.setSpeed(100);
            }
            --> Kết quả: 100.

        + VD2: Hàm có tham số và có giá trị trả về
            class Motor
            {
            private:
                int speed = 0;

            public:
                int setSpeed(int s)
                {
                    speed = s;
                    return speed;
                }
            };

            int main()
            {
                Motor m;
                int result = m.setSpeed(150);

                std::cout << result;
            }
            --> Kết quả: 150.

    - Function Overloading trong class: Function Overloading là việc định nghĩa nhiều hàm cùng tên nhưng có danh sách tham số khác nhau.
        + VD:
            class Calculator
            {
            public:
                int add(int a, int b)
                {
                    return a + b;
                }

                double add(double a, double b)
                {
                    return a + b;
                }

                int add(int a, int b, int c)
                {
                    return a + b + c;
                }
            };
            Calculator c;

            c.add(2, 3);         // Hàm thứ nhất
            c.add(2.5, 3.5);     // Hàm thứ hai
            c.add(1, 2, 3);      // Hàm thứ ba

            * Compiler lựa chọn hàm phù hợp dựa trên danh sách đối số.

        + Lưu ý quan trọng: Không thể Function Overloading chỉ bằng cách thay đổi kiểu trả về.
            int getValue();
            double getValue(); // Lỗi: không overload được chỉ bằng kiểu trả về

    - Phân biệt Member Function và Non-member Function

        Tiêu chí                        Member Function             Non-member Function
        Thuộc về class	                    Có	                        Không
        Cách gọi	                        m.start()	                start(m)
        Truy cập private trực tiếp	        Có	                        Không
        Có this	                            Có, nếu không phải static	Không
        Có thể nhận object làm tham số	    Có	                        Có

    - VD:
        class Motor
        {
        private:
            int speed = 100;

        public:
            int getSpeed()
            {
                return speed;
            }
        };

        void printSpeed(const Motor& m)
        {
            std::cout << m.getSpeed();
        }

        int main()
        {
            Motor m;
            printSpeed(m);
        }

        + Ở đây, getSpeed() là Member Function, còn printSpeed() là Non-member Function.
        + printSpeed() không thể truy cập trực tiếp m.speed vì speed là private. Nó phải thông qua hàm getSpeed() công khai.

    - Hoặc:
        class Motor
        {
        private:
            int speed = 100;

        public:
            int getSpeed()
            {
                return speed;
            }
        };

        int main()
        {
            Motor m;
            std::cout << m.getSpeed();
        }

    - VD TEST:
        #include <iostream>
        class Motor
        {
        public:
            int speed = 100;

            void showSpeed()
            {
                std::cout << speed;
            }
        };

        int main()
        {
            const Motor m;
            std::cout << m.speed;   // OK vì chỉ đọc dữ liệu
            m.showSpeed();          // lỗi vì C++ mặc định không biết hàm showSpeed() có làm thay đổi m hay không
        }

        * m là một đối tượng const, nghĩa là không được phép thay đổi trạng thái của nó thông qua m.    m.speed = 200;  // Lỗi
        * Hàm showSpeed() chưa có từ khóa const ở cuối. Khi một hàm thành viên không có const, 
        trình biên dịch mặc định xem hàm đó có khả năng thay đổi trạng thái của đối tượng. Vì vậy, C++ không cho phép gọi hàm này thông qua đối tượng const.
        * Cách sửa: Chỉ cần thêm const vào cuối hàm showSpeed():
            void showSpeed() const
            {
                std::cout << speed;
            }

    - VD TEST:
        #include <iostream>
        class Motor
        {
        public:
            int speed = 100;
            static int count;
            void setSpeed(int s)
            {
                speed = s;
            }

            int getSpeed() const
            {
                return speed;
            }

            static void showCount()
            {
                std::cout << count << " ";
            }
        };
        int Motor::count = 2;
        void printMotor(const Motor& m)     // m tham chiếu trực tiếp đến m1 hoặc m2, const có nghĩa là thông qua m không được sửa m1.
        {
            std::cout << m.getSpeed() << " ";
        }

        int main()
        {
            Motor m1, m2;
            m1.setSpeed(200);
            m2.setSpeed(300);

            printMotor(m1);
            printMotor(m2);
            Motor::showCount();
        }
        --> Output 200 300 2
        + Ở đây m là const reference, nên chỉ có thể gọi những member function const.
        + (const Motor m) → truyền bằng giá trị, CÓ copy, m là object riêng. Sau khi printMotor() kết thúc, bản copy m bị hủy.
            (Chi tiết level3)

    - VD TEST:
        int getSpeed() const
        {
            return speed;
        }
        void printMotor(const Motor& m)
        {
            std::cout << m.getSpeed() << " ";
        }
        * const sau dấu ): cam kết hàm không thay đổi trạng thái của đối tượng thông qua this.
        * const Motor& m: Không cho phép thay đổi đối tượng thông qua m
        * Nếu:
            int getSpeed() const
            {
                speed = 200;
                return speed;
            }
            --> báo lỗi

4. this Pointer
    - this là pointer trỏ đến object hiện tại.
    - Chỉ tồn tại trong non-static member function.
    - Truy cập member bằng this->.
    - Phân biệt this->speed và speed.
    - Giải quyết xung đột tên giữa parameter và member variable.
    - Trả về *this để hỗ trợ method chaining.
    - this có kiểu pointer tới class hiện tại; trong const member function, nó trỏ tới đối tượng const.
    --------------------------------------------------------------------
        class Motor
        {
        private:
            int speed;

        public:
            void setSpeed(int speed)
            {
                this->speed = speed;
            }
        };

    - this là gì?
        + this là một con trỏ đặc biệt có sẵn bên trong các hàm thành viên không phải static của class.
        + Nó trỏ đến đối tượng hiện tại đang gọi hàm thành viên đó.
        + VD:
            #include <iostream>
            class Motor
            {
            public:
                int speed = 100;
                void showSpeed()
                {
                    std::cout << this->speed;
                }
            };
            int main()
            {
                Motor m1;
                Motor m2;

                m1.showSpeed();
                std::cout << "\n";
                m2.showSpeed();
            }
            --> Output: 100
                        100         
            --> this là con trỏ kiểu Motor* trỏ đến đối tượng hiện tại
                * Khi gọi m1.showSpeed():
                    this trỏ đến đối tượng m1.
                    this->speed truy cập biến speed của m1, có giá trị 100.
                * Khi gọi m2.showSpeed():
                    this trỏ đến đối tượng m2.
                    this->speed truy cập biến speed của m2, cũng có giá trị 100.

    - this->speed và speed khác nhau thế nào?        
        class Motor
        {
        public:
            int speed = 100;
            void showSpeed()
            {
                std::cout << speed;
            }
        };        
        * std::cout << speed; hoàn toàn tương đương với std::cout << this->speed;
        * Khi không có biến cục bộ hoặc tham số nào che khuất tên speed, trình biên dịch có thể tự hiểu đó là biến thành viên của đối tượng hiện tại.
        * Vì thế, bạn không bắt buộc phải viết this-> mỗi khi truy cập biến thành viên. Tuy nhiên, khi có biến trùng tên, this-> trở nên rất hữu ích

    - Trường hợp quan trọng nhất: this giải quyết trùng tên biến
        #include <iostream>
        class Motor
        {
        public:
            int speed = 100;
            void setSpeed(int speed)
            {
                speed = speed;      // Cả hai vế đều tham chiếu đến tham số của hàm. Câu lệnh này không thay đổi biến thành viên.
            }
        };
        int main()
        {
            Motor m;
            m.setSpeed(300);
            std::cout << m.speed;
        }
        --> Kết quả là 100, vì tham số speed che khuất biến thành viên cùng tên.
        --> Sử dụng this để giải quyết:
                void setSpeed(int speed)
                {
                    this->speed = speed;
                }
                --> m.speed = 300.

    - this có kiểu dữ liệu gì?
        class Motor
        {
        public:
            int speed = 100;

            void showSpeed()
            {
                std::cout << this->speed;
            }
        };   
        --> Trong hàm thành viên không phải static của Motor, this có kiểu tương đương Motor*.

    - this chỉ tồn tại trong hàm thành viên không phải static        
        class Motor
        {
        public:
            int speed = 100;

            static void showSpeed()
            {
                std::cout << this->speed;
            }
        };
        --> báo lỗi
        --> showSpeed() là hàm static, không gắn với một đối tượng cụ thể nên không có this. Do đó, không thể truy cập speed theo cách trên.
    
    - Dùng return *this để trả về chính đối tượng hiện tại
        #include <iostream>
        class Motor
        {
        public:
            int speed = 100;
            Motor& setSpeed(int s)      // → trả về reference đến chính m, không tạo object copy. x và m cùng chỉ đến một object.
            {
                this->speed = s;
                return *this;
            }
        };
        int main()
        {
            Motor m;
            m.setSpeed(200).setSpeed(300);
            std::cout << m.speed;
        }
        --> Output: 300

        + m.setSpeed()
            → this nhận địa chỉ của object m
            → this trỏ đến m
            → this->speed là speed của m
        + Motor& setSpeed(int s) 
            → hàm trả về reference đến chính object hiện tại
            → không tạo object copy
        + return *this
            → *this là chính object mà this đang trỏ tới
        + Nếu viết: Motor setSpeed(int s)
            → hàm trả về một Motor object
            → với return *this, object được trả về là bản copy của object hiện tại
                    Motor setSpeed(int s)
                    {
                        this->speed = s;
                        return *this;
                    }
                    --> Output: 200
        + m.setSpeed(200).setSpeed(300);
        Vì kết quả của m.setSpeed(200) lại là một object Motor&, và object/reference đó có thể tiếp tục gọi hàm thành viên setSpeed().
        Đây chính là bản chất của method chaining.
        Nếu hàm thành viên là void viết như vậy sẽ lỗi
                    Motor setSpeed(int s)
                    {
                        this->speed = s;
                        return *this;   // trả về bản copy
                    }
                    m.speed = 100

                    m.setSpeed(200)
                        ↓
                    m.speed = 200
                        ↓
                    trả về COPY 1 (speed = 200)
                        ↓
                    COPY 1.setSpeed(300)
                        ↓
                    COPY 1.speed = 300
                        ↓
                    trả về COPY 2 (speed = 300)
                        ↓
                    COPY 2.setSpeed(500)
                        ↓
                    COPY 2.speed = 500
            --> cuối cùng std::cout << m.speed; vẫn in: 200


5. Constructor: special member function được tự động gọi khi object được tạo
    Constructor là hàm thành viên đặc biệt, được gọi khi object được khởi tạo. Constructor có cùng tên với class và không có kiểu trả về.
    Các loại Constructor:
        Default constructor: constructor không có tham số bắt buộc.
            class Motor {
            public:
                Motor() {
                    speed = 100;
                }
                int speed;
            };
            Motor m; // → gọi Motor().

        Parameterized constructor: constructor có tham số.
            class Motor {
            public:
                Motor(int s) {
                    speed = s;
                }

                int speed;
            };
            Motor m(200);
            → speed = 200.

        Constructor overloading: nhiều constructor với danh sách tham số khác nhau.
            + VD1:
            class Motor {
            public:
                Motor() : speed(100) {}
                Motor(int s) : speed(s) {}
                Motor(int s, int max) : speed(s), maxSpeed(max) {}

            private:
                int speed;
                int maxSpeed;
            };
            Motor m1;
            Motor m2(200);
            Motor m3(200, 500);

            + VD2: hai constructor này kết hợp lại thành constructor overloading.
            class Motor
            {
            private:
                int speed;
                const int id;

            public:
                Motor() : speed(0), id(1) {}                // Default constructor
                Motor(int s, int i) : speed(s), id(i) {}    // Parameterized constructor
            }


        Explicit constructor: ngăn một số phép chuyển đổi ngầm định thông qua constructor. 
            class Motor {
            public:
                Motor(int speed) : speed(speed) {}

            private:
                int speed;
            };
            Motor m = 200; // Nó ngầm hiểu: Motor m(200);

            Nếu thêm: explicit Motor(int speed) : speed(speed) {}
            thì: 
                Motor m = 200;   // ❌
                Motor m(200);    // ✅

        Delegating constructor: một constructor gọi constructor khác trong cùng class. (ít gặp)
            class Motor {
            public:
                Motor() : Motor(100) {}

                Motor(int s) : speed(s) {}

            private:
                int speed;
            };


        Initializer list là cách để khởi tạo giá trị ban đầu cho các member ngay khi object được tạo.
        + VD1:
            #include <iostream>
            class Motor
            {
            private:
                int speed;
                int id;

            public:
                Motor(int s, int i) : speed(s), id(i)
                {
                }

                void show()
                {
                    std::cout << "Speed = " << speed << "\n";
                    std::cout << "ID = " << id << "\n";
                }
            };
            int main()
            {
                Motor m(200, 5);
                m.show();
            }
            --> Output:
                        Speed = 200
                        ID = 5

        + VD2:
            + Với const member: bắt buộc phải dùng initializer list
                class Motor
                {
                private:
                    const int id;

                public:
                    Motor(int i) : id(i)
                    {
                    }
                };
                Motor m(10);

            + gán trong thân constructor:
                class Motor
                {
                private:
                    const int id;

                public:
                    Motor(int i)
                    {
                        id = i;       // ❌ LỖI
                    }
                };
                --> Vì id là const.

            + Thấy rõ hơn với member là một object
                class Engine
                {
                public:
                    Engine(int power)
                    {
                        std::cout << "Engine created\n";
                    }
                };

                class Motor
                {
                private:
                    Engine engine;

                public:
                    Motor(int power) : engine(power)
                    {
                        std::cout << "Motor created\n";
                    }
                };
                Motor m(100);
            
            + Ko dùng initializer list
                class Motor
                {
                private:
                    Engine engine;

                public:
                    Motor(int power)
                    {
                        engine = Engine(power);   // ❌
                    }
                };
                --> Vấn đề là engine phải được khởi tạo trước khi thân constructor chạy.
                    Nó phải tạo engine trước, nhưng Engine trong ví dụ này không có default constructor: Engine engine;




6. Constructor + destructor, object lifetime
    Constructor: object enters life → tạo object
    Destructor: object leaves life → hủy object
    constructor: được gọi khi object được tạo.
    destructor: được gọi khi object bị hủy.

        class Motor
        {
        private:
            int speed;
            const int id;

        public:
            Motor() : speed(0), id(1) {}

            Motor(int s, int i) : speed(s), id(i) {}
        };

        Destructor không nhận tham số và không có kiểu trả về.
            ~Motor(int x) {}   // ❌
            ~Motor() {}
            ~Motor(int x) {}   // ❌      
        Chỉ có:
            ~Motor()
            {
            }           
            

7. Object Lifetime (Vòng đời object)
    - Object bắt đầu và kết thúc lifetime.
        class Motor
        {
        public:
            Motor()
            {
                std::cout << "Constructor\n";
            }

            ~Motor()
            {
                std::cout << "Destructor\n";
            }
        };
        int main()
        {
            Motor m;

            std::cout << "Running\n";
        }
        --> Output:
                    Constructor
                    Running
                    Destructor     

    - Automatic storage duration: object cục bộ thông thường.
        void test()
        {
            Motor m1;

            {
                Motor m2;
            } // m2 bị hủy ở đây

        } // m1 bị hủy ở đây

    - Static storage duration: object toàn cục và biến cục bộ static.
        + Global object
            Motor g_motor;
            int main()
            {
            }
            --> g_motor có static storage duration.
            --> Nó được tạo trước khi main() chạy và tồn tại cho đến khi chương trình kết thúc.

        + Local static
            void test()
            {
                static Motor m;
            }
            test();
            test();
            test();        
            --> m không được tạo lại mỗi lần gọi test().
            --> Không có static → Constructor 3 lần, Destructor 3 lần.



    - Dynamic storage duration: object được tạo bằng new.
            int main()
            {
                Motor* p = new Motor();
                // sử dụng p
                delete p;
            }

    - Temporary object và temporary lifetime.

    - Thời điểm constructor và destructor được gọi.
        + Dynamic
            Motor* p = new Motor();     // → constructor chạy.
            delete p;                   // → destructor chạy.  

        + Static
            static Motor m;
            → constructor chạy khi static object được khởi tạo.
            → destructor chạy khi chương trình kết thúc.

    - Thứ tự khởi tạo và hủy object. Object tạo trước → hủy sau. Last In → First Out.
        int main()
        {
            Motor m1;
            Motor m2;
            Motor m3;
        }
        Khởi tạo: m1 --> m2 --> m3
        Hủy: m3 --> m2 --> m1

    - Lifetime của object thành viên.
        class Engine
        {
        public:
            Engine()
            {
                std::cout << "Engine constructor\n";
            }

            ~Engine()
            {
                std::cout << "Engine destructor\n";
            }
        };

        class Motor
        {
        private:
            Engine engine;

        public:
            Motor()
            {
                std::cout << "Motor constructor\n";
            }

            ~Motor()
            {
                std::cout << "Motor destructor\n";
            }
        };
        Motor m;
            Engine constructor
                ↓
            Motor constructor

            Motor destructor
                ↓
            Engine destructor

    - Lifetime của object cục bộ trong block.

    - Phân biệt lifetime với scope và storage duration.
        + Scope nói về "tên m dùng được ở đâu".
        + Lifetime nói về "object m sống bao lâu".
        + Storage duration nói về "storage của m có thời gian tồn tại theo cơ chế nào".


8. Const Member Function và Const Object
    - const member function: hàm thành viên không được phép thay đổi trạng thái của object thông qua this.
    - Cú pháp: int getSpeed() const;
    - const object: object không cho phép thay đổi trạng thái thông qua các thao tác thông thường.
    - const object chỉ gọi được các member function phù hợp với tính const.
    - Overloading member function theo const.
    - Từ khóa mutable cho phép một số member thay đổi ngay cả trong const member function.
    ------------------------------------------------------------------------------------------
        class Motor
        {
        private:
            int speed;

        public:
            int getSpeed() const
            {
                return speed;
            }
        };

    - const object
        class Motor
        {
        public:
            int speed = 100;
        };

        int main()
        {
            const Motor m;

            std::cout << m.speed;  // ✅
            m.speed = 200;         // ❌
        }

    - const member function
        int getSpeed() const        // Hàm này cam kết không thay đổi trạng thái của object thông qua this.
        {
            return speed;
        }
        const Motor m;
        m.getSpeed();  // ✅
        --> const object chỉ gọi được const member function

    - const reference                                                                   ???
    - Overloading member function theo const                                            ???
        + C++ cho phép có hai member function cùng tên, chỉ khác const.
            * VD1:
                class Motor
                {
                public:
                    void show()
                    {
                        std::cout << "non-const\n";
                    }

                    void show() const
                    {
                        std::cout << "const\n";
                    }
                };
                Motor m;
                const Motor cm;
                m.show();
                cm.show();
                --> Output: non-const   const

            * VD2:    - 
                class Motor
                {
                public:
                    int& speed()
                    {
                        return m_speed;
                    }

                    const int& speed() const
                    {
                        return m_speed;
                    }

                private:
                    int m_speed = 100;
                };
                ** Object thường:
                    Motor m;
                    m.speed() = 500; // được
                ** Nhưng const object:
                    const Motor m;
                    m.speed() = 500; // ❌
                ** C++ sẽ chọn:
                    const int& speed() const

                ** Nếu viết int speed() là trả về giá trị int, không phải reference đến m_speed. Phải viết:
                    std::cout << m.speed();
                    --> Nhưng muốn thay đổi m_speed, bạn cần một hàm setter riêng.


    - mutable — ngoại lệ
            class Motor
            {
            private:
                int speed = 100;

            public:
                void show() const
                {
                    speed = 200; // ❌
                }
            };    
            --> Không được vì show() là const.
            Nhưng nếu: mutable int count = 0;
            thì:
                class Motor
                {
                private:
                    int speed = 100;
                    mutable int count = 0;

                public:
                    void show() const
                    {
                        count++;  // ✅
                    }
                };
                --> count vẫn được phép thay đổi dù function là const.

    - mutable dùng để làm gì? Một ví dụ thực tế hơn là đếm số lần một hàm được gọi.
            class Motor
            {
            private:
                int speed = 100;
                mutable int callCount = 0;

            public:
                int getSpeed() const
                {
                    callCount++;
                    return speed;
                }
            };
            const Motor m;
            m.getSpeed();
            m.getSpeed();
            m.getSpeed();
            --> callCount = 3

    - Tổng hợp:
            class Motor
            {
            private:
                int speed = 100;
                mutable int count = 0;

            public:
                void setSpeed(int s)
                {
                    speed = s;
                }

                int getSpeed() const
                {
                    count++;
                    return speed;
                }
            };
            Motor m;
            m.setSpeed(200);   // ✅
            m.getSpeed();      // ✅

            const Motor cm;
            cm.setSpeed(200);  // ❌
            cm.getSpeed();     // ✅


9. Static Member và Static Member Function
    - Non-static data member → mỗi object có một bản riêng.

    - static data member: thuộc về class, được chia sẻ giữa các object.
        #include <iostream>
        using namespace std;

        class Motor {
        public:
            int speed = 100;
            static int count;
        };
        int Motor::count = 0;
        int main() {
            Motor m1;
            Motor m2;

            // Truy cập bằng tên class
            Motor::count = 10;

            // Truy cập bằng object
            m1.count = 20;

            cout << Motor::count << endl;
            cout << m2.count << endl;
        }
        --> hai object có hai speed riêng, chỉ có một count, cả m1 và m2 đều dùng chung.

    - static member function: hàm thuộc về class, không cần object để gọi.
        #include <iostream>
        using namespace std;
        class Motor {
        public:
            static int count;

            static void showCount() {
                cout << count << endl;
            }
        };
        int Motor::count = 5;
        int main() {
            Motor m1;

            // Cách 1: Gọi bằng tên class — nên dùng
            Motor::showCount();

            // Cách 2: Gọi bằng object — vẫn hợp lệ
            m1.showCount();
        }
        --> Static function có thể gọi bằng object, nhưng nên dùng ClassName::function() để thể hiện rõ đây là hàm thuộc về class, không phụ thuộc vào
        object.



        + Static member function không có this.                             
            * Member function bình thường có this.
                void showSpeed()
                {
                    std::cout << this->speed;
                }

            * Static member function không có this.
                static void showCount()
                {
                    std::cout << count;
                }

        + Static function có thể truy cập static member
            class Motor
            {
            public:
                static int count;

                static void showCount()
                {
                    std::cout << count;  // ✅
                }
            };
            int Motor::count = 5;

        + Static member function không thể truy cập trực tiếp non-static member nếu không có object.            
            class Motor
            {
            public:
                int speed = 100;

                static void showSpeed()
                {
                    std::cout << speed;  // ❌
                }
            };
            + Mỗi object có speed riêng:
                m1.speed
                m2.speed
                m3.speed
            + Nhưng showSpeed() không có this, nên nó không biết phải lấy:
                m1.speed?
                m2.speed?
                m3.speed?

        + Nhưng static function vẫn có thể truy cập non-static member nếu có object                                 
            class Motor
            {
            public:
                int speed = 100;
                static void showSpeed(const Motor& m)
                {
                    std::cout << m.speed;
                }
            };
            Motor m1;
            m1.speed = 200;
            Motor::showSpeed(m1);
            --> Output: 200



    - Static local variable trong member function.
        class Motor
        {
        public:
            void test()
            {
                static int count = 0;
                count++;
                std::cout << count << "\n";
            }
        };
        Motor m1;
        Motor m2;

        m1.test();
        m1.test();
        m2.test();
        m2.test();

        --> Output: 1 2 3 4

    - VD tổng hợp:
        #include <iostream>
        class Motor
        {
        public:
            int speed = 100;            // non-static member
            static int count;           // static data member

            void showSpeed()            // non-static member function
            {
                std::cout << speed << "\n";
            }

            static void showCount()     // static member function
            {
                std::cout << count << "\n";
            }

            void test()
            {
                static int calls = 0;   // static local variable
                calls++;

                std::cout << calls << "\n";
            }
        };

        int Motor::count = 2;

        int main()
        {
            Motor m1;
            Motor m2;

            m1.speed = 200;
            m2.speed = 300;

            m1.showSpeed();       // 200
            m2.showSpeed();       // 300

            Motor::showCount();   // 2

            m1.test();            // 1
            m2.test();            // 2
            m1.test();            // 3
        }

                                                      Member thường                                 Static Member
        Static member function truy cập               Không trực tiếp — phải có object cụ thể       Được
        Non-static member function truy cập           Được                                          Được


    - inline
        + Tối ưu lời gọi hàm (ý nghĩa truyền thống)
            * Compiler có thể thay lời gọi square(5) bằng phép tính tương đương 5 * 5, giúp tránh một phần chi phí gọi hàm.
            * Tuy nhiên, compiler hiện đại tự tối ưu rất tốt, kể cả khi bạn không viết inline. Vì vậy, không nên nghĩ rằng cứ thêm inline là chương trình chạy nhanh hơn.

        + Định nghĩa hàm trong file header (điểm đáng nhớ hơn)
            * Nếu Motor.h được include trong nhiều file .cpp, định nghĩa hàm inline có thể xuất hiện trong nhiều translation unit mà vẫn hợp lệ theo quy tắc ODR của C++.
            * Đây là một lý do quan trọng khiến inline vẫn hữu ích trong C++ hiện đại.
            * Lưu ý: hàm được định nghĩa ngay bên trong class cũng tự động được xem là inline theo quy tắc ngôn ngữ, nên bạn thường không cần ghi từ khóa này.

        + VD1:
            #include <iostream>
            using namespace std;

            inline int square(int x) {
                return x * x;
            }

            int main() {
                cout << square(5);
            }
            --> Output: 25

        + VD2:
            class Motor
            {
            public:
                inline static int count = 0;

                Motor()
                {
                    ++count;
                }

                static int getCount()
                {
                    return count;
                }
            };
            int main()
            {
                Motor a;
                Motor b;

                int n = Motor::getCount(); // 2
            }

10. Pointer to Object
    - Khai báo pointer tới object: Motor* p.
    - Gán địa chỉ object cho pointer.
    - Truy cập member bằng ->.
    - Truy cập member bằng (*p).member.
    - Pointer tới object const.
    - Pointer có thể là nullptr.
    - Phân biệt pointer trỏ tới object và object được tạo bằng new.
    -----------------------------------------------------------------------
    - VD:
        class Motor {
        public:
            int speed = 100;

            void showSpeed() {
                std::cout << speed << '\n';
            }
        };
        Motor m;
        Motor* p = &m;
        std::cout << p->speed << '\n';

        p->speed = 250;
        p->showSpeed();
        --> Output: 100 250
        
        (*p).speed = 300;
        p->showSpeed();
        --> Output: 300

    - Pointer to const object
        Motor m;
        const Motor* p = &m;
        std::cout << p->speed << '\n';  // OK
        // p->speed = 200;              // Lỗi

    - Có hai điều cần phân biệt:
        const Motor* p: không được sửa object qua p, nhưng được đổi địa chỉ của p.
        Motor* const p = &m1: không được đổi địa chỉ của p, nhưng vẫn có thể sửa m1 thông qua p.

    - Pointer có thể là nullptr
        * Pointer có thể chưa trỏ đến object nào. Khi đó, bạn có thể gán nullptr.
            Motor* p = nullptr;

        * Kiểm tra:
            Motor* p = nullptr;
            if (p != nullptr) {
                p->showSpeed();
            }
            --> Output: 100

    - Object tạo bằng new
        Motor* p = new Motor();
        p->speed = 500;
        delete p;
        p = nullptr;
            
            * new Motor() tạo một object có dynamic storage duration.
            * p lưu địa chỉ object đó.
            * delete p kết thúc lifetime của object và giải phóng vùng nhớ tương ứng.
            * Sau delete, gán p = nullptr giúp tránh vô tình tiếp tục sử dụng pointer cũ.

11. Reference to Object
    - Khai báo reference tới object: Motor& ref = m.
    - Truy cập member bằng ..
    - Truyền object bằng reference.
    - Truyền object bằng const reference.
    - Phân biệt reference và pointer tới object.
    -----------------------------------------------------------
    - VD:
        Motor m;
        Motor& ref = m;
        ref.speed = 250;
        std::cout << m.speed << '\n';
        --> Output: 250

    - So sánh:
        Motor* p = &m;
        p->speed = 400;  // Pointer dùng ->

        Motor& ref = m;
        ref.speed = 400; // Reference dùng .

    - Truyền object bằng reference:
        * Reference thường được dùng để truyền object vào hàm mà không phải tạo bản sao.

            void setSpeed(Motor& m, int s) {
                m.speed = s;
            }
            int main() {
                Motor motor;
                setSpeed(motor, 500);
                std::cout << motor.speed;
            }
            --> Output: 500
            --> Hàm setSpeed() nhận Motor& m, tức là tham chiếu đến object được truyền vào. 
                Vì vậy, khi sửa m.speed, object motor ở bên ngoài cũng thay đổi.

    - Truyền object bằng const reference (chỉ đọc)
        void showMotor(const Motor& m) {
            std::cout << m.speed << '\n';
        }
        int main() {
            Motor motor;
            showMotor(motor);
        }
        --> Output: 100

    - Reference có thể đổi sang object khác không?
        Motor m1;
        Motor m2;
        Motor& ref = m1;
        ref.speed = 200;
        ref = m2;
        --> thực chất là gán giá trị của object m2 cho object m1. Reference ref vẫn tham chiếu đến m1.




    Đặc điểm	                                        Pointer Motor*	                                Reference Motor&
    Khai báo	                                        Motor* p = &m;	                                Motor& ref = m;
    Truy cập member	                                    p->speed	                                    ref.speed
    Có thể không trỏ/tham chiếu đến object hợp lệ?	    Có thể là nullptr	                            Reference thông thường phải được khởi tạo với object hợp lệ
    Đổi sang object khác	                            Có, bằng cách gán địa chỉ khác	                Không thể gắn lại
    Có thể sửa object gốc?	                            Có, nếu kiểu và quyền truy cập cho phép	        Có, nếu không phải const reference
    Có thể truyền vào hàm	                            Có	                                            Có
    Có thể sở hữu object?	                            Bản thân pointer không thể hiện quyền sở hữu	Bản thân reference không thể hiện quyền sở hữu



12. Function nhận và trả về Object
        #include <iostream>
        using namespace std;

        class Motor {
        public:
            int speed;
            Motor(int s) : speed(s) {}
        };
        Motor m(100);
        
    - Truyền object bằng value — Pass by Value
        void changeSpeed(Motor m) {
            m.speed = 200;
        }
        int main() {
            Motor motor(100);
            changeSpeed(motor);
            cout << motor.speed;
        }    
        --> Output: 100


    - Truyền object bằng reference — Pass by Reference
        void changeSpeed(Motor& m) {
            m.speed = 200;
        }
        int main() {
            Motor motor(100);
            changeSpeed(motor);
            cout << motor.speed;
        }
        --> Output: 200
        --> m tham chiếu đến chính object motor, không tạo một bản sao độc lập.

    - Truyền object bằng const reference.
        void showMotor(const Motor& m) {
            cout << m.speed << '\n';
            // m.speed = 200;  // Lỗi: không được sửa qua m
        }
        int main() {
            Motor motor(100);
            showMotor(motor);
            cout << motor.speed;
        }
        --> Output: 100
                    100
        + Trong C++ thực tế, const Motor& rất phổ biến khi truyền object tương đối lớn mà hàm chỉ cần đọc dữ liệu.            

    - Trả về object bằng value.
        Motor createMotor() {
            Motor m(300);
            return m;
        }
        int main() {
            Motor result = createMotor();
            cout << result.speed;
        }
        --> Output: 300
        + Giải thích:
            * Trong createMotor(), một object m được tạo với speed = 300.
            * return m; trả về giá trị object.
            * result nhận kết quả và tồn tại độc lập sau khi hàm kết thúc.

        Motor createMotor() {
            return Motor(300);
        }

    - Trả về reference tới object: Motor& getMotor();
        Motor& getMotor(Motor& m) 
        {
            return m;
        }

        int main() {
            Motor motor(100);
            Motor& ref = getMotor(motor);
            ref.speed = 500;
            cout << motor.speed;
        }
        --> Output: 500

    - Những rủi ro khi trả về reference tới biến cục bộ.
        + VD1:
            Motor& createMotor() {
                Motor m(300);
                return m;  // NGUY HIỂM
            }

        + VD2:
            Motor& createMotor() {
                Motor m(300);
                return m;
            }
            int main() {
                Motor& ref = createMotor();
                cout << ref.speed;  // Undefined behavior
            }

    - Phân biệt object được truyền vào và bản sao của object.
        #include <iostream>
        using namespace std;

        class Motor 
        {
        public:
            int speed;
            Motor(int s) : speed(s) {}
        };

        void f1(Motor m) 
        {
            m.speed = 200;
        }

        void f2(Motor& m) 
        {
            m.speed = 300;
        }

        void f3(const Motor& m) 
        {
            cout << m.speed << '\n';
        }

        int main() 
        {
            Motor motor(100);

            f1(motor);
            cout << motor.speed << '\n';

            f2(motor);
            cout << motor.speed << '\n';

            f3(motor);
        }
        --> Output: 100      
                    300
                    300

