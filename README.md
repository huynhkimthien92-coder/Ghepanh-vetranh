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

### Dự án
Mỗi **dự án** lưu riêng: ảnh mẫu, khung 4 góc, lưới nắn cong, vùng khoanh, vật đã chọn và toàn bộ cài đặt (thu phóng, nét viền, lưới ô, độ đậm…). Mọi chỉnh sửa **tự lưu** vào dự án đang mở, ngay trên trình duyệt của máy đó (IndexedDB).
- **Chuyển sang hình khác, giữ hình cũ**: bấm **Dự án mới**. Dự án cũ còn nguyên, muốn quay lại thì chọn trong danh sách.
- **Cùng bức tường, thử hình khác**: bấm **Nhân bản** (giữ nguyên khung và cài đặt) rồi **Chọn / đổi ảnh**.
- **Đổi ảnh của dự án hiện tại**: bấm **Chọn / đổi ảnh** là ảnh được thay, còn khung, lưới, vùng khoanh và cài đặt giữ nguyên. Nếu tỉ lệ ảnh mới khác ảnh cũ, bấm **Khớp khung theo tỉ lệ ảnh**.
- **Mang sang máy khác**: **Xuất file** tạo file `.ghepanh.json` (có cả ảnh). Ở máy kia bấm **Nhập file**.
- Cài đặt chung của máy (mã xem, chất lượng phát, máy chủ kết nối, ẩn/hiện tay cầm) **không** nằm trong dự án.
- Nền trống đã chụp chỉ dùng trong phiên đang mở, không lưu. Mở lại trang thì chụp lại.
- Dữ liệu nằm trong trình duyệt: xoá dữ liệu duyệt web hoặc dùng tab ẩn danh sẽ mất. Dự án quan trọng nên **Xuất file** để dự phòng.

### Các bước

1. **Bật camera** (hoặc **Mở video thử** để test bằng một file video).
2. **Chọn ảnh** (hoặc kéo thả ảnh vào khung). PNG nền trong suốt giữ nguyên độ trong.
3. **Đặt khung vùng**, có 4 cách:
   - **Tự khớp vào vật** (dùng khi vật đã có sẵn hình dáng giống ảnh mẫu: tấm xốp cắt hình, tượng, mảng tường cắt sẵn…): bấm nút rồi **chạm vào vật**. App tách hình dáng vật theo màu và lấy đường viền ngoài của ảnh mẫu (nét vẽ trên nền trắng được tự nối kín; PNG trong suốt thì dùng phần không trong suốt). Sau đó app tự xoay, thu phóng, nắn phối cảnh, tự **lật ngang** nếu vật bị lật, và **uốn lưới** nếu vật cắt hơi méo, để hai đường viền trùng nhau. Vật tách thiếu hoặc lan ra nền thì chỉnh **Độ rộng màu** (mục khoanh vùng) rồi bấm lại.
   - **Tự dò khung**: app tìm hình chữ nhật có thật trong ảnh camera (bảng, khung tranh, cửa, hoặc viền băng keo bạn dán quanh vùng cần vẽ). Có nhiều khung thì bấm lại để chuyển sang khung khác.
   - **Kéo 4 chấm tròn** bằng tay. Khi thả, góc tự **hút vào góc thật gần nhất** (dấu băng keo, góc bảng), tắt được bằng ô “Hút góc…”.
   - **Đặt lại vùng** để về khung mặc định.
   - Ảnh được nắn phối cảnh theo đúng 4 góc đó.
   - **Tỉ lệ thật**: mặc định app **tự tính tỉ lệ thật** của vùng theo phối cảnh, nên khi camera nhìn xiên ảnh vẫn không bị méo. Muốn chính xác tuyệt đối thì chọn *Nhập kích thước vùng* và gõ rộng × cao (cm). Dòng chữ dưới ô chọn cho biết tỉ lệ đang dùng.
   - **Khớp khung theo tỉ lệ ảnh**: thu khung lại (vẫn giữ phối cảnh) để ảnh lấp đầy vừa khít, không thừa viền.
   - **Bề mặt không phẳng (tượng, cột, tường cong, góc nhà)**: bật **Lưới nắn cong**, chọn cỡ lưới, rồi kéo các **chấm vàng** tới đúng điểm mốc trên bề mặt (đỉnh vai, khuỷu tay, mép cột…). Ảnh sẽ uốn mềm theo đúng các điểm đó, nội suy Catmull-Rom đi qua từng điểm. Ảnh khi đó được “chiếu” đúng theo **góc nhìn của camera**, nên phải giữ camera cố định.
   - **Chỉ cho mẫu hiện trên vật / vùng tự do**:
     - *Chỉ trong vùng khoanh tay*: bấm **Bắt đầu khoanh**, chạm lần lượt quanh viền (kéo chấm cam để sửa), xong bấm **Xong**.
     - *Chỉ trên vật chạm chọn*: bấm **Chạm chọn vật** rồi chạm vào vật (ví dụ tượng chưa sơn). App tự tách vùng cùng màu với điểm chạm. Còn hụt chỗ nào thì **Chạm thêm vùng**; lan quá thì giảm **Độ rộng màu**.
4. **Chỉnh ảnh như sticker**: thanh nhỏ dưới khung camera có 3 chế độ.
   - **Ảnh** (mặc định): kéo **ô vuông ở góc ảnh** để phóng to/thu nhỏ (giữ tỉ lệ), kéo **nút tròn** để xoay (gần 0° tự hút về thẳng), kéo bên trong để dời, chụm 2 ngón để phóng/xoay. Ảnh luôn nằm đúng phối cảnh của tường, và nhãn dưới ảnh cho biết kích thước.
     - **Đúng kích thước thật**: ở mục 3 chọn *Tỉ lệ thật: nhập kích thước vùng*, nhập kích thước khung mặt phẳng (ví dụ khung băng keo 200 × 150 cm), rồi gõ **chiều rộng ảnh mong muốn (cm)** ở mục 4, chiều cao tự tính.
   - **Mặt phẳng**: chỉ hiện 4 chấm góc. Dùng để khớp một hình chữ nhật thật trên tường, giúp app biết phối cảnh. Kéo bên trong để dời cả khung.
   - **Lưới**: chỉ hiện khi bật lưới nắn cong. Trên tường phẳng thì tắt lưới (hoặc bấm *Làm phẳng lại*) cho gọn.
   Ngoài ra vẫn có các thanh trượt: chọn *Vừa khung / Phủ kín / Kéo giãn*, rồi chỉnh *Thu phóng, Xoay, Lệch ngang/dọc, Độ đậm*. Cũng dùng được: lăn chuột hoặc chụm 2 ngón để phóng/xoay, kéo bên trong vùng để dời ảnh (chuyển sang “dời cả vùng” để dời luôn khung).
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
| Tự khớp vào vật: tấm xốp trên sàn gạch hoa, xoay 25° + nhìn xiên | viền lệch trung bình ≈ 2 px, chạy 0,4 s |
| Tự khớp khi tấm xốp bị lật ngang so với bản vẽ | tự nhận ra và lật ảnh, viền lệch ≈ 2 px |
| Tự khớp khi tấm xốp cắt méo (không giống hệt bản vẽ) | tự uốn lưới, viền lệch còn ≈ 1,4 px (trước khi uốn 4,2 px) |
| Camera tối đi 40% + rung 1 px + tay thò vào | bản cũ chỉ còn 15% nét mẫu; bản mới giữ 100% nét ở chỗ không che, ẩn đúng 99% trên tay |
| Dự án: tạo mới, quay lại, tải lại trang, xuất/nhập file | khung, ảnh, cài đặt khôi phục đúng 100% |
| Lưới nắn: bật lưới phẳng / kéo điểm giữa | phẳng thì giống hệt khi tắt; kéo điểm thì ảnh uốn theo đúng điểm |
| Khoanh vùng / chạm chọn tấm bảng | ngoài vùng hiện hình camera; chọn vật đúng tấm bảng (25% khung), chừa phần người đứng chắn |
| Tỉ lệ thật khi nhìn xiên (mô phỏng camera pinhole) | đúng 1,500 / 0,625 / 4,000, trong khi đo theo hình trên màn sai tới 35% |

## 6. Mẹo & giới hạn
- **Camera tự đổi sáng** (iPhone tự chỉnh phơi sáng khi tay/người vào khung): app **tự bù sáng** mỗi khung hình, HUD hiện “bù sáng ×…”. So sánh nền cũng chịu được camera rung/bám lệch khoảng 2 px. Nếu vẫn báo nhầm: chụp nền lại hoặc giảm Độ nhạy.
- **Khi vẽ bằng tay:** chọn *Chỗ bị che: mẫu mờ đi* (mặc định), như vậy tay vẫn được nhận ra nhưng nét mẫu chỉ mờ đi trên tay chứ không mất, và bạn vẫn thấy nét ngay dưới đầu bút. Bấm **Chế độ vẽ tường** để tắt luôn so sánh nền, vì nét bút/sơn mới vẽ sẽ khác nền đã chụp và làm mất mẫu đúng chỗ vừa vẽ.
- Vật che **cùng màu với nền** (áo trắng trước tường trắng) thì so sánh nền khó bắt, khi đó AI người sẽ đỡ phần này.
- Chế độ cầm tay cần vùng hoặc xung quanh vùng có **hoạ tiết/cạnh**. Tường trơn tuyệt đối thì không có gì để bám, hãy dùng chân máy hoặc kéo vùng rộng ra.
- Vật được đặt **cố định lâu** trước vùng sẽ tiếp tục được coi là vật che, bấm “Chụp nền trống” lại nếu muốn ảnh phủ lên nó.
- Máy yếu: app tự giãn nhịp chạy AI (HUD góc trái hiện thời gian xử lý).
- Lưới nắn cong là để uốn ảnh theo bề mặt **nhìn từ camera**: hình vẽ ra sẽ đúng khi đứng ở vị trí camera nhìn vào, giống máy chiếu. Với tượng 3D, mặt nào camera không thấy thì cần đặt camera ở góc khác và làm thêm một dự án riêng cho góc đó (dùng **Nhân bản**).
- Tự khớp so sánh **đường viền ngoài**, nên ảnh mẫu cần có viền rõ (nét vẽ khép kín hoặc PNG nền trong suốt). Ảnh chụp kín cả khung, không có nền trắng, thì app chỉ khớp được theo hình chữ nhật của ảnh.
- Chạm chọn vật tách theo màu nên hợp nhất với vật **một màu** (tượng thạch cao, mảng tường trơn). Vật nhiều hoa văn thì dùng khoanh tay.
- Tự dò khung / hút góc cần tải OpenCV (~10 MB) ở lần dùng đầu. Tường trơn không có mốc thì không có gì để dò: dán 4 dấu băng keo ở 4 góc vùng cần vẽ là dò và hút góc rất chuẩn.
- Tỉ lệ thật tự tính chính xác nhất khi vùng là hình chữ nhật thật (đánh dấu bằng băng keo vuông góc) và camera nhìn xiên theo cả 2 chiều. Nếu camera chỉ nghiêng 1 chiều, app dùng tiêu cự mặc định nên sai lệch khoảng vài %. Cần chuẩn tuyệt đối thì nhập kích thước.
