# MusicA — Thương mại điện tử tác quyền âm nhạc

> Vue 3 prototype của **MusicA**, nền tảng thương mại điện tử kết nối người mua (Creator, doanh nghiệp, SME) với nghệ sĩ / nhà phát hành để **giao dịch tác quyền** tác phẩm âm nhạc trên môi trường số.

![tech](https://img.shields.io/badge/Vue-3.5-42b883) ![vite](https://img.shields.io/badge/Vite-6-646cff) ![pinia](https://img.shields.io/badge/Pinia-2-ffd859) ![router](https://img.shields.io/badge/Vue%20Router-4-42b883)

---

## ✨ Tính năng prototype hiện có

| Trang             | Mô tả                                                                             |
|-------------------|-----------------------------------------------------------------------------------|
| `/`               | Hero, search, filter theo thể loại, grid tác phẩm, quy trình 4 bước, CTA, artists |
| `/product/:id`    | Player gọn, cấu hình gói tác quyền (YouTube / Biểu diễn), giá real-time, deliverables |
| `/cart`           | Giỏ hàng + tổng quan giao dịch                                                    |
| `/checkout`       | Stepper 3 bước: thông tin → ký hợp đồng (canvas chữ ký) → thanh toán              |
| `/success`        | Xác nhận giao dịch + mã, confetti                                                 |

## 🚀 Bắt đầu

```bash
npm install
npm run dev      # dev server tại http://localhost:5173
npm run build    # production bundle vào dist/
npm run preview  # preview bản build
```

Yêu cầu Node 18+ (đã test trên Node 22).

## 📂 Cấu trúc thư mục

```
musica/
├─ public/                      # static assets phục vụ trực tiếp
├─ src/
│  ├─ assets/                   # ảnh / icon dùng trong bundle
│  ├─ components/
│  │  ├─ layout/                # AppHeader, AppFooter, BrandLogo
│  │  ├─ ui/                    # primitives tái sử dụng: WaveBars, CheckList, SectionHead
│  │  ├─ product/               # ProductCard và các block liên quan tác phẩm
│  │  └─ checkout/              # (mở rộng: SignaturePad, PaymentMethods,…)
│  ├─ composables/              # logic tái dùng: useReveal (IntersectionObserver)
│  ├─ data/                     # mock data: catalog (products, artists, categories)
│  ├─ router/                   # vue-router config
│  ├─ stores/                   # pinia stores: cart
│  ├─ styles/                   # design tokens + global CSS
│  ├─ views/                    # mỗi route 1 file
│  ├─ App.vue                   # shell: header + <router-view> + footer
│  └─ main.js                   # entry
├─ docs/                        # tài liệu cho người mới & AI
│  ├─ DESIGN_SYSTEM.md          # ngôn ngữ thiết kế (token, component, animation)
│  ├─ ARCHITECTURE.md           # cách tổ chức code, conventions
│  └─ BUSINESS_MODEL.md         # domain + glossary tiếng Việt
├─ .claude/
│  ├─ launch.json               # cấu hình dev server cho Claude Code preview
│  └─ skills/
│     └─ musica-design/SKILL.md # skill auto-load khi Claude làm việc trên repo
├─ index.html
├─ vite.config.js
└─ package.json
```

## 🎨 Design language tóm tắt

- **Tông**: trắng → xanh biển (`#1f6df0`) → xanh ngọc (`#14b8a6`)
- **Font**: Plus Jakarta Sans (display + body)
- **Bo góc**: nhiều radius levels, default cards là `--radius-lg` (22px), full-pill cho button/chip
- **Hiệu ứng**: scroll-reveal (IntersectionObserver), float, pulse-ring, equalizer wave, page transitions
- Chi tiết đầy đủ: [`docs/DESIGN_SYSTEM.md`](docs/DESIGN_SYSTEM.md)

## 🧠 Cho AI / Claude Code

Repo này có sẵn skill **`musica-design`** ở `.claude/skills/musica-design/SKILL.md` — Claude Code sẽ tự đọc khi làm việc trong project, đảm bảo:

- Dùng đúng design tokens / utility class thay vì tạo mới
- Giữ nhất quán terminology (`thương mại điện tử`, `giao dịch tác quyền`, tránh wording cũ mang nghĩa nhượng quyền pháp lý)
- Tái sử dụng các primitive trong `components/ui/` thay vì viết lại

Trước khi build feature mới, đọc lần lượt: `docs/BUSINESS_MODEL.md` → `docs/ARCHITECTURE.md` → `docs/DESIGN_SYSTEM.md`.

## 📜 Roadmap ngắn (sau prototype)

1. Auth + onboarding nghệ sĩ
2. Dashboard nghệ sĩ (đăng tác phẩm, thống kê)
3. Tích hợp thanh toán thật (VNPay, Stripe)
4. Hợp đồng eKYC + chữ ký số có CA
5. API + database (Supabase / Postgres)
6. Auto-verify tác quyền trên YouTube qua Content ID

## License

Prototype mục đích trình bày — chưa public-release.
