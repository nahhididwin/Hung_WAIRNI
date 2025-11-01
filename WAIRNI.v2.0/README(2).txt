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
hãy xem hình ảnh này như 1 ví dụ : 
https://github.com/nahhididwin/Hung_WAIRNI/blob/main/WAIRNI.v2.0/p1.png
Tôi đã lấy hình ảnh đó tại đây : https://en.wikipedia.org/wiki/Pan%E2%80%93Tompkins_algorithm


Và hãy xem ảnh này (Do tôi tự làm, nên nguồn sẽ là ở ngay đây) : https://github.com/nahhididwin/Hung_WAIRNI/blob/main/WAIRNI.v2.0/p2.png
NÓ LẤY MẪU ĐẾN TẬN 73 CÁI CHO TAM GIÁC ABC (ý tôi là đường tuyến tính ABC, còn cái gạch đen ở dưới là trục x thôi)!!!!
Vầng, biết vì sao lại như thế không, tại vì tọa độ của điểm C "không được đẹp" (thế nào là không được đẹp, tôi cảm thấy việc
bản thân mình cứ kiểu sử dụng từ ngữ không khoa học mà thường ngày như này không ổn cho lắm, tôi đoán mình sẽ sửa sau, nhưng
ý tôi là kiểu như này là đẹp nè x = 3, còn này là xấu nè 3.59, còn này là ối dồi ôi siêu xấu nè : 3.86818618618618618618618).
