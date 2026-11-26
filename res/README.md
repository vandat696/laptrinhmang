# Tài nguyên giao diện cờ vua

Đặt bộ quân cờ và theme bạn thích vào đây. Công nghệ giao diện chưa chốt; các tài nguyên sẽ được tích hợp khi làm client.

```text
res/
├── pieces/
│   ├── white/    # 6 hình quân Trắng
│   └── black/    # 6 hình quân Đen
└── themes/       # Hình bàn cờ, nền hoặc mẫu giao diện yêu thích
```

## Hình quân cờ

Nếu thuận tiện, dùng tên dưới đây cho cả hai thư mục white và black:

| Quân | Tên file gợi ý |
| --- | --- |
| Vua | king.png |
| Hậu | queen.png |
| Xe | rook.png |
| Tượng | bishop.png |
| Mã | knight.png |
| Tốt | pawn.png |

Ví dụ: pieces/white/king.png là vua Trắng, pieces/black/king.png là vua Đen. Mỗi bên chỉ cần một hình cho mỗi loại quân, tổng cộng 12 hình.

Ưu tiên PNG nền trong suốt, khung vuông và cùng kích thước để dễ căn trên bàn cờ. Nếu bộ có sẵn dùng SVG, ảnh ghép hoặc tên khác thì cứ giữ nguyên; sẽ xem và điều chỉnh cách nạp khi tích hợp.

## Theme

Đặt ảnh nền, ảnh bàn cờ hoặc ảnh mẫu giao diện vào themes. Có thể thêm file ghi chú màu sắc bạn muốn. Chưa cần chuẩn bị CSS hoặc file cấu hình theo một công nghệ cụ thể.

Các quân cờ và màu sắc trong output/chess-desktop-preview.html chỉ là bản phác thảo, không phải lựa chọn giao diện đã chốt.
