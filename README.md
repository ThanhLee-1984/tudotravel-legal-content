# tudotravel-legal-content

Nguồn dữ liệu doanh nghiệp **dùng chung** cho mọi site Visa Hub theo quốc gia của
Tự Do Travel (site Mỹ `visamy-us-hub` và các site quốc gia sau này).

Thông tin pháp lý của công ty (tên, mã số thuế, giấy phép, địa chỉ, email bảo mật,
luật bảo vệ dữ liệu áp dụng) được giữ ở **một chỗ duy nhất** thay vì lặp lại và
lệch nhau giữa các site.

## Nội dung repo

| File | Vai trò |
|---|---|
| [`company-profile.json`](./company-profile.json) | Dữ liệu doanh nghiệp đã xác nhận qua audit Task-167/170 của `visamy-us-hub`. Đây là nguồn dùng chung. |
| [`TEMPLATE-tuyen-bo-phap-ly.md`](./TEMPLATE-tuyen-bo-phap-ly.md) | Hướng dẫn viết 3 tuyên bố pháp lý bắt buộc (quyền lợi khách hàng, bảo vệ dữ liệu cá nhân, tuân thủ pháp luật) cho một site quốc gia mới. |

## Cách site mới lấy dữ liệu

Fetch file JSON qua `raw.githubusercontent.com` **lúc build**, trong **server
component** (hoặc bước build/`generateMetadata`/`generateStaticParams`).
**Không fetch phía client** (browser), vì:

- Nội dung pháp lý phải có sẵn trong HTML server-render để Google và công cụ AI
  đọc được, không phụ thuộc JavaScript.
- Tránh nhấp nháy nội dung, phụ thuộc mạng và CORS ở phía người dùng.

URL:

```
https://raw.githubusercontent.com/ThanhLee-1984/tudotravel-legal-content/main/company-profile.json
```

Ví dụ (server component, Next.js App Router):

```tsx
// lib/company-profile.ts — chạy phía server
export async function getCompanyProfile() {
  const res = await fetch(
    "https://raw.githubusercontent.com/ThanhLee-1984/tudotravel-legal-content/main/company-profile.json",
    { cache: "force-cache" } // chốt dữ liệu lúc build
  );
  if (!res.ok) throw new Error(`company-profile fetch failed: ${res.status}`);
  return res.json();
}
```

Gợi ý khi tích hợp:

- **Fail build khi fetch lỗi** (như ví dụ trên), không âm thầm render rỗng — trang
  pháp lý thiếu thông tin doanh nghiệp còn tệ hơn build thất bại.
- Có thể **pin theo commit SHA** thay vì `main` nếu muốn site không đổi khi repo
  này thay đổi ngoài ý muốn: thay `main` trong URL bằng SHA của commit.
- Không lưu bản sao JSON vào code của site rồi sửa tay; nếu cần sửa, sửa ở repo
  này.

## Khi công ty đổi thông tin

1. Sửa `company-profile.json` **ở repo này** (một chỗ duy nhất), commit và push
   `main`.
2. **Mỗi site phải redeploy lại** để lấy bản mới. Việc này **không tự động**: dữ
   liệu chỉ được đọc ở build time, nên site đang chạy vẫn hiển thị bản cũ cho tới
   lần build kế tiếp.
3. Sau khi redeploy, kiểm tra thực tế trang pháp lý và footer của từng site (không
   chỉ dựa vào việc build xanh).

Danh sách site cần redeploy khi đổi dữ liệu: mọi site Visa Hub đang dùng repo này
(hiện có `visamy.com.vn`). Cập nhật danh sách khi có site mới.

## Lưu ý

- **Repo public.** Chỉ đưa vào đây thông tin doanh nghiệp vốn đã công khai (đăng
  ký kinh doanh, giấy phép lữ hành, địa chỉ, email liên hệ chính thức). Không đưa
  API key, webhook, mật khẩu, thông tin khách hàng hay dữ liệu nội bộ.
- **Không thêm field ngoài dữ liệu đã xác nhận.** Chỉ thêm khi Tự Do Travel duyệt và
  ghi rõ nguồn.
- Nội dung ở đây không phải tư vấn pháp lý. Nội dung pháp lý của từng site quốc gia
  phải được Tự Do Travel duyệt trước khi lên domain thật.
