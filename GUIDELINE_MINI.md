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
| `drive_...` | | | |
| `drive_...` | | | |
| `drive_...` | | | |
