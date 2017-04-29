+++
date = "2017-03-19T11:34:00"
draft = false
tags = ["tập code", "C", "practice code"]
title = "☘Tập Code: Viết hàm xếp loại học sinh"
summary = """
Viết một hàm đánh giá học lực học sinh dựa trên điểm thi một số môn.
"""
math = false
+++

# Đặt vấn đề

*Hôm nay cuối tuần rảnh rỗi nên viết bài đầu tiên về Series Tập Code, đến bây giờ cũng code được một vài năm bây giờ nhớ lại những bài toán năm xưa tuy đơn giản nhưng làm mình bối rối khôn cùng.*

Cùng xem chi tiết bài toán nhé.

Điều kiện đánh giá và xếp loại học sinh như sau:

> - Học sinh thi 3 môn Toán, Lý, Hoá được đánh giá `học lực` trên thang điểm `10`. Thang xếp loại học lực từ cao đến thấp thứ tự như sau: `Giỏi, Khá, Trung Bình, Yếu`.

> - Học sinh cũng được đánh giá `rèn luyện` trên thang điểm `10`. Thang xếp loại tương tự thang xếp loại học lực: `Tốt, Khá, Trung Bình, Yếu`. Xếp loại phải tương ứng với năng lực.

> - Điểm trung bình chung nhỏ hơn 5.0 thì `xếp loại ban đầu` *Yếu*.

> - Điểm trung bình chung từ 5.0 đến dưới 6.5 thì xếp loại ban đầu *Trung Bình*.

> - Điểm trung bình chung từ 6.5 đến dưới 8.0 thì xếp loại ban đầu *Khá*.

> - Điểm trung bình chung từ 8.0 trở lên thì xếp loại ban đầu *Giỏi*.

> - Học sinh `xếp loại ban đầu` *Khá, Giỏi* chỉ khi có điểm rèn luyện tương đương và điểm thành phần 3 môn có xếp loại tương ứng từ *Trung Bình, Khá* trở lên. Nếu không thì giảm một bậc.

> - Trong các điểm thành phần, nếu có bất kì điểm nào nhỏ hơn 3.0, thì lập tức `xếp loại` Yếu.


 `Yêu cầu`

>Viết một hàm đánh giá bằng ngôn ngữ C, đầu vào điểm thi 3 môn và điểm rèn luyện, đầu ra là một chuỗi đánh giá xếp loại thuộc bộ `{4, 3, 2, 1, 0}` tương ứng thang xếp loại trong đề bài và `0` là không đánh giá được.

Bài toán ai cũng biết, đã làm lập trình tất cả đều đã làm qua nhưng mà mình vẫn thấy nó có ích nên viết lại. Để ngắn gọn thì những điểm cần lưu ý mình đã đánh dấu nổi bật ngay trong đề bài bên trên. Lưu ý ngôn ngữ C ở đây  là `Standard C` chứ không phải C++ hoặc C# nhé (vì lúc viết bài này máy mình chỉ có C thôi).

Sau khi đọc xong đề bài thì lập tức tất cả đều có thể hình dung ra nguyên mẫu hàm như sau, dòm qua để nhảy sang phần sau nhé.

```C
int danh_gia(float toan, float ly, float hoa, float rl);
```

# Chuẩn bị những gì để bắt đầu
*Với những bạn sử dụng Windows thì có thể dùng bất kì cái gì đó code C được như DevCpp, hoặc CodeBlock hay gì gì đó khác là ok rồi. Mình chia tay Windows rồi thì sử dụng Terminal thay thế vậy, máy mình cũng chẳng cài gì nên chỉ có ngôn ngữ C là làm được trực tiếp mà thôi.*

Trước tiên chúng ta di chuyển đến đâu đó trong máy tính, tạo một thư mục để chứa tệp mã nguồn. Ở đây mình sẽ tạo ngay giữa `Desktop` một thư mục `tap-code`, sau đó tạo tệp `main.c`:

```sh
$ cd ~/Desktop/
$ mkdir tap-code && cd tap-code
$ touch main.c
```

Mở nó ra và chiến thôi, tuỳ các bạn muốn mở nó trong IDE hay Notepad, còn nếu ai đó nhác như mình thì xử lí ngay tại cái Terminal với `nano`:
```sh
$ nano main.c
```

[![Hello world](/img/post_img/tap-code-01/Helloworld.gif)](/img/post_img/tap-code-01/Helloworld.mp4 "Click to play HD quality")
Phải kiểm tra mọi thứ thực sự ổn trước khi bắt đầu. :joy_cat:

{{% alert warning %}}
Hồi xưa học cấp 2 lúc đang làm thi thực hành Pascal, vừa đọc đề xong lao vào code no xôi chán chè thì build không được, nguyên nhân là do máy bị xì ke từ sớm, lúc chuyển sang máy khác thì không kịp nữa rồi.
{{% /alert %}}
{{% alert note %}}
Trước khi bắt tay làm gì phải đảm bảo HelloWorld chạy được, mất có 16 giây thôi các bạn ạ.
{{% /alert %}}


# Quay lại giải quyết bài toán

*Có thể thấy là bài toán này có quá nhiều điều kiện lằng nhằng với nhau, nhưng chúng ta hãy khoan code ngay mà đánh giá nó trước. Phải tập trung xử lí những điều kiện mang tính quyết định trước.*

## 1. Đầu tiên là hợp lệ dữ liệu
Chúng ta phải đảm bảo rằng người ta nhập điểm từ 0.0 ~ 10.0, đầu vào sai mà còn nhây lì tính toán là không được. Cho nên nếu dữ liệu sai thì phải trả về `0` ngay lập tức.  :smiling_imp:
```c
int danh_gia(float toan, float ly, float hoa, float rl) {
  if (toan < 0 || ly < 0 || hoa < 0 || rl < 0 || toan > 10 || ly > 10 || hoa > 10 || rl > 10) {
    return 0; // --KHÔNG ĐÁNH GIÁ ĐƯỢC
  }
  // ... CODE NẾU HỢP LỆ
}
```
Sau câu lệnh `return` không một dòng code nào được thực thi nữa, cho nên lúc này ta có thể viết tiếp mà không cần cụm `else { ... }` nữa. Viết xuống bên dưới.

## 2. Điều kiện loại bỏ, những sự thật hiển nhiên
> ... Xếp loại phải tương ứng với năng lực.
> Trong các điểm thành phần, nếu có bất kì điểm nào nhỏ hơn 3.0, thì lập tức xếp loại Yếu.

Đây là trường hợp thấy xếp loại ngay mà không cần tính toán, :relieved: viết tiếp xuống bên dưới:

```c
int danh_gia(float toan, float ly, float hoa, float rl) {
  if (toan < 0 || ly < 0 || hoa < 0 || rl < 0 || toan > 10 || ly > 10 || hoa > 10 || rl > 10) {
    return 0; // --KHÔNG ĐÁNH GIÁ ĐƯỢC
  }
  if (toan < 3 || ly < 3 || hoa < 3 || rl < 5) { return 1; } // --YẾU
  // ... CODE NẾU CẦN TÍNH TOÁN
}
```

## 3. Xác định xếp loại ban đầu
Cần tính điểm trung bình chung của 3 môn học mới xét tiếp được.
```c
int danh_gia(float toan, float ly, float hoa, float rl) { // [ ... ]
  // ... CODE NẾU CẦN TÍNH TOÁN
  float tb = (toan + ly + hoa) / 3;
  int xep_loai = tb < 5.0 ? 1 : (tb < 6.5 ? 2 : (tb < 8.0 ? 3 : 4));
  // ... CODE SÀNG LỌC
}
```
Đoạn code tính xếp loại ban đầu như trên sử dụng toán tử điều kiện `?:` cho ngắn gọn bởi vì mỗi nhánh rẽ ra nó chỉ thực hiện 1 công việc duy nhất là gán dữ liệu. Diễn giải ra thì như thế này, quá dài phải không:
```c
int xep_loai;
if (tb < 5.0) { xep_loai = 1; }      // --YẾU
else if (tb < 6.5) { xep_loai = 2; } // --TRUNG BÌNH
else if (tb < 8.0) { xep_loai = 3; } // --KHÁ
else { xep_loai = 4; }               // --GIỎI
```

## 4. Sàng lọc xếp loại
Tiêu chuẩn sàng lọc ở đây chỉ áp dụng cho học sinh *Khá* trở lên, gọi là thu gọn nhuệ khí của các anh ấy bớt, để hoà đồng hơn với những đứa học lực trung bình như mình.

> Đùa đấy :joy:

Đơn giản là xét cái `xep_loai` đã tính được ở ban nãy:
```c
int danh_gia(float toan, float ly, float hoa, float rl) { // [ ... ]
  // ... CODE SÀNG LỌC
  if (xep_loai == 3) { // --NẾU ANH ẤY TIẾN BỘ?
    if (toan < 5.0 || ly < 5.0 || hoa < 5.0 || rl < 6.5) { // --MỐC HẠ KHÁ->TB
      xep_loai--;
    }
  }
  if (xep_loai == 4) { // --NẾU ANH ẤY TIẾN BỘ HƠN?
    if (toan < 6.5 || ly < 6.5 || hoa < 6.5 || rl < 8.0) { // --MỐC HẠ GIỎI->KHÁ
      xep_loai--;
    }
  }
  // ...
}
```
Bạn có thấy 4 lệnh if chồng nhau hơi vô duyên không :smile: rút nó ngắn bớt lại:
```c
int danh_gia(float toan, float ly, float hoa, float rl) { // [ ... ]
  // ... CODE SÀNG LỌC
  if (xep_loai == 3 && (toan < 5.0 || ly < 5.0 || hoa < 5.0 || rl < 6.5)) {
    xep_loai--;
  }
  if (xep_loai == 4 && (toan < 6.5 || ly < 6.5 || hoa < 6.5 || rl < 8.0)) {
    xep_loai--;
  }
  return xep_loai;
}
```
Cuối cùng phải trả về kết quả xếp loại, thêm câu lệnh `return` vào cuối hàm.

## 5. Xây dựng bộ thử
Để biết cái mình code có chạy đúng hay không thì cần xây dựng một vài trường hợp thử để kiểm tra.
Giả sử ta biết chắc một vài trường hợp thì có thể chọn ra để thử. Ví dụ bảng sau:

![Test cases](/img/post_img/tap-code-01/002.png)
Dựa vào thông tin trên ta thiết kế một hàm `kiem_tra()` để thử xem hàm `danh_gia()` có hoạt động đúng như mong đợi hay không.
```c
void kiem_tra() {
  const int dong = 9, cot = 5;
  // -- Dữ liệu: { Toán; Lý; Hoá; Rèn Luyện; Kết Quả Mong Đợi },
  float data[dong][cot] = {
    { 11 , 9.0, 9.0, 9.0, 0 },
    { 9.0, 9.0, 9.0, 9.0, 4 },
    { 7.0, 7.0, 7.0, 7.0, 3 },
    { 6.0, 6.0, 6.0, 6.0, 2 },
    { 3.5, 4.0, 6.0, 6.5, 1 },

    { 2.5, 9.0, 8.0, 6.0, 1 },
    { 6.0, 9.0, 9.0, 8.0, 3 },
    { 7.0, 8.0, 9.0, 4.5, 1 },
    { 4.0, 9.0, 8.0, 6.0, 2 }
  };
  int i, ketqua;
  printf("==== BAT DAU KIEM TRA =====\n");
  for (i = 0; i < dong; i++) {
    ketqua = danh_gia(data[i][0], data[i][1], data[i][2], data[i][3]);
    printf("DanhGia(%2.1f, %2.1f, %2.1f, %2.1f) = %i | %i | %10s.\n",
      data[i][0], data[i][1], data[i][2], data[i][3], ketqua,
      (int)data[i][4],
      ((int)data[i][4] == ketqua ? "[o]" : ">> [x]")
    );
  }
  printf("======== KET THUC =========\n");
}
```
{{% alert note %}}
Cái mảng 2 chiều đó nên là nạp dữ liệu từ file để tiện đường chỉnh sửa sau này, phạm vi bài viết mình đặt luôn vào đó cho gọn.
{{% /alert %}}

## 6. Tận hưởng thành quả
Chúng ta cần phải viết một chương trình hoàn chỉnh để kiểm tra:
```c
#include <stdio.h>

int danh_gia(float, float, float, float);
void kiem_tra();

int main() {
  //printf("Hello world \n");
  kiem_tra();
  return 0;
}

int danh_gia(float toan, float ly, float hoa, float rl) {
  if (toan < 0 || ly < 0 || hoa < 0 || rl < 0 || toan > 10 || ly > 10 || hoa > 10 || rl > 10) { return 0; }
  if (toan < 3 || ly < 3 || hoa < 3 || rl < 5) { return 1; } // --YẾU
  float tb = (toan + ly + hoa) / 3;
  int xep_loai = tb < 5.0 ? 1 : (tb < 6.5 ? 2 : (tb < 8.0 ? 3 : 4));
  if (xep_loai == 3 && (toan < 5.0 || ly < 5.0 || hoa < 5.0 || rl < 6.5)) {
    xep_loai--; // --HẠ KHÁ > TB
  }
  if (xep_loai == 4 && (toan < 6.5 || ly < 6.5 || hoa < 6.5 || rl < 8.0)) {
    xep_loai--; // --HẠ GIỎI > KHÁ
  }
  return xep_loai;
}

void kiem_tra() {
  const int dong = 9, cot = 5;
  // -- Dữ liệu: { Toán; Lý; Hoá; Rèn Luyện; Kết Quả Mong Đợi },
  float data[dong][cot] = {
    { 11 , 9.0, 9.0, 9.0, 0 },
    { 9.0, 9.0, 9.0, 9.0, 4 },
    { 7.0, 7.0, 7.0, 7.0, 3 },
    { 6.0, 6.0, 6.0, 6.0, 2 },
    { 3.5, 4.0, 6.0, 6.5, 1 },

    { 2.5, 9.0, 8.0, 6.0, 1 },
    { 6.0, 9.0, 9.0, 8.0, 3 },
    { 7.0, 8.0, 9.0, 4.5, 1 },
    { 4.0, 9.0, 8.0, 6.0, 2 }
  };
  int i, ketqua;
  printf("==== BAT DAU KIEM TRA =====\n");
  for (i = 0; i < dong; i++) {
    ketqua = danh_gia(data[i][0], data[i][1], data[i][2], data[i][3]);
    printf("DanhGia(%2.1f, %2.1f, %2.1f, %2.1f) = %i | %i | %10s.\n",
      data[i][0], data[i][1], data[i][2], data[i][3], ketqua,
      (int)data[i][4],
      ((int)data[i][4] == ketqua ? "[o]" : ">> [x]")
    );
  }
  printf("======== KET THUC =========\n");
}
```

Thực hiện biên dịch và chạy thử chương trình, nếu bạn đang dùng terminal như mình:
```sh
$ gcc -o main main.c && ./main
```
Thì đây là kết quả: :sunglasses:
![Tested cases](/img/post_img/tap-code-01/003.png)

Cố tình phá hỏng dữ liệu trường hợp thử thứ 6 thành: `{ 2.5, 9.0, 8.0, 6.0, 2 },` và kết quả mới sẽ là...
![Tested cases failed](/img/post_img/tap-code-01/004.png)

:ok_hand: Quá hay phải không, nếu mỗi lần sửa đổi hàm `danh_gia()` (có thể sẽ dẫn đến sai, mà đa phần là không biết sai ở đâu) thì chỉ cần gọi hàm `kiem_tra()` một lần, ta sẽ biết được trường hợp nào sai, từ đó dễ dàng khoanh vùng để tìm lỗi hơn là mỗi kiểm tra phải nhập đi nhập lại.


# Kết luận
1. Lời chào cao hơn mâm cỗ, chạy Hello world trước khi bắt đầu công việc.
2. Kiểm tra dữ liệu có hợp lệ hay không trước khi xử lí dữ liệu.
3. Thiết kế bộ thử song song việc phát triển một chức năng nào đó.

*Chúc các bạn code tốt và code vui, hôm nay trời có nhiều mây* :cloud: *mình lại hết xèng* :money_with_wings: *nhịn đói* :crying_cat_face: *viết bài lởm dởm chắc ai đó sẽ sai sót, mong được góp ý. Cảm ơn bạn đã nghé qua.*

:four_leaf_clover: ngbaanh :four_leaf_clover:
- - -
