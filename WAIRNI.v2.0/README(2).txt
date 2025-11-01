Hung_WAIRNI: Hưng Wave Analysis, Hung's Interpolation Rule and Đ.Đ.P.Hưng Numerical Integration.

Creator : Đặng Đình Phú Hưng (Dang Dinh Phu Hung, https://github.com/nahhididwin)
Date of birth (Dang Dinh Phu Hung) (DD/MM/YYYY): 20/06/2011
Languages ​​used: Vietnamese, English.
Publication date (DD/MM/YYYY): not this time

--
Các Phần Chính :
1. Vấn đề "chia", bất đối xứng, và kiểu dữ liệu liên quan
2. Khả năng của đạo hàm đối với việc tìm đỉnh với dạng dữ liệu liên quan.
3. AUC (Area under the Curve) bằng Hung_WAIRNI
4. Bổ sung thêm biên độ, cực trị
--

--
1. Vấn đề "chia", bất đối xứng, và kiểu dữ liệu liên quan:


Trước hết bạn cần biết như này (biết rồi thì thôi) :
Quá trình chuyển đổi tín hiệu liên tục (sóng từ môi trường) thành tín hiệu số trên máy tính được gọi là số hóa (digitization), bao gồm hai bước chính: 
Lấy mẫu (Sampling) và Lượng tử hóa (Quantization).
Khoảng cách đều nhau: Trong hầu hết các ứng dụng phổ biến (như xử lý âm thanh, hình ảnh, video, tín hiệu vô tuyến thông thường, đo lường cảm biến...),
các thiết bị chuyển đổi tương tự-số (ADC - Analog-to-Digital Converter) sẽ thực hiện lấy mẫu tín hiệu tại các thời điểm cách nhau một khoảng thời gian cố định,
gọi là chu kỳ lấy mẫu (T_s).
Tính toán và Khôi phục dễ dàng: Việc lấy mẫu cách đều nhau giúp cho việc xử lý tín hiệu số (DSP - Digital Signal Processing) sau này trở nên đơn giản và hiệu quả hơn rất nhiều.
Quan trọng hơn, nó cho phép khôi phục lại tín hiệu tương tự gốc một cách chính xác nhất có thể (trong giới hạn của Định lý Nyquist–Shannon)
nếu tần số lấy mẫu (F_s = 1/T_s) đủ lớn.


Định lý Nyquist–Shannon
trên wikipedia (một định lý được sử dụng trong lĩnh vực lý thuyết thông tin):
https://vi.wikipedia.org/wiki/%C4%90%E1%BB%8Bnh_l%C3%BD_l%E1%BA%A5y_m%E1%BA%ABu_Nyquist%E2%80%93Shannon

Và vì dữ liệu đa phần (gần như chắc chắn) đều là "bất đối xứng", và vô cùng nhiều vị trí "lấy mẫu",
hãy xem hình ảnh này (p1.png) như 1 ví dụ : 
https://github.com/nahhididwin/Hung_WAIRNI/blob/main/WAIRNI.v2.0/p1.png
Tôi đã lấy hình ảnh đó tại đây : https://en.wikipedia.org/wiki/Pan%E2%80%93Tompkins_algorithm


Và hãy xem ảnh (p2.png) này (Do tôi tự làm, nên nguồn sẽ là ở ngay đây) : https://github.com/nahhididwin/Hung_WAIRNI/blob/main/WAIRNI.v2.0/p2.png
NÓ LẤY MẪU ĐẾN TẬN 73 CÁI CHO TAM GIÁC ABC (ý tôi là đường tuyến tính ABC, còn cái gạch đen ở dưới là trục x thôi)!!!!
Vầng, biết vì sao lại như thế không, tại vì tọa độ của điểm C "không được đẹp" (thế nào là không được đẹp, tôi cảm thấy việc
bản thân mình cứ kiểu sử dụng từ ngữ không khoa học mà thường ngày như này không ổn cho lắm, tôi đoán mình sẽ sửa sau, nhưng
ý tôi là kiểu như này là đẹp nè x = 3, còn này là xấu nè 3.59, còn này là ối dồi ôi siêu xấu nè : 3.86818618618618618618618).
Cùng nhau tưởng tượng nhé? nếu như có 1 đồ thị có 2 trục x,y. Và ông cũng biết mà, trên máy tính thì sẽ không có đường tuyến tính
hay là phi tuyến tính gì cả, mà nó sẽ chỉ có các điểm ("mẫu") mà tôi đã nhắc đến ở bên trên kia, để mô phỏng cho mấy cái đường đó.
Về cơ bản nó trông như này : Điểm 1 (x:1;y:1); Điểm 2 (x:2;y:2). Vầng đó là 1 đường tuyến tính =))), vì chúng ta sẽ nối 2 điểm đó
lại với nhau. Còn đường phi tuyến, giả sử như đường cong như ở trong đồ thị sin đi nhé? Nó thực chất cũng là kiểu nhiều đường thẳng
nối lại với nhau để giả cong mà thôi.
Vậy điều này có ý nghĩa gì?
Giả sử có đường tuyến tính AB nằm trong 1 signal, được tạo ra bởi 2 điểm AB nối nhau. Và tọa độ điểm A (x:1;y:1); tọa độ điểm B (x:2;y:2).
Bro sẽ chỉ cần 2 điểm đo đạc cách đều nhau 1 đơn vị (có thể là giây, phút hay cái gì đó, nhưng chúng ta sẽ gọi nó là unit nhé).

Nhưng đời không như mơ khi dữ liệu thực tế gần như không bao giờ đẹp đẽ như thế, tưởng tượng thử nhé, nhịp tim của ông liệu có đập đúng vào
lúc 1 giây sau khi bắt đầu đo rồi đúng 1 giây sau đập tiếp rồi cứ như vậy đến vô hạn không, dĩ nhiên là không =)). 
Mà nó sẽ kiểu đập lúc đầu tiên vào 0.59 giây sau khi bắt đầu đo, rồi sau khoảng 1.255 giây gì đó lại đập thêm phát nữa, rồi sau khoảng 0.85 giây lại đập
tiếp 1 phát nữa. Vầng thật ra nó cũng cho vui thôi chứ thực tế khả năng cũng chả phải 0.85 giây đâu, mà ở ngoài thực tế thì khả năng lại là 0.85861681686186181863861836816813863168618681368.... cũng không chừng, nhưng tôi đoán người ta sẽ không đo kỹ như vậy, tầm 0.85 hay 0.858 hoặc nhiều hơn 1 khoảng thôi.

Cho nên nếu như điểm "đo đạc/lấy mẫu" mà ở vị trí trên trục x kiểu nó có những chữ số sau dấu phẩy ",", thì nó sẽ thường sẽ tăng đáng kể số điểm đo đạc, ờ thật ra
điều này là hiển nhiên.

Vậy cùng nhau quay lại với Tích phân số cơ bản nha, tôi sẽ không nói đến Riemann Sum Hình chữ nhật vì nó thường không ngon bằng Trapezoidal Rule =)).
Đọc wiki : https://en.wikipedia.org/wiki/Trapezoidal_rule
Như chúng ta thấy, nó tính diện tích dưới "hình thang" hoặc hiếm lắm thì "hình tam giác" của các điểm (đo đạc/lấy mẫu). Haha các ông thấy gì chưa,
vì tọa độ của các điểm "đo đạc/lấy mẫu" không đẹp tý nào nên là nó sẽ tăng số lượng các điểm này lên kinh dị.
