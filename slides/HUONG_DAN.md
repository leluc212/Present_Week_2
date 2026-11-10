# Hướng dẫn trình chiếu

Slide làm bằng [Slidev](https://sli.dev). Nội dung nằm trong `slides.md`.

## Cài đặt (chỉ làm một lần trên máy mới)

1. Cài **Node.js** bản LTS (từ 22.12 trở lên): https://nodejs.org
2. Cài **pnpm**: mở PowerShell và chạy `npm i -g pnpm`
3. Trong thư mục `slides/`, chạy `pnpm install`

> Nếu PowerShell báo "running scripts is disabled", dùng `pnpm.cmd` thay cho `pnpm`.

## Trình chiếu

Trong thư mục `slides/`, chạy:

```bash
pnpm dev
```

- Màn hình chiếu: mở http://localhost:3030
- Màn hình của người nói (presenter mode): mở http://localhost:3030/presenter
  - Thấy ghi chú thuyết trình, slide kế tiếp và đồng hồ 15 phút
  - Bấm chuyển slide ở đây, màn hình chiếu chạy theo

## Phím tắt

| Phím | Tác dụng |
|---|---|
| `→` hoặc `Space` | Bước tiếp theo (hiện ý tiếp theo hoặc sang slide sau) |
| `←` | Lùi lại |
| `F` | Toàn màn hình |
| `O` | Xem tổng quan tất cả slide |
| `C` | Bật chế độ vẽ để khoanh trên slide |
| `G` | Nhảy tới số slide |

## Các slide có hiệu ứng click

Slide 3, 6, 9, 15, 17, 19. Ở các slide này cần bấm `→` vài lần để hiện hết nội dung trước khi sang slide sau.

## File khác

- `slides-export.pptx`: bản PowerPoint 23 trang. Mỗi trang là ảnh chụp slide ở trạng thái cuối, **không sửa chữ được** và **không có hiệu ứng**. Ghi chú thuyết trình nằm trong phần Notes.
- Xuất lại PPTX sau khi sửa nội dung: `pnpm export`
- Tạo bản web tĩnh: `pnpm build` (kết quả ở thư mục `dist/`)
- `public/images/CREDITS.md`: nguồn ảnh (Pexels)
