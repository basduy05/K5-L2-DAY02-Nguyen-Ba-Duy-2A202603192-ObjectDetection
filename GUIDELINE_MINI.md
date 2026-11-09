# Mini guideline — bài làm cá nhân

> Điền nội dung này trước khi bắt đầu gán nhãn. Giữ lại các quyết định sau khi self-QC.

## Định nghĩa lớp

| Lớp | Định nghĩa của bạn | Ví dụ/điểm phân biệt |
| --- | --- | --- |
| `car` | Xe con chở người loại nhỏ, sedan, hatchback, SUV, taxi, xe bán tải (pickup cabin nhỏ). | Không gán cho xe van lớn, xe buýt hay xe tải thùng. Xe đang đỗ vẫn tính là car. |
| `truck` | Xe tải chở hàng hóa hạng vừa và nặng, xe đầu kéo container, xe ben. | Thùng hàng tách biệt cabin hoặc xe chuyên dụng hạng nặng. Không gán cho xe bán tải thông thường (xe bán tải tính là car). |
| `bus` | Xe buýt công cộng nội đô, xe khách liên tỉnh lớn, xe đưa đón học sinh/tuyến cố định. | Xe thân dài, sức chứa nhiều người, nhiều hàng cửa kính hành khách. |
| `van` | Xe van thùng kín hình hộp (box-like), minivan chở người hoặc hàng cỡ trung | Cao hơn và hình hộp hơn car, nhưng nhỏ gọn hơn bus lớn và không phải tuyến xe buýt công cộng. |

## Quy tắc chung

- Box bám sát vật thể nghĩa là: 4 cạnh box phải ôm sát các pixel ngoài cùng nhìn thấy được của thân xe. Tuyệt đối không lấy lấn ra mặt đường, bầu trời, vỉa hè hay lấn sang thân xe bên cạnh.
- Vật thể bị che khuất: Nếu một xe bị xe khác hoặc cột điện/cây cối che khuất một phần, chỉ vẽ box ôm trọn phần nhìn thấy được của chiếc xe đó nếu vẫn nhận diện rõ lớp. Không suy đoán phần bị che khuất.
- Vật thể bị cắt bởi rìa ảnh: Box chạm đúng điểm (0 hoặc 1). Không vẽ lọt box ra ngoài mép ảnh.
- Vật thể rất nhỏ/mơ hồ: Nếu xe ở quá xa, kích thước quá bé (dưới ~15-20 pixel) hoặc mờ nhòe không thể nhận diện chắc chắn loại xe, bỏ qua và ghi chép lý do.

## Ghi lại quyết định cho ít nhất ba ca mơ hồ

| Ảnh | Tình huống | Quyết định cuối | Lý do theo schema |
| --- | --- | --- | --- |
| `drive_003.jpg` | Ngã tư đông đúc, các xe nối đuôi sát nhau che khuất một phần và có xe bán tải cabin nhỏ. | Gán nhãn car cho xe bán tải; các xe bị che vẽ box riêng ôm phần nhìn thấy được. | Schema quy định xe bán tải cabin nhỏ thuộc lớp car; quy tắc occlusion chỉ gán box phần nhìn thấy, không đoán phần bị che.
| `drive_004.jpg` | Cảnh ngã tư ban đêm, ánh sáng yếu, có người đi bộ và xe máy di chuyển cắt ngang luồng xe. | Chỉ gán box cho xe con (`car`) và xe van (`van`); tuyệt đối không gán xe máy và người đi bộ. | Schema chỉ gồm 4 lớp ô tô (`car`, `truck`, `bus`, `van`); người đi bộ và phương tiện 2 bánh thuộc phạm vi cấm gán (out-of-scope). |
| `drive_014.jpg` | Xe cứu hộ giao thông màu trắng (`公安`) gắn cần cẩu kéo xe ở phía sau tiền cảnh. | Gán nhãn `truck` (Class 1). | Xe chuyên dụng chở/cứu hộ phương tiện có kết cấu cabin và khung công tác tách rời, thuộc lớp `truck`, không phải xe `car` hay `van`. |
