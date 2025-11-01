Hung_WAIRNI: Hưng Wave Analysis, Hung's Interpolation Rule and Đ.Đ.P.Hưng Numerical Integration.

Creator : Đặng Đình Phú Hưng (Dang Dinh Phu Hung, https://github.com/nahhididwin)
Date of birth (Dang Dinh Phu Hung) (DD/MM/YYYY): 20/06/2011
Languages ​​used: Vietnamese, English.
Publication date (DD/MM/YYYY): not this time

--
Các Phần Chính :
1. Vấn đề "chia", bất đối xứng, và kiểu dữ liệu liên quan
2. Khả năng của đạo hàm đối với việc tìm đỉnh với dạng dữ liệu liên quan, sử dụng phương pháp Hung_WAIRNI
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


Và hãy xem ảnh (p2.png) này (Do tôi tự làm, nên nguồn sẽ là ở ngay đây, à thật ra
thì còn nữa nhưng những ảnh tôi lấy của người ta thì sẽ ghi nguồn, 
hoặc lấy của người ta đem về sửa lại cũng sẽ ghi nguồn nha) : https://github.com/nahhididwin/Hung_WAIRNI/blob/main/WAIRNI.v2.0/p2.png
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
Nói đơn giản thì từ "unit" là 1 từ tôi bịa ra để chỉ ra khoảng cách nhỏ nhất giữa 2 điểm có trong signal nhé, ví dụ như là trong signal có 3 điểm,
điểm 1 cách điểm 2 1 giây, điểm 2 cách điểm 3 0.5 giây, vậy 1 unit sẽ bằng 0.5 giây cho toàn bộ signal.

Nhưng đời không như mơ khi dữ liệu thực tế gần như không bao giờ đẹp đẽ như thế, tưởng tượng thử nhé, nhịp tim của ông liệu có đập đúng vào
lúc 1 giây sau khi bắt đầu đo rồi đúng 1 giây sau đập tiếp rồi cứ như vậy đến vô hạn không, dĩ nhiên là không =)). 
Mà nó sẽ kiểu đập lúc đầu tiên vào 0.59 giây sau khi bắt đầu đo, rồi sau khoảng 1.255 giây gì đó lại đập thêm phát nữa, rồi sau khoảng 0.85 giây lại đập
tiếp 1 phát nữa. Vầng thật ra nó cũng cho vui thôi chứ thực tế khả năng cũng chả phải 0.85 giây đâu, mà ở ngoài thực tế thì khả năng lại là 0.85861681686186181863861836816813863168618681368.... cũng không chừng, nhưng tôi đoán người ta sẽ không đo kỹ như vậy, tầm 0.85 hay 0.858 hoặc nhiều hơn 1 khoảng thôi.

Cho nên nếu như điểm "đo đạc/lấy mẫu" mà ở vị trí trên trục x kiểu nó có những chữ số sau dấu phẩy ",", thì nó sẽ thường sẽ tăng đáng kể số điểm đo đạc, ờ thật ra
điều này là hiển nhiên.

Vậy cùng nhau quay lại với Tích phân số cơ bản nha, tôi sẽ không nói đến Riemann Sum Hình chữ nhật vì nó thường không ngon bằng Trapezoidal Rule =)).
Đọc wiki : https://en.wikipedia.org/wiki/Trapezoidal_rule
Như chúng ta thấy, nó tính diện tích dưới "hình thang" hoặc hiếm lắm thì "hình tam giác" của các điểm (đo đạc/lấy mẫu). Haha các ông thấy gì chưa,
vì tọa độ của các điểm "đo đạc/lấy mẫu" không đẹp tý nào nên là nó sẽ tăng số lượng các điểm này lên kinh dị. Dẫn đến việc tăng sức nặng cho việc tính toán.
Để dễ dàng hình dung cho mức độ tăng độ phức tạp tính toán lên đáng kinh ngạc thì hãy so sánh giữa 2 ảnh này :
Ảnh này có tọa độ của các điểm đẹp nè (p3.png) : https://github.com/nahhididwin/Hung_WAIRNI/blob/main/WAIRNI.v2.0/p3.png
Ảnh này có tọa độ của các điểm xấu nè (p2.png) : https://github.com/nahhididwin/Hung_WAIRNI/blob/main/WAIRNI.v2.0/p2.png

Vậy nó có gì hot? Hãy cùng tính sơ sơ chi phí tính toán của phương pháp hình thang nhé :
- Tọa độ của các điểm "đo đạc/mẫu" đã có sẵn rồi, nó trông như này nếu như có 2 điểm đo đạc 1(x_1;y_1);2(x_2;y_2);
- Chúng ta sẽ lấy tọa độ x của điểm thứ 2 trừ cho tọa độ x của điểm thứ nhất, để ra được số delta_x hoặc unit nếu như toàn bộ tất cả các điểm đo đạc cách đều nhau (thường là như vậy);
- Sau đó lấy tọa độ y của điểm thứ 1 * unit * tọa độ y của điểm thứ 2 = sum_1; Nói chung kiểu cứ lặp lại như vậy rồi cộng các sum_1 lại với nhau là ra diện tích thôi;
- Tôi đã dùng kinh nghiệm của mình để cho ra 1 cách ước lượng sơ sơ gần đúng cho chi phí tính toán 
của phương pháp hình thang là như này : "số điểm đo đạc/mẫu" * 3 = tổng chi phí (kiểu số phép tính phải thực hiện, bao gồm cộng/trừ/nhân chia);
Vậy thì với p2.png thì sẽ mất tầm 73*3 = 219 phép tính cho hình tam giác ABC;
Còn p3.png thì mất tầm 6*3 = 18 cho tam giác ABC;
Vầng, tuy là 2 tam giác gần như y hệt nhau nhưng mà thật sự chi phí đã bị đội lên kinh dị =)), mặc dù chỉ là 2 đường "tuyến tính". Thực tế thì nếu là các đường
phi tuyến (ví dụ như 1 nửa đường tròn của hình tròn ấy) thì gần như chắc chắn "unit" sẽ rất bé, và số điểm đo đạc sẽ lớn hơn nhiều, cho dù là ít nhiễu hay không ít nhiễu hay thậm chí không có nhiễu trong signal, ừ ý tôi là điều này không hiếm. Tức là nếu tưởng tượng tốt thì các ông sẽ thấy rằng chẳng hạn như trong đồ thị hình sin ấy, cho dù là không có nhiễu trong signal thì unit vẫn sẽ rất bé bởi vì có "đường phi tuyến", tức là các đường "tuyến tính" vẫn sẽ phải đối mặt với chi phí tính toán ngang ngửa với "đường phi tuyến". Hoặc cho dù không có đường phi tuyến thì trong dữ liệu vẫn có nguy cơ rất cao đối mặt với vụ "0.858 giây" mà tôi từng đề cập tới ở bên trên kia.

Vậy nếu dữ liệu có nhiễu thì sao? 
Hãy xem p1.png : https://github.com/nahhididwin/Hung_WAIRNI/blob/main/WAIRNI.v2.0/p1.png
Đây là hình ảnh Raw ECG Signal (thật sự nhiều nhiễu) mà thuật toán Pan–Tompkins algorithm được sinh ra để đối mặt với nó. Thực tế thì trong rất nhiều tác vụ trên thực tế đều có nhiễu, nhưng nhiễu thường sẽ có "độ cao trục y" thấp (khả năng vô cùng vô cùng cao, chẳng hạn như nhìn trong ECG đi thì hiểu). Và đúng rồi, thực sự thì hiện nay có rất nhiều phương pháp lọc nhiễu vô cùng tiên tiến. Và hiện nay người ta cũng thường lọc nhiễu trước rồi mới tích phân để tính diện tích bên dưới signal.
--

