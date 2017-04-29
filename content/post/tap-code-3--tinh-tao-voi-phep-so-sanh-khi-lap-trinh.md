+++
date = "2017-04-08T23:15:00"
draft = false
tags = ["tập code", "C", "Java", "practice code"]
title = "☘Tập Code 3: Tỉnh táo với phép so sánh khi lập trình"
summary = """
Một số bẫy chết người khi thực hiện các phép so sánh. Có thể bạn đã biết rồi.
"""
math = false
+++



![comparison - (c) Personal Trainer Land](/img/post_img/tap-code-03/comparison.jpg)
*Chú ý: Các vấn đề đang bàn luận chỉ xét trong phạm vi ngôn ngữ* `C`, `C++` *và* `Java`. *Các ngôn ngữ khác sẽ có những đặc trưng riêng có thể không giống với những gì bên dưới*

# 1. So sánh hai chuỗi
Và đây là lỗi mà đa số các bạn mới học `C` hoặc `Java` mắc phải:
```c
if (string1 == string2) {
  // làm gì đó
}
```
Cảm quan thì 'có vẻ' đúng lắm nhưng tiếc là không đúng với ý tưởng người tạo ra ngôn ngữ. Nguyên nhân sâu xa thì mình không biết nhưng mà chỉ nhớ phải theo các quy tắc của người ta.
Với C/C++:
```c
if ( strcmp(string1, string2) == 0 ) {
  // làm gì đó
}
```
Ở đây `strcmp` là viết tắt của `string comparison`, thuộc `string.h`, có chức năng so sánh hai chuỗi theo thứ tự chữ cái trong từ điển. Giá trị trả về `0` cho biết 2 chuỗi bằng nhau, giá trị trả về `âm` thì `string1` ở trước `string2`, giá trị trả về `dương` thì `string1` ở sau `string2`.

Còn với Java:
```c
if ( string1.equals(string2) ) {
  // làm gì đó
}
```
Với Java thì nó đơn giản dễ hiểu hơn nhiều, còn với C thì ta cần quay lại một chút. Xem ví dụ bên dưới:
```c
#include <string.h>
#include <stdio.h>

void so_sanh(const char* chuoi_1, const char* chuoi_2) {
    int gia_tri = strcmp(chuoi_1, chuoi_2);
    printf("gia_tri = %d \n", gia_tri);
    if (gia_tri == 0) {
      printf("'%s' BANG '%s' \n", chuoi_1, chuoi_2);
    } else if (gia_tri < 0) {
      printf("'%s' NHO HON '%s' \n", chuoi_1, chuoi_2);  
    } else if(gia_tri > 0) {
      printf("'%s' LON HON '%s' \n", chuoi_1, chuoi_2);
    }
    printf("-------------\n\n");
}

int main() {
    const char* chuoi = "Xin chao";
    so_sanh(chuoi, "Xin chao!"); // co them dau '!'
    so_sanh(chuoi, "Xin chao");
    so_sanh(chuoi, "Xin chao ban");
    so_sanh(chuoi, "Chao");
}
```
Kết quả là:
```sh
gia_tri = -33
'Xin chao' NHO HON 'Xin chao!'
-------------

gia_tri = 0
'Xin chao' BANG 'Xin chao'
-------------

gia_tri = -32
'Xin chao' NHO HON 'Xin chao ban'
-------------

gia_tri = 21
'Xin chao' LON HON 'Chao'
-------------
```
Rõ rồi nhưng mà cái `gia_tri` là cái gì mà mỗi lần so sánh nó lại ra một số khác nhau thế? Để kiểm tra lại nó cụ thể là cái gì thì có thể sửa lại trong hàm `so_sanh`:
```c
// ...
#include <stdlib.h>

void so_sanh(const char* chuoi_1, const char* chuoi_2) {
    int gia_tri = strcmp(chuoi_1, chuoi_2);
    printf("gia_tri = %d ---> '%c' \n", gia_tri, abs(gia_tri));
    //...
}
//...
```
Kết quả là:
```sh
gia_tri = -33 ---> '!'
'Xin chao' NHO HON 'Xin chao!'
-------------

gia_tri = 0 ---> ''
'Xin chao' BANG 'Xin chao'
-------------

gia_tri = -32 ---> ' '
'Xin chao' NHO HON 'Xin chao ban'
-------------

gia_tri = 21 ---> ''
'Xin chao' LON HON 'Chao'
-------------
```
Rõ rồi nhé, giá trị chính là số ~~thứ tự~~ chênh lệch của kí tự khác nhau đầu tiên trong bảng ASCII. 33 là kí tự chấm than trừ null, còn 32 là kí tự trắng trừ null. Còn dấu thì chỉ để xem cái nào nhỏ hơn hay lớn hơn mà thôi. Còn số 21 là kí tự 'X' - 'C', không in ra được nên sẽ có `''`.

{{% alert note %}}
Chú ý hàm strcmp() so sánh kiểu thứ tự từ điển, bảng alphabet của tiếng anh sẽ có khác một số ngôn ngữ khác, khi đó giải pháp thay thế là strcoll(), mình sẽ không giải thích ở đây.
{{% /alert %}}

# 2. So sánh hai số thực
Đa số các ngôn ngữ lập trình đều có 2 kiểu số thực là `float` và `double`. Nhiều khi bất cẩn ta có thể sẽ có những phép so sánh như sau:
```c
float a = 0.123;
double b = 0.123;
printf("A = %f \nB = %f\n", a, b);
if (a == b) {
  printf("BANG NHAU \n");
} else {
  printf("KHAC NHAU \n");
}
```
Thực tiễn hai số đó bằng nhau nhưng vào máy thì 🤣
```sh
A = 0.123000
B = 0.123000
KHAC NHAU
```
Code chạy ngon lành nhưng đôi khi không biết lỗi ở đâu ra, cái này thực sự rất nguy hiểm.

# 3. Thực hiện phép tính trong mệnh đề điều kiện
Cái này một số bạn vẫn sẽ mắc bẫy vì đôi khi muốn rút gọn phép tính vào ngay trong lúc so sánh. Ví dụ bài toán kiểm tra số nguyên `a` có gấp ba số `b` hay không?
```c
int a = 30, b = 10;
if (a / 3 == b) {
  printf("A gap ba B");
} else {
  printf("A khong gap ba B");
}
```
Kết quả:
```sh
A gap ba B
```
Cũng đoạn trên thay thành `a = 10, b = 3`, ta có kết quả mới:
```sh
A gap ba B
```
Vi diệu chưa? Sai tè le luôn. Bởi vì kết quả phép chia 2 số nguyên sẽ luôn là một số nguyên. Việc tính toán ở đây phải cẩn thận vì bên cạnh trường hợp làm tròn số thì còn lỗi tràn nữa:
```c
char a = 200;
if (a * 2 > 100) {
  printf ("2A > 100");
} else {
  printf ("2A <= 100");
}
```
Kết quả:
```sh
2A <= 100
```
Vâng, `400 <= 100` chỉ vì nó tràn kiểu dữ liệu `char` (tối đa 255 nên 2 * 200 = -112, và máy nó hiểu -112 <= 100 là đúng).

{{% alert warning %}}
Đặt phép tính vào lúc so sánh, là hành động bóp d*i thằng debugger, chiêu thức này ẩn chứa nguy hiểm tiềm tàng có thể dẫn đến vô sinh xin các bạn cẩn thận.
{{% /alert %}}

# 4. So sánh: x == 0 hay 0 == x?
Lúc đi học thì gần như 100% người học đều được dạy rằng muốn so sánh A với một hằng B thì cứ nhất nhất là bên trái là A, bên phải là B. Nhưng mà với kiểu dữ liệu tham chiếu, liên quan đến con trỏ và đối tượng thì đây mới là vấn đề.

> Bạn sẽ gặp NULL như chưa từng gặp với kiểu so sánh thứ nhất.

Ví dụ 1 đoạn code Java kiểm tra 1 thằng có học lớp "CNTT" (là một chuỗi hằng) hay không.
```java
class HocSinh {
	String hoTen;
	Lop lop;
}
class Lop {
	String tenLop;
	String giaoVienChuNhiem;
}

public class Main {
	public static void main(String[] args) {
		HocSinh baAnh = new HocSinh();
		baAnh.lop = new Lop(); // Quên gán tên lớp
		if (baAnh.lop.tenLop.equals("CNTT")) { // So sánh
			System.out.print("Học lớp CNTT");
		} else {
			System.out.print("Không học lớp CNTT");
		}
	}
}
```
Thử chạy phát:
```sh
Exception in thread "main" java.lang.NullPointerException
	at Main.main(Main.java:15)
```
Do `baAnh.lop.tenLop` chưa được khởi tạo nên dẽ dính `null` sẽ bị lỗi. Giải pháp nghĩ đến đầu tiên là kiểm tra khác null trước khi so sánh.
```java
if (baAnh.lop.tenLop != null && baAnh.lop.tenLop.equals("CNTT")) { ... }
```
Và nó đã chạy:
```sh
Không học lớp CNTT
```
May quá mới so sánh 1 thằng, lỡ so sánh vài thằng thì đúng là vêu mồm tập 2 😭, giải pháp cực kì đơn giản là đảo vị trí của 2 cái cho nhau, như thế này:
```java
if ("CNTT".equals(baAnh.lop.tenLop)) { ... }
```
Cho ra kết quả tương tự:
```sh
Không học lớp CNTT
```
Đơn giản phải không nào, né được biết bao nhiêu là phiền phức, tương tự như vậy cứ đảo hết giá trị hằng so sánh lên trước, biến hoặc đối tượng cần so sánh ra sau. Ok nhé 👍

Chúc bạn cuối tuần vui vẻ.


:four_leaf_clover: ngbaanh :four_leaf_clover:
- - -
