# Ghép ảnh vào camera — hướng dẫn

App là một file duy nhất, `index.html`, chạy trên trình duyệt (Chrome / Edge / Safari bản mới, máy tính hoặc điện thoại). Không cần server riêng, không cần API trả phí.

## 1. Chạy app

Trình duyệt chỉ cho mở camera khi trang chạy qua **https://** hoặc **localhost**.

**Trên máy tính (nhanh nhất)**
```bash
cd thu-muc-chua-index.html
python -m http.server 8000
```
Mở `http://localhost:8000` trên Chrome.

**Deploy lên GitHub Pages (https, miễn phí)**
1. Đưa `index.html` (và `README.md`) vào **thư mục gốc** của repo, push lên nhánh `main`.
2. Repo → **Settings → Pages** → *Source: Deploy from a branch* → Branch `main`, thư mục `/ (root)` → Save.
3. Sau 1–2 phút app chạy tại `https://<tên-github>.github.io/<tên-repo>/`. Mở link này trên điện thoại làm máy quay.

(Vercel / Netlify Drop cũng được, chỉ cần https.)

Lần đầu app cần internet để tải thư viện (MediaPipe ~ vài MB, OpenCV ~10 MB khi dùng chế độ cầm tay, mô hình AI 16 MB). Sau đó trình duyệt sẽ cache lại.

## 2. Cách dùng

1. **Bật camera** (hoặc **Mở video thử** để test bằng một file video).
2. **Chọn ảnh** (hoặc kéo thả ảnh vào khung). PNG nền trong suốt giữ nguyên độ trong.
3. **Đặt khung vùng**, có 3 cách:
   - **Tự dò khung**: app tìm hình chữ nhật có thật trong ảnh camera (bảng, khung tranh, cửa, hoặc viền băng keo bạn dán quanh vùng cần vẽ). Có nhiều khung thì bấm lại để chuyển sang khung khác.
   - **Kéo 4 chấm tròn** bằng tay. Khi thả, góc tự **hút vào góc thật gần nhất** (dấu băng keo, góc bảng), tắt được bằng ô “Hút góc…”.
   - **Đặt lại vùng** để về khung mặc định.
   - Ảnh được nắn phối cảnh theo đúng 4 góc đó.
   - **Tỉ lệ thật**: mặc định app **tự tính tỉ lệ thật** của vùng theo phối cảnh, nên khi camera nhìn xiên ảnh vẫn không bị méo. Muốn chính xác tuyệt đối thì chọn *Nhập kích thước vùng* và gõ rộng × cao (cm). Dòng chữ dưới ô chọn cho biết tỉ lệ đang dùng.
   - **Khớp khung theo tỉ lệ ảnh**: thu khung lại (vẫn giữ phối cảnh) để ảnh lấp đầy vừa khít, không thừa viền.
4. **Thu phóng cho vừa**: chọn *Vừa khung / Phủ kín / Kéo giãn*, rồi chỉnh *Thu phóng, Xoay, Lệch ngang/dọc, Độ đậm*. Cũng dùng được: lăn chuột hoặc chụm 2 ngón để phóng/xoay, kéo bên trong vùng để dời ảnh (chuyển sang “dời cả vùng” để dời luôn khung).
5. **Chụp nền trống**: bấm khi vùng đặt ảnh **không có ai/vật gì đứng trước**.
6. Chọn **kiểu camera**:
   - *Cố định (chân máy)*: vùng đứng yên tuyệt đối trên khung hình, nên không bao giờ bị trôi hay mất.
   - *Cầm tay / di chuyển*: đặt 4 góc lúc vùng đang trống → bấm **Khoá vùng & bám theo**. Ảnh sẽ bám theo mặt phẳng khi camera di chuyển.
7. **Xuất**: Chụp ảnh (PNG), Quay video (WebM/MP4), Toàn màn hình (phím `F`; phím `H` ẩn/hiện tay cầm khi trình chiếu).
8. **Xem trực tiếp trên máy khác**: xem mục 3 bên dưới.

Trên điện thoại cầm dọc, khung camera **luôn dính ở trên cùng**, phần cài đặt cuộn bên dưới, nên chỉnh tới đâu thấy kết quả tới đó. Cầm ngang thì cài đặt nằm cột bên phải.

Cài đặt và ảnh được lưu lại trong trình duyệt, mở lại trang sẽ còn nguyên.

## 3. Vẽ tường theo mẫu với máy xem trực tiếp

**Bố trí:** máy quay (điện thoại/laptop) đặt **cố định trên chân máy** nhìn cả bức tường, người vẽ cầm **điện thoại thứ hai** đứng tại tường.

1. Trên máy quay: bật camera, chọn ảnh, kéo 4 góc khớp vùng tường cần vẽ, chỉnh thu phóng cho đúng kích thước.
2. Bấm **Chế độ vẽ tường**: mẫu chuyển sang *nét viền*, AI người bật (tay và người hiện trước mẫu), *So sánh nền* tắt (vì nét sơn mới sẽ làm tường khác nền đã chụp).
3. Bấm **Phát cho máy khác** → hiện **mã QR + mã 6 ký tự**.
4. Điện thoại của người vẽ: quét QR (hoặc mở app → nhập mã → *Xem*). Màn hình chỉ còn hình trực tiếp và thanh nút:
   - **Ẩn/Hiện mẫu, Mờ −/Đậm +, Nét viền/Ảnh màu, Lưới, Nhấp nháy**: điều khiển ngược về máy quay, nên không phải đi lại chỗ chân máy.
   - Chụm 2 ngón để **phóng to chi tiết**, kéo để dời, **Xoay** khi cầm điện thoại dọc, chạm đúp hoặc *Vừa màn* để về như cũ.
5. Mẹo khi tô:
   - Bật **Nhấp nháy** để thấy nét đã vẽ lệch so với mẫu chỗ nào.
   - **Lưới ô vuông** giúp chia tường kẻ ô trước (đánh dấu góc ô bằng phấn), sau đó vẽ theo từng ô.
   - Chọn **màu nét** tương phản với tường; *Chi tiết nét* thấp = chỉ đường chính, cao = nhiều chi tiết.
   - Không được xê dịch chân máy. Nếu lỡ chạm, kéo lại 4 góc theo dấu phấn đã đánh.

**Kết nối:** hai máy kết nối trực tiếp với nhau (WebRTC), hình không đi qua máy chủ nào, độ trễ thường dưới nửa giây khi cùng Wi-Fi. Máy chủ PeerJS miễn phí chỉ dùng để hai máy tìm thấy nhau.
- Có thể nhiều điện thoại xem cùng lúc.
- **Mã xem được giữ nguyên** giữa các lần mở app, nên in QR dán ở tường dùng được nhiều buổi. Bấm *Đổi mã* nếu muốn mã mới.
- Máy quay tải lại trang hoặc rớt mạng thì máy xem **tự nối lại**, không cần quét lại.
- Giữ app máy quay **luôn mở trên màn hình** (app tự giữ màn hình sáng). Nếu chuyển sang app khác, trình duyệt sẽ ngừng phát hình.
- *Chất lượng* 3–6 Mb là đủ cùng Wi-Fi. Mạng yếu thì giảm xuống.
- Hai máy khác mạng (4G ↔ Wi-Fi) đa số vẫn nối được. Nếu báo lỗi hoặc không có hình, vào **Nâng cao** thêm TURN server (ví dụ tài khoản miễn phí của metered.ca hoặc tự chạy coturn), hoặc tự chạy máy chủ kết nối: `npx peerjs --port 9000` (phải có https/wss nếu app chạy trên GitHub Pages).

## 4. Vì sao không bị mất và không nhận nhầm khi có người/vật đi ngang

**a) Hiện người/vật đè lên ảnh (che khuất đúng thứ tự trước–sau)**, dùng hai lớp và lấy kết quả nào phát hiện được:
- *AI nhận diện người* (MediaPipe, chạy ngay trên máy): người đi qua sẽ hiện trước ảnh. Chọn mô hình “Người + vật phổ biến” nếu cần thêm xe, ghế, thú cưng…
- *So sánh với nền trống*: pixel nào trong vùng khác hẳn nền đã chụp thì coi là vật che, nên bắt được **mọi loại vật**, kể cả thứ AI không biết tên. Nền tự cập nhật chậm theo ánh sáng, chỉ ở những chỗ không bị che.
- Bật **Tô đỏ vùng bị che** để nhìn mask rồi chỉnh **Độ nhạy**.

**b) Vùng không trôi theo người đi qua (chế độ cầm tay)**
- Bám bằng ~100–250 điểm đặc trưng trên vùng và phần tường quanh nó (Lucas-Kanade).
- Điểm nằm trên người (theo AI) bị loại; điểm bị che/trượt bị loại nhờ kiểm tra tiến–lùi.
- Vị trí vùng tính bằng homography + RANSAC: vật đi ngang chuyển động khác mặt phẳng nên bị loại như nhiễu, không “kéo” được ảnh theo.
- Vị trí mới quá vô lý (nhảy xa, méo, lật) thì bị từ chối. Khi đó app **giữ nguyên vị trí cũ** (nhãn “Bị che — giữ nguyên vị trí”).
- Khi bị che lâu hoặc che kín: app không đoán mò mà chỉ chấp nhận vị trí mới khi đối chiếu đặc trưng ORB với khung tham chiếu lúc khoá. Ngoài ra cứ ~1 giây app tự kiểm tra lại bằng ORB một lần, để sửa nếu có khoá lệch.
- Góc được làm mượt bằng bộ lọc One-Euro nên ảnh không bị rung.

## 5. Kết quả test (video giả lập 640×360: camera lắc + xoay + phóng, người đi ngang qua vùng, rồi che kín ống kính 1,2 giây trong lúc camera dịch đi)

| Tình huống | Kết quả |
|---|---|
| Camera di chuyển, không che | sai số góc 1,5–4 px |
| Người đi ngang che ~½ vùng | vẫn “Đang bám”, sai số 1,6–3,3 px; người hiện đè lên ảnh |
| Che kín cả khung | giữ nguyên vị trí cũ, không nhảy |
| Bỏ che (camera đã dịch 60 px) | tìm lại đúng vị trí trong ~0,3 s, sai số 0,9–3 px |
| Phát sang máy xem (2 trình duyệt riêng) | nhận hình 640×360 liên tục; nút trên máy xem đổi mẫu bên máy quay tức thì |
| Máy quay tải lại trang | giữ nguyên mã, máy xem tự nối lại sau ~1,4 s |
| Tự dò khung (tấm bảng 280×200 trên tường nhiều hoạ tiết) | tìm đúng, lệch 4 px; tỉ lệ đo 1,39 (thật 1,40) |
| Hút góc khi thả lệch ~15 px | về đúng góc, còn lệch 1,5 px |
| Tỉ lệ thật khi nhìn xiên (mô phỏng camera pinhole) | đúng 1,500 / 0,625 / 4,000, trong khi đo theo hình trên màn sai tới 35% |

## 6. Mẹo & giới hạn
- **Camera tự đổi sáng** làm “so sánh nền” báo nhầm: bấm **Khoá sáng** (nếu camera hỗ trợ), giữ bật “Tự cập nhật nền”, hoặc chụp nền lại.
- Vật che **cùng màu với nền** (áo trắng trước tường trắng) thì so sánh nền khó bắt, khi đó AI người sẽ đỡ phần này.
- Chế độ cầm tay cần vùng hoặc xung quanh vùng có **hoạ tiết/cạnh**. Tường trơn tuyệt đối thì không có gì để bám, hãy dùng chân máy hoặc kéo vùng rộng ra.
- Vật được đặt **cố định lâu** trước vùng sẽ tiếp tục được coi là vật che, bấm “Chụp nền trống” lại nếu muốn ảnh phủ lên nó.
- Máy yếu: app tự giãn nhịp chạy AI (HUD góc trái hiện thời gian xử lý).
- Tự dò khung / hút góc cần tải OpenCV (~10 MB) ở lần dùng đầu. Tường trơn không có mốc thì không có gì để dò: dán 4 dấu băng keo ở 4 góc vùng cần vẽ là dò và hút góc rất chuẩn.
- Tỉ lệ thật tự tính chính xác nhất khi vùng là hình chữ nhật thật (đánh dấu bằng băng keo vuông góc) và camera nhìn xiên theo cả 2 chiều. Nếu camera chỉ nghiêng 1 chiều, app dùng tiêu cự mặc định nên sai lệch khoảng vài %. Cần chuẩn tuyệt đối thì nhập kích thước.
