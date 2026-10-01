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
| [`contact-channels.json`](./contact-channels.json) | 4 kênh chat (Zalo OA, Messenger, Viber, WhatsApp) dùng chung cho mọi site quốc gia. Xem mục [Kênh chat dùng chung](#kênh-chat-dùng-chung-contact-channelsjson). |
| [`websites.json`](./websites.json) | Danh sách website của Tự Do Travel **đã chạy thật** + tên miền chính của công ty + tên miền email dùng chung. Xem mục [Danh sách website](#danh-sách-website-websitesjson). |
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

## Kênh chat dùng chung (`contact-channels.json`)

`contact-channels.json` giữ **4 kênh chat** mà khách dùng để liên hệ Tự Do Travel:
Zalo OA, Facebook Messenger, Viber và WhatsApp.

Bốn kênh này **dùng chung cho MỌI site Visa Hub quốc gia**, không tách theo quốc gia
(quyết định 19/09/2026): khách nhắn vào cùng một Zalo OA / Messenger / Viber /
WhatsApp bất kể họ vào site Mỹ, Canada hay nước khác.

**Hotline KHÔNG nằm trong file này.** Hotline là giá trị riêng của từng site, cấu
hình qua biến môi trường `NEXT_PUBLIC_HOTLINE` của site đó.

| Field | Dùng cho |
|---|---|
| `zaloOaId` | ID Zalo Official Account — dùng cho thuộc tính `data-oaid` của widget Zalo |
| `zaloOaUrl` | Link mở chat Zalo OA |
| `messengerUrl` | Link `m.me` mở chat Facebook Messenger |
| `viberUrl` | Link mở chat Viber |
| `whatsappUrl` | Link mở chat WhatsApp |

Cách fetch **giống hệt** `company-profile.json`: fetch ở **build time**, trong
**server component**, không fetch phía client.

```
https://raw.githubusercontent.com/ThanhLee-1984/tudotravel-legal-content/main/contact-channels.json
```

### Site nào fetch trực tiếp, site nào không

- **`visamy-us-hub` (site Mỹ, site đầu tiên) — CHƯA fetch trực tiếp từ đây.** Site
  này đang chạy production thật trên `visamy.com.vn`; không thêm phụ thuộc mạng
  ngoài vào một site đang chạy. Site Mỹ **giữ giá trị local và đồng bộ tay** với
  file này mỗi khi giá trị thay đổi.
- **Các site quốc gia SAU (từ Canada trở đi) — fetch trực tiếp từ đây.**

Hệ quả: khi sửa bất kỳ kênh chat nào trong file này, phải **sửa tay cả bản local của
site Mỹ** rồi redeploy, chứ không chỉ sửa ở repo này.

## Danh sách website (`websites.json`)

Danh sách website của Tự Do Travel **đã go-live** (anh Thạnh duyệt 30/09/2026), dùng cho:
trang "Hệ thống website Tự Do Travel" trên từng site, schema `parentOrganization`, và
liên kết giữa các site theo quy tắc `docs/nhan-ban/LIEN-KET-MANG-LUOI.md` của repo
`visa-hub-template`.

| Field | Ý nghĩa |
|---|---|
| `company.url`, `company.primaryDomain` | Website chính của công ty: **dulichtudo.vn** |
| `company.otherDomains` | Tên miền khác của công ty (dulichtudo.com) |
| `company.emailDomain` | **Mọi site chỉ dùng email liên hệ trên `dulichtudo.vn`** (nhất quán; anh Thạnh chốt 30/09/2026) — không dùng email theo tên miền của từng site |
| `sites[]` | Mỗi site: `domain` (đúng host chính, có/không `www`), `url`, `type` (`visa-country`…), `country` (ISO), `nameVi`, `nameEn`, `launched` (ngày go-live) |

Quy tắc:

- **Chỉ thêm site khi đã go-live** trên tên miền chính (Phiên 10 của site) — repo này
  public, không đưa site đang dựng hay tên miền chưa dùng.
- Site đổi tên miền chính → sửa `domain` + `url`, tên miền cũ không ghi vào đây (chỉ 301).
- Chưa site nào fetch file này (30/09/2026). Khi khung `visa-hub-template` bắt đầu dùng,
  sai định dạng sẽ chặn build giống 2 file trên.

## Khi công ty đổi thông tin

1. Sửa `company-profile.json` **ở repo này** (một chỗ duy nhất), commit và push
   `main`.
2. **Mỗi site phải redeploy lại** để lấy bản mới. Việc này **không tự động**: dữ
   liệu chỉ được đọc ở build time, nên site đang chạy vẫn hiển thị bản cũ cho tới
   lần build kế tiếp.
3. Sau khi redeploy, kiểm tra thực tế trang pháp lý và footer của từng site (không
   chỉ dựa vào việc build xanh).

Danh sách site cần redeploy khi đổi dữ liệu: mọi site Visa Hub đang dùng repo này.
Cập nhật danh sách khi có site mới.

- `visamy.com.vn` — site Mỹ (đồng bộ tay, xem mục trên).
- `visa-anh.com` — site Anh, repo `visa-uk-hub`, Vercel project `visa-uk-hub` (go-live
  29/09/2026). Fetch `company-profile.json` + `contact-channels.json` lúc build
  (`scripts/sync-legal-content.ts`, lỗi/lệch → chặn build). Redeploy trên Vercel là lấy
  bản mới; sau đó commit bản chụp `data/legal/*.json` vào repo site.
- `visa-halan.com` — site Hà Lan, repo `visa-halan-hub`, Vercel project `visa-halan-hub`
  (go-live 30/09/2026). Cùng cách đồng bộ như `visa-anh.com`.
- `visa-duc.com` — site Đức, repo `visa-duc-hub`, Vercel project `visa-duc-hub`
  (go-live 01/10/2026). Cùng cách đồng bộ như `visa-anh.com`.
- `visaitaly.com.vn` — site Ý, repo `visa-italy-hub`, Vercel project `visa-italy-hub`
  (go-live 01/10/2026). Cùng cách đồng bộ như `visa-anh.com`.
- `visataybannha.com` — site Tây Ban Nha, repo `visa-taybannha-hub`, Vercel project
  `visa-taybannha-hub` (go-live 02/10/2026). Cùng cách đồng bộ như `visa-anh.com`.

## Lưu ý

- **Repo public.** Chỉ đưa vào đây thông tin doanh nghiệp vốn đã công khai (đăng
  ký kinh doanh, giấy phép lữ hành, địa chỉ, email liên hệ chính thức). Không đưa
  API key, webhook, mật khẩu, thông tin khách hàng hay dữ liệu nội bộ.
- **Không thêm field ngoài dữ liệu đã xác nhận.** Chỉ thêm khi Tự Do Travel duyệt và
  ghi rõ nguồn.
- Nội dung ở đây không phải tư vấn pháp lý. Nội dung pháp lý của từng site quốc gia
  phải được Tự Do Travel duyệt trước khi lên domain thật.
