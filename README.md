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
## Ví dụ tiếp theo về Buffer Overflow.






