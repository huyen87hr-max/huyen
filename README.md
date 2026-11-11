# Material Master — ERP Training

Ứng dụng HTML/CSS/JS thuần theo đề bài 10/11/2026. Hỗ trợ chế độ A (GAS/Sheet riêng) và chế độ B (frontend kết nối GAS của giảng viên).

## Chạy giao diện ngay
1. Giải nén ZIP, mở **cả thư mục** `material-master-student` bằng VS Code.
2. Cài tiện ích **Live Server**; nhấp phải `index.html` → **Open with Live Server**.
3. Khi mở, ứng dụng tự kết nối URL GAS đã được cấu hình. Nếu GAS không phản hồi, ứng dụng báo lỗi và không giả lập dữ liệu đồng bộ. Khi không cấu hình URL, ứng dụng hiển thị **4 vật tư DEMO cục bộ**. Đây không phải dữ liệu đã lưu trên Google Sheets, và nút cấp mã chính thức bị khóa.
4. Thử đổi 4 nhóm / 11 chủng loại, sinh tên, đối chiếu trùng, tìm kiếm, lọc, xem chi tiết và mã vạch.

## Chế độ A — Tạo Google Sheets riêng
1. Tạo Google Sheets mới (chỉ dùng dữ liệu DEMO), mở Extensions → Apps Script.
2. Dán **toàn bộ** `gas/Code.gs` vào Apps Script gắn với Sheet đó. Lưu.
3. Chạy hàm `setup()` một lần, cấp quyền cần thiết. Script sẽ tạo `MATERIALS`, `NUMBER_RANGES`, `AUDIT_LOG`; mặc định khóa quyền tạo public.
4. Chạy `setTrainingEnabled(true)` từ trình biên tập Apps Script khi sẵn sàng mở demo. Để tắt, chạy `setTrainingEnabled(false)`. **Không** dùng dữ liệu công ty.
5. Deploy → New deployment → Web app → Execute as **Me**, Who has access **Anyone** (chỉ khi chấp nhận rủi ro demo công khai). Sao chép URL `/exec`.
6. Trong `assets/js/cloud-bridge.js`, thay `GAS_EXEC_URL=''` bằng URL `/exec` được cấp, rồi chạy lại Live Server. Nhấn Đồng bộ.
7. Kiểm tra việc cấp mã bằng **Sheet thực tế**; thử tạo lại vật tư giống nhau để xác nhận bị chặn.

## Chế độ B — GAS chung giảng viên
- Không triển khai `gas/Code.gs` vào hệ thống giảng viên.
- Chỉ điền URL `/exec` do giảng viên cấp tại `assets/js/cloud-bridge.js`.
- Frontend gọi đúng `training.status`, `training.list`, `training.create` và kiểm tra `erp-attribute-v1`. Nếu backend tắt training hoặc schema không khớp, nút cấp mã bị khóa.

## GitHub Pages
Upload `index.html`, `assets/`, icon, manifest và service worker lên repo. Settings → Pages → Deploy from branch. Dùng đường dẫn tương đối để chạy trong repo con. Không cần upload `gas/`.

## Bảo mật / giới hạn
- Public GAS **không có đăng nhập thực**, tên học viên chỉ là nhãn demo. Người biết endpoint có thể gọi API trực tiếp.
- Không sử dụng để quản lý dữ liệu nội bộ thực. Tắt training khi kết thúc.
- Google Sheets không phải cơ sở dữ liệu ACID; trường hợp lỗi giữa ghi dòng và cập nhật bộ đếm cần kiểm tra/audit thủ công.
- Không tự xác nhận PASS cho các test cần môi trường GAS hoặc nhiều máy khi chưa thử.
- Phần kiểm tra vật tư lịch sử thiếu thuộc tính trên GAS chọn phương án an toàn: chặn cấp mã khi có dữ liệu lịch sử cùng nhóm chưa thể đối chiếu.

## GAS đã cấu hình trong bản này
- URL `/exec` được cung cấp đã được đặt trong `assets/js/cloud-bridge.js`.
- Ứng dụng chỉ gọi `training.status`, `training.list`, và khi người dùng chủ động nhấn Lưu mới gọi `training.create`.
- Nếu GAS chung chưa hỗ trợ API hoặc phiên bản `erp-attribute-v1`, giao diện báo lỗi/khóa nút tạo mã. Không sửa GAS chung.
- Chưa kiểm thử thực tế quyền truy cập và dữ liệu Sheet; cần mở Live Server và nhấn Đồng bộ để xác nhận.
