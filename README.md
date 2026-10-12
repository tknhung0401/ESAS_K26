# Lộ trình học tập NNTM&BV – Khóa 2026

Trang web giúp sinh viên Khóa 2026 ngành Nông nghiệp thông minh và bền vững (Trường ĐH Việt Nhật, ĐHQGHN) theo dõi tiến độ 165 tín chỉ. Sinh viên đăng nhập bằng email `@st.vju.ac.vn`. Giảng viên được cấp quyền xem tiến độ của cả lớp và tải về dạng CSV.

| Tệp | Nội dung |
|---|---|
| `index.html` | Trang lộ trình: sơ đồ cây, 8 học kỳ, cẩm nang tư vấn, theo dõi lớp |
| `huong-dan.html` | Hướng dẫn cho sinh viên (gửi link này kèm link trang chính) |
| `firestore.rules` | Quy tắc bảo mật dữ liệu, dán vào Firebase |

Trang chạy trên **GitHub Pages**, một dịch vụ chỉ phục vụ trang tĩnh. Phần đăng nhập và lưu tiến độ dùng **Firebase** (miễn phí). Bạn cần cài đặt một lần, mất khoảng 20 phút.

---

## Bước 1 · Đưa trang lên GitHub Pages

1. Đăng nhập [github.com](https://github.com) và tạo repository mới (**New repository**), ví dụ đặt tên `lo-trinh-nntmbv-k2026`. Chọn **Public**.
2. Bấm **Add file → Upload files** và tải lên cả 4 tệp trong thư mục này, rồi bấm **Commit changes**.
3. Vào **Settings → Pages**. Ở mục *Build and deployment*, chọn **Source: Deploy from a branch**, **Branch: `main`**, thư mục **`/ (root)`**, rồi bấm **Save**.
4. Sau 1–2 phút, trang có địa chỉ dạng `https://<tên-tài-khoản>.github.io/lo-trinh-nntmbv-k2026/`.

Chưa cấu hình Firebase thì trang chạy ở **chế độ thử**: dùng được đầy đủ, nhưng tiến độ chỉ lưu trên trình duyệt. Bạn có thể xem thử trước khi làm tiếp.

## Bước 2 · Tạo dự án Firebase

1. Vào [console.firebase.google.com](https://console.firebase.google.com), bấm **Create a project** (hoặc *Add project*) và đặt tên, ví dụ `lotrinh-nntmbv`. Có thể tắt Google Analytics.
2. **Bật đăng nhập Google:** vào **Build → Authentication → Get started → Sign-in method → Google**, bấm **Enable**, chọn email hỗ trợ, rồi **Save**.
3. **Cho phép tên miền GitHub:** vẫn trong Authentication, vào **Settings → Authorized domains → Add domain** và nhập `<tên-tài-khoản>.github.io`. Chỉ nhập tên miền, không kèm `https://` hay đường dẫn.
4. **Tạo cơ sở dữ liệu:** vào **Build → Firestore Database → Create database**. Chọn vị trí `asia-southeast1 (Singapore)` và chế độ **production**.
5. **Dán quy tắc bảo mật:** trong Firestore, mở tab **Rules**, xoá nội dung cũ, dán toàn bộ nội dung tệp `firestore.rules`, rồi bấm **Publish**.

## Bước 3 · Gắn Firebase vào trang

1. Trong Firebase, bấm biểu tượng bánh răng → **Project settings**. Ở mục *Your apps*, bấm biểu tượng **`</>`** (Web), đặt tên app và bấm **Register app**. Không cần bật Firebase Hosting.
2. Firebase hiện một đoạn `const firebaseConfig = { ... }`. Chép 4 giá trị `apiKey`, `authDomain`, `projectId`, `appId`.
3. Trên GitHub, mở `index.html` và bấm biểu tượng bút chì (Edit). Tìm khối gần đầu đoạn `<script>`:

   ```js
   const FIREBASE_CONFIG={
     apiKey:"",
     authDomain:"",
     projectId:"",
     appId:""
   };
   ```

   Điền 4 giá trị vào giữa các dấu ngoặc kép, rồi bấm **Commit changes**.

> `apiKey` của Firebase được phép để công khai trong mã trang web, nó không phải mật khẩu. Dữ liệu được bảo vệ bằng `firestore.rules`.

## Bước 4 · Cấp quyền giảng viên

Giảng viên đăng nhập bằng một tài khoản Google bất kỳ (ví dụ email `@vju.ac.vn` hoặc Gmail), nhưng phải có tên trong danh sách:

1. Firestore Database → tab **Data** → **Start collection**, đặt Collection ID là **`lecturers`**.
2. **Document ID** = email của giảng viên, **viết thường toàn bộ**, ví dụ `nguyenvana@vju.ac.vn`. Thêm một trường bất kỳ, ví dụ `ten` (string) = `Nguyễn Văn A`, rồi bấm **Save**.
3. Mỗi giảng viên thêm là một document. Xoá document khi muốn thu hồi quyền.

Giảng viên mở trang, bấm **“đăng nhập bằng email giảng viên”** ở dưới nút đăng nhập, và sẽ thấy mục **Theo dõi lớp**. Mục này có số sinh viên, TC trung bình, số sinh viên chọn A/B, bảng chi tiết từng sinh viên (bấm vào dòng để xem từng nhóm học phần), nút **Tải CSV** và **Sao chép bảng**.

## Bước 5 · Kiểm tra trước khi gửi sinh viên

- [ ] Đăng nhập bằng một email `@st.vju.ac.vn`, nhập MSSV, đánh dấu vài môn. Dòng trạng thái phải báo “Đã lưu lúc …”.
- [ ] Tải lại trang: các môn đã đánh dấu vẫn còn.
- [ ] Đăng nhập bằng Gmail cá nhân (không có trong `lecturers`): trang phải báo “Email chưa đúng”.
- [ ] Đăng nhập bằng email giảng viên: thấy sinh viên thử ở mục Theo dõi lớp, và tải được CSV.
- [ ] Gửi sinh viên **link trang chính** và **link `huong-dan.html`**.

## Quyền truy cập dữ liệu

| Người dùng | Đọc | Ghi |
|---|---|---|
| Sinh viên `@st.vju.ac.vn` | Chỉ tiến độ của mình | Chỉ tiến độ của mình |
| Giảng viên trong `lecturers` | Tiến độ cả lớp | Không |
| Email khác | Không | Không |

Không ai xoá được dữ liệu từ trang web. Quản trị viên xoá được trong Firebase Console (Firestore → Data → `progress`).

## Lưu ý

- **Sinh viên phải mở bằng trình duyệt** (Chrome, Safari). Google chặn đăng nhập bên trong Zalo, Messenger và Facebook; trang tự nhắc khi phát hiện trường hợp này.
- **Nếu email trường bị chặn đăng nhập:** quản trị Google Workspace của trường có thể không cho tài khoản sinh viên đăng nhập ứng dụng bên ngoài. Khi đó sinh viên gặp thông báo kiểu “Access blocked” hoặc “ứng dụng bị chặn”. Hãy đề nghị bộ phận CNTT của trường cho phép ứng dụng OAuth của dự án Firebase. Client ID nằm ở Google Cloud Console → APIs & Services → Credentials.
- **Gói miễn phí (Spark)** cho phép 50.000 lượt đọc và 20.000 lượt ghi mỗi ngày, dư dùng cho vài trăm sinh viên.
- **Cập nhật khung chương trình:** dữ liệu học phần nằm trong `index.html`. Hằng `TREE` chứa cấu trúc 165 TC; hằng `ROAD` chứa lộ trình 8 học kỳ; `SUG` và `TRK` chứa gợi ý theo định hướng. Mã học phần tạm (`SAS30xx`, `SAS20xx`) có mã nội bộ riêng; khi có mã chính thức, chỉ sửa tham số đầu tiên (mã hiển thị), giữ nguyên mã nội bộ ở tham số cuối, để tiến độ đã lưu của sinh viên không bị mất.

Nguồn dữ liệu: ESAS5 – Khung chương trình đào tạo sau điều chỉnh (sheet ESAS2026 và Lộ trình đào tạo) và Cẩm nang tư vấn lộ trình học tập K2026.
