# Toàn bộ hành trình xây dựng F1.G5 Landing Page

---

## Giai đoạn 1 — Khởi động & Định hướng

### Bạn cung cấp ảnh tham chiếu
- Upload file `godly.website_website_atlantis-technology-245.png` — screenshot website Atlantis Technology
- Đây là bước quan trọng: thay vì mô tả bằng lời, bạn cho AI thấy trực tiếp layout, cấu trúc section, phong cách illustration và typography mà bạn muốn theo
- Yêu cầu **thay đổi tông màu sang tối** (dark tech) thay vì white như bản gốc

---

## Giai đoạn 2 — Lên kế hoạch (`CLAUDE.md`)

### Tạo file CLAUDE.md trước khi viết code

Đây là bước nhiều người bỏ qua — thay vì code ngay, bạn yêu cầu tạo một **design document** trước. Ý nghĩa:

- `CLAUDE.md` là "bản thiết kế kiến trúc" của dự án — bất kỳ AI nào đọc file này đều hiểu ngay dự án cần gì mà không cần giải thích lại
- Định nghĩa **color tokens** (7 màu với tên biến CSS) thay vì dùng hex cứng trong code
- Quy định **typography scale**, **spacing**, **component rules**
- Phác thảo **nội dung chi tiết từng section** — ai, làm gì, layout ra sao
- Định nghĩa **Definition of Done** — checklist để biết khi nào website xem là hoàn chỉnh

> **Ý nghĩa thực tế:** Nếu sau này bạn cần nhờ developer khác tiếp tục, hoặc mở conversation AI mới, chỉ cần đọc `CLAUDE.md` là hiểu toàn bộ dự án.

---

## Giai đoạn 3 — Build website lần đầu

### Tạo `index.html` — single file hoàn chỉnh

Lý do chọn single HTML file thay vì Next.js/React:
- Không cần build step, không cần `npm install`
- Mở thẳng trên browser được ngay
- Deploy lên bất kỳ hosting nào (GitHub Pages, Netlify, FTP server...)
- Phù hợp với landing page tĩnh không cần backend

File bao gồm:
- **CSS** nhúng trong `<style>` — design tokens, layout, animations
- **SVG illustrations** viết tay — không phụ thuộc file ảnh ngoài
- **JavaScript** nhúng trong `<script>` — scroll animation, mobile menu, counter
- **Google Fonts** load qua CDN — Inter + JetBrains Mono

### 7 Sections được xây dựng

| # | Section | Nội dung chính |
|---|---------|----------------|
| 1 | **Hero** | Headline lớn, illustration nhóm kỹ sư, CTAs, social proof |
| 2 | **Pain Points** | "You're a CTO..." — 2 alternating rows + illustrations |
| 3 | **Solution** | Hub diagram + 3-step process (01 Discover / 02 Match / 03 Deliver) |
| 4 | **Value Prop** | 4 cards: Best Practices, Fast Scaling, Deep Expertise, Always Available |
| 5 | **Delivery Model** | 2-column: headline + 4 feature rows (Embedded, Agile, Ownership, Docs) |
| 6 | **Outcomes** | "Chaos solved." + 3 cards + metrics **50+ / 200+ / 95%** |
| 7 | **Final CTA** | Gradient heading, pulse button, contact info |

---

## Giai đoạn 4 — Vòng lặp Screenshot & Sửa lỗi

### Cài Puppeteer để chụp screenshot tự động

Đây là bước kỹ thuật quan trọng — thay vì mở browser tay mỗi lần, AI tự chụp ảnh để so sánh:

```bash
npm install puppeteer
# → viết script Node.js → mở browser headless → chụp PNG
```

> **Ý nghĩa:** Tạo ra vòng lặp "build → screenshot → so sánh → sửa" hoàn toàn tự động, không cần bạn làm thủ công

### Lần chụp 1 — Phát hiện lỗi lớn
- Tất cả sections bên dưới Hero đều **bị ẩn hoàn toàn** (màn hình đen)
- Nguyên nhân: CSS animation `opacity: 0` + JavaScript IntersectionObserver chưa trigger vì không có scroll
- Fix: Script tự inject JavaScript để force tất cả elements hiển thị trước khi chụp

### Lần chụp 2 — Phát hiện lỗi class mismatch
- CSS dùng class `.fi.v` nhưng script cũ thêm class `.visible`
- Sections vẫn ẩn một phần
- Fix: Đồng bộ lại tên class + dùng `el.style.opacity = "1"` để chắc chắn

### Lần chụp 3 — Phát hiện layout không khớp reference
- Pain Points section dùng grid 2×2 card — khác với reference (alternating rows)
- Metrics hiển thị `0+` thay vì `50+` vì counter animation bị capture giữa chừng
- Fix: Redesign Pain Points thành alternating layout + pre-set giá trị cuối cho metrics

### Lần chụp 4 — Chụp từng section riêng lẻ
- Scroll đến từng section: pain, solution, value, delivery, outcomes, CTA
- So sánh chi tiết với reference từng phần
- Xác nhận design đã sát với inspiration

---

## Giai đoạn 5 — Cập nhật nội dung theo yêu cầu

### Đổi tên: Project Insight → F1.G5
- Cập nhật `<title>`, logo text, logo mark (`PI` → `G5`), footer copyright
- Dùng `replace_all: true` để đổi tất cả vị trí cùng lúc

### Thêm thông tin liên hệ: `G5.DCL@fpt.com` · `0912 111 888`
- Bổ sung vào **2 vị trí chiến lược**:
  - **CTA Section** (cuối trang) — nơi khách hàng có intent cao nhất
  - **Footer** — nơi người dùng tìm thông tin liên lạc theo thói quen
- Dùng `href="mailto:"` và `href="tel:"` để click là mở email/gọi điện trên mobile

---

## Giai đoạn 6 — Chuẩn bị Deploy

### Tạo `.gitignore`
- Loại bỏ `node_modules/`, screenshot PNG, script tạm khỏi repository
- Chỉ giữ lại `index.html` — thứ duy nhất cần thiết để website chạy
- Ý nghĩa: Repo gọn, không upload hàng trăm MB `node_modules` lên GitHub

### Git init + commit
```bash
git init          # Khởi tạo git repository local
git config        # Cấu hình tên + email (do máy chưa set)
git add           # Stage các file cần commit
git commit        # Tạo snapshot code tại thời điểm này
```

---

## Giai đoạn 7 — Deploy lên GitHub Pages

### Push code lên GitHub
```bash
git remote add origin   # Kết nối repo local với GitHub
git branch -M main      # Đổi tên branch thành main
git push -u origin main # Upload code lên GitHub
```

### Bật GitHub Pages
- Settings → Pages → chọn branch `main`
- GitHub tự động build và host static file
- Website live tại `https://letuannsn-fpt.github.io/f1g5/`
- **Hoàn toàn miễn phí**, tự động cập nhật mỗi khi push code mới

---

## Tổng kết

| Giai đoạn | Công cụ | Kết quả |
|-----------|---------|---------|
| Lên kế hoạch | CLAUDE.md | Design document |
| Build | HTML/CSS/JS thuần | 950 dòng code |
| Test | Puppeteer + Node.js | 4 vòng lặp screenshot |
| Cập nhật nội dung | Claude Code editor | Tên + contact info |
| Quản lý code | Git | Version control |
| Hosting | GitHub Pages | URL public, miễn phí |

### Kết quả cuối cùng
- 1 file `index.html` (~950 dòng)
- Zero dependencies (chỉ Google Fonts CDN)
- Fully responsive (mobile / tablet / desktop)
- Animations: fade-in scroll, counter metrics, pulse CTA button
- **Live:** https://letuannsn-fpt.github.io/f1g5/
