+++
date = "2017-04-06T20:42:00"
draft = false
tags = ["tập code", "C", "practice code"]
title = "☘Tập Code 2: Vòng lặp vô tận WHILE hay FOR?"
summary = """
Giữa while và for, cái nào thích hợp hơn để tạo một vòng lặp vô tận?
"""
math = false
+++

*Vòng lặp, là một trong những điều cơ bản nhất của một tư duy lập trình. Và 2 đại diện kinh điển cho nó chính là for và while. Vậy cụ thể chúng có gì khác nhau và cách tốt nhất để thiết lập một vòng lặp vô tận.*


# Vòng lặp là...

Khi phải thực hiện những công việc giống nhau, hoặc gần giống nhau, nôm na là lặp đi lặp lại một cái gì đó nhàm chán, thay vì phải sao chép một đống mã y hệt nhau để chia việc làm thì chúng ta cần một cấu trúc nào đó tinh gọn và hiệu quả.

`Cách mà một học sinh bình thường chép phạt`

> Em xin hứa sẽ không nói chuyện riêng trong lớp nữa.

> Em xin hứa sẽ không nói chuyện riêng trong lớp nữa.

> Em xin hứa sẽ không nói chuyện riêng trong lớp nữa.

> Em xin hứa sẽ không nói chuyện riêng trong lớp nữa.

> ... (994 lần)

> Em xin hứa sẽ không nói chuyện riêng trong lớp nữa.

> Em xin hứa sẽ không nói chuyện riêng trong lớp nữa.

`Cách mà lập trình viên chép phạt`

```c
for (int i = 0; i < 1000; i++) printf("Em xin hứa sẽ không nói chuyện riêng trong lớp nữa");
```

Vậy thì theo hiểu biết móp méo của mình, vòng lặp là `luồng điều khiển` cho phép các đoạn mã thực hiện lặp đi lặp lại. Thông thường các vòng lặp đều có 3 thành phần chính: `các giá trị khởi tạo`, `điều kiện duy trì vòng lặp`, `các mã lệnh cần lặp`. Vòng lặp sẽ dừng lại khi không thoả mãn điều kiện duy trì cho nó nữa.

Có nhiều cấu trúc để thể hiện vòng lặp như `for do`, `while do`, `repeat until`, `do while`,... Nhưng trong phạm vi bài viết này mình chỉ xét đến 2 cái đầu tiên.

# Vòng lặp với FOR

Hơi ngược thứ tự với tiêu đề chút, bởi vì ví dụ bên trên lỡ dùng FOR nên nói về nó trước.
Vòng lặp FOR là một vòng lặp cho phép người lập trình áp dụng đầy đủ các thành phần đã nêu bên trên. Ngoài ra nó còn có thêm 1 `biểu_thức` là nơi để tính toán sau khi mỗi vòng lặp hoàn thành.

Thông thường sẽ có cấu trúc (với các ngôn ngữ thuộc họ C)
```c
for (các_giá_trị_khởi_tạo; điều_kiện; biểu_thức) {
  mã_lệnh_cần_thực_thi;
}
```
Ngoài ra nó còn có 1 biến thể khác gọi là `for each`, dành cho các công việc như duyệt 1 chiều từ đầu đến cuối 1 chuỗi, `biểu_thức` không thực sự quá cần thiết.

Ví dụ: Cho x=0, y=0. Lặp 10 lần, cứ mỗi lần thì tăng x lên 2, y lên 3.
```c
int i, x, y, dx, dy;
for (i = 0, x = 0, y = 0, dx = 2, dy = 3; i < 10; i++) {
  x = x + dx;
  y = y + dy;
}
```

Và khi thiếu khuyết các yếu tố thì vòng for vẫn chạy bình thường. Đoạn mã sau tương đương đoạn bên trên.

```c
int i = 0, x = 0, y = 0, dx = 2, dy = 3;
for (; i < 10;) {
  x = x + dx;
  y = y + dy;
  i++;
}
```
Chú ý là có thể vứt gì đó cũng được nhưng mà không được vứt dấu `;` nhé.

# Vòng lặp với WHILE

Hơi khác một chút với for. Thông thường thì vòng lặp `for` sẽ biết trước được trước số lần lặp (mặc dù vẫn có vài trường hợp cá biệt ra), còn vòng lặp `while` chắc chắn sẽ không biết trước nó lặp bao nhiêu lần cả, và nó chỉ có 1 điều kiện đầu vào mà thôi, thiếu nó thì không được, nó sẽ lặp cho đến khi nào điều kiện không thoả mãn nữa thì dừng. `for` vẫn dễ tính hơn `while` nhỉ!

Cấu trúc:
```c
while (điều_kiện) {
  mã_lệnh_cần_thực_thi;
}
```

Lấy lại ví dụ bên trên, ta có cách biểu diễn với `while`:

```c
int i = 10, x = 0, y = 0, dx = 2, dy = 3;
while (i > 0) {
  x = x + dx;
  y = y + dy;
  i--;
}
```


![Infinite Loop - (c) John Robson](/img/post_img/tap-code-02/infinite-loop.jpg)
# Vòng lặp vô tận

Có những lúc chúng ta cần một đoạn mã chạy đi chạy lại mà không cần dừng (ví dụ cần duy trì kết nối chẳng hạn), thì chỉ cần làm cho vòng lặp luôn thoả mãn điều kiện lặp. Lúc này cách duy nhất để thoát ra từ bên trong là sử dụng từ khoá `break`

Cả `for` và `while` đều làm được điều này. Bài viết đang sử dụng ngôn ngữ C làm chuẩn, thì ta có như sau:
```c
for (;;) {
  mã_lệnh_cần_thực_thi;
}

while (1) {
  mã_lệnh_cần_thực_thi;
}
```
{{% alert warning %}}
Chú ý: Làm việc với thể loại này cần hết sức cẩn thận, bởi vì không có điều kiện cho nó dừng nên hoặc là nó sẽ tàn phá dữ liệu của bạn, hoặc là gây ngập bộ nhớ hoặc một số hậu quả khác.
{{% /alert %}}

Code 'thông minh':
```c
// ...
for(;;);
// ...
```
# Cái nào tốt hơn?

Mình dám cá là khoảng 80% coder sẽ chọn cách thứ 2 bên trên, tức là dùng `while`.
Lí do? Đơn giản là nhìn cách đầu tiên quá kì cục, không dễ hiểu bằng cách thứ 2.

> Mình cũng từng ở trong 80% này và bây giờ đã nhảy qua 20% còn lại.

Lí do? Là bởi vì cái cách mà trình biên dịch đối xử với mệnh đề điều kiện trong dấu `()`.
Và dễ dàng thấy đoạn code trên trong `C` thì đúng, nhưng trong `C++` và `Java` và một số ngôn ngữ khác nữa sẽ phải là `while(true)`

Trong khi đó nếu sử dụng `for(;;)` thì mọi chuyện sẽ đều êm đẹp, không cần phải phân vân nhiều.
Với lại `while(1)` sẽ giống `while(10)`, giả sử có một đoạn code `while(12345) { ... }` thì sao, vẫn đúng nhưng gây ức chế cho người nào bảo trì nó.

Cũng bởi vì giống nhau trong nhiều ngôn ngữ lập trình, nên `for(;;)` gần như biến thành một `từ khoá`. Như vậy code trong sẽ xinh xắn hơn nhiều.

Cá nhân mình, từng sử dụng while và bây giờ đã nhảy sang for. Còn bạn, bạn đã đổi qua chưa?

:four_leaf_clover: ngbaanh :four_leaf_clover:
- - -
