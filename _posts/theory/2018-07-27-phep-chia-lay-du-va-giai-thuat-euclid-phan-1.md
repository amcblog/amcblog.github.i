---
layout: single
category: theory
title: "Phép chia lấy dư và giải thuật Euclid (Phần 1)"
tags:
  - math
  - modulo
  - algorithm
  - euclid
excerpt: "Hiểu thêm về cơ sở toán học của giải thuật Euclid" 
teaser: /images/theory/amc-blog-theory-1.jpg
---

## 1.  Chút kiến thức căn bản cần chuẩn bị
#### 1.1)   Một vài định nghĩa cơ bản về số học
1. Khi ta nói rằng ```m``` chia hết cho ```n``` ( ```m```,``` n``` là các số nguyên) thì ta viết một cách ngắn gọn như sau: ```n|m```. Nghĩa là ```n.a = m``` (```a``` là số nguyên).
2. Một số nguyên ```p > 1``` là *số nguyên tố* nếu nó chỉ chia hết cho 1 và chính nó.
3. Một số nguyên ```q > 1``` nếu không phải là *số nguyên tố* thì được gọi là *hợp số*.  
#### 1.2) Định lý
Với a, b, c, x, y là các số nguyên, thì: 
1. Nếu ```a|b``` và ```b|a``` thì ```a = b``` và ```a = -b```.
2. Nếu ```a|b``` và ```b|c``` thì ```a|c```.
3. Nếu ```c|a``` và ```c|b``` thì ```c|(a.x + b.y)```.
Việc chứng minh các định lý trên không quá phức tạp, mình sẽ nhường công việc thú vị này cho bạn đọc. Nếu bạn có thắc mắc gì hãy thoải mái comment xuống bên dưới bài viết nhé! :D

Mình bonus 2 vấn đề nho nhỏ sau với bạn đọc hiếu kỳ :3
 1) Hãy chứng minh nếu một số nguyên```n``` là một số lẻ thì ```n^2 - 1``` luôn chia hết cho 8.
 2) Liệu rằng ```n^2 + n + 17``` là *số nguyên tố* với mọi số nguyên ```n > 1``` ?
### 1.3) Một vài tính chất của phép chia lấy dư
  Phép chia lấy dư có lẽ đã rất quen thuộc với nhiều người, chúng ta đã được học nó từ cấp 1, ví dụ như 9 chia 8 dư 1.
  Người ta ký hiệu phép chia lấy dư là ```mod```, trong nhiều ngôn ngữ lập trình chúng ta có ký hiệu ```%``` với ý nghĩa tương đương.
  Ví dụ: ```9 mod 8 = 1``` hay ```9%8 = 1```
#### 1.3.1) Định nghĩa
Với ```n``` và ```m (m > 0)``` là các số nguyên, ta định nghĩa phép chia lấy dư như sau:

```
n mod  m = n - [n/m].m
```
Trong đó ```[n/m]``` là phép chia lấy phần nguyên. Ví dụ, ```9/8 = 1.125``` nên  ```[9/8] = 1```.
Bạn có thể thử vài số bất kỳ để kiểm chứng định nghĩa trên. :D
Có nhiều ứng dụng khá hay của phép chia lấy dư nhưng mình sẽ không đề cập ở bài viết này để tránh lan man.
### 1.4)  Ước chung lớn nhất (greatest common divisor)
#### 1.4.1) Định nghĩa 
1. Một số nguyên ```d``` được gọi là *ước chung* của ```a``` và ```b``` nếu ```a``` và ```b``` cùng chia hết cho ```d```, hay nói cách khác, khi ```d|a``` và ```d|b```.
2. *Ước chung lớn nhất* của ```a``` và ```b```, hai số nguyên khác 0, được định nghĩa là số nguyên ```d``` lớn nhất trong các *ước chung* của ```a``` và ```b```. Ta ký hiệu là như sau:

```
d = gcd(a, b)
```

#### 1.4.2) Định lý
1. Nếu ```a``` và ```b``` là các số nguyên thì ```gcd(a,b) = gcd(|a|,|b|)```.
2. Nếu ```a``` và ```b``` là các số nguyên thì ```gcd(a,b) = gcd(b,a)```.
3. Nếu ```a```, ```b``` và ```k``` là các số nguyên thì ```gcd(a,b) = gcd(a + kb,b)```

Định lý 1 và 2 mình sẽ để cho bạn đọc tự chứng minh, mình chỉ chứng minh định lý 3, đây cũng chính là định lý nền tảng cho giải thuật Euclid. ;-)
##### Chứng minh Định lý 3:
Ta cần chứng minh rằng *ước chung* của ```a``` và ```b``` giống với *ước chung* của ```a + kb``` và ```b```.
Giả sử rằng ```d|a``` và ```d|b``` thì ```a = x.d``` và ```b = y.d``` (```x```, ```y```  là các số nguyên). Khi đó ta có:

```
a + k.b = x.d + k.y.d = (x + k.y).d	
```

Từ đó suy ra ```d``` là ước của ```a + k.b```. Vậy ```d``` là *ước chung* của ```a + k.b``` và ```b```.
Ngược lại, ta giả sử  ``c`` là *ước chung* của ```a + k.b``` và ```b``` thì  ```a + k.b = x.c``` và ```b = y.c``` với ```x```,```y``` là các số nguyên. Khi đó ta có:

```
a = x.c - k.b = x.c - k.y.c = (x - k.y).c
```

Từ đó suy ra ```c``` là *ước chung* của ```a``` và ```b```.

Từ hai chứng minh trên ta suy ra tập *ước chung* của ```(a,b)``` giống với tập *ước chung* của ```(a + kb,b)```. Do vậy ```gcd(a,b) = gcd(a + kb,b)```.

Từ định lý 3 ta rút ra 1 hệ quả quan trọng là nền tảng cho giải thuật Euclid.
#### 1.4.3) Hệ quả Định lý 3

##### Hệ quả:
Với ```a``` và ```b``` là các số nguyên, ```b > 0``` thì ```gcd(a,b) = gcd(a mod b,b)```
##### Chứng minh:
Nhớ lại chút về định nghĩa của phép chia lấy dư: ```a mod b = a - [a/b].b```, suy ra ```a mod b = a + k.b``` với ```k = -[a/b]```. Do đó hệ quả là đúng.
##### Áp dụng hệ quả:
Bây giờ chúng ta sẽ sử dụng **hệ quả** trên kết hợp với **Định lý 2** mục **1.4.2)** để tính *ước chung lớn nhất* của 2 số nguyên bất kỳ. Ví dụ, ta muốn tính ```gcd(64,24)```, quy trình như sau:

```
gcd(64,24) = gcd(64 mod 24, 24)
	   = gcd(16,24)
	   = gcd(24 mod 16, 16)
     	   = gcd(8, 16)
           = gcd(16 mod 8, 8)
           = gcd(0, 8)
           = 8
```

Như vậy ```gcd(64,24) = 8```. Bạn có thể thử với các số khác nếu muốn. :)
## 2. Giải thuật Euclid
Thực ra khi thực hiện ví dụ trên là bạn đang chạy giải thuật Euclid rồi đấy. :))

Giải thuật Euclid thực chất là giải thuật giúp bạn tính được *ước chung lớn nhất* của 2 số nguyên bất kỳ, dựa trên cơ sở toán học mà chúng ta đã đề cập và chứng minh ở trên.
Có thể bạn đang cảm thấy giải thuật này cũng chẳng giúp ích được gì lắm đúng không ? Điểm thú vị của giải thuật Euclid ở chỗ nó giúp chúng thực hiện việc tính ```gcd()``` với các số lớn khá nhanh. Bài viết sau mình sẽ demo việc này cũng như thử xem xét độ phức tạp của giải thuật này.

Bây giờ chúng ta sẽ implement giải thuật Euclid bằng code C++ nhé. Ta làm đúng theo các bước như khi ta thực hiện ví dụ trước thôi.
{% highlight cpp linenos %}
int gcd(int a, int b) {
   int c = b;
   a = abs(a);
   b = abs(b); 
   while(b > 0) {
      c = b;
      b = a%b;
      a = c;
   }
   return a;
}
{% endhighlight %}
  Kiến thức có vẻ nhiều mà khi code có mấy dòng nhỉ. :))
  
Ta nhận thấy ở đây có sự lặp lại của chính hàm ```gcd()```, do vậy ta có thể đệ quy đến khi nào một trong hai số ```a``` hoặc ```b``` bằng 0. Sau đây là implement giải thuật Euclid theo kiểu đệ quy.
{% highlight cpp linenos %}
int gcdRecursion(int a, int b) {
   int c; 
   a = abs(a);
   b = abs(b);
	if(b > 0) {
       c = gcdRecursion(b,a%b);
    }
   else c = a;
   return c;
}
{% endhighlight %}
Mình tạm kết phần 1 tại đây, trong bài viết sau mình sẽ phân tích độ phức tạp của giải thuật Euclid và đề cập giải thuật Euclid mở rộng.
Bạn đọc nếu có gì muốn góp ý gì về bài viết thoải mái *comment* và nếu cảm thấy bài viết bổ ích thì hãy *share* để bạn bè cùng đọc nhé! 

Chúc bạn một ngày tốt lành ! :D 
