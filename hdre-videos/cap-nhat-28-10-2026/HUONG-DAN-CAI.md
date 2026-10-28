# Hướng dẫn cài bản cập nhật 28/10/2026

Thư mục này gồm:
- `user_habits_cap_nhat.md` — các dòng thói quen mới/thay thế + 1 dòng sổ tay chuyên gia.
- `dung-video/SKILL.md` — bản đầy đủ đã sửa (thêm luật 14–16, bước chọn Bộ chữ ở 1b, cột «Lớp» ở giai đoạn 4, cổng G4c, bỏ 3D).
- `hoang-dung-auto-style-learner/SKILL.md` — bản đầy đủ đã sửa (thêm «Chế độ học effect từ video mẫu», agent Thợ Dựng Lại, Thẻ Bộ chữ, ngưỡng 80/100).

## Bước 1 · Thói quen (máy tính)
1. Mở `HDRE_Library\training\user_habits.md`.
2. Dòng trùng mã → thay bằng dòng mới; mã chưa có → thêm vào đúng mảng.
3. Thêm dòng `[K-phap_ly-001]` vào `HDRE_Library\training\expert_notes.md`.
4. Tạo thư mục `asset\phong_cach\bo_chu\` (nơi lưu Thẻ Bộ chữ).
Hoặc mở Claude trên máy và nói: «Cập nhật thói quen theo file user_habits_cap_nhat.md».

## Bước 2 · Hai skill
1. Vào claude.ai → Settings → Capabilities/Skills.
2. Với `dung-video` và `hoang-dung-auto-style-learner`: tải bản cũ về để lưu, rồi thay `SKILL.md` bằng file trong thư mục này (giữ nguyên các thư mục con `references/`, `textfx-kit/`, `commands/`).
3. Mở phiên Claude mới để skill mới được nạp.

## Kiểm tra sau khi cài
Nói với Claude: «Xem thói quen dung» và «Học effect video này: <file>». Nếu thấy luật 14–16 và Chế độ học effect từ video mẫu thì đã cài đúng.
