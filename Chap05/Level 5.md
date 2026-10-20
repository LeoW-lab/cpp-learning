```cpp
                    Level 5
1. STL Container
2. Iterator
3. STL Algorithm
4. Lambda
5. Template
6. Function Wrapper
7. Smart Pointer unique_ptr, shared_ptr, weak_ptr
------------------------------------------------------
1. STL Container
    1.1. vector: 
    - vector là container dùng để lưu nhiều phần tử cùng kiểu dữ liệu. Nó tự quản lý bộ nhớ và có thể tự tăng sức chứa khi cần.
        std::vector<int> v = {10, 20, 30};

    - Các thao tác cơ bản
        v[i];          // Truy cập phần tử tại vị trí i (bắt đầu từ 0)
        v.push_back(x); // Thêm phần tử x vào cuối
        v.pop_back();   // Xóa phần tử cuối
        v.size();       // Số phần tử hiện có
        v.empty();      // Kiểm tra vector có rỗng không
        v.clear();      // Xóa tất cả phần tử

        v.size();      // Số phần tử hiện có
        v.capacity();  // Sức chứa bộ nhớ đã cấp phát
            size(): số phần tử đang có trong vector.
            capacity(): số phần tử vector có thể chứa với vùng nhớ hiện tại trước khi cần cấp phát lại. Giá trị này luôn lớn hơn hoặc bằng size().

        v.resize(5);   // Đổi số phần tử thành 5
        v.reserve(10); // Đảm bảo capacity ít nhất là 10
            resize(n): thay đổi số phần tử. Nếu tăng kích thước, các phần tử mới được khởi tạo.
            reserve(n): chỉ yêu cầu thêm sức chứa bộ nhớ, không làm tăng số phần tử.

        v.begin(); // Iterator trỏ đến phần tử đầu tiên
        v.end();   // Iterator trỏ đến vị trí sau phần tử cuối

        
    - Duyệt vector bằng range-based for: auto
        + Dùng auto x: lấy giá trị từng phần tử
            for (auto x : v)
            {
                std::cout << x << " ";
            }
            * x là bản sao của mỗi phần tử. Thay đổi x không làm thay đổi phần tử trong vector.

        + Dùng auto& x: tham chiếu trực tiếp đến phần tử
            for (auto& x : v)
            {
                x *= 2;
            }
            // v = {20, 40, 60}
            * x tham chiếu đến phần tử thật nên thay đổi x sẽ làm thay đổi dữ liệu trong vector.

        + Chỉ đọc:
            for (const auto& x : v)
            {
                std::cout << x << " ";
            }

    1.2. std::map
    - Sắp xếp theo toán tử so sánh của kiểu key, mặc định là std::less<Key>
        std::map<std::string, int> score =
        {
            {"An", 80},
            {"Binh", 90},
            {"Cuong", 70}
        };

    - Các thao tác cơ bản
        score["An"];              // Lấy điểm của An
        score["An"] = 85;         // Cập nhật điểm của An
        score["Dung"] = 75;       // Thêm Dung nếu chưa tồn tại

        score.size();             // Số cặp key-value
        score.empty();            // Kiểm tra map có rỗng không
        score.clear();            // Xóa tất cả phần tử
        score.erase("Binh");      // Xóa phần tử có key "Binh"

        insert() và emplace(): Dùng để thêm phần tử nếu key chưa tồn tại. Nếu key đã tồn tại, value cũ không bị ghi đè.
            score.insert({"An", 100});
            score.emplace("Lan", 95);        

        find() — tìm key
            auto it = score.find("Binh");
            if (it != score.end())
            {
                std::cout << it->first;   // Key: Binh
                std::cout << it->second;  // Value: 90
            }

            + find(key): tìm phần tử có key tương ứng.
            + Nếu tìm thấy: trả về iterator đến phần tử đó.
            + Nếu không tìm thấy: trả về score.end().
            + Phải kiểm tra iterator trước khi truy cập it->first hoặc it->second.

        count() — kiểm tra key có tồn tại không
            score.count("An");
                + 1: key tồn tại.
                + 0: key không tồn tại.
                + Vì std::map không cho phép key trùng nhau nên kết quả chỉ có thể là 0 hoặc 1.

        at() — lấy value theo key
            int x = score.at("An");
            + Nếu key tồn tại: trả về value tương ứng.
            + Nếu key không tồn tại: ném ngoại lệ std::out_of_range.
            + Khác với operator[], at() không tự tạo phần tử mới. 
            
    - Duyệt map           
        for (const auto& item : score)
        {
            std::cout << item.first << ": "
                    << item.second << '\n';
        }
        + item.first: key.
        + item.second: value.
        + const auto&: đọc từng cặp dữ liệu mà không tạo bản sao và không sửa phần tử.

    1.3. std::unordered_map
    - std::unordered_map lưu dữ liệu theo cặp key → value, tương tự std::map, nhưng sử dụng hash table và không đảm bảo thứ tự khi duyệt phần tử.
        #include <iostream>
        #include <string>
        #include <unordered_map>

        std::unordered_map<std::string, int> score =
        {
            {"An", 80},
            {"Binh", 90},
            {"Cuong", 70}
        };
    - Các thao tác cơ bản:
        score["An"] = 85;             // Truy cập / thêm / cập nhật
        score.insert({"Dung", 75});   // Chèn nếu key chưa tồn tại
        score.emplace("Lan", 95);     // Chèn phần tử
        score.erase("Binh");          // Xóa theo key

        score.size();                 // Số phần tử
        score.empty();                // Kiểm tra rỗng
        score.clear();                // Xóa tất cả phần tử
        score.count("An");            // 1 nếu có, 0 nếu không
        score.at("An");               // Lấy value, ném ngoại lệ nếu không có key

    - Tìm kiếm bằng find()
        auto it = score.find("An");
        if (it != score.end())
        {
            std::cout << it->first;   // Key
            std::cout << it->second;  // Value
        }
        * find(key): trả về iterator nếu tìm thấy, ngược lại trả về end().
        * it->first: lấy key.
        * it->second: lấy value.
        * Phải kiểm tra iterator trước khi truy cập nếu key có thể không tồn tại.

    - Duyệt unordered_map
        for (const auto& item : score)
        {
            std::cout << item.first << ": "
                    << item.second << '\n';
        }
        * Thứ tự duyệt không được đảm bảo. Không nên dựa vào thứ tự xuất hiện của các phần tử.

    - Phân biệt map và unordered_map
        Đặc điểm	                        std::map	                        std::unordered_map
        Cấu trúc thường dùng	            Cây tìm kiếm cân bằng	            Hash table
        Thứ tự key	                        Có sắp xếp	                        Không đảm bảo
        Tìm kiếm trung bình	                O(log n)	                        O(1)
        Trường hợp xấu nhất	                O(log n)	                        O(n)
        Tìm kiếm theo khoảng key	        Có lower_bound(), upper_bound()	    Không hỗ trợ hai hàm này




    1.4. std::set: Nó giống map ở chỗ tự xử lý dữ liệu unique, nhưng không có key → value.
    - std::set lưu các giá trị không trùng nhau và tự sắp xếp theo comparator. Khác với map, nó không lưu cặp key → value; bản thân mỗi giá trị chính là key.
        #include <iostream>
        #include <set>

        std::set<int> numbers = {30, 10, 20, 10, 30};
        // Kết quả lưu: {10, 20, 30}  
        
    - Các thao tác cơ bản    
        numbers.insert(40);       // Thêm nếu chưa tồn tại
        numbers.emplace(50);      // Chèn phần tử
        numbers.erase(20);        // Xóa giá trị 20

        numbers.size();           // Số phần tử
        numbers.empty();          // Kiểm tra rỗng
        numbers.clear();          // Xóa tất cả phần tử
        numbers.count(10);        // 1 nếu tồn tại, 0 nếu không
        numbers.find(30);         // Tìm iterator
        + insert() và emplace() không tạo giá trị trùng lặp. Nếu giá trị đã tồn tại, set vẫn chỉ giữ một phần tử đó.
    
    - Tìm kiếm bằng find()
        auto it = numbers.find(20);

        if (it != numbers.end())
        {
            std::cout << *it;   // 20
        }
        + Vì set dùng giá trị làm key nên không có it->first hay it->second; dùng *it để lấy giá trị.

    - lower_bound() và upper_bound(): Hai hàm này tận dụng thứ tự đã sắp xếp của set.
        std::set<int> numbers = {10, 20, 30, 40, 50};
        auto it1 = numbers.lower_bound(30);  // Trỏ đến 30
        auto it2 = numbers.upper_bound(30);  // Trỏ đến 40

        + lower_bound(x): iterator đến phần tử đầu tiên không nhỏ hơn x (>=x).
        + upper_bound(x): iterator đến phần tử đầu tiên lớn hơn x (>x).
        + Nếu không có phần tử phù hợp, kết quả là end().

    - Duyệt set
        for (int x : numbers)
        {
            std::cout << x << " ";
        }
        // 10 20 30 40 50

        + Không thể sửa giá trị qua iterator của set, vì việc sửa trực tiếp có thể phá vỡ thứ tự của container. Muốn thay đổi một giá trị, hãy xóa giá trị cũ rồi chèn giá trị mới.

    1.5. std::unordered_set: container dùng để lưu các giá trị không trùng nhau
        #include <iostream>
        #include <unordered_set>

        std::unordered_set<int> numbers = {30, 10, 20, 10, 30};

        // Chỉ lưu 3 giá trị phân biệt: 10, 20, 30
        // Thứ tự duyệt không được đảm bảo.

    - Các thao tác cơ bản
        numbers.insert(40);       // Thêm nếu chưa tồn tại
        numbers.emplace(50);      // Chèn phần tử
        numbers.erase(20);        // Xóa giá trị 20

        numbers.size();           // Số phần tử
        numbers.empty();          // Kiểm tra rỗng
        numbers.clear();          // Xóa tất cả phần tử
        numbers.count(10);        // 1 nếu có, 0 nếu không
        numbers.find(30);         // Tìm iterator

    - Tìm kiếm và duyệt
        auto it = numbers.find(20);
        if (it != numbers.end())
        {
            std::cout << *it;
        }

        + Duyệt toàn bộ phần tử:
        for (int x : numbers)
        {
            std::cout << x << " ";
        }

    - Phân biệt set và unordered_set
        Đặc điểm	                    std::set	        std::unordered_set
        Giá trị trùng nhau	            Không	            Không
        Thứ tự	                        Có sắp xếp	        Không đảm bảo
        Tìm kiếm trung bình	            O(log n)	        O(1)
        Trường hợp xấu nhất	            O(log n)	        O(n)
        lower_bound(), upper_bound()	Có	                Không

2. Iterator
    - Iterator là đối tượng dùng để truy cập và di chuyển qua các phần tử trong container, ví dụ vector, map, set.
    - Có thể hiểu iterator giống như một con trỏ dùng để duyệt dữ liệu.

        std::vector<int> v = {10, 20, 30};
        auto it = v.begin();

    - Các thao tác cơ bản:
        v.begin();   // Iterator trỏ đến phần tử đầu tiên
        v.end();     // Vị trí sau phần tử cuối, không phải phần tử cuối

        ++it;        // Di chuyển đến phần tử tiếp theo
        *it;         // Lấy giá trị tại vị trí iterator
        *it = 100;   // Sửa giá trị phần tử nếu iterator cho phép ghi

    - begin() và cbegin()
        const_iterator: auto it = v.cbegin();

        begin(): trả về iterator có thể sửa phần tử nếu container cho phép.
        cbegin(): trả về iterator chỉ đọc, không thể sửa phần tử thông qua iterator đó.
        end(): vị trí kết thúc, nằm sau phần tử cuối.
        cend(): vị trí kết thúc tương ứng với cbegin().
      
            auto it1 = v.begin();
            *it1 = 100;         // Hợp lệ

            auto it2 = v.cbegin();
            // *it2 = 100;      // Lỗi: không được sửa qua const_iterator

    2.1. Duyệt container bằng iterator
        - Duyệt bằng vòng lặp for:
            std::vector<int> v = {10, 20, 30};
            for (auto it = v.begin(); it != v.end(); ++it)
            {
                std::cout << *it << " ";
            }
            Kết quả: 10 20 30

        - Duyệt chỉ đọc bằng cbegin() và cend():
            for (auto it = v.cbegin(); it != v.cend(); ++it)
            {
                std::cout << *it << " ";
            }

    2.2. Thay đổi giá trị qua iterator
        std::vector<int> v = {10, 20, 30};
        for (auto it = v.begin(); it != v.end(); ++it)
        {
            *it *= 2;
        }
        Kết quả: {20, 40, 60}

        Với cbegin() và cend(), không thể thay đổi giá trị phần tử thông qua iterator.

    2.3. Các loại iterator
        Input Iterator	            Đọc tuần tự
        Output Iterator	            Ghi tuần tự
        Forward Iterator	        Đọc, ghi và tiến về phía trước
        Bidirectional Iterator	    Tiến và lùi (++, --)
        Random Access Iterator	    Truy cập ngẫu nhiên (+, -, [])
        Contiguous Iterator	        Các phần tử nằm liên tiếp trong bộ nhớ

        Ví dụ:
            vector: Random Access Iterator, đồng thời là Contiguous Iterator.
            list: Bidirectional Iterator.
            map, set: Bidirectional Iterator.
            forward_list: Forward Iterator.

    2.4. Iterator Invalidation — Iterator mất hiệu lực
    - Iterator đang trỏ đến một phần tử có thể trở nên không hợp lệ khi container bị thay đổi.
        std::vector<int> v = {10, 20, 30};
        auto it = v.begin();
        v.push_back(40);  // Có thể cấp phát lại vùng nhớ

        // Nếu vector cấp phát lại vùng nhớ,
        // it không còn hợp lệ.

    2.5. Di chuyển iterator — advance()
        std::vector<int> v = {10, 20, 30, 40};
        auto it = v.begin();
        std::advance(it, 2);
        std::cout << *it; // 30

            - std::advance(it, n): di chuyển iterator thêm n vị trí.
            - Cần #include <iterator>.
            - Với list, nên dùng std::advance() thay vì it + n.
            - Phải bảo đảm iterator không vượt quá phạm vi hợp lệ.

    2.6. Tính khoảng cách — distance()
        std::vector<int> v = {10, 20, 30, 40};
        auto n = std::distance(v.begin(), v.end());
        std::cout << n; // 4

            - std::distance(first, last): tính số bước từ first đến last.
            - Cần #include <iterator>.
            - Với iterator của vector, phép tính khoảng cách có độ phức tạp O(1); với list, thường là O(n).

    2.7. rbegin() và rend() — Duyệt ngược
        std::vector<int> v = {10, 20, 30};
        for (auto it = v.rbegin(); it != v.rend(); ++it)
        {
            std::cout << *it << " ";
        }
        // Kết quả: 30 20 10

            - rbegin(): reverse iterator trỏ đến phần tử cuối.
            - rend(): vị trí nằm trước phần tử đầu trong hướng duyệt ngược.
            - ++it di chuyển về phía phần tử đứng trước trong container.
            - crbegin() và crend() dùng để duyệt ngược chỉ đọc.
            Lưu ý: Không phải mọi container đều hỗ trợ reverse iterator; ví dụ std::forward_list không hỗ trợ.

    2.8. std::next() và std::prev()
        std::vector<int> v = {10, 20, 30, 40};

        auto it = v.begin();

        auto it2 = std::next(it, 2);
        std::cout << *it2; // 30

        auto it3 = std::prev(v.end());
        std::cout << *it3; // 40

            - std::next(it, n): trả về iterator mới tiến n vị trí, không thay đổi it.
            - std::prev(it, n): trả về iterator mới lùi n vị trí, không thay đổi it.
            - Cần #include <iterator>.
            - Phải bảo đảm iterator mới nằm trong phạm vi hợp lệ.
            Khác với std::advance(), hai hàm này trả về iterator mới, còn advance() thay đổi iterator được truyền vào.

3. STL Algorithm
    3.1. sort() - sort(), greater
        std::sort(v.begin(), v.end());                      // Tăng dần
        std::sort(v.begin(), v.end(), std::greater<int>()); // Giảm dần

            + Cần #include <algorithm>.
            + std::greater<int> cần #include <functional>.
            + Dùng được với vector, array; không dùng trực tiếp với list.

    3.2. find() — Tìm giá trị
        auto it = std::find(v.begin(), v.end(), 30);
            + Tìm thấy: trả về iterator đến phần tử.
            + Không tìm thấy: trả về v.end().

    3.3. find_if() — Tìm phần tử theo điều kiện
        auto it = std::find_if(v.begin(), v.end(), [](int x)
        {
            return x % 2 == 0 && x > 10;
        });
        + Trả về iterator đến phần tử đầu tiên thỏa điều kiện, hoặc end() nếu không tìm thấy.
        
    reverse()
        std::reverse(v.begin(), v.end());

    3.4. count() — Đếm giá trị
        int n = std::count(v.begin(), v.end(), 20);
        + Đếm số phần tử có giá trị bằng 20.

    3.5. count_if() — Đếm theo điều kiện & Lambda
        int n = std::count_if(v.begin(), v.end(), [](int x)
        {
            return x > 10;
        });
        + Đếm số phần tử thỏa điều kiện.

    3.6. reverse() — Đảo thứ tự
        std::reverse(v.begin(), v.end());
        + Đảo thứ tự phần tử hiện có; không tự sắp xếp tăng hoặc giảm dần.

    3.7. for_each() — Thực hiện thao tác trên từng phần tử
        std::for_each(v.begin(), v.end(), [](int& x)
        {
            x += 3;
        });
        + Dùng int& x để sửa trực tiếp từng phần tử.

    3.8. transform() — Biến đổi phần tử
        std::vector<int> result(v.size());
        std::transform(v.begin(), v.end(), result.begin(), [](int x)
        {
            return x * 2;
        });

        + Đọc từng phần tử của v, nhân đôi rồi ghi vào result.
        + result phải có đủ chỗ để nhận kết quả.
        + Có thể biến đổi ngay trên container ban đầu nếu sử dụng đúng iterator đầu ra.

    3.9. min_element() và max_element()
        auto minIt = std::min_element(v.begin(), v.end());
        auto maxIt = std::max_element(v.begin(), v.end());

        + Trả về iterator đến phần tử nhỏ nhất/lớn nhất.
        + Dùng *minIt, *maxIt để lấy giá trị.
        + Nếu container rỗng, kết quả là end().

    3.10. remove() — Loại bỏ theo giá trị
        auto newEnd = std::remove(v.begin(), v.end(), 20);
        v.erase(newEnd, v.end());

        + remove() dồn các phần tử không cần xóa lên phía trước.
        + Không tự giảm size() của container.
        + Dùng erase() để xóa phần đuôi còn lại.

    3.11. remove_if() — Loại bỏ theo điều kiện
        auto newEnd = std::remove_if(v.begin(), v.end(), [](int x)
        {
            return x % 2 == 0;
        });
        v.erase(newEnd, v.end());
        + Ví dụ trên loại bỏ các số chẵn khỏi vector.

    3.12. all_of(), any_of(), none_of() — Kiểm tra điều kiện
        bool a = std::all_of(v.begin(), v.end(), [](int x)
        {
            return x > 0;
        });

        bool b = std::any_of(v.begin(), v.end(), [](int x)
        {
            return x % 2 == 0;
        });

        bool c = std::none_of(v.begin(), v.end(), [](int x)
        {
            return x < 0;
        });

        + all_of(): tất cả phần tử đều thỏa điều kiện.
        + any_of(): có ít nhất một phần tử thỏa điều kiện.
        + none_of(): không có phần tử nào thỏa điều kiện.


4. Lambda
    4.1. lambda + capture
    - Lambda là một hàm không cần đặt tên, được viết trực tiếp tại nơi cần sử dụng.
    - Hàm riêng:
        int add(int x, int n)
        {
            return x + n;
        }

        int main()
        {
            std::cout << add(5, 10); // 15
        }
    - lambda
        auto f = [](int x)
        {
            return x + 10;
        };

        std::cout << f(5); // 15

    - Giải thích:
        + auto f: compiler tự xác định kiểu của biến f. Ở đây, f lưu một lambda.
        + []: capture list, quy định lambda lấy các biến bên ngoài như thế nào.
        + (int x): tham số đầu vào.
        + { return x + 10; }: phần thân lambda.
        + f(5): gọi lambda với đầu vào 5.

    - Lambda cũng có thể không nhận tham số:
        auto f = []()
        {
            std::cout << "Hello";
        };
        f(); // Hello

    - Capture cho phép lambda sử dụng biến được khai báo bên ngoài lambda.
        int n = 10;
        auto f = [n](int x)
        {
            return x + n;
        };
        std::cout << f(5); // 15

        + Ở đây, n không phải tham số của lambda. Lambda sử dụng được n nhờ capture [n].
        + Nếu không capture n mà vẫn sử dụng nó bên trong lambda, chương trình sẽ báo lỗi biên dịch:
            int n = 10;
            auto f = [](int x)
            {
                return x + n; // Lỗi: n chưa được capture
            };

        + Parameter: dữ liệu truyền vào mỗi lần gọi hàm, ví dụ x.
        + Capture: biến bên ngoài mà lambda muốn sử dụng, ví dụ n.

    4.2. Capture bằng value và mutable
    - Mặc định, khi capture bằng value, lambda lưu một bản sao của biến bên ngoài.
        int n = 10;
        auto f = [n]()
        {
            //n *= 2;  // Lỗi biên dịch
            return n * 2;
        };

        std::cout << f(); // 20
        std::cout << n;   // 10
        + Lambda dùng bản sao của n để tính toán. Biến gốc vẫn là 10.

    - Thông thường, lambda capture bằng value không được phép sửa bản sao đã capture trong phần thân lambda. Từ khóa mutable cho phép sửa bản sao đó.
        int n = 10;
        auto f = [n]() mutable
        {
            n *= 2;
            return n;
        };
        std::cout << f(); // 20
        std::cout << n;   // 10

        + Diễn biến:
            1. Biến bên ngoài n có giá trị 10.
            2. [n] tạo bản sao n bên trong lambda.
            3. mutable cho phép sửa bản sao đó thành 20.
            4. Biến n bên ngoài vẫn là 10.

    4.3. [=] — capture bằng value
    - [=] có nghĩa là capture bằng value các biến cục bộ bên ngoài được sử dụng trong lambda.
        int a = 10;
        int b = 20;

        auto f = [=]()
        {
            return a + b;
        };
        std::cout << f() << '\n'; // 30

        4.3.1. Chuyện gì xảy ra bên trong?
            - Bên ngoài lambda
                a = 10
                b = 20

            - Bên trong lambda
                Bản sao của a = 10
                Bản sao của b = 20

            - Khi gọi f(), lambda tính 10 + 20 và trả về 30.



        4.3.2. Nếu biến bên ngoài thay đổi thì sao?
            int a = 10;
            auto f = [=]()
            {
                return a;
            };
            a = 100;
            std::cout << f(); // 10

            --> Kết quả vẫn là 10, vì lambda đã lưu bản sao của a với giá trị 10. 
            Thay đổi a bên ngoài thành 100 không làm bản sao bên trong thay đổi theo.

        4.3.3. Có thể sửa biến đã capture bằng [=] không?
            int a = 10;
            auto f = [=]()
            {
                a = 100; // Lỗi biên dịch
            };

        - Muốn sửa bản sao đó thì phải thêm mutable:
            int a = 10;
            auto f = [=]() mutable
            {
                a = 100;
            };
            f();
            std::cout << a; // 10

        4.4.4. Có thể chỉ định từng biến
            int a = 10;
            int b = 20;
            int c = 30;

            auto f = [a, b]()
            {
                return a + b;
            };
            std::cout << f(); // 30        
        
                + [a, b]: chỉ capture a và b bằng value.
                + c không được capture nên không thể sử dụng trực tiếp trong lambda.
                + a, b bên trong lambda là bản sao của các biến bên ngoài.


    4.4. [&] — capture bằng reference
    - [&] nghĩa là capture các biến bên ngoài mà lambda sử dụng bằng reference (tham chiếu). 
    - Lambda làm việc trực tiếp với các biến gốc bên ngoài, không tạo bản sao riêng như [=].

        4.4.1. VD
            int a = 10;
            int b = 20;

            auto f = [&]()
            {
                a += 10;
                b += 20;
            };
            f();

            std::cout << a << '\n'; // 20
            std::cout << b << '\n'; // 40

            - Giải thích:
                [&]: capture a và b bằng reference.
                a += 10: thay đổi trực tiếp a bên ngoài từ 10 thành 20.
                b += 20: thay đổi trực tiếp b bên ngoài từ 20 thành 40.
                Gọi f() thì các thay đổi được thực hiện trên chính biến gốc.

        4.4.2. So sánh [=] và [&]
            [=]
                - Capture bằng value
                    + Tạo bản sao của biến.
                    + Sửa bản sao không làm thay đổi biến gốc.
                    + Muốn sửa bản sao trong lambda thì dùng mutable.

            [&]
                - Capture bằng reference
                    + Không tạo bản sao riêng cho biến được capture.
                    + Lambda làm việc với biến gốc.
                    + Có thể thay đổi biến gốc mà không cần mutable.

        4.3.3. Nếu biến bên ngoài thay đổi thì sao?
            int a = 10;
            auto f = [&]()
            {
                return a;
            };
            a = 100;
            std::cout << f(); // 100

            - Khi a thay đổi thành 100, gọi f() sẽ trả về 100.

        4.4.4. Có thể chỉ định từng biến bằng reference
            int a = 10;
            int b = 20;

            auto f = [&a]()
            {
                a += 5;
            };

            f();

            std::cout << a << '\n'; // 15
            std::cout << b << '\n'; // 20

            - Note:
                + [&]: capture các biến bên ngoài được sử dụng bằng reference.
                + Thay đổi biến bên trong lambda có thể làm thay đổi biến gốc.
                + Không cần mutable để sửa biến được capture bằng reference.
                + [&a]: chỉ capture riêng biến a bằng reference.



    4.5. Capture nhiều biến

            Cú pháp	                Ý nghĩa
            [=]	                    Capture các biến được sử dụng bằng value
            [&]	                    Capture các biến được sử dụng bằng reference
            [a, b]	                Chỉ capture a, b bằng value
            [&a, &b]	            Chỉ capture a, b bằng reference
            [a, &b]	                Capture a bằng value, b bằng reference            

        - VD1 kết hợp:
            int a = 10;
            int b = 20;

            auto f = [a, &b]() mutable
            {
                a += 5;
                b += 5;
            };

            f();

            std::cout << a << '\n'; // 10
            std::cout << b << '\n'; // 25

                + a là bản sao, nên thay đổi a bên trong lambda không ảnh hưởng biến gốc.
                + b là reference, nên thay đổi b bên trong lambda làm biến gốc thay đổi.
                + mutable cần thiết vì lambda sửa biến a đã capture bằng value.

        - VD2:
            int a = 10;
            int b = 20;

            auto f = [a, &b]() mutable
            {
                a += 5;
                b += 5;

                std::cout << "Inside: a = " << a << '\n'; // 15
                std::cout << "Inside: b = " << b << '\n'; // 25
            };

            f();

            std::cout << "Outside: a = " << a << '\n'; // 10
            std::cout << "Outside: b = " << b << '\n'; // 25

            --> Output:
                    Inside: a = 15
                    Inside: b = 25
                    Outside: a = 10
                    Outside: b = 25

    
    4.6. Cú pháp tổng quát
        [capture](parameters) mutable -> return_type
        {
            // statements
        };

        4.1. Ý nghĩa từng thành phần
            capture:        Cách lambda lấy các biến từ bên ngoài, ví dụ [a], [&b], [=], [&].
            parameters:     Danh sách tham số đầu vào, giống tham số của hàm thông thường.
            mutable:        Tùy chọn, cho phép sửa các biến được capture bằng value bên trong lambda.
            return_type:    Tùy chọn, chỉ định kiểu dữ liệu trả về.    
            statements:     Thân lambda, chứa các câu lệnh cần thực hiện.

        4.2. Ví dụ
            int n = 10;
            auto f = [n](int x) mutable -> int
            {
                n += x;
                return n;
            };
            std::cout << f(5); // 15

            - Giải thích:
                + [n]: capture bản sao của n, ban đầu bằng 10.
                + (int x): nhận tham số x.
                + mutable: cho phép thay đổi bản sao n.
                +  int: khai báo kiểu trả về là int.
                + n += x: tăng bản sao n thêm x.
                + return n: trả về kết quả.
                Biến n bên ngoài vẫn bằng 10.

        4.3. Có thể bỏ mutable và return_type
            - Nếu không cần sửa biến capture bằng value và compiler tự suy luận được kiểu trả về, bạn có thể viết ngắn hơn:
                int n = 10;
                auto f = [n](int x)
                {
                    return n + x;
                };
                std::cout << f(5); // 15

                + Không cần mutable vì không sửa bản sao n.
                + Không cần -> int vì compiler suy luận kiểu trả về là int.

            - mutable và -> return_type đều là thành phần tùy chọn, không phải lambda nào cũng cần chúng.

5. Template
    5.1. Template là gì?
    - Template giúp viết một khuôn mẫu, rồi compiler tạo phiên bản phù hợp với kiểu dữ liệu.
    - VD không dùng template:
        int addInt(int a, int b)
        {
            return a + b;
        }
        double addDouble(double a, double b)
        {
            return a + b;
        }

    - Dùng template:
        template <typename T>
        T add(T a, T b)
        {
            return a + b;
        }
        std::cout << add(2, 3) << '\n';       // 5
        std::cout << add(2.5, 3.5) << '\n';   // 6

    5.2. Function template
        5.2.1. Một tham số kiểu T
            template <typename T>
            T add(T a, T b)
            {
                return a + b;
            }
            std::cout << add(10, 20); // 30

            - template <typename T>: khai báo tham số kiểu T.
            - T add(...): kiểu trả về là T.
            - T a, T b: hai tham số có cùng kiểu T.
            Lưu ý: add(10, 2.5) thường bị lỗi suy luận kiểu vì compiler không thể suy ra một T vừa là int vừa là double.

        5.2.2. Hai tham số kiểu T và U
            template <typename T, typename U>
            auto add(T a, U b)
            {
                return a + b;
            }
            std::cout << add(10, 2.5); // 12.5

            + T và U có thể là hai kiểu dữ liệu khác nhau.
            + auto cho phép compiler suy luận kiểu trả về từ biểu thức a + b.

        - typename và class đều có thể dùng để khai báo tham số kiểu trong template:
                template <typename T>
                T add(T a, T b);
            Tương đương:
                template <class T>
                T add(T a, T b);

    5.3. Class template
        - Class template là khuôn mẫu để tạo class có thể làm việc với nhiều kiểu dữ liệu.
        - Ví dụ:
            template <typename T>
            class Box
            {
            private:
                T value;

            public:
                Box(T v) : value(v) {}
                T getValue()
                {
                    return value;
                }
            };

            Box<int> b1(10);
            Box<double> b2(2.5);

            std::cout << b1.getValue() << '\n'; // 10
            std::cout << b2.getValue() << '\n'; // 2.5

            - Giải thích:
                + Box<T> là class template.
                + Box<int> tạo một kiểu Box lưu giá trị int.
                + Box<double> tạo một kiểu Box lưu giá trị double.
                + T được thay bằng kiểu dữ liệu tương ứng khi sử dụng.

    5.4. STL Container có phải template không?
    - Những container này đều là class template trong STL.

            Container	        Là template?	        Ví dụ
            vector	                ✅	                vector<int>
            map	                    ✅	                map<string, int>
            unordered_map	        ✅	                unordered_map<string, int>
            set	                    ✅	                set<int>
            unordered_set	        ✅	                unordered_set<int>

6. Function Wrapper - std::function
    - Cần #include <functional>
    - Có thể gán lambda, function thông thường hoặc function có chữ ký phù hợp.
    - Truyền std::function vào hàm khác

    6.1. std::function là gì?
    - std::function là một function wrapper — đối tượng có thể lưu và gọi một callable có chữ ký phù hợp, chẳng hạn lambda hoặc hàm thông thường.
    - Thư viện: #include <functional>
    - Ví dụ: std::function<int(int)> f; --> Cái hộp f chỉ nhận thứ có dạng: nhận int --> trả int
    - VD:
        #include <iostream>
        #include <functional>

        int main()
        {
            std::function<int(int, int)> f;

            f = [](int a, int b) {
                return a + b;
            };
            std::cout << f(10, 20); // 30
        }
    - Giải thích std::function<int(int, int)>:
        + int đầu tiên: kiểu giá trị trả về.
        + (int, int): hai tham số đầu vào, đều có kiểu int.
        + f: biến có thể lưu callable phù hợp với chữ ký đó.
        + f(10, 20): gọi callable đang được lưu trong f.

    6.2. Gán lambda hoặc hàm thông thường
        6.2.1. Gán lambda
            std::function<int(int)> f = [](int x) {
                return x * 2;
            };

            std::cout << f(5); // 10

        - f = std::function
        - lambda = thứ được lưu bên trong f

        6.2.2. Gán hàm thông thường
            int doubleValue(int x)
            {
                return x * 2;
            }

            std::function<int(int)> f = doubleValue;
            std::cout << f(5); // 10


    6.3. Truyền std::function vào hàm khác
            #include <iostream>
            #include <functional>

            void execute(std::function<int(int)> f)
            {
                std::cout << f(5) << '\n';
            }

            int main()
            {
                std::function<int(int)> f = [](int x) {
                    return x * 2;
                };

                execute(f); // 10
            }
        - Luồng hoạt động:
            + f lưu lambda.
            + Truyền f vào execute().
            + execute() gọi f(5).
            + Lambda trả về 10, rồi kết quả được in ra.

    6.4. Note
        - Cần #include <functional>.
        - std::function<return_type(parameters)> xác định chữ ký callable.
        - Có thể lưu lambda hoặc hàm thông thường có chữ ký phù hợp.
        - Có thể truyền std::function vào hàm khác.
        - Nếu std::function chưa chứa callable nào mà được gọi, chương trình sẽ ném ngoại lệ std::bad_function_call.

    6.5. So sánh
        6.5.1. Không dùng std::function
                auto f = [](int x) {
                    return x * 2;
                };

                std::cout << f(5); // 10
                --> Ở đây, f có kiểu do compiler suy luận từ kiểu lambda cụ thể.

        6.5.2. Dùng std::function
                std::function<int(int)> f = [](int x) {
                    return x * 2;
                };

                std::cout << f(5); // 10
                --> Ở đây, f có kiểu std::function<int(int)>, có thể lưu các callable khác nhau miễn là chúng phù hợp với chữ ký yêu cầu.

            - Sau đó có thể gán một hàm khác cho f:
                int square(int x)
                {
                    return x * x;
                }
                f = square;
                std::cout << f(5); // 25

    6.6. Ưu điểm của std::function
        6.6.1. Có thể thay đổi callable linh hoạt
                #include <iostream>
                #include <functional>

                int main()
                {
                    std::function<int(int)> f;

                    f = [](int x) { return x * 2; };
                    std::cout << f(5) << '\n'; // 10

                    f = [](int x) { return x * x; };
                    std::cout << f(5) << '\n'; // 25
                }
            - Cùng một biến f, có thể thay lambda thực hiện phép nhân đôi bằng lambda tính bình phương.
            - Nếu dùng auto f = lambda, kiểu của f là kiểu lambda cụ thể và không thể gán tùy ý một lambda khác có kiểu closure khác vào f.

                auto f = [](int x) { return x * 2; };
                // f = [](int x) { return x * x; }; // Lỗi biên dịch

        6.6.2. Truyền callback vào hàm khác
        - Ví dụ, một hàm thực hiện công việc nhưng cho phép bên gọi quyết định cách xử lý:
                void process(std::function<void(int)> callback)
                {
                    callback(100);
                }
        - Có thể truyền những cách xử lý khác nhau:
                process([](int x) {
                    std::cout << "Value: " << x;
                });

                process([](int x) {
                    std::cout << "Received: " << x;
                });
                --> Hàm process() không cần biết cụ thể lambda nào được truyền vào, miễn là nó phù hợp với void(int).

        - Nếu viết:
                void process()
                {
                    std::function<void(int)> callback;

                    callback = [](int x) {
                        std::cout << "Value: " << x;
                    };

                    callback(100);
                }
                --> Thì process() tự quyết định cách xử lý. Bên ngoài không thể truyền callback khác vào để thay đổi hành vi của hàm.

        6.6.3. Lưu callable để sử dụng về sau
            - Có thể khai báo std::function như một thành viên của class hoặc một biến trong chương trình, 
            sau đó gán callback ở nơi khác và gọi khi cần. Cách này hữu ích trong event handling, GUI, callback và một số hệ thống điều khiển.


    6.7. Khi nào nên dùng?
    - Lambda + auto: Khi tạo lambda và dùng trực tiếp, không cần thay đổi callable.
    - std::function: Khi cần một kiểu giao diện chung để lưu hoặc truyền nhiều callable khác nhau, chẳng hạn callback có thể thay đổi trong lúc chạy.
    - std::function có thể phát sinh chi phí lưu trữ và gọi gián tiếp; 
    mức độ phụ thuộc compiler và callable. Nó không phải lúc nào cũng chậm đáng kể, nhưng cũng không miễn phí.





7. Smart Pointer unique_ptr, shared_ptr, weak_ptr
    - Cần #include <memory>

    7.1. std::unique_ptr:
        7.1.1. Tạo và sử dụng:
            std::unique_ptr<int> p = std::make_unique<int>(10);
            std::cout << *p; // 10

        - Giải thích:
            + std::make_unique<int>(10) tạo một đối tượng int có giá trị 10 và trả về unique_ptr quản lý nó.
            + p sở hữu đối tượng đó.
            + *p truy cập giá trị của đối tượng.
        - Khi p hết vòng đời, đối tượng được tự động giải phóng. Bạn không cần gọi delete.

        7.1.2. Chuyển quyền sở hữu bằng std::move()
            auto p1 = std::make_unique<int>(10);
            auto p2 = std::move(p1);
            std::cout << *p2;  // 10
            if (!p1) std::cout << "empty";

        - Giải thích: 
            + p2 nhận quyền sở hữu đối tượng.
            + p1 không còn sở hữu đối tượng; với unique_ptr, nó trở thành rỗng (nullptr).
            + Đối tượng vẫn tồn tại và được quản lý bởi p2.

        - Lưu ý: std::move() tự nó không chuyển quyền sở hữu. 
        Nó chuyển biểu thức sang dạng có thể move; phép move của unique_ptr mới thực hiện việc chuyển quyền sở hữu.

        7.1.3 get(), reset() và release()
            p.get(); // Lấy raw pointer
                auto p = std::make_unique<int>(10);
                int* raw = p.get();
                std::cout << *raw; // 10

                + get() trả về con trỏ thông thường (int*) để truy cập đối tượng. Nó không chuyển quyền sở hữu cho raw. Đối tượng vẫn do p quản lý.

            p.reset(); // giải phóng đối tượng đang sở hữu
                auto p = std::make_unique<int>(10);
                p.reset();
                if (!p)
                    std::cout << "empty";

                - Sau reset() không có đối số:
                    + Đối tượng int cũ bị hủy.
                    + p trở thành rỗng (nullptr).

                1. Xóa object đang được quản lý
                2. p không còn sở hữu gì
                3. p trở thành nullptr

            p.release();    // nhả quyền sở hữu mà không xóa đối tượng
                auto p = std::make_unique<int>(10);
                int* raw = p.release();
                std::cout << *raw; // 10
                delete raw;        // Phải tự giải phóng

                - Sau release():
                    + Đối tượng vẫn tồn tại.
                    + p trở thành rỗng.
                    + Con trỏ thô raw nhận địa chỉ đối tượng.
                    + Phải tự đảm bảo đối tượng được giải phóng đúng cách, nếu không sẽ rò rỉ bộ nhớ.

                → KHÔNG XÓA object
                → p = nullptr
                → trả địa chỉ object ra ngoài
                → phải tự delete

                Lệnh	            Object trên heap        unique_ptr sau đó
                p.reset()	            ❌ bị xóa	            nullptr
                p.release()	            ✅ vẫn còn	            nullptr
                std::move(p)	        ✅ vẫn còn	            ownership chuyển sang pointer khác

        7.1.4. Quản lý object/class + toán tử 
            - unique_ptr không chỉ quản lý kiểu dữ liệu cơ bản mà còn quản lý object của class.
                class Dog
                {
                public:
                    void bark()
                    {
                        std::cout << "Woof!\n";
                    }
                };

                auto dog = std::make_unique<Dog>();
                dog->bark(); // Woof!

            - Dùng -> để gọi thành viên của đối tượng được quản lý. Khi dog hết vòng đời, đối tượng Dog được tự động hủy.



        7.1.5. Truyền unique_ptr vào hàm / return từ hàm
        - Truyền bằng giá trị — hàm nhận quyền sở hữu
            void process(std::unique_ptr<int> p)
            {
                std::cout << *p;
            }

            auto p = std::make_unique<int>(10);
            process(std::move(p));

        - Truyền bằng reference — có thể thay đổi owner
            void resetPointer(std::unique_ptr<int>& p)
            {
                p.reset();
            }
            --> Hàm thao tác trực tiếp trên unique_ptr của bên gọi. Trong ví dụ này, gọi resetPointer(p) sẽ giải phóng đối tượng và làm p rỗng.

        - Truyền bằng const reference — không thay đổi quyền sở hữu của pointer
            void printValue(const std::unique_ptr<int>& p)
            {
                if (p)
                    std::cout << *p;
            }        
            --> Hàm không thể reset, move hoặc gán lại chính p, nhưng vẫn có thể đọc đối tượng. 
            Lưu ý: const unique_ptr<T>& không đồng nghĩa với đối tượng T cũng là const.

            + Trả về từ hàm: Có thể trả unique_ptr trực tiếp để chuyển quyền sở hữu cho bên nhận.
                std::unique_ptr<int> createValue()
                {
                    return std::make_unique<int>(100);
                }
                auto p = createValue();
                --> Không cần tự gọi std::move() khi trả về trực tiếp một unique_ptr cục bộ theo cách thông thường này.

            Nhớ 3 kiểu truyền unique_ptr
                unique_ptr<T> p
                → muốn chuyển ownership
                → std::move()

                unique_ptr<T>& p
                → reference, có thể sửa owner

                const unique_ptr<T>& p
                → reference, chỉ đọc

    7.2. std::shared_ptr
    - shared_ptr dùng khi nhiều smart pointer cùng sở hữu một đối tượng. Nó sử dụng reference counting để theo dõi số chủ sở hữu.
        7.2.1. Tạo và copy

        auto p1 = std::make_shared<int>(10); 
        auto p2 = p1;   
        std::cout << *p2;            // 10 
        std::cout << p1.use_count(); // 2

        - Giải thích:
            + p1 và p2 cùng sở hữu một đối tượng int.
            + Copy p1 sang p2 làm số chủ sở hữu tăng lên.
            + Khi không còn shared_ptr nào sở hữu đối tượng, đối tượng sẽ được giải phóng.

        7.2.2. Các hàm thường dùng
            p.use_count();  // Số shared_ptr đang sở hữu 
            p.get();        // Lấy raw pointer 
            p.reset();      // Bỏ quyền sở hữu

            - Copy shared_ptr làm tăng số chủ sở hữu.
            - reset() làm giảm số chủ sở hữu; nếu về 0, đối tượng được giải phóng.

                auto p1 = std::make_shared<int>(10);
                auto p2 = p1;

                p1.reset();

                std::cout << p2.use_count(); // 1
                std::cout << *p2;            // 10
            --> p1.reset() chỉ bỏ quyền sở hữu của p1. Đối tượng vẫn tồn tại vì p2 còn sở hữu nó.

    7.3. std::weak_ptr
    - weak_ptr dùng để quan sát đối tượng được quản lý bởi shared_ptr mà không sở hữu đối tượng đó.
        (unique_ptr sở hữu độc quyền, không có cơ chế reference counting để weak_ptr theo dõi vòng đời đối tượng.)
        
        7.3.1. Tạo và truy cập đối tượng
            auto p = std::make_shared<int>(10);
            std::weak_ptr<int> w = p;

            if (auto s = w.lock())
            {
                std::cout << *s;  // 10
            }

            - Giải thích:
                + w quan sát đối tượng mà p sở hữu.
                + w không làm tăng số chủ sở hữu của đối tượng.
                + w.lock() trả về một shared_ptr nếu đối tượng còn tồn tại; nếu đối tượng đã bị hủy, nó trả về shared_ptr rỗng.
                + Trong ví dụ này, s tạm thời trở thành một chủ sở hữu nữa khi lock() thành công.

        7.3.2. Các hàm thường dùng
            w.lock();       // Lấy shared_ptr nếu đối tượng còn tồn tại 
            w.expired();    // Kiểm tra đối tượng đã hết hạn 
            w.use_count();  // Số shared_ptr sở hữu đối tượng 
            w.reset();      // Xóa weak_ptr đang quan sát

            - lock(): trả về shared_ptr nếu đối tượng còn tồn tại; nếu không, trả về shared_ptr rỗng.
            - expired(): trả về true nếu đối tượng đã bị hủy.
            - Nên dùng lock() để lấy quyền truy cập an toàn thay vì kiểm tra expired() rồi mới sử dụng.

        7.3.3. Vì sao cần weak_ptr?
            - Một công dụng quan trọng là tránh vòng tham chiếu giữa các shared_ptr.
            - Ví dụ, nếu object A sở hữu B bằng shared_ptr, đồng thời B cũng sở hữu A bằng shared_ptr, số chủ sở hữu có thể không bao giờ về 0 dù bên ngoài không còn sử dụng chúng. Dùng weak_ptr cho một chiều liên kết có thể giải quyết vấn đề này.




