# TEMPLATE — 3 tuyên bố pháp lý bắt buộc cho site quốc gia mới

Hướng dẫn viết 3 tuyên bố pháp lý bắt buộc cho một site Visa Hub theo quốc gia
của Tự Do Travel (VD: Canada, Úc, Anh, Schengen...).

Nguyên tắc chung:

- Thông tin **doanh nghiệp** (tên pháp lý, MST, giấy phép, địa chỉ, email bảo
  mật, luật bảo vệ dữ liệu) lấy từ [`company-profile.json`](./company-profile.json)
  — **không gõ tay lại** vào site.
- Phần **gắn với quốc gia đích** (cơ quan chính phủ, Đại sứ quán/Lãnh sự quán,
  cổng/trang chính thức, quy định phí) **PHẢI viết riêng** cho từng quốc gia.
  **Không dịch/copy nguyên văn từ site Mỹ** rồi thay tên nước — dễ sót tên cơ
  quan Mỹ, sai thẩm quyền, sai luật.
- Mốc thời gian luôn ghi "tham khảo", quy về thẩm quyền của cơ quan chính phủ.
- Từ cấm trong mọi nội dung: `cam kết đậu`, `bao đậu`, `chạy visa`, `chắc rớt`,
  `hồ sơ yếu quá`, `luyện phỏng vấn`, `100% thành công` và mọi cách nói ngụ ý bảo
  đảm kết quả visa.

Các đoạn trích bên dưới lấy từ site Mỹ (`visamy-us-hub`) chỉ để **minh hoạ cấu
trúc**, không phải văn bản để sao chép.

---

## Bảng field dùng chung (`company-profile.json`)

| Field | Dùng cho tuyên bố |
|---|---|
| `legalName` | 1, 2, 3 — tên pháp lý đầy đủ, luôn dùng đúng chuỗi này |
| `legalNameEn` / `legalNameAbbr` | Chỉ khi site có bản tiếng Anh / cần tên viết tắt |
| `taxID` | 2 (đơn vị kiểm soát dữ liệu), 3 (nhận diện pháp nhân) |
| `businessLicense.*` | 3 — số, cơ quan cấp, ngày cấp |
| `travelLicense.*` | 3 — loại, số GP, cơ quan cấp, ngày cấp |
| `address.*` | 2 (đơn vị kiểm soát dữ liệu), 3. Bản đầy đủ dùng `addressLocality`; chỗ hẹp (footer, liên hệ) dùng `addressLocalityShort` |
| `privacyContactEmail` | 1, 2 — kênh nhận khiếu nại/yêu cầu dữ liệu |
| `dataProtectionLaw.name` / `.effectiveDate` | 2 |
| `foundingDate` | Không bắt buộc trong 3 tuyên bố; dùng cho trang Giới thiệu/schema |

**Không có trong `company-profile.json`** (mỗi site tự cấu hình, không hardcode
trong nội dung): hotline, email chung, mạng xã hội, logo. Nếu cần trong tuyên bố,
đọc từ cấu hình của site đó.

---

## Tuyên bố 1 — Quyền lợi khách hàng

**Mục đích:** cam kết minh bạch phí, quyền của khách, quy trình phản hồi/khiếu
nại. Ví dụ tham chiếu: `ChinhSach.content.tsx` (site Mỹ, slug `chinh-sach`).

### (a) Field dùng từ `company-profile.json`

- `legalName` — chủ thể cam kết.
- `privacyContactEmail` — kênh gửi khiếu nại, kèm hotline/form liên hệ đọc từ
  cấu hình site.

### (b) Phần PHẢI viết riêng theo quốc gia

- **Cách phân tách phí:** tên và bản chất khoản phí do chính phủ nước đó thu
  (phí lãnh sự / phí xét hồ sơ / phí trung tâm tiếp nhận hồ sơ...) và chính sách
  hoàn phí của nước đó. Site Mỹ ghi phí lãnh sự "không hoàn lại" — **không được
  áp nguyên** sang nước khác khi chưa đối chiếu quy định chính thức.
- **Ranh giới tư vấn:** nêu đúng cơ quan quyết định visa của nước đó, không dùng
  tên cơ quan Mỹ.
- **Quy trình khiếu nại:** thời hạn phản hồi nếu site cam kết con số cụ thể phải
  do Tự Do Travel duyệt; mặc định nên giữ diễn đạt "trong thời gian hợp lý".

### Cấu trúc tham chiếu (site Mỹ, 5 mục)

1. Cam kết minh bạch về phí
2. Quyền lợi khách hàng
3. Quy trình tiếp nhận phản hồi/khiếu nại
4. Tuân thủ pháp luật
5. Liên hệ

### Trích minh hoạ (ngắn)

> "Mọi báo giá phân tách rõ: phí dịch vụ ... và phí lãnh sự/phí thu hộ ..."

Ý cần giữ khi viết cho nước mới: báo giá **luôn tách** phí dịch vụ và phí chính
phủ/thu hộ, và **không phát sinh phí ẩn** ngoài báo giá đã xác nhận.

---

## Tuyên bố 2 — Bảo vệ dữ liệu cá nhân

**Mục đích:** nêu dữ liệu thu thập, mục đích, thời gian lưu, chia sẻ, chuyển ra
nước ngoài, quyền chủ thể dữ liệu. Ví dụ tham chiếu:
`ChinhSachBaoMat.content.tsx` (site Mỹ, slug `chinh-sach-bao-mat`, 12 mục).

### (a) Field dùng từ `company-profile.json`

- `dataProtectionLaw.name` và `dataProtectionLaw.effectiveDate` — căn cứ pháp lý
  (mục Tổng quan và mục Quyền chủ thể dữ liệu).
- `legalName`, `taxID`, `address.streetAddress` + `address.addressLocalityShort`
  — mục "Đơn vị kiểm soát/xử lý dữ liệu".
- `privacyContactEmail` — mục "Cách thực hiện quyền" và "Liên hệ". **Bắt buộc
  dùng field này**, không dùng email chung của công ty.

### (b) Phần PHẢI viết riêng theo quốc gia

- **Mục "Chuyển dữ liệu ra nước ngoài":** liệt kê đúng cơ quan/trung tâm tiếp nhận
  của nước đích (Đại sứ quán/Lãnh sự quán, cơ quan xuất nhập cảnh, trung tâm nộp
  hồ sơ). Site Mỹ nêu USCIS và NVC — **không được giữ lại** trong site nước khác.
- **Mục "Loại dữ liệu thu thập":** chỉ liệt kê loại dữ liệu mà hồ sơ visa nước đó
  thực sự yêu cầu (VD sinh trắc học, khám sức khỏe, lý lịch tư pháp có bắt buộc
  hay không tuỳ nước).
- **Mục "Mục đích xử lý" / "Tiết lộ cho bên thứ ba":** tên cơ quan chức năng và đối
  tác dịch vụ đi kèm của nước đó.

### Phần cần Tự Do Travel chốt, KHÔNG suy ra từ profile

- **Thời hạn lưu trữ:** site Mỹ ghi hủy hồ sơ nhạy cảm trong 180 ngày sau khi kết
  thúc dịch vụ. Đây là chính sách nội bộ, không nằm trong `company-profile.json`;
  site mới phải xác nhận lại con số trước khi dùng.
- **Ngày Quốc hội thông qua luật:** site Mỹ có ghi ngày thông qua (26/6/2025);
  `company-profile.json` chỉ có ngày hiệu lực. Chỉ ghi ngày thông qua khi đã đối
  chiếu văn bản gốc.
- **Điều khoản trích dẫn:** site Mỹ trích "Điều 4" cho danh sách quyền chủ thể dữ
  liệu. Đối chiếu văn bản luật trước khi giữ số điều.

### Trích minh hoạ (ngắn)

> "Chính sách được xây dựng theo Luật Bảo vệ dữ liệu cá nhân số 91/2025/QH15 ..."

Trong site mới, chuỗi này phải render từ `dataProtectionLaw.name`, không gõ tay.

---

## Tuyên bố 3 — Tuân thủ pháp luật (miễn trừ trách nhiệm + không phải cơ quan chính phủ)

**Mục đích:** nói rõ Tự Do Travel là công ty tư vấn tư nhân, không phải cơ quan
chính phủ, không đại diện Đại sứ quán/Lãnh sự quán; quyết định visa thuộc thẩm
quyền cơ quan nước đích. Ví dụ tham chiếu: `MienTruTrachNhiem.content.tsx` (site
Mỹ, slug `mien-tru-trach-nhiem`, 6 mục) và mục "Tuân thủ pháp luật" của
`ChinhSach.content.tsx`.

### (a) Field dùng từ `company-profile.json`

- `legalName` — chủ thể tuyên bố.
- `businessLicense.number`, `.issuer`, `.issueDate` — giấy chứng nhận đăng ký
  doanh nghiệp.
- `travelLicense.type`, `.number`, `.issuer`, `.issueDate` — giấy phép kinh doanh
  lữ hành. Chỉ ghi các chi tiết này khi trang thực sự nhắc tới giấy phép; đừng ghi
  chung chung "giấy phép hợp pháp" nếu đã có số thật để hiển thị.
- `taxID`, `address.*` — nhận diện pháp nhân.

### (b) Phần PHẢI viết riêng theo quốc gia

- **Tên cơ quan quyết định visa:** Đại sứ quán / Lãnh sự quán / cơ quan di trú của
  nước đích. Câu chủ chốt ở site Mỹ nói quyết định visa thuộc thẩm quyền cơ quan
  lãnh sự Hoa Kỳ — site mới phải thay bằng đúng tên cơ quan tương ứng, không "dịch
  từ Mỹ".
- **Liên kết bên thứ ba:** danh sách website chính thức của nước đích (site Mỹ
  liệt kê `travel.state.gov`, `uscis.gov`, `ustraveldocs.com`) — thay bằng cổng
  chính thức của nước đó, kiểm tra từng tên miền trước khi đưa vào.
- **Phí và thời gian xử lý:** nói rõ do cơ quan chính phủ nước đích quy định, có
  thể thay đổi; nội dung trên site chỉ mang tính tham khảo.
- **Phí không hoàn lại:** chỉ khẳng định khi quy định của nước đích đúng như vậy.

### Trích minh hoạ (ngắn)

> "không phải cơ quan chính phủ, không đại diện Đại sứ quán/Lãnh sự quán Hoa Kỳ"

Giữ nguyên **cấu trúc câu**, thay **tên nước/cơ quan**. Ý "không cam kết, không
bảo đảm kết quả visa" áp dụng cho mọi quốc gia, giữ nguyên.

---

## Checklist trước khi publish site quốc gia mới

- [ ] Mọi thông tin doanh nghiệp render từ `company-profile.json`, không gõ tay.
- [ ] Đã rà và loại bỏ mọi tên cơ quan/trang web/luật của nước khác (đặc biệt Mỹ:
      USCIS, NVC, travel.state.gov, ustraveldocs.com, "Hoa Kỳ").
- [ ] Các con số chính sách nội bộ (thời hạn lưu trữ, thời hạn phản hồi) đã được
      Tự Do Travel xác nhận riêng cho site này.
- [ ] Không có từ cấm; mốc thời gian đều ghi "tham khảo".
- [ ] Tự Do Travel duyệt nội dung pháp lý trước khi lên domain thật.
