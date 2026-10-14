```cpp                    
                    Level 4 – Inheritance và Polymorphism
Nhóm A — Nền tảng Inheritance
1. Inheritance
2. Phân biệt các kiểu kế thừa
3. Constructor in Inheritance
4. Destructor in Inheritance

Nhóm B — Tra cứu và chọn hàm
5. Name Hiding
6. Function Overriding và Function Overloading

Nhóm C — Virtual Function và Polymorphism
7. Virtual Function (Hàm ảo)
8. override
9. Virtual Destructor
10. Polymorphism (Tính đa hình)
11. Object Slicing trong Inheritance

Nhóm D — Abstract Class và Interface
12. Pure Virtual Function
13. Abstract Class (Lớp trừu tượng)
14. Interface Concept (Khái niệm giao diện)

Nhóm E — Các quy tắc nâng cao
15. final
16. Covariant Return Type
17. Virtual Dispatch trong Constructor và Destructor

Nhóm F — Multiple Inheritance
18. Multiple Inheritance (Đa kế thừa)
19. Virtual Inheritance (Kế thừa ảo)
--------------------------------------------------------------------------
Nhóm A — Nền tảng Inheritance
1. Inheritance: class con (Derived class) kế thừa các thành viên của class cha (Base class)
    - Cú pháp: 
        class Dog : public Animal

2. Phân biệt các kiểu kế thừa
    - public inheritance
    - protected inheritance
    - private inheritance   

3. Constructor in Inheritance
    - Constructor của class cha được gọi trước constructor của class con.
        class Animal
        {
        public:
            Animal(int age)
            {
                std::cout << "Animal ";
            }
        };

        class Dog : public Animal
        {
        public:
            Dog() : Animal(5)
            {
                std::cout << "Dog ";
            }
        };

        int main()
        {
            Dog d;
        }    
        --> Kết quả: Animal Dog

4. Destructor in Inheritance
    - Destructor của class con chạy trước destructor của class cha.
        class Animal
        {
        public:
            ~Animal()
            {
                std::cout << "Animal ";
            }
        };

        class Dog : public Animal
        {
        public:
            ~Dog()
            {
                std::cout << "Dog ";
            }
        };

        int main()
        {
            Dog d;
        }
        --> Kết quả: Dog Animal

Nhóm B — Tra cứu và chọn hàm
5. Name Hiding
    - Hàm trong class con che khuất hàm cùng tên trong class cha.
    - Name Hiding xảy ra ngay cả khi danh sách tham số khác nhau.
    - Phân biệt Name Hiding và Function Overriding.
    - Gọi tường minh hàm của class cha bằng toán tử ::.
    
    5.1. Name Hiding là gì?
        class Animal
        {
        public:
            void sound()
            {
                std::cout << "Animal";
            }

            void sound(int n)
            {
                std::cout << n;
            }
        };

        class Dog : public Animal
        {
        public:
            void sound()
            {
                std::cout << "Dog";
            }
        };

        int main()
        {
            Dog d;
            d.sound();          // Dog
            d.Animal::sound();  // Animal
            // d.sound(10);     // Lỗi: sound(int) bị che khuất
        }   
        --> Output: Dog Animal

    5.2. Cách khắc phục Name Hiding                                                                                     ??????
    - Cách 1: Gọi tường minh hàm của class cha bằng ::
        d.Animal::sound(10);

    - Cách 2: Dùng using để đưa các overload của class cha vào phạm vi tìm kiếm của class con
        class Dog : public Animal
        {
        public:
            using Animal::sound;

            void sound()
            {
                std::cout << "Dog ";
            }
        };    
        Dog d;

        d.sound();     // Dog
        d.sound(10);   // 10

        using Animal::sound; cho phép các overload của Animal::sound tham gia vào việc tìm kiếm tên sound trong Dog. Nó không tạo ra một bản sao các hàm đó.

6. Function Overriding và Function Overloading
    6.1. Function Overloading — Nạp chồng hàm
        class Animal
        {
        public:
            void sound()
            {
                std::cout << "Animal\n";
            }

            void sound(int n)
            {
                std::cout << "Sound: " << n << '\n';
            }

            void sound(double volume)
            {
                std::cout << "Volume: " << volume << '\n';
            }
        };
        Animal a;

        a.sound();
        a.sound(10);
        a.sound(2.5);
        --> Output: Animal
                    Sound: 10
                    Volume: 2.5

    6.2. Function Overriding — Ghi đè hàm
        - Overriding xảy ra khi class con cung cấp cách triển khai mới cho một hàm virtual đã được khai báo trong class cha, với chữ ký hàm tương ứng.

            #include <iostream>
            class Animal
            {
            public:
                virtual void sound()
                {
                    std::cout << "Animal\n";
                }
            };

            class Dog : public Animal
            {
            public:
                void sound() override
                {
                    std::cout << "Dog\n";
                }
            };

            int main()
            {
                Dog d;
                d.sound();
            }
            --> Output: Dog

Nhóm C — Virtual Function và Polymorphism
7. Virtual Function (Hàm ảo)
    virtual: Nó cho phép runtime polymorphism: khi gọi function thông qua pointer/reference của class cha, C++ có thể chọn function của object thực tế.
    override: chủ yếu giúp compiler kiểm tra rằng function của class con thực sự đang override function virtual của class cha.

    - VD1:
        #include <iostream>
        using namespace std;

        class Animal
        {
        public:
            void sound()
            {
                cout << "Animal";
            }
        };

        class Dog : public Animal
        {
        public:
            void sound()
            {
                cout << "Dog";
            }
        };

        int main()
        {
            Dog d;
            d.sound();
        }
        --> Output: Dog
        - Dog::sound() che/hide Animal::sound() trong ngữ cảnh gọi qua Dog
        - Nếu muốn gọi hàm của cha thì phải chỉ rõ: d.Animal::sound();
        - Còn: d.sound(); → Dog

    - VD2:
        class Animal
        {
        public:
            virtual void sound()
            {
                cout << "Animal";
            }
        };

        class Dog : public Animal
        {
        public:
            void sound() override
            {
                cout << "Dog";
            }
        };

        int main()
        {
            Animal* p = new Dog();

            p->sound();
        }
        --> Output: Dog
        - Nếu sound() không có virtual, C++ sẽ dựa vào kiểu của pointer p: → Animal.
        - Nhưng ở đây: virtual void sound() --> "Đừng chỉ nhìn kiểu của pointer. Hãy xem object thực sự mà pointer đang trỏ tới." --> Dog
        - Nếu sau này gọi sound() thông qua pointer/reference của class cha, hãy chọn function dựa trên object thực tế đang được trỏ tới.


    - Tóm lại:
        Có virtual
        → dựa vào OBJECT THỰC TẾ mà pointer đang trỏ tới
        → runtime quyết định function nào chạy

        Không có virtual
        → dựa vào KIỂU CỦA POINTER
        → compiler quyết định function nào chạy

    🧠 Vậy virtual hoạt động với cả pointer và reference
    Animal* p = &d;
    p->sound();       // Dog

    Animal& ref = d;
    ref.sound();      // Dog

8. override: đặt ở hàm con để compiler kiểm tra việc ghi đè.
    class Animal
    {
    public:
        virtual void sound() const {}
    };

    class Dog : public Animal
    {
    public:
        void sound() const override {}      // bỏ const Compiler sẽ báo lỗi vì Dog::sound() không ghi đè Animal::sound() const.
    };

9. Virtual Destructor
    - CÓ virtual
        class Animal
        {
        public:
            virtual ~Animal()
            {
                cout << "A ";
            }
        };

        class Dog : public Animal
        {
        public:
            ~Dog()
            {
                cout << "D ";
            }
        };

        int main()
        {
            Animal* p = new Dog();

            delete p;
        }
        Output: D A

        + Khi xóa đối tượng Dog qua con trỏ Animal*, destructor ảo bảo đảm destructor của Dog chạy trước, sau đó destructor của Animal chạy.

    - KHÔNG có virtual
        class Animal
        {
        public:
            ~Animal()
            {
                cout << "A ";
            }
        };
        class Dog : public Animal
        {
        public:
            ~Dog()
            {
                cout << "D ";
            }
        };

        int main()
        {
            Animal* p = new Dog();

            delete p;
        }
        + Lúc này destructor không virtual, nên delete p không dùng cơ chế dynamic dispatch để chọn destructor của Dog.
        → hành vi khi delete object con qua pointer cha là undefined behavior.
        --> không được coi A là output đảm bảo.

    - virtual KHÔNG dùng cho constructor
        Constructor tạo object → phải biết chính xác đang tạo loại object nào → không cần và không thể là virtual.
        Virtual dùng cho hành vi của object sau khi object tồn tại → function/destructor có thể dùng virtual.

10. Polymorphism (Tính đa hình)
    10.1. Bản chất của polymorphism    
    - Polymorphism = dùng interface/kiểu của class cha để làm việc với object class con, và nhờ virtual, gọi đúng function của class con.
    - Con trỏ class cha có thể trỏ tới object class con. Nhưng con trỏ class con không thể tùy tiện trỏ tới object class cha.
    - VD1:
        class Animal
        {
        public:
            virtual void sound()
            {
                std::cout << "Animal ";
            }
        };

        class Dog : public Animal
        {
        public:
            void sound() override
            {
                std::cout << "Dog ";
            }
        };

        int main()
        {
            Dog d;
            Animal* p = &d;

            p->sound();  // Dog
        }

    - VD2:
        class Animal
        {
        public:
            virtual void sound() = 0;
            virtual ~Animal() = default;
        };

        class Dog : public Animal
        {
        public:
            void sound() override
            {
                std::cout << "Dog ";
            }
        };

        class Cat : public Animal
        {
        public:
            void sound() override
            {
                std::cout << "Cat ";
            }
        };

        int main()
        {
            Dog d;
            Cat c;

            Animal* animals[] = {&d, &c};

            for (Animal* p : animals)
            {
                p->sound();
            }
        }
        --> Dog Cat

        + Giải thích:
            * Animal* có thể trỏ đến đối tượng Dog hoặc Cat vì cả hai đều kế thừa công khai từ Animal.
            * Mảng animals chứa các con trỏ kiểu Animal*.
            * Khi gọi p->sound(), virtual giúp chọn đúng hàm của đối tượng thực tế.
            * Vì vậy, cùng một câu lệnh p->sound() có thể gọi Dog::sound() hoặc Cat::sound().
            Đây chính là runtime polymorphism.

    10.2. Vì sao con trỏ cha trỏ được đến object con?
        Dog d;

        Animal* p = &d;  // Hợp lệ
        Animal& r = d;   // Hợp lệ

    10.3. Một lợi ích rất lớn: viết code không phụ thuộc class cụ thể
        void startMotor(Motor& motor)
        {
            motor.start();
        }   
        
        DCMotor dc;
        ServoMotor servo;

        startMotor(dc);
        startMotor(servo);

        - Cả hai đều dùng được cùng một hàm startMotor(). Hàm này không cần biết bên trong motor sử dụng driver, PWM hay giao thức điều khiển nào.
        - Đây là một trong những lợi ích chính của lập trình hướng đối tượng: code phụ thuộc vào giao diện chung thay vì phụ thuộc vào từng class cụ thể.

11. Object Slicing trong Inheritance
    Object Slicing không phải là một kỹ thuật có lợi ích riêng. Nó là một hệ quả khi bạn copy object con vào object cha bằng value.

    class Animal
    {
    public:
        virtual void sound()
        {
            std::cout << "Animal ";
        }
    };

    class Dog : public Animal
    {
    public:
        int legs = 4;

        void sound() override
        {
            std::cout << "Dog ";
        }
    };

    int main()
    {
        Dog d;

        Animal a = d;  // Object Slicing
        a.sound();

        Animal& ref = d;  // Không slicing
        ref.sound();
    }
    --> Output: Animal Dog

Nhóm D — Abstract Class và Interface
12. Pure Virtual Function
    #include <iostream>
    using namespace std;

    class Animal
    {
    public:
        virtual void sound() = 0;
    };

    class Dog : public Animal
    {
    public:
        void sound() override
        {
            cout << "Dog";
        }
    };

    int main()
    {
        Dog d;
        d.sound();
    }
    --> Output: Dog

    - = 0 biến sound() thành pure virtual function.
    - Animal nói: “Animal có một hàm sound(), nhưng tôi không cung cấp cách thực hiện. Class con phải tự viết.”
    - Vì Animal có pure virtual function nên Animal trở thành abstract class.
    - Do đó:
            Animal a;        // ❌ không được
            Animal* p;       // ✅ được
            Animal& r = d;   // ✅ được
    - abstract class = class dùng làm khuôn/interface, không thể tạo object trực tiếp.
    - Dù thêm constructor thì Animal vẫn không tạo được object, vì vấn đề không nằm ở constructor.
    - Nhưng constructor của Animal vẫn có thể chạy khi tạo object của class con.

13. Abstract Class (Lớp trừu tượng)

    class Animal
    {
    public:
        virtual void sound() = 0;

        void eat()
        {
            std::cout << "Eat";
        }

        virtual ~Animal() = default;
    };

    class Dog : public Animal
    {
    public:
        void sound() override
        {
            std::cout << "Dog";
        }
    };
    --> Animal vừa có hàm thuần ảo sound(), vừa có hàm thông thường eat(). Đây là một abstract class.

14. Interface Concept (Khái niệm giao diện)
    - C++ không có từ khóa interface riêng như một số ngôn ngữ khác.
    - Dùng abstract class để xây dựng interface.
    - Interface thường chứa các pure virtual function.
    - Sử dụng virtual destructor để hỗ trợ hủy object qua pointer interface.
    - Derived class triển khai các hàm được quy định bởi interface.
    - Phân biệt interface thuần túy và abstract class có sẵn phần triển khai.    

    C++ không có keyword interface như một số ngôn ngữ khác. Thường dùng abstract class + pure virtual function để tạo một interface.
        class Motor
        {
        public:
            virtual void start() = 0;
            virtual void stop() = 0;

            virtual ~Motor() = default;
        };
    Hiểu đơn giản: Motor đưa ra hợp đồng: motor nào kế thừa nó thì bắt buộc phải có start() và stop().
    Abstract class = khái niệm của C++, Có thể chứa cả function đã có code:
        class Animal
        {
        public:
            void eat()
            {
                cout << "Eat";
            }
            virtual void sound() = 0;
        };

Nhóm E — Các quy tắc nâng cao
15. final
    - final trên virtual function: ngăn class con tiếp tục override hàm đó.
    - final trên class: ngăn class khác kế thừa class đó.
    - Phân biệt final và override.
    - Kết hợp override và final.

    15.1. final trên virtual function: Ngăn class con tiếp tục override hàm đó.
    - VD:
        #include <iostream>
        class Animal
        {
        public:
            virtual void sound()
            {
                std::cout << "Animal ";
            }
        };

        class Dog : public Animal
        {
        public:
            void sound() override final
            {
                std::cout << "Dog ";
            }
        };

        class Puppy : public Dog
        {
        public:
            void run() {}                   // Hợp lệ nếu khai báo hàm này
            // void sound() override {}     // Lỗi
        };

        + Ở đây:
            * virtual trong Animal cho phép override.
            * override xác nhận Dog::sound() ghi đè hàm virtual của cha.
            * final ngăn các class dẫn xuất tiếp tục override Dog::sound().
        + Lưu ý: final trên hàm không ngăn class con kế thừa class cha. Nó chỉ ngăn override chính hàm đó.

    15.2. final trên class: Ngăn class khác kế thừa class đó.
    - VD:
        class Animal
        {
        };

        class Dog final : public Animal
        {
        };

        // class Puppy : public Dog {}; // Lỗi

    - Tuy nhiên, cần phân biệt hai trường hợp:
        +
            class Dog final : public Animal
            {
            public:
                void sound() override {}
            };
            --> Dog final ngăn kế thừa toàn bộ class.

        +
            class Dog : public Animal
            {
            public:
                void sound() override final {}
            };
            --> Ở đây, chỉ sound() bị khóa override tiếp; Dog vẫn có thể được kế thừa.


        + final trên class → ngăn class khác kế thừa class đó.
        + final trên virtual function → ngăn class con tiếp tục override function đó.

16. Covariant Return Type
    - Khái niệm Covariant Return Type.
    - Cho phép hàm override trả về pointer hoặc reference tới kiểu dẫn xuất của kiểu trả về ở hàm cha.
    - Điều kiện áp dụng.
    - Không áp dụng tương tự cho kiểu trả về theo value.
    - Ứng dụng trong các hàm đa hình trả về object cùng hệ thống kế thừa.

    16.1. Khái niệm
        - Covariant Return Type cho phép hàm override trong class con trả về pointer hoặc reference tới kiểu dẫn xuất của kiểu trả về tương ứng ở class cha.
        - VD:
            class Animal
            {
            public:
                virtual Animal* clone()
                {
                    return this;
                }

                virtual ~Animal() = default;
            };

            class Dog : public Animal
            {
            public:
                Dog* clone() override
                {
                    return this;
                }
            };
            --> Dog* là kiểu dẫn xuất của Animal*, vì vậy kiểu trả về trong hàm override trên là hợp lệ.
    
    16.2. Điều kiện áp dụng
    - Các điều kiện quan trọng:
        1. Hàm ở class cha phải là virtual.
        2. Hàm ở class con phải override hàm đó.
        3. Kiểu trả về phải là pointer hoặc reference tới class; kiểu dẫn xuất phải có quan hệ kế thừa phù hợp với kiểu trả về ở cha.
        4. Các điều kiện về truy cập và tính đầy đủ của kiểu cũng phải được thỏa mãn theo quy tắc C++.

    - VD:
        virtual Animal* clone();
        Dog* clone() override;
            hoặc:
        virtual Animal& getAnimal();
        Dog& getAnimal() override;

    16.3. Không áp dụng tương tự cho kiểu trả về theo value
        class Animal
        {
        public:
            virtual Animal clone()
            {
                return Animal{};
            }

            virtual ~Animal() = default;
        };

        class Dog : public Animal
        {
        public:
            // Dog clone() override { return Dog{}; } // Lỗi
        };


    16.4. Ví dụ sử dụng
        #include <iostream>
        class Animal
        {
        public:
            virtual Animal* getSelf()
            {
                std::cout << "Animal ";
                return this;
            }

            virtual ~Animal() = default;
        };

        class Dog : public Animal
        {
        public:
            Dog* getSelf() override
            {
                std::cout << "Dog ";
                return this;
            }
        };

        int main()
        {
            Dog d;

            Animal* p = &d;

            p->getSelf();
            d.getSelf();
        }
        --> Output: Dog Dog
    Tóm lại, có hai vấn đề riêng biệt:
    - virtual quyết định phiên bản hàm nào chạy dựa trên đối tượng thực tế.
    - Covariant Return Type cho phép hàm override trả về Dog* thay vì Animal*.


17. Virtual Dispatch trong Constructor và Destructor
    - Thứ tự gọi constructor và destructor trong kế thừa.
    - Quy tắc gọi virtual function bên trong constructor.
    - Quy tắc gọi virtual function bên trong destructor.
    - Trong constructor của base class, virtual dispatch không gọi override ở derived class chưa được khởi tạo.
    - Trong destructor của base class, virtual dispatch không gọi override ở derived class đã bị hủy.
    - Rủi ro khi gọi virtual function trong constructor hoặc destructor.

    17.1. Virtual function trong constructor
        class Animal
        {
        public:
            Animal()
            {
                sound();
            }

            virtual void sound()
            {
                std::cout << "Animal ";
            }

            virtual ~Animal() = default;
        };

        class Dog : public Animal
        {
        public:
            void sound() override
            {
                std::cout << "Dog ";
            }
        };

        int main()
        {
            Dog d;
        }
        --> Kết quả: Animal
        --> Khi constructor của Animal đang chạy, phần Dog chưa được khởi tạo hoàn tất. Vì thế, lời gọi virtual function trong constructor này gọi Animal::sound().


    17.2. Virtual function trong destructor

        #include <iostream>
        class Animal
        {
        public:
            virtual void sound()
            {
                std::cout << "Animal ";
            }

            virtual ~Animal()
            {
                sound();
            }
        };

        class Dog : public Animal
        {
        public:
            void sound() override
            {
                std::cout << "Dog ";
            }

            ~Dog() override
            {
                sound();
            }
        };

        int main()
        {
            Dog d;
        }
        --> Output: Dog Animal

    17.3. Vì sao gọi virtual function trong constructor/destructor có thể nguy hiểm?
    - Ví dụ 1: Constructor gọi virtual function để khởi tạo dữ liệu
        + Giả sử Animal gọi initialize() trong constructor, và ta mong Dog tự khởi tạo dữ liệu của nó.

            #include <iostream>
            class Animal
            {
            public:
                Animal()
                {
                    initialize();
                }

                virtual void initialize()
                {
                    std::cout << "Animal initialize\n";
                }

                virtual ~Animal() = default;
            };

            class Dog : public Animal
            {
            public:
                int age;

                Dog() : age(10)
                {
                }

                void initialize() override
                {
                    std::cout << "Dog initialize, age = "
                            << age << '\n';
                }
            };

            int main()
            {
                Dog d;
            }
            --> Output: Animal initialize 

        + Khi tạo Dog d, thứ tự thực hiện là:
            1. Constructor của Animal chạy trước.
            2. Animal() gọi initialize().
            3. Trong giai đoạn này, Dog chưa được khởi tạo hoàn tất, nên lời gọi virtual chỉ chạy Animal::initialize().
            4. Sau đó, các thành viên của Dog được khởi tạo và constructor Dog chạy.

        + Nguy hiểm ở đây: 
            Nếu bạn nghĩ Dog::initialize() đã chạy để chuẩn bị dữ liệu cho Dog, nhưng thực tế nó không chạy, chương trình có thể hoạt động sai hoặc dữ liệu chưa được thiết lập như mong đợi.

            Cách tốt hơn là khởi tạo dữ liệu trong constructor tương ứng của mỗi class, thay vì trông chờ virtual function trong constructor của class cha.

    - Ví dụ 2: Destructor gọi virtual function sau khi tài nguyên của class con đã bị giải phóng
        + Ví dụ này mô phỏng một class Dog quản lý vùng nhớ động.
            #include <iostream>
            class Animal
            {
            public:
                virtual void cleanup()
                {
                    std::cout << "Animal cleanup\n";
                }

                virtual ~Animal()
                {
                    std::cout << "Animal destructor\n";
                    cleanup();
                }
            };

            class Dog : public Animal
            {
            public:
                int* data;

                Dog() : data(new int[3]{10, 20, 30})
                {
                }

                void cleanup() override
                {
                    std::cout << "Dog cleanup, data[0] = "
                            << data[0] << '\n';
                }

                ~Dog() override
                {
                    std::cout << "Dog destructor\n";

                    delete[] data;
                    data = nullptr;
                }
            };

            int main()
            {
                Dog d;
            }
            --> Output: Dog destructor                           
                        Animal destructor
                        Animal cleanup

        + Khi d bị hủy:
            1. Dog::~Dog() chạy trước.
            2. Dog giải phóng vùng nhớ bằng delete[] data.
            3. Sau đó Animal::~Animal() chạy.
            4. Trong destructor của Animal, lời gọi cleanup() gọi Animal::cleanup(), không gọi Dog::cleanup().

        + Điều này có nghĩa là virtual dispatch không gọi xuống phần Dog đã bị hủy.
        + Nguy hiểm ở đây: Nếu thiết kế chương trình dựa vào việc destructor của Animal gọi virtual function để giải phóng tài nguyên riêng của Dog, logic đó sẽ không hoạt động như mong đợi. Trong ví dụ này, tài nguyên của Dog đã được giải phóng trong destructor của chính Dog.
        Nếu gọi trực tiếp một hàm thành viên không virtual mà truy cập data sau khi vùng nhớ đã giải phóng, đó còn có thể là lỗi nghiêm trọng.

Nhóm F — Multiple Inheritance
18. Multiple Inheritance (Đa kế thừa)
    18.1. Khái niệm
    - Một class trong C++ có thể kế thừa từ nhiều class cha cùng lúc.
        #include <iostream>
        class Printer
        {
        public:
            void print()
            {
                std::cout << "Print ";
            }
        };

        class Scanner
        {
        public:
            void scan()
            {
                std::cout << "Scan ";
            }
        };

        class Machine : public Printer, public Scanner
        {
        };

        int main()
        {
            Machine m;

            m.print();
            m.scan();
        }
        --> Output: Print Scan

    18.2. Khi nào xảy ra vấn đề?
    - Giả sử cả hai class cha đều có hàm start():
        class Printer
        {
        public:
            void start()
            {
                std::cout << "Printer ";
            }
        };

        class Scanner
        {
        public:
            void start()
            {
                std::cout << "Scanner ";
            }
        };

        class Machine : public Printer, public Scanner
        {
        };
        Machine m;
        // m.start(); // Lỗi: không rõ gọi hàm nào

    - Cách gọi tường minh:
        m.Printer::start();
        m.Scanner::start();
        --> Output: Printer Scanner

19. Virtual Inheritance (Kế thừa ảo)
    19.1. Diamond Problem là gì?
    - Hybrid kế thừa Dog và Cat; cả hai lại kế thừa Animal.

    19.2. Không dùng virtual inheritance
        #include <iostream>
        class Animal
        {
        public:
            int age = 0;
        };

        class Dog : public Animal {};
        class Cat : public Animal {};

        class Hybrid : public Dog, public Cat {};

        int main()
        {
            Hybrid h;

            h.Dog::age = 3;
            h.Cat::age = 5;

            std::cout << h.Dog::age << ' ';
            std::cout << h.Cat::age << '\n';

            // h.age = 10; // Lỗi: không rõ age thuộc nhánh nào
        }   
        --> Output: 3 5

    19.3. Dùng virtual inheritance
        class Dog : virtual public Animal {};
        class Cat : virtual public Animal {};

        class Hybrid : public Dog, public Cat {};

        int main()
        {
            Hybrid h;
            h.age = 10;
            std::cout << h.age << '\n';
        }
        --> Output: 10

    - Lúc này Hybrid chỉ có một phần Animal chung được chia sẻ qua hai nhánh kế thừa.
    - virtual trong virtual public Animal ở đây có nghĩa là virtual inheritance, không phải virtual function mà bạn đã học ở phần Polymorphism.

    19.4. So sánh nhanh
            Kế thừa thông thường	                   Virtual inheritance
        Dog và Cat có phần Animal riêng	           Dog và Cat chia sẻ phần Animal cơ sở ảo
        Hybrid có hai phần Animal	               Hybrid có một phần Animal chung
        h.age bị mơ hồ	                           h.age truy cập được trực tiếp

    19.5. Một quy tắc thêm về constructor
    - Khi dùng virtual inheritance, class dẫn xuất nhất chịu trách nhiệm khởi tạo virtual base.
        class Animal
        {
        public:
            int age;
            Animal(int a) : age(a) {}
        };

        class Dog : virtual public Animal
        {
        public:
            Dog() : Animal(1) {}
        };

        class Cat : virtual public Animal
        {
        public:
            Cat() : Animal(2) {}
        };

        class Hybrid : public Dog, public Cat
        {
        public:
            Hybrid() : Animal(10) {}
        };

    - Khi tạo Hybrid h, Animal(10) trong constructor của Hybrid quyết định cách khởi tạo virtual base Animal. Các lời gọi Animal(1) và Animal(2) trong constructor Dog và Cat bị bỏ qua khi chúng là các base subobject của Hybrid.

    - Bạn chỉ cần nhớ 3 ý ở mục này:
        1. Diamond Problem có thể tạo ra hai bản sao của cùng một base class.
        2. virtual public Animal giúp các nhánh kế thừa chia sẻ một virtual base Animal.
        3. Virtual inheritance khác hoàn toàn với virtual function.












