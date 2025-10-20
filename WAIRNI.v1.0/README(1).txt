Hung_WAIRNI: Hưng Wave Analysis, Hung's Interpolation Rule and Đ.Đ.P.Hưng Numerical Integration.

Creator : Đặng Đình Phú Hưng (Dang Dinh Phu Hung, https://github.com/nahhididwin)
Date of birth (Dang Dinh Phu Hung) (DD/MM/YYYY): 20/06/2011
Languages ​​used: Vietnamese, English.
Publication date (DD/MM/YYYY):

--
Các Phần chính :
0. The problem is the "illusion" that it was right. (in stopping the process of getting information about
the function, because the computer cannot get infinite information).
1. What is "wave", what is area (Rule)?
2. How the "wave" forms are converted to "wave law" form for comparison (Rule)?
3. Details on how to "draw" tangent lines for "measurement points/locations" located on the "wave".
4. How to calculate?
5. Hưng Wave Analysis's Goal.
6. Numerical Integration.
7. Hưng Wave Analysis.
8. How to apply Hung_WAIRNI in practice?
--

--
0. The problem is the "illusion" that it was right. (in stopping the process of getting information about
the function, because the computer cannot get infinite information).
Hung_WAIRNI (very strong/failed), xem ảnh pic_0_1.png;
Simpson's Rule, Riemann Sum, Phương Pháp Hình Thang,... (Failed), xem ảnh pic_0_1.png;
--

--
Rules :
1. What is "wave", what is area?
Xem ảnh pic_0_2.png;
Calculating the area of a non-linear shape is easy if you "draw/approximate" the "wave" of the shape exactly. So the problem
will be the "wave".



2. How the "wave" forms are converted to "wave law" form for comparison:
Xem ảnh pic_1.png
Good Case: α_1 > α_2 * 2
Required (Always) :
Đường tiếp tuyến sẽ phải cắt nhau (2 đường cắt nhau), (Tiếp tuyến không song song với nhau), nếu không thì chỗ đó xài phương pháp khác, ex :
Simpson's Rule, Phương pháp hình thang, Riemann Sum,....
--

--
3. Details on how to "draw" tangent lines for "measurement points/locations" located on the "wave".
Xem ảnh pic_0_3.png;

Jump distance (JD);
--

--
4. How to calculate:
Xem ảnh pic_0_3.png;

Tọa độ điểm A: (x_A, y_A)
Tọa độ điểm B: (x_B, y_B)
Hàm số đường phi tuyến màu xanh : y = f(x)

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

Yes, điều này giả định 2 tiếp tuyến không song song

Công thức cho y_C :

y_C = f'(x_A)(x_C - x_A) + y_A
--


--
5. Hưng Wave Analysis's Goal
- Độ dốc
- Cực trị
- Hình dạng của wave
- Và dĩ nhiên là có thể thêm nhiều thứ khác nữa (vận tốc,...).
--

--
6. Numerical Integration:
Tính diện tích của các hình con được tạo bởi đường tiếp tuyến,... là ra được xấp xỉ diện tích thật.
Khi số lượng "vị trí" -> vô hạn; khoảng cách giữa các "vị trí" -> 0; thì diện tích tính được ngày càng đúng/gần hơn với diện tích thật sự.

example : xem ảnh pic_0_4.png;

+ Chọn JD (Jump Distance) nếu thích, không thì cứ chia đều; JD là khoảng cách giữa các điểm "vị trí", hình bên trên là JD = 4 units; Tức A cách B
4 units, B cách C ....
+ Sau đó thì như bạn có thể thấy, khi số lượng điểm "vị trí" (như A;B;C;D;E) tăng lên thì "wave" ngày càng được đo đạc chính xác hơn, vậy nên :

Số lượng "vị trí" -> vô hạn; khoảng cách giữa các "vị trí" -> 0; thì chính xác hoàn toàn!
+ Sau đó tính diện tích của các S thôi (hình thang);
--

--
7. Hưng Wave Analysis
--

--
8. How to apply Hung_WAIRNI in practice?

Đây là phần quan trọng giúp Hung_WAIRNI thật sự hiệu quả trên thực tế, vầng tôi lấy ý tưởng 1 chút từ Dynamic Programming (DP) để làm việc này. 
Đó là như bạn đã thấy, vì chúng ta đã tính được rất nhiều thông tin về "wave", các kiểu,... nên chúng ta vừa có thể Wave Analysis, 
vừa tìm được cực trị, độ dốc,... mà vừa có thể nội suy, tính diện tích. 
Và đó là thứ tôi muốn nói, đó là sử dụng những thứ đã được tính toán từ trước cho một thứ khác! 
Và bạn có thể thỏa sức cải tiến Hung_WAIRNI theo cách của bạn, tôi đã nêu ý tưởng.
--


Kết thúc :

Cuối cùng, tôi cảm ơn vì bạn đã đọc nó, nó là công sức nghiên cứu của tôi, dù gì thì do
tôi thật sự đâm đầu vào nghiên cứu cái thứ này vào năm lớp 9 nên cũng không dễ dàng gì, có thể có sai sót
. Nếu bạn thấy nó hay và học được điều gì hoặc ứng dụng nó, nếu có thể bạn có thể
ghi tên tôi vào phần danh sách những người đóng góp hoặc không.
