Hung_WAIRNI: Hưng Wave Analysis, Hung's Interpolation Rule and Đ.Đ.P.Hưng Numerical Integration.
(GitHub : https://github.com/nahhididwin/Hung_WAIRNI)

Creator : Đặng Đình Phú Hưng (Dang Dinh Phu Hung, https://github.com/nahhididwin)
Date of birth (Dang Dinh Phu Hung) (DD/MM/YYYY): 20/06/2011
Languages ​​used: Vietnamese, English.
Publication date (DD/MM/YYYY): 01/11/2025

Nhắc lại, project này sử dụng giấy phép này (MIT LICENSE) : https://github.com/nahhididwin/Hung_WAIRNI/blob/main/LICENSE

--
Các Phần Chính :
1. Vấn đề "chia", bất đối xứng, và kiểu dữ liệu liên quan
2. Khả năng của đạo hàm đối với việc tìm đỉnh với dạng dữ liệu liên quan, sử dụng phương pháp Hung_WAIRNI
3. AUS (Area under the Signal) bằng Hung_WAIRNI
4. Bổ sung thêm biên độ, cực trị
5. Lời kết
--

--
1. Vấn đề "chia", bất đối xứng, và kiểu dữ liệu liên quan:


Trước hết bạn cần biết như này (biết rồi thì thôi, à mà vấn đề "chia" là do tôi tự nghĩ ra nên khả năng bạn chưa biết hết đâu nên cứ đọc đi) :

Hãy đọc về Derivative (Đạo hàm) : https://vi.wikipedia.org/wiki/%C4%90%E1%BA%A1o_h%C3%A0m

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

Tóm lại : 
Vấn đề "chia" là vấn đề kiểu như 0.85 giây mà tôi nói rồi đấy, nó khiến cho số lượng "mẫu/điểm đo đạc" tăng lên kinh khủng.
Và "kiểu dữ liệu liên quan" mà tôi muốn các bạn nhớ đến nhiều nhất là kiểu dữ liệu bị vấn đề chia, bất đối xứng, và "nhiễu có độ cao trục y thấp", và có 1 đoạn tuyến tính, và có đoạn phi tuyến và nó nằm ở khoảng chỗ "đỉnh" (yes nó kiểu như ECG (QRS) hay đồ thị sin,...), vầng nó vô cùng phổ biến trên thực tế trong nhiều tác vụ hiện nay. Tôi đoán nó như kiểu dữ liệu dạng pulse nhưng đã được lọc nhiễu, dù gì trên thực tế người ta cũng phải lọc nhiễu trước rồi mới tích phân cho dù là phương pháp tích phân nào y chang nữa =)).

Ví dụ 1 xíu :

Dữ liệu raw data khi chưa lọc thường như này (p1.png) : https://github.com/nahhididwin/Hung_WAIRNI/blob/main/WAIRNI.v2.0/p1.png
Sau khi qua bộ lọc thường như này (p99.jpg) : https://github.com/nahhididwin/Hung_WAIRNI/blob/main/WAIRNI.v2.0/p99.jpg
À mà ảnh p99.jpg tôi lấy tại đây : https://ocaclinic.vn/tin-tuc/hinh-dang-va-co-che-hinh-thanh-cac-song-tren-dien-tam-do

Tôi đoán có thể 1 số ông sẽ biết đến cái thứ này : Bộ lọc Savitzky-Golay, Band-pass filter, Low-pass filter, High-pass filter, Notch filter, Adaptive Filter, Wavelet Transform, Phân tích thành phần độc lập (ICA),...
--


--
2. Khả năng của đạo hàm đối với việc tìm đỉnh với dạng dữ liệu liên quan, sử dụng phương pháp Hung_WAIRNI

Được rồi, vậy thị "đạo hàm" thì liên quan gì chứ? Hãy cùng xem lại ảnh p2.png (https://github.com/nahhididwin/Hung_WAIRNI/blob/main/WAIRNI.v2.0/p2.png).
Wow, bro thấy tiềm năng gì ko? Nếu như thay vì chúng ta làm thủ công "duyệt" từng điểm "đo đạc/mẫu" từ điểm A trên trục x đến điểm C trên trục x, lý do chính là
để tính được luôn diện tích bao gồm cả đỉnh B cho nó chuẩn. Ừ vì điểm B bất đối xứng với tam giác ABC cho nên chúng ta không thể xài cách đó là lấy đại 1 điểm ở giữa được. Và nếu làm như vậy rồi tính diện tích thì đúng rồi đấy, nó là phương pháp hình thang của tích phân số, riemann sum cũng na ná thế thôi. Haha, vậy thì chúng ta hãy đạo hàm tại điểm A, rồi đạo hàm tại điểm cách điểm A 30 unit, rồi cứ như vậy thôi, thấy gì chưa!? Nó tiện tay dò ra luôn điểm B với chi phí thấp vô cùng!

Chi tiết hơn về phép tính (để tìm giao điểm của 2 đường tiếp tuyến tạo bởi đạo hàm, à nó không liên quan đến bất kỳ hình nào tôi đưa cho ông nhé, đừng nhầm lẫn!) cho các ông dễ hình dung :

Tọa độ điểm A: (x_A, y_A)
Tọa độ điểm B: (x_B, y_B)
Hàm số đường phi tuyến màu xanh : y = f(x)
(Lưu ý quan trọng, trên thực tế, dữ liệu là dạng các điểm rời rạc, nên lúc đấy chúng ta sẽ "xấp xỉ" đạo hàm! Tôi để hàm số cho dễ hình dung thôi).

Vui lòng không nhầm lẫn : f'(x) là ký hiệu của đạo hàm! Không phải là f(x), ký hiệu của hàm số!

Hệ số góc của tiếp tuyến AC tại A(x_A, y_A) là : m_AC = f'(x_A)
Hệ số góc của tiếp tuyến BC tại B(x_B, y_B) là : m_BC = f'(x_B)
Phương trình AC : y - y_A = m_AC (x - x_A) => y = f'(x_A)(x - x_A) + y_A
Phương trình BC : y - y_B = m_BC (x - x_B) => y = f'(x_B)(x - x_B) + y_B

Tại C thì tọa độ y 2 đường thẳng bằng nhau nên :
f'(x_A)(x_C - x_A) + y_A = f'(x_B)(x_C - x_B) + y_B
=> f'(x_A)x_C - f'(x_A)x_A + y_A = f'(x_B)x_C - f'(x_B)x_B + y_B
=> f'(x_A)x_C - f'(x_B)x_C = f'(x_A)x_A - f'(x_B)x_B + y_B - y_A
=> x_C [f'(x_A) - f'(x_B)] = f'(x_A)x_A - f'(x_B)x_B + y_B - y_A

Công thức cho x_C :

x_C = [f'(x_A)x_A - f'(x_B)x_B + y_B - y_A]/[f'(x_A) - f'(x_B)]

Yes, điều này giả định 2 tiếp tuyến không song song (nếu nó song song thì chúng ta sẽ gọi đây là trường hợp đặc biệt
và đem đi xử lý riêng nhé, các bạn đủ giỏi để làm mà)

Công thức cho y_C :

y_C = f'(x_A)(x_C - x_A) + y_A




Và đúng rồi đấy, nó mất tầm 18 phép tính thôi =))), thay vì là khoảng 15*3 = 45 nếu Sử dụng tích phân số phương pháp hình thang (À mà cho dù là simpson's rule thì nếu như là đồ thị phi tuyến hay gặp vấn đề "chia" thì cũng chậm hơn Hung_WAIRNI thôi); tức là nhanh hơn khoảng 40%, thực tế nó có thể hơn nữa.

Đọc simpson's rule : https://en.wikipedia.org/wiki/Simpson%27s_rule


Vậy nếu như không phải đường tuyến tính mà là phi tuyến thì sao? Thực tế thì thường dữ liệu có 1 đoạn tuyến tính (mà đoạn này sẽ tận dụng đạo hàm để rút bớt sức nặng tính toán), và 1 đoạn phi tuyến tính thường ở đỉnh nha, kiểu như đồ thị sin hay ECG (QRS) nào đó.
Xin lỗi vì đã để top những thứ quan trọng nhất của tích phân ở tít dưới này =))). Nhưng mà thực tế thì tôi đoán 1 số ông đoán ngay ra cách rồi.
Hãy cùng xem lại cái ảnh này nữa (p2.png) : https://github.com/nahhididwin/Hung_WAIRNI/blob/main/WAIRNI.v2.0/p2.png
Biết sao tôi để thêm cái điểm B'' và điểm B' không =))? Giờ hãy tưởng tượng có 1 cái đường cong bắt đầu từ B' rồi "đỉnh" của đường cong đó là điểm B đi, rồi phần kết thúc của đường cong đó là điểm B''. Ừ có thể nghĩ đó là parabol cũng được, tùy mấy ông. Ừ thì phần này đơn giản thôi, vì chúng ta đã kiểu "nội suy/ngoại suy" ra được điểm B rồi, thì chúng ta sẽ check lại thêm 1 lần nữa (sau này tôi sẽ giải thích) vào điểm "đo đạc/mẫu" của dữ liệu thực tế hoặc dữ liệu đã được lọc nhiễu gần tọa độ của điểm B chúng ta đã làm ra nhất. Rồi sau đó tính tích phân bên trái và bên phải của điểm đó, ý là các điểm 2 bên (trái/phải) của điểm đó, dồn ra 2 phía (giữa sang trái, và trái sang phải). Vầng áp dụng phương pháp tích phân số cổ điển cho việc này (chẳng hạn như simpson's rule/phương pháp hình thang), nhưng nhớ là chỉ áp dụng cho phần cong thôi nhé, có thể đặt sẵn "số lượng điểm đo đạc/mẫu" gần điểm đã được chúng ta làm ra bằng giao điểm của 2 đường tiếp tuyến của đạo hàm sẽ được áp dụng tính tích phân cổ điển. Yên tâm đi với dữ liệu đã qua bộ lọc thì dư sức xài tốt.
Bằng cách nào đó thì tôi linh cảm rằng 1 số ông sẽ bảo tôi ko biết cái cầu phương của gauss nào đó cho tích phân số =))) "https://en.wikipedia.org/wiki/Gauss%E2%80%93Legendre_quadrature", vãi thật, trong thực tế thì "các điểm mẫu/đo đạc" cách đều nhau, thậm chí là méo cách đều nhau (nhưng ko có trúng cái điểm mà gauss cần đâu) cơ =))). Ko xài đc đâu, ngon thì ngon thật nhưng thực tế vẫn là thực tế =)), hèn gì phương pháp hình thang/simpson's rule phổ biến vãi :3.


Haizz, tôi cảm thấy như kiểu O(n) xuống xấp xỉ O(1) vậy á =))))). Kiểu các ông để ý thì ở phần tuyến tính thì nó sẽ kiểu từ O(n) -> xấp xỉ O(1), còn phần phi tuyến tính thì nó sẽ tương đương với các phương pháp tích phân số cổ điển hiện nay thôi :3.

Nói chứ thật ra nếu các ông xem p99.png thì các ông cũng thấy rằng Hung_WAIRNI vẫn chưa đủ mạnh để thay thế Tích phân số, vì tuy là nó là dữ liệu đã rất ngon rồi, nhưng mà khi để ý kỹ lại thì, ở phần sóng Q ấy, đấy, nó nhỏ vãi, hiểu ý tôi rồi đó, chúng ta cần 1 cái dạng signal mà nó vừa phải lấy mẫu nhiều trên đường tuyến tính và đường tuyến tính cũng cần phải trải kha khá dài nữa =))).

Vậy nên chúng ta sẽ chuyển sang Tín hiệu dòng điện xoay chiều (AC) và điện áp nhé, vì chúng thường có hình dạng sin.

Do tôi yêu wikipedia nên tôi sẽ để ở đây (Điện xoay chiều) : https://vi.wikipedia.org/wiki/%C4%90i%E1%BB%87n_xoay_chi%E1%BB%81u

Tác vụ: Tính toán các đại lượng vật lý liên quan đến năng lượng (Power), công (Work), điện tích (Charge), hoặc thông lượng (Flux).
example : Tính năng lượng tiêu thụ W trong một chu kỳ T của dòng điện, được tính bằng tích phân của công suất P(t): W = ∫^(T)_0 P(t)dt. Nếu tín hiệu điện áp v(t) và dòng điện i(t) là dạng sin, công suất P(t) = v(t) * i(t) cũng là một hàm lượng giác, và tích phân số được dùng để tính toán giá trị này từ các mẫu dữ liệu thu thập được.

Cơ học/Dao động: Phân tích chuyển động dao động điều hòa (Harmonic Motion) hoặc sóng âm/cơ học.
Tác vụ: Tính quãng đường dịch chuyển (displacement) từ tín hiệu vận tốc, hoặc tính vận tốc từ tín hiệu gia tốc (gia tốc có thể có dạng sin).
Trong thực tế, tín hiệu được thu thập (bằng bộ chuyển đổi A/D) dưới dạng các mẫu rời rạc tại các thời điểm lấy mẫu nhất định.
Tích phân giải tích (Analytical Integration) chỉ có thể áp dụng nếu ta có công thức hàm x(t) chính xác. Nhưng với dữ liệu rời rạc, cách duy nhất để tính diện tích dưới đường cong là dùng các phương pháp tích phân số (hình thang, Simpson, v.v.).

Tần số Tín hiệu Thường gặp (f) : 50Hz ở Việt nam hoặc 60hz =).
Theo Định lý Nyquist, F_s phải lớn hơn 2f Tức là lớn hơn 100 mẫu/giây =)).
Mức Thực tế (Cho độ Chính xác Tích phân): Các thiết bị đo lường chất lượng điện năng thường chọn F_s là bội số của f và rất cao để đảm bảo độ chính xác của phép tính tích phân số và khả năng phân tích sóng hài. Phổ biến là 6.4 kS/s (kilo-mẫu/giây), 10 kS/s hoặc cao hơn, idk.
Số Mẫu Trên 1 Chu kỳ (1 Lần "Lên Xuống")?
Số mẫu trong một lần "lên xuống" (một chu kỳ T) được tính bằng : N = F_s/f.
Tức là nếu là 10000S/s thì 200 mẫu =)).

 Yên tâm đi, sóng sin chiếm tới hơn 90% lận, còn % chúng ta sẽ tìm cách sử lý bằng thuật toán riêng hoặc gì chăng?

À quên nói, chỗ tuyến tính (ý là rất gần tuyến tính ấy, ừm tôi đoán ko cần quá lo lắng đâu, vì thực chất việc chuyển từ "sóng sin" ngoài thực tế lên máy tính bằng các điểm rời rạc đã lòi ra 1 đống sai số rồi) thì Hung_WAIRNI, chỗ phi tuyến thì tích phân cổ điển nha.

Haha và để biết chính xác chỗ nào có sóng sin tốt và chỗ nào có sóng hài, đừng tưởng tôi không biết FFT =)), tôi còn biết cả DWT (Biến đổi wavelet rời rạc), CWT,... đấy.

Đọc Fast Fourier Transform (FFT) tại đây nha : https://en.wikipedia.org/wiki/Fast_Fourier_transform

Nói chung thì để biết chính xác chỗ nào có sóng sin tốt và chỗ nào có sóng hài, chắc chúng ta sẽ xài FFT :v.

Vầng, tôi còn để ý rằng phương pháp tích phân số này ngon vãi ra cho Triangular Wave (sóng tam giác), và cả sóng kiểu nhìn như hình vuông nữa, nhưng mà tôi gà quá nên chưa biết nó để làm gì, chắc sau này tôi sẽ xem thêm.

Tôi nói thêm nha :

Phổ biến nhất: Sóng Sin chuẩn có Méo Hài (Harmonic Distortion)

Hầu hết điện năng từ máy phát điện và điện lưới được thiết kế để tạo ra dạng sóng sin chuẩn (Pure Sine Wave) vì nó là dạng sóng hiệu quả và ít gây căng thẳng nhất cho thiết bị (động cơ, máy biến áp...).

Tuy nhiên, do các yếu tố như tải phi tuyến tính (máy tính, đèn LED, bộ nguồn chuyển mạch...), máy biến áp không lý tưởng, và các điều kiện thực tế khác, dạng sóng thực tế thường bị méo mó (Distorted) một chút. Mức độ méo này được gọi là Tổng méo hài (THD).

Dù bị méo, dạng sóng này vẫn rất gần với sóng sin lý tưởng (THD thấp), nó cong mượt ở cả phần đỉnh/đáy và phần thân, không có đoạn nào thẳng hoàn toàn =))).

Trong nhiều ứng dụng kỹ thuật, sai số giữa đường cong sin chuẩn và đường thẳng xấp xỉ ở phần thân (gần điểm 0) là chấp nhận được

Bạn hoàn toàn có thể xấp xỉ tuyến tính phần thân của sóng sin chuẩn. Sai số này được chấp nhận rộng rãi :3.

Thôi, nói chung thì tôi nhận ra rằng dữ liệu thực tế và dữ liệu trên lý thuyết khác nhau vãi nhót =)). Nên chúng ta sẽ tập trung vào thứ phổ biến nha. Hm... theo tiêu chuẩn phổ biến tôi thấy thì Tổng độ méo hài (THD) sẽ là tầm ~5%. Vầng, nếu THD quá cao thì sẽ không được cho phép nha. Và đây là tiêu chuẩn quốc tế IEEE 519 nha, đọc : https://www.elspec-ltd.com/ieee-519-2014-standard-for-harmonics/

Đồng thời, chúng ta sẽ lấy 1 cái dữ liệu thực tế ra nhé, cùng xem ảnh p98.png (https://github.com/nahhididwin/Hung_WAIRNI/blob/main/WAIRNI.v2.0/p98.png) nhé. Ảnh này tôi lấy tại đây : https://www.elspec-ltd.com/ieee-519-2014-standard-for-harmonics/

Như bro có thể thấy, nó thật sự kha khá giống sóng sin đó, nhưng mà đã bị biến dạng cũng cũng kha khá rồi. Đồng thời nó đã lộ rõ điều này (phổ biến trong dữ liệu thực tế) :
+ Nó lấy RẤT NHIỀU MẪU (điểm đo đạc/mẫu) ở các vị trí "cong" (phi tuyến) lẫn "thẳng" (tuyến tính).
+ Và đỉnh của nó, vị trí của nó đã bị bất đối xứng

Vầng, và có lẽ chúng ta sẽ tập trung vào việc xấp xỉ diện tích của nó ở phần tuyến tính, bro hiểu sơ sơ ý tôi rồi đó. Và chúng ta tiện thể sẽ làm thêm quả "giao điểm của 2 đường tiếp tuyến tạo bởi đạo hàm xấp xỉ 1 cách hợp lý" yes, giao điểm của nó sẽ có tọa độ x;y, chúng ta chỉ lấy tọa độ x thôi, rồi chúng ta check chính xác dữ liệu thực tế xem điểm rời rạc nào gần nhất với điểm đã "nội suy/ngoại suy" đó. YESSS, thay vì phải tìm kiếm tuyến tính từng điểm rời rạc một (chi phí tìm kiếm = chi phí cho các phép toán so sánh = chi phép các phép cộng; yea nó cũng tương đương nhau), tức là O(n) -> ~O(1).

Với cả, việc "đạo hàm" trên phần "thân" của sóng sin ấy, kiểu phần gần chỗ OV ấy, trời đụ, khi số lượng mẫu -> vô hạn, nó GẦN NHƯ ĐÉO THAY ĐỔI HAY THÊM NHIỄU, còn ở đỉnh thì nhiễu nhiều vc, heheeh, tiện vc =))), quá thuận lợi, quá ngon.

Tức là chúng ta tốn khoảng ~17 phép tính toán (cộng/trừ/nhân/chia) nhưng đã TÌM ĐƯỢC ĐỈNH của 0.5 chu kỳ sóng sin rồi, thay vì phải Linear Search hết 200 điểm, vầng có thể là cả hơn 1000 điểm bởi vì ông biết mà, số mẫu càng lớn thì càng căng, chẳng hạn như trong p98.png ấy. Vầng hiện nay người ta vẫn đang Linear Search để tìm được đỉnh của sóng sin (0.5 chu kỳ sóng sin) :3, và tôi 1 công đôi việc, vừa tìm đỉnh, vừa đi tính tích phân, yes sau khi ~17 phép tính chúng ta thêm 1 số phép tính con phụ để tính diện tích phần "gần như tuyến tính". Còn phần "phi tuyến rõ rệt ở đỉnh" thì các ông biết phải làm gì rồi mà.

Vầng, bro có để ý ra ko, tuy là THD (tổng méo hài) đã được limit dưới 8% nên sóng sin tín hiệu điện xoay chiều (AC) sẽ không quá mức biến dạng, nhưng mà nó vẫn biến dạng kha khá, vầng ở mức THD 8% trở xuống (rất phổ biến và cũng là tiêu chuẩn quốc tế), thì chủ yếu là sẽ biến dạng tại đỉnh =)). Vầng, khi chúng ta xài đạo hàm, 2 đường tiếp tuyến để nội/ngoại suy ra được tọa độ x gần đúng của đỉnh, thực tế thì nếu đỉnh biến dạng kha khá (sẽ xảy ra ở mức PHD dưới 8%), thì có thể chúng ta sẽ có sai lệch tầm khoảng ~10% cho việc tìm đỉnh, vầng điều này là phổ biến, khá là căng đó. Nhưng yên tâm, chúng ta có cách này, chúng ta vẫn sẽ xài đạo hàm, 2 đường tiếp tuyến (ừ, để tránh trường hợp sóng sin bất đối xứng) để nội/ngoại suy ra "khu vực khả nghi ở đỉnh của 1 nửa chu kỳ sóng sin", sau đó chúng ta sẽ duyệt qua các điểm gần đó, tức là chỉ duyệt các điểm quanh đỉnh, tức là chúng ta ko cần phải duyệt các điểm ở trên chỗ "thân" của sóng sin rồi =)), vầng, điều này khá là tương tự với việc tính tích phân.


.... còn tiếp ....

--


--
3. AUS (Area under the Signal) bằng Hung_WAIRNI

Tôi sẽ sớm bổ sung phần chứng minh lỗi, và công thức tổng quát chính thức,...

--

--
4. Bổ sung thêm biên độ, cực trị
Tôi sẽ sớm bổ sung...
--


--
5. Lời kết

Cảm ơn vì đã đọc. Nó có thể có sai sót, nhưng tôi đã luôn cố gắng sửa, nếu có vấn đề gì hãy liên hệ riêng tôi! Dù gì tôi thật sự làm cái này vào năm lớp 9 nên còn non lém :3.
Hiếm khi lại có dự án khiến tôi hào hứng và khiến tôi bị "vắt cực khô" não như
cái project này =))).
Nếu bạn thấy nó hay và học được điều gì hoặc ứng dụng nó, nếu có thể bạn có thể
ghi tên tôi vào phần danh sách những người đóng góp hoặc không, nhưng bạn sẽ không thể phủ nhận việc
bạn đã sử dụng ý tưởng của tôi nếu tôi bảo bạn sử dụng ý tưởng của tôi.
--
