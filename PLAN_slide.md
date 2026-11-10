# Plan: Slide "Báo cáo quá trình học việc" bằng Slidev

> Trạng thái: **plan đã chốt, chờ anh/chị cho phép chạy bước 1**. Chưa tạo project, chưa cài đặt gì.
> Nguồn: `Báo cáo quá trình học việc.pdf` (10 trang) và `Mẫu slide.pdf` (17 slide, xuất từ Canva).
> Nguyên tắc: **em không tự chạy bất kỳ bước nào**. Trước mỗi bước trong mục 8, em hỏi và chờ anh/chị đồng ý.

## 0. Các quyết định đã chốt

| Hạng mục | Quyết định |
|---|---|
| Thời lượng | 15 phút, 20 slide |
| Ngôn ngữ | Tiếng Việt. Thuật ngữ kỹ thuật giữ tiếng Anh như trong báo cáo (Network, Storage, Container, snapshot, firewall...) |
| Logo | Bỏ, không dùng logo nào |
| Ảnh | Bỏ toàn bộ ảnh trong mẫu. Dùng ảnh miễn phí trên mạng (Unsplash / Pexels) |
| Cài Node.js, pnpm | Em cài bằng `winget`, sau khi anh/chị cho phép. Git để sau, cài ở giai đoạn đưa lên web |
| Trình chiếu | **Trình duyệt** (giữ đủ hiệu ứng click, có presenter mode xem notes) |
| File PPTX | Bản phụ để lưu / gửi. 20 trang, mỗi slide ở trạng thái cuối, không có hiệu ứng |
| Hiệu ứng | Vừa phải, ưu tiên ít |
| Slide ánh xạ sang K8s / AWS | Giữ |
| Slide Q&A | 1 slide, chỉ có chữ **"Q&A"**, đặt trước slide Cảm ơn |
| Đưa lên web | Có, đồng ý công khai. **Code trên GitHub, web chạy trên Vercel** (phương án A). Không dùng GitHub Pages |
| Repo GitHub | Anh/chị tự tạo sau. Làm slide trước, phần đưa lên web làm sau cùng |

---

## 1. Thiết kế lấy từ mẫu (`Mẫu slide.pdf`)

Chỉ giữ **phong cách**: màu, font, hình khối, bố cục. Không dùng ảnh và logo của mẫu.

| Yếu tố | Mô tả trong mẫu | Cách làm trong Slidev |
|---|---|---|
| Tỉ lệ | 16:9 | `aspectRatio: 16/9`, `canvasWidth: 1280` |
| Màu | Xanh navy (~`#0B4DA2`), cam (~`#F7941D`), nền trắng, mảng xám xanh nhạt (~`#EEF2FA`), chữ navy đậm (~`#0F1E3D`) | Biến CSS trong `styles/index.css` |
| Font | Tiêu đề sans-serif đậm, IN HOA. Nội dung sans-serif thường | `Be Vietnam Pro` (Google Fonts, đủ dấu tiếng Việt) |
| Trang trí | Khối vát góc xanh/cam, lưới chấm, cụm 4 vòng tròn nhỏ, đường kẻ cam dưới tiêu đề | Component: `DotGrid`, `Circles4`, `ChamferShape`, `TitleLine` |
| Khung ảnh | Khung vát góc viền trắng | `ChamferImage` dùng `clip-path`, đặt ảnh miễn phí vào |
| Bố cục | (a) Cover: chữ trái + ảnh phải · (b) Agenda: 2 cột pill · (c) Ảnh/khối xanh trái + tiêu đề phải · (d) Tiêu đề trái + card trắng trên nền xanh phải · (e) Khối xanh trái có nhãn pill + nội dung phải · (f) Thank you: ảnh trái + pill xanh phải | 6 layout trong `layouts/` |

---

## 2. Dàn ý 20 slide

Mạch của báo cáo được giữ nguyên: **thứ tự để một máy chủ đi vào hoạt động** (Máy → Mạng → Ổ đĩa → Quyền → Dịch vụ & Firewall → Giám sát → Container). Mỗi slide 3–5 ý ngắn, tối đa khoảng 40 chữ. Lệnh chỉ giữ 3–6 dòng tiêu biểu. Giải thích đầy đủ nằm trong presenter notes.

| # | Slide | Nội dung rút gọn | Layout | Ảnh | Hiệu ứng |
|---|---|---|---|---|---|
| 1 | **Cover** | "BÁO CÁO QUÁ TRÌNH HỌC VIỆC" · pill "Linux doanh nghiệp · Ảo hóa · Container" · Lê Tấn Lực (Intern) · 11/2026 | (a) | Phòng máy chủ | Không |
| 2 | **Nội dung** | 01 Ảo hóa & VM · 02 Network · 03 Storage · 04 User & Permission · 05 Service & Firewall · 06 Quản lý tài nguyên · 07 Container · 08 Tổng kết & Định hướng | (b) | Không | Không |
| 3 | **Cách tiếp cận** | Học theo vòng đời máy chủ: đường ống 7 bước. Đã thực hành trên Rocky Linux + Docker; hypervisor mới ở mức lý thuyết | (c) | Rack server | `v-clicks` cho 7 bước |
| 4 | **01 · Hypervisor** | Định nghĩa 1 câu · Type 1 (ESXi, Hyper-V, KVM, Proxmox) vs Type 2 (VirtualBox, Workstation) · Hyper-V thực chất là Type 1 | (d) | Không | `v-mark` vào "Type 1" |
| 5 | **VM & vòng đời VM** | VM = file cấu hình + file đĩa ảo (thin/thick) · Tạo → Cài đặt → Vận hành → Bảo trì → Thu hồi · Template/clone, live migration, overcommit | (c) | Data center | Không |
| 6 | **Snapshot ≠ Backup** | Snapshot = nút Undo, nhanh, nằm cạnh VM · Backup = bản sao cất ở nơi khác · Quy tắc 3-2-1 | (e) | Không | `v-click` cho cột Backup |
| 7 | **02 · Network** | IP / subnet / gateway · 4 chế độ mạng VM (NAT, Host-only, Bridged, Internal) · `nmcli` đặt IP tĩnh · **Bài học:** 2 default route → mất Internet | (d) | Không | Không |
| 8 | **Kiểm tra mạng theo lớp** | ip addr → ip route → ping gateway → ping IP đích → ping tên (DNS) | (c) | Cáp mạng / switch | `v-clicks` từng bậc |
| 9 | **03 · Storage & LVM** | Không để dữ liệu chung ổ với OS · PV → VG ("bể chứa") → LV · fstab dùng UUID · **Bài học:** `mount -a` trước reboot; XFS chỉ nới, không thu nhỏ | (c) | Không (dùng sơ đồ LVM) | Không |
| 10 | **04 · User, group & permission** | rwx = 4/2/1, ví dụ 750 · Tình huống An & Bình: `/data/web` = 2770 + setgid · Least privilege qua sudoers · **Bài học:** `usermod -aG` | (d) | Không | `v-mark` dưới `-a` |
| 11 | **05 · systemd** | start ≠ enable, `enable --now` · Unit file chạy bằng user An · `kill -9` → systemd tự chạy lại nhờ `Restart=on-failure` | (c) | Không | Highlight dòng code `Restart=on-failure` |
| 12 | **Firewall & SELinux** | firewalld theo zone · `--permanent` + `--reload` · Lỗi 403 do SELinux → `semanage` + `restorecon` · "Web không vào được" có 4 nguyên nhân | (e) | Không | Không |
| 13 | **06 · Quản lý tài nguyên** | Tình huống "web01 chậm" · Load 3.95 trên 2 CPU = quá tải; `wa` cao = nghẽn disk · RAM: nhìn cột `available` · systemd giới hạn bằng cgroups | (d) | Không | Không |
| 14 | **07 · Container vs VM** | VM = nhà riêng (kernel riêng, vài GB, khởi động vài phút) · Container = căn hộ chung cư (chung kernel, namespaces + cgroups, vài giây) | (e) | Nhà riêng / chung cư | `v-click` cho cột Container |
| 15 | **Docker trong thực hành** | Grid 2×2: Image & Registry · Volume · Network · Resource limit · **Bài học:** `-p` tự mở port, bỏ qua firewalld | (b) biến thể | Không | Không |
| 16 | **Tổng kết: lab xuyên suốt + 4 bài học** | Sơ đồ web01 / db01 · 4 bài học: kiến thức liên kết · kiểm tra từng lớp · đọc log trước khi sửa · luôn có đường lui | (c) | Không (dùng sơ đồ) | `v-clicks` cho 4 bài học |
| 17 | **Tự đánh giá & Định hướng** | Đã nắm / còn yếu (vòng đời VM, SELinux, subnet, script) · Timeline: Củng cố → Kubernetes → AWS SAA-C03 → RHCSA (song song) → RHCE | (d) | Không | Không |
| 18 | **Ánh xạ kiến thức → K8s / AWS** | Bảng: VM→EC2 · Network→VPC · Firewall→Security Group · Storage→EBS/EFS/S3 · User→IAM · Snapshot/Backup→EBS snapshot/AWS Backup · Container→ECR/ECS/EKS · cgroups→requests/limits · Giám sát→CloudWatch | (b) | Không | Không |
| 19 | **Q&A** | Chỉ có chữ **"Q&A"** cỡ lớn, căn giữa, kèm hình khối trang trí theo mẫu | layout `center` + trang trí | Không | Không |
| 20 | **Cảm ơn** | "Cảm ơn anh/chị đã lắng nghe" · link / mã QR tới bản web (thêm sau khi có link Vercel) | (f) | Người làm việc với màn hình | Không |

**Phân bổ thời gian (15 phút):** mở đầu slide 1–3 khoảng 1,5 phút · phần 01–07 (slide 4–15) khoảng 10 phút · tổng kết và định hướng (slide 16–18) khoảng 3 phút · Q&A và cảm ơn (slide 19–20) khoảng 0,5 phút trước khi mở phần hỏi đáp.

---

## 3. Hiệu ứng (mức vừa phải)

Chỉ dùng những hiệu ứng sau:

- **Chuyển slide:** `fade` cho toàn bộ deck.
- **Hiện theo click:** `v-click` / `v-clicks` ở 5 slide (3, 6, 8, 14, 16).
- **Đánh dấu từ khóa:** `v-mark` ở 2 slide (4 và 10).
- **Code:** tô sáng dòng quan trọng ở slide 11.
- **Sơ đồ:** Mermaid cho LVM (slide 9) và lab web01 / db01 (slide 16).
- **Icon:** Iconify (Linux, Docker, AWS, Kubernetes, cảnh báo cho card "Bài học").
- **Presenter notes** cho mọi slide, `duration: 15min`.

Không dùng: Magic Move, `v-motion`, thanh tiến độ, hiệu ứng hover.

---

## 4. Ảnh miễn phí

- Nguồn: **Unsplash** và **Pexels**. Cả hai cho dùng miễn phí, không bắt buộc ghi nguồn. Em vẫn ghi lại tác giả và link trong `public/images/CREDITS.md`.
- Tông ảnh: công nghệ, tối, xanh dương, hợp với màu của mẫu.
- Dự kiến 6 ảnh, cho slide 1, 3, 5, 8, 14 và 20.
- Ảnh được tải về `public/images/`, nên khi trình chiếu và trên bản web không phụ thuộc trang gốc.

---

## 5. Trình chiếu bằng trình duyệt

- Lúc thuyết trình chạy `pnpm dev` trong thư mục `slides/`, mở `http://localhost:3030`.
- Presenter mode: mở `http://localhost:3030/presenter` trên màn hình của người nói (xem notes, slide kế tiếp, đồng hồ), còn màn hình chiếu mở trang slide thường.
- Phím tắt: `→` / `Space` sang bước tiếp, `←` lùi, `F` toàn màn hình, `O` xem tổng quan slide.
- Dự phòng khi máy trình chiếu không có Node.js: dùng link bản web (mục 7) hoặc file PPTX.
- Em sẽ viết file `slides/HUONG_DAN.md` ghi lại các lệnh và phím tắt này.

---

## 6. File PPTX

- Xuất 20 trang, mỗi slide ở trạng thái cuối (đã hiện hết các bước click). PPTX chỉ để lưu và gửi người khác xem.
- Slidev xuất PPTX bằng cách **chụp ảnh từng slide**, nên chữ **không sửa được** trong PowerPoint. Presenter notes vẫn được đưa vào phần ghi chú của PPTX.
- Cần cài thêm `playwright-chromium` (trình duyệt chạy ngầm để chụp slide).

---

## 7. Đưa lên web (GitHub + Vercel)

Phương án đã chốt: **code để trên GitHub, web chạy trên Vercel**. Không dùng GitHub Pages. Phần này làm **sau cùng**, khi slide đã xong và anh/chị đã tự tạo repo.

### 7.1 Chuẩn bị sẵn trong lúc làm slide (không cần repo)

- `.gitignore`: loại `node_modules/`, `dist/`, file PPTX / PDF xuất ra.
- `vercel.json`: mọi đường dẫn slide (ví dụ `/5`) trỏ về `index.html`, tránh lỗi 404 khi tải lại trang.
- Build thử `pnpm build` ở máy để chắc chắn bản web chạy được.

### 7.2 Khi anh/chị đã tạo repo

- Repo **công khai**, chỉ chứa thư mục `slides/`. **Không đưa lên:** 2 file PDF gốc và `PLAN_slide.md` (các file này nằm ngoài `slides/`).
- Cài **Git** (`winget install Git.Git`). Anh/chị tự đăng nhập GitHub khi Git hỏi. Em không đăng nhập thay anh/chị.
- Push code lên repo.

### 7.3 Vercel

- Anh/chị đăng nhập Vercel bằng tài khoản GitHub, chọn **Add New → Project → Import** repo ở trên.
- Cấu hình: build command `pnpm build`, output directory `dist`. Nếu repo chứa thư mục `slides/` thay vì nội dung của nó thì đặt root directory là `slides`.
- Mỗi lần push lên GitHub, Vercel tự build lại.
- Link dạng: `https://<tên-project>.vercel.app`.

### 7.4 Lưu ý

- Link là **công khai** (anh/chị đã đồng ý). Ai có link đều xem được nội dung báo cáo.
- Trên bản web, người xem bấm qua được từng bước click như khi trình chiếu.

---

## 8. Các bước thực hiện

Mỗi bước dưới đây em **hỏi và chờ anh/chị cho phép rồi mới chạy**.

### Giai đoạn 1: Làm slide

1. **Cài môi trường:** `winget install OpenJS.NodeJS.LTS`, rồi `npm i -g pnpm`. Kiểm tra `node -v` ≥ 22.12 và `pnpm -v`.
2. **Khởi tạo project** `slides/`, cài Slidev, theme default và `playwright-chromium`.
3. **Dựng thiết kế:** màu, font, component trang trí, 6 layout. Làm trước slide 1, 2, 4 để anh/chị xem.
   - ⏸ **Điểm dừng 1:** gửi ảnh chụp 3 slide mẫu để anh/chị duyệt phong cách.
4. **Chọn ảnh** trên Unsplash / Pexels.
   - ⏸ **Điểm dừng 2:** gửi danh sách ảnh (link + slide dùng) để anh/chị duyệt rồi mới tải về.
5. **Viết nội dung** 20 slide + presenter notes theo mục 2.
6. **Thêm hiệu ứng** theo mục 3.
7. **Kiểm tra:** chạy dev server, rà từng slide (tràn chữ, dấu tiếng Việt, thứ tự click, presenter mode).
   - ⏸ **Điểm dừng 3:** gửi ảnh chụp toàn bộ slide để anh/chị duyệt nội dung.
8. **Xuất PPTX** 20 trang, mở lại file để kiểm tra đủ trang và hiển thị đúng. Viết `HUONG_DAN.md`, `.gitignore`, `vercel.json` và build thử bản web.
   - ⏸ **Điểm dừng 4:** gửi file PPTX và hướng dẫn trình chiếu. Slide đã dùng được từ đây, kể cả khi chưa đưa lên web.

### Giai đoạn 2: Đưa lên web (khi anh/chị đã tạo repo)

9. **Cài Git và push:** cài Git, anh/chị đăng nhập GitHub, em push thư mục `slides/` lên repo.
   - ⏸ **Điểm dừng 5:** anh/chị xác nhận lần cuối trước khi em push.
10. **Deploy Vercel:** anh/chị import repo vào Vercel. Em kiểm tra link chạy đúng (mở được, chuyển slide, tải lại trang không lỗi).
11. **Cập nhật slide 20:** thêm link và mã QR của bản web, push lại và xuất PPTX lại.
    - ⏸ **Điểm dừng 6:** báo kết quả cuối, gửi link web và file PPTX mới.

---

## 9. Câu hỏi còn lại

Không còn câu hỏi nào về plan. Em chờ anh/chị cho phép chạy **bước 1: cài Node.js LTS bằng `winget` và cài pnpm bằng `npm i -g pnpm`**.
