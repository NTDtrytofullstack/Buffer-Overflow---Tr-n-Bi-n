# Buffer-Overflow---Tr-n-Bi-
## 1.Tràn Biến là gì?
- Tràn biến (hay "tràn số" trong tiếng Anh là "overflow") là một sự cố trong lập trình máy tính khi kết quả của một phép tính vượt quá giới hạn giá trị mà kiểu dữ liệu có thể lưu trữ. Khi tràn biến xảy ra, giá trị của biến có thể được ghi đè với một giá trị không mong muốn, dẫn đến kết quả không chính xác hoặc lỗi chương trình.
- Cụ thể hơn, mỗi kiểu dữ liệu trong lập trình có một giới hạn giá trị nhất định mà nó có thể lưu trữ. Ví dụ, với kiểu dữ liệu int trong ngôn ngữ lập trình C, giá trị lớn nhất có thể lưu trữ là 2,147,483,647 (khi sử dụng 32-bit). Nếu kết quả của một phép tính vượt quá giá trị này, tràn số sẽ xảy ra và giá trị lưu trữ sẽ không chính xác.
- Để ngăn chặn tràn biến, bạn có thể sử dụng kiểu dữ liệu với phạm vi lớn hơn hoặc kiểm tra các giá trị trước khi thực hiện các phép tính để đảm bảo chúng không vượt quá giới hạn cho phép.
---
## 2.Thế còn Buffer Overflow ?.
- Trong lĩnh vực an ninh máy tính và lập trình, lỗi Buffer overflow hay tiếng Việt gọi là lỗi tràn bộ nhớ đệm/lỗi tràn bộ đệm là khi mà bộ nhớ bị ghi đè nhiều lần trên ngăn xếp. Lỗi này thường xuyên xảy ra do người dùng gửi một lượng lớn dữ liệu tới server ứng dụng, điều này làm cho dữ liệu bị bắt phải đè lên các vị trí bộ nhớ liền kề đó. Đây là một lỗi lập trình có thể gây ra một ngoại lệ truy nhập bộ nhớ máy tính và chương trình bị kết thúc, hoặc khi người dùng cố tình phá hoại, có thể lợi dụng lỗi này để phá vỡ an ninh hệ thống.
## 2.1 Nguyên nhân gây ra lỗi Buffer Overflow.
- Không thực hiện đầy đủ, hoặc không kiểm tra biên. 
-Các ngôn ngữ lập trình như C, bản thân nó đã luôn có những tiềm ẩn các lỗ hổng mà hacker dễ dàng có thể tấn công vào. Trong ngôn ngữ lập trình C còn tồn tại các hàm không kiểm tra những buffer được cấp phát trên stack có kích thước lớn hơn dữ liệu được copy và bộ đệm hay không.
---
![image](https://user-images.githubusercontent.com/130078745/232111061-e5314845-55f2-401a-818e-c07db16e30b7.png)

- Các bạn có thể hiểu như hình trên.
---
## Ví dụ 1:
- Với 1 số bài tập cơ bản như sau với 1 biến **BUF** đc khai báo 16 byte.
![image](https://user-images.githubusercontent.com/130078745/232111802-59b4669d-21e9-44f4-afed-fd1b0b5d8e9f.png)
- nhưng phía dưới với dòng lệnh **READ** đọc tới tận 48 byte.
![image](https://user-images.githubusercontent.com/130078745/232112135-f9b32694-2c80-4934-b6a9-be56da8e9646.png)
- Khi chúng ta bắt đầu nhập quá lưu lượng 16 byte đc chỉ định thì , từ byte thứ 17 sẽ tràn xuống biến **v5**, và khi chúng nhập dữ liệu lớn đến 40 byte thì dữ liệu ấy sẽ tràn xuống **v7** ( với mỗi 1 biến 64bit sẽ bằng 8byte ).
![image](https://user-images.githubusercontent.com/130078745/232113296-d0799c94-c3ee-4a4d-80f6-4f6ed609ba08.png)
- Mục tiêu của chúng ta là thu thâpj thư mục shell do chương trình tạo ra với điều kiện **v5** **v6** và **v7** khác 0.
![image](https://user-images.githubusercontent.com/130078745/232115900-e8eefce2-f60a-4991-956d-fa9929145bd5.png)
---
- khi đã khởi tạo 1 file chạy bằng Python , gán 1 dữ liệu vào **BUF** và sau khi tràn biến ta in ra đc a ,b ,c tương ứng với v7 v6 và v5 như sau.
![image](https://user-images.githubusercontent.com/130078745/232143187-6d9946f0-16c1-424d-834b-f9d933cfcbd3.png)
### sau đó chúng ta cùng tìm hiểu chi tiết cách nó hoạt động:
+ ta sử dụng gdb va debug file để xem chi tiết cách nó diễn ra trong stack.
+ sử dụng lệnh ni để chay tới dòng read.
+ Sử dụng x/xg $rbp-0x20 để xem biến v5 ở địa chỉ nào.
+ ta có thể sử dụng **pattern** tạo 1 mẫu để xác định số lương byte từ chuỗi nhận vào đến biến. và cùng tạo **pattern create 0x30** sử dung **ni** để nhập dữ liệu.
![image](https://user-images.githubusercontent.com/130078745/232145634-23bfd3e2-33cd-40e6-9f42-d8308ec77777.png)
![image](https://user-images.githubusercontent.com/130078745/232145723-3495c5f7-29a4-4b62-8f20-a89d7893aeaf.png)
![image](https://user-images.githubusercontent.com/130078745/232146307-322783c9-5089-4074-8c8e-67c357c5371e.png)
---
## Ví dụ 2: 
- Sau đây là 1 bài cơ bản nữa sử dụng lỗi Buffer Overflow và Pwn tool để khai thác 1 chương trình.
- Mục tiêu của chúng ta trong bài này là thiết lập cho các biến 64bit trên ( a,b,c ) bằng với giá trị như trong đk.
![image](https://user-images.githubusercontent.com/130078745/232248965-76285792-2221-4b9a-8287-d9d11d473bd4.png)
- Nhưng các kí tự trên ko nhập đc , vì vậy chúng ta phải viết tool , và sử dụng 1 cái framework trong python đấy là pwntool.
- Để có thể giải bài tập trên chúng ta cần tạo 1 file python để tiến hành viết tool.
---
- Bước đầu ta cần phải thực hiện câu lệnh để có thể đọc file.
![image](https://user-images.githubusercontent.com/130078745/232249388-f28d02c8-6c5f-4a3d-b410-cd759bf2c553.png)
- Sau khi đọc file và nhập dữ liệu ta thu đc như hình.
![image](https://user-images.githubusercontent.com/130078745/232249461-00add3ce-c1df-45d2-a403-d8f1cc20066d.png)
- Xài tiếp gdp để có thể tìm offset từ biến **Buf** đến biến c.
- Chúng ta tìm đc địa chỉ của c nằm ở e050 bằng câu lệnh **x/xg $-0x20**.
- Như ta có thể thấy với địa chỉ e050 ta tìm thấy c ở offset 16 bằng câu lệnh **pattern search (địa chỉ biến )**.
![image](https://user-images.githubusercontent.com/130078745/232250041-eb3e0fc9-e25b-49b7-90d3-977987990176.png)
- Bằng cách hoàn thành tool python sau chúng ta đã hoàn thành bài trên là lấy đc shell.
![image](https://user-images.githubusercontent.com/130078745/232250245-38093347-ea47-48b6-b8de-4ec73b8e73e6.png)
![image](https://user-images.githubusercontent.com/130078745/232250257-c6b7e5a4-2571-4038-8afb-6a18feeb7fe2.png)
---
## 3.Các kiểu khai thác lỗi Buffer Overflow.
- Khai thác lỗi tràn bộ đệm trên stack.
- Khai thác lỗi tràn bộ đệm trên heap
- Một số cách khai thác khác như : dựa vào các lỗ hổng phần mềm thông qua ngôn ngữ lập trình và các trang web có tương tác người dùng nhưng không ràng buộc dữ liệu nhập như các trường hợp username, password,...
## 4.Biện pháp ngăn chặn.
- Việc xử lý bộ đệm trước khi đọc hay thực thi có thể làm thất bại các cuộc khai thác lỗi tràn bộ đệm nhưng vẫn không ngăn chặn được một cách tuyệt đối. Việc xử lý bao gồm:
- Chuyển từ chữ hoa thành chữ thường.
- Loại bỏ các ký tự đặc biệt và lọc các xâu không chứa kí tự là chữ số hoặc chữ cái. Ngoài ra, vẫn còn có các kỹ thuật để tránh việc lọc và xử lý này: +Alphanumeric - code: mã gồm toàn chữ và số.
- Polumorphic code: mã đa hình.
- Seft-modifying code: mã tự sửa đổi.
- Tấn công kiểu return – to – libc. Vì vậy, để tránh các nguy cơ bị khai thác lỗi buffer overflow chúng ta cần sử dụng các biện pháp phòng tránh hiệu quả hơn
- Lựa chọn ngôn ngữ lập trình: Ngôn ngữ lập trình có một ảnh hưởng lớn đối với sự xuất hiện lỗi tràn bộ đệm - Ngôn ngữ lập trình C và C++ là hai ngôn ngữ lập trình thông dụng, nhưng hạn chế của nó là không kiểm tra việc truy cập hoặc ghi đè dữ liệu thông qua các con trỏ. Cụ thể nó không kiểm tra dữ liệu copy vào một mảng có phù hợp với kích thước của mảng hay không. -Cyclone: một biến thể của C, giúp ngăn chặn các lỗi tràn bộ đệm bằng việc gắn thông tin về kích thước mảng với các mảng.
- Ngôn ngữ lập trình sử dụng nhiều kỹ thuật đa dạng để tránh gần hết việc sử dụng con trỏ và kiểm tra biên do người dùng xác định.
Nhiều ngôn ngữ lập trình khác cung cấp việc kiểm tra tại thời gian chạy. Việc kiểm tra này cung cấp một ngoại lệ hay một cảnh báo khi C hay C++ ghi đè dữ liệu ví dụ như Pythol, Ada, Lisp,...
- Ngoài ra, các môi trường Java hay .Net cũng đòi hỏi kiểm tra biên đối với tất cả các mảng.
- Sử dụng các thư viện an toàn: Sử dụng các thư viện được viết tốt và đã được kiểm thử dành cho các kiểu dữ liệu trừu tượng mà các thư viện này thực hiện tự động việc quản lý bộ nhớ, trong đó có kiểm tra biên có thể làm giảm sự xuất hiện và ảnh hưởng của các hiện tượng tràn bộ đệm. Các thư viện an toàn gồm có: The Better String Library, Arri Buffer API, Vstr.
- Chống tràn bộ đệm trên stack: Stack – smashing protection là kỹ thuật dùng để phát hiện các hiện tượng tràn bộ đệm phổ biến nhất. Kỹ thuật này kiểm tra xem stack đã bị sửa đổi hay chưa khi một hàm trả về. Nếu stack đã bị sửa đổi, chương trình kết thúc bằng một lỗi segmentation fault.Chế độ Data Execution Prevention (cấm thực thi dữ liệu) của Microsoft bảo vệ các con trỏ và không cho chúng bị ghi đè.Có thể bảo vệ stack bằng cách phân tán stack thành hai phần, một phần dành cho dữ liệu và một phần dành cho các bước trả về hàm, sự phân chia này được dùng trong ngôn ngữ Forth.một phần dành cho các bước trả về hàm, sự phân chia này được dùng trong ngôn ngữ Forth.
- Bảo vệ không gian thực thi: Kỹ thuật này ngăn chặn việc thực thi mã tại stack hay heap. Hacker có thể sử dụng tràn bộ đệm để chèn một đoạn mã tùy ý vào bộ nhớ của chương trình, với việc bảo vệ không gian thực thi thì mọi cố gắng chạy đoạn mã đó sẽ gây ra một ngoại lệ. Một số CPU hỗ trợ tính năng có tin bit NX (No eXecute) hoặc bit XD (eXecute Disable). Khi kết hợp với phần mềm các tính năng này có thể được dùng để đánh dấu những trang dữ liệu (chẳng hạn như các trang chứa stack và heap) là đọc được nhưng không thực hiện được. Ngẫu nhiên hóa sơ đồ không gian địa chỉ (Address space layout randomization ASLR): Một tính năng an ninh máy tính có liên quan đến việc sắp xếp các vùng dữ liệu quan trọng (thường bao gồm nơi chứa mã thực thi và vị trí các thư viện, heap và stack) một cách ngẫu nhiên trong không gian địa chỉ của một tiến trình.
- Kiểm tra sâu đối với gói tin: (deep packet inspection - DPI) có thể phát hiện việc cố gắng khai thác lỗi tràn bộ đệm từ xa ngay từ biên giới mạng. Các kỹ thuật này có khả năng ngăn chặn các gói tin có chứa chữ ký của một vụ tấn công đã biết hoặc chứa các chuỗi dài các lệnh No- Operation (NOP – lệnh rỗng không làm gì). Việc rà quét gói tin không phải một phương pháp hiệu quả vì nó chỉ có thể ngăn chặn các cuộc tấn công đã biết và có nhiều cách để mã hóa một lệnh NOP.








