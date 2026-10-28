---
name: "dung-video"
description: "Bộ định tuyến dựng video của Dũng, một lệnh /video: chọn phong cách (tên style + link mẫu đã kiểm), cắt dựng, màu D-Log, phụ đề, chữ hiệu ứng + SFX theo nghĩa lời, nhạc nền bắt buộc, phối 4 lớp Effects/Animation/B-roll/MG, Bộ chữ theo video đã học, tự kiểm rồi xuất bản cuối H.265."
---

# dung-video — /video

> Bản cập nhật 27/10/2026: đã gộp `dung-text-fx` (chữ hiệu ứng + SFX theo nghĩa lời) vào đây; thay `hoang-dung-video-effect-lab` bằng `hoang-dung-viral-style-scout` (phân tích video mẫu, font, prompt dựng lại, thư viện F1-F4/E1-E9) + `hoang-dung-auto-style-learner` (thư viện phong cách); áp quy trình nhanh Dũng chốt 27/10/2026 (3 agent song song + 1 QA, render 1 lần, giao thẳng bản cuối).

Lệnh:
- `/video <đường dẫn footage> [bds|promo|talking|explainer|music] [style:<tên phong cách>] [mau:<link/file video mẫu>] [ghi chú]`
- `/video text-fx <dự án>` — lập bảng mốc chữ → SFX theo nghĩa → chấm → báo cáo (giai đoạn 4–6).
- `/video sfx <moments.json>` — chỉ chạy quy trình SFX (`textfx-kit/references/08-sfx.md`).
- `/video hoc <mẫu>` — học hiệu ứng chữ + SFX từ mẫu (video, ảnh, template, link, video hướng dẫn) → chuyển `hoang-dung-viral-style-scout` Chế độ B, kết quả ghi vào thư viện để dựng.

Skill này là **bộ định tuyến**: không chứa kiến thức dựng, chỉ quyết định giai đoạn nào gọi skill/MCP nào. Luôn trả lời Dũng bằng tiếng Việt.

## Luật cứng
1. Skill Hoàng Dũng được dùng: CHỈ `hoang-dung-agent-orchestrator`, `hoang-dung-bds-habit-memory`, `hoang-dung-viral-style-scout`, `hoang-dung-auto-style-learner`.
   Các skill này có nhắc skill Hoàng Dũng khác (đã gỡ hoặc không có trong phiên) → bỏ qua, ghi 1 dòng "bỏ tham chiếu: <mục>" vào sơ đồ phân công.
2. Nguồn còn lại chỉ lấy theo `references/routing.md`. Không chép nội dung skill khác vào dự án.
3. Luật chi tiết nằm trong memory dự án, chỉ đọc khi cần, không viết lại:
   `C:\Users\Admin\.claude\projects\C--Users-Admin-HDRE-AI\memory\`
4. Chữ trên màn hình CHỈ gồm: phụ đề đúng lời thoại + thông tin Dũng cung cấp (giá, địa chỉ, diện tích, kết cấu, hotline); nhãn bản đồ là ngoại lệ duy nhất.
   Địa chỉ chỉ ghi mức khu vực (đường lớn, phường/quận); KHÔNG ghi số nhà, số hẻm, tên chủ nhà.
   MG minh họa đúng thông tin đó. KHÔNG tự thêm slogan, tiêu đề, CTA, con số.
5. Chữ: không khung/nền tối sau chữ; không lặp chữ; MỌI chữ có SFX; chữ hero chạm đúng từ nói ±2 khung; vùng an toàn TikTok (~230px trên, ~480px dưới, ~160px phải).
6. Không zoom/Ken Burns/punch-in/rung footage — muốn nhấn thì làm chữ động (luật cứng của bộ text-fx, thắng mọi gợi ý punch-in ở style mẫu hay thư viện).
7. Không bao giờ xuất video câm hoặc chỉ có SFX. Không tổng hợp nhạc nền bằng code. Không dùng 3D (kể cả khối nhà MG22, 3D clay R-L012): thay bằng animation minh họa 2D vẽ bằng code, mỗi animation có SFX khớp nghĩa (H-hieu_ung_3d-005, Dũng duyệt 28/10/2026).
8. Không tuyên bố "100 điểm" khi chưa có scorecard do script sinh. Không nới ngưỡng. AI không nghe được âm thanh: mọi kết luận về SFX là số đo.
9. Font: dùng font giống mẫu nhất (tên font lấy từ phiếu font của viral-style-scout); font lỗi tiếng Việt → báo Dũng đúng mẫu cảnh báo do `textfx-kit/scripts/vn_font_gate.py` in ra và dùng font thay thế có tiếng Việt.
10. Chỉ hỏi Dũng trước khi: ghi/đè file skill, gọi API tốn credit (Higgsfield, HeyGen), đăng/gửi ra ngoài. Cài package cần cho việc, render, chọn hướng trung gian: tự quyết (Dũng chỉ duyệt bản cuối).
11. Không sửa memory, không sửa file trong `plugins\cache`.
12. Tên phong cách và tên hiệu ứng: chỉ dùng tên có nguồn (nhãn [Tên có nguồn] trong Từ điển hiệu ứng, hoặc tên Dũng đã xác nhận). Không tự đặt tên.
13. Link video mẫu: chỉ dùng link thật đã mở và đo (có trong danh sách video đã học hoặc Thẻ phong cách, kèm mốc thời gian). Không bịa link, không đưa link chưa kiểm.
14. **Luôn phối 4 lớp Effects · Animation · B-roll · Motion Graphics** (H-dung-005, Dũng duyệt 28/10/2026). Chọn lớp theo bảng quyết định trong `hoang-dung-bds-habit-memory` (nội dung × cảnh người dẫn / cảnh không gian / không có cảnh quay). Mỗi 3–5s có ít nhất 1 lớp thay đổi; mỗi lúc tối đa 1 lớp nền + 1 lớp đồ họa đè + 1 cụm chữ. Có cảnh quay: ~55–65% cảnh quay (đã đè đồ họa), ~35–45% animation/MG toàn khung; không có cảnh quay: 100% animation + MG.
15. **Bộ chữ = Font + Hiệu ứng chữ + Bố cục + Màu + SFX luôn đi cùng nhau** (H-hieu_ung_3d-006). Lấy từ mọi video đã học (`asset/phong_cach/`, E1–E9, `asset/phong_cach/bo_chu/`), không giới hạn ở bộ đã duyệt, nhưng không tự chế tổ hợp không có video gốc. Chọn theo vai trò từng câu chữ; mỗi video 6–8 kiểu, mỗi kiểu ≤2 lần, không lặp khuôn giữa các video liên tiếp. Màu theo nghĩa (vàng giá, xanh ngọc vị trí, san hô nhấn, hồng pháp lý), tối đa 2 màu nhấn/khung, hợp màu cảnh quay.
16. Video không có lời thoại (toàn animation): CTA mặc định «NHẮN TIN NGAY»; luật 4 vẫn áp cho mọi chữ khác.

## Bộ text-fx (gộp từ dung-text-fx)
Thư mục: `<thư mục skill dung-video>/textfx-kit/` (chép nguyên `references/`, `scripts/`, `library/`, `fonts/`, `recipes/`, `vendor/` từ skill `dung-text-fx` cũ). Chưa có → tìm ở thư mục `dung-text-fx/` cũ; không có cả hai → ghi `missing` trong edit-state, báo Dũng 1 dòng, KHÔNG đoán nội dung script.

| Việc | File | Script | Trạng thái |
|---|---|---|---|
| SFX theo ý nghĩa lời: phân loại 3 trục, chọn, canh, trộn, chấm 100 | `references/08-sfx.md` | `sfx_tag.py`, `sfx_plan.py`, `sfx_render.py`, `sfx_mix.py`, `sfx_audit.py` | 2 dự án 100/100, 24/24 lỗi cố ý bị bắt (23/10/2026, Dũng duyệt) — xem `library/verification-log.md` |
| Cổng tiếng Việt cho font | `references/06-font-chu.md` [chưa có] | `vn_font_gate.py` | script chạy được |
| Đo + chấm chữ hiệu ứng so với mẫu | `references/07-kiem-chung.md` [chưa có] | `measure.py`, `score.py`, `colorlab.py` | script chạy được; đang kiểm chứng |
| Dựng recipe chữ (HyperFrames) | `recipes/*.html` | `build_recipe.py` | r01 render được |
| Quy tắc thép, phân loại, thời điểm, quy trình, thư viện chữ | `references/text-fx/01`–`05` của dung-video | — | dùng bản trong dung-video |

Đường dẫn thật: thư viện SFX `C:\Users\Admin\Downloads\SOUNDEFFECT TAKING HEARD` (863 file) · danh mục gốc `C:\Users\Admin\HDRE AI\sfx-catalog\` (dùng `textfx-kit/library/sfx-tags.json` đã đo lại) · font đã qua cổng `textfx-kit/fonts/files/`, ảnh bằng chứng `textfx-kit/fonts/proof/` · GSAP cục bộ `textfx-kit/vendor/gsap.min.js` (3.14.2).

## Tiết kiệm token
- Mỗi agent chỉ nạp skill và đúng mục của giai đoạn mình (xem cột "giai đoạn" trong routing.md); đọc frontmatter + mục cần dùng, không đọc toàn văn.
- viral-style-scout chỉ agent đồ họa nạp, chỉ mục cần: chữ/hiệu ứng → Thư viện E1-E9 + luật R-L; font → Bước B5; có video mẫu → Chế độ B đủ bước. Không có mẫu thì bỏ phân tích mẫu.
- Xong mỗi giai đoạn: ghi tóm 3–5 dòng vào `<dự án>/edit-state.json`. Giai đoạn sau CHỈ đọc file này, không đọc lại output cũ.
- Output công cụ dài → ghi ra file, chỉ đọc phần cần.

Mẫu `edit-state.json`:
```json
{"footage":"", "preset":"", "style":{"name":"", "source":"dung_chon|so_thich|video_mau", "ref_videos":[], "bo_chu":[]},
 "engine":"hyperframes",
 "stages":{"0":"", "1":"", "1b":"", "2":"", "2b":"", "3":"", "4":"", "5":"", "6":"", "7":""},
 "missing":[], "skipped_refs":[], "lufs":{}, "final":""}
```

## Đội và ngân sách thời gian (quy trình nhanh, Dũng chốt 27/10/2026)
- Video ≤60s: **3 agent làm việc song song** — Dựng (giai đoạn 2, 2b, 3), Đồ họa (4 phần chữ/MG, 5, 5b), Âm thanh (4 phần SFX, 6) — + **1 agent QA độc lập** (giai đoạn 7). Video dài hơn: chia thêm theo đoạn, vẫn mỗi agent 1 nhiệm vụ.
- Mỗi cổng kiểm 1 lần bằng script + ảnh khung làm bằng chứng; sửa thì chỉ kiểm lại phần đã sửa; tối đa 3 lượt sửa mỗi cổng. Agent sai cùng một lỗi sau 2 lượt sửa → thay agent mới đúng skill, kế thừa thông tin, làm tiếp từ chỗ dở.
- Không render preview riêng; render bản cuối 1 lần sau khi qua mọi cổng. Thời gian video ≤60s: mục tiêu 90 phút, trần 2 giờ (vượt trần → báo Dũng 1 dòng kèm lý do).
- Studio của dự án (HyperFrames Studio hoặc Remotion Studio) mở từ lúc bắt đầu dựng tới khi render xong để Dũng xem timeline bất cứ lúc nào.

## Luồng giai đoạn

**0. Khởi động** — nạp `hoang-dung-bds-habit-memory` (chỉ mảng video).
`hoang-dung-agent-orchestrator` lập sơ đồ phân công (gửi như thông báo, chạy ngay): mỗi agent 1 nhiệm vụ, kèm nguyên văn LUẬT CỨNG VIDEO của orchestrator.
Tạo thư mục dự án cạnh footage: `<footage>_dung-video\`. Mở Studio.

**1. Brief** — quét footage bằng ffprobe (độ phân giải, fps, khung, thời lượng, có phải D-Log) và xem nhanh nội dung (người nói hay không, trong nhà/ngoài trời, có cảnh đi xuyên nhà, có flycam).
Chọn preset theo `references/presets.md`. Chọn engine theo `references/engines.md`.
Có video mẫu → `hoang-dung-viral-style-scout` Chế độ B (gọi từ /video: không hỏi phạm vi, mặc định "Toàn bộ + Nhân bản cấu trúc") → bản đồ cấu trúc + chuẩn phong cách MG/chữ/SFX + phiếu font ghi vào edit-state, `style.source = video_mau`.

**1b. Chọn phong cách (trước khi cắt dựng)**
- Có video mẫu ở bước 1 → dùng phong cách đó, bỏ qua câu hỏi.
- Lệnh đã có `style:<tên>` hoặc Dũng đã nêu rõ phong cách → xác nhận 1 dòng KÈM 1–2 link video mẫu của style đó, đi tiếp.
- Chưa có: đọc Thẻ phong cách + Từ điển hiệu ứng trong `asset/phong_cach/`, chọn 2–3 phong cách khớp footage và nội dung, hỏi Dũng MỘT câu (gộp câu hỏi preset nếu chưa rõ). Mỗi gợi ý BẮT BUỘC gồm:
  - **Từ khóa tên style** (tên có nguồn), ví dụ: `3 Essential Shots (Detail shot · Wide push · FPV)`, `Speed Ramp walkthrough`, `Daily Vlog + Timestamp caption`, `Talking head phân tích + bản đồ vệ tinh`, `Text Behind Subject title`.
  - 1 dòng mô tả cách dựng: nhịp cắt, kiểu chữ, chuyển cảnh, MG chính.
  - **1–2 link video mẫu để Dũng xem trước**, mỗi link kèm mốc thời gian đoạn tiêu biểu (ví dụ `…/shorts/foeDvpNrVtI · 9–16s Wide push`). Ưu tiên video có lượt xem cao nhất trong danh sách đã học.
  - Đánh dấu gợi ý phù hợp nhất là "(khuyên dùng)".
- Style nào chưa có ít nhất 1 link mẫu đã kiểm → KHÔNG đưa vào gợi ý. Không đủ 2 style có link → báo Dũng 1 dòng, đề xuất chạy `hoang-dung-auto-style-learner` (học hằng ngày) hoặc `hoang-dung-viral-style-scout` Chế độ A (nghiên cứu sâu) cho thể loại đó.
- Dùng AskUserQuestion nếu có (link đặt trong phần mô tả lựa chọn); không có thì hỏi bằng tin nhắn. Luôn kèm lựa chọn "Theo sở thích của Dũng".
- **Dũng không chọn, từ chối gợi ý, chọn "Theo sở thích của Dũng", hoặc không trả lời (phiên không người trực)** → dựng theo sở thích đã lưu: quy tắc dựng trong memory dự án + `hoang-dung-bds-habit-memory` (phong cách Dũng duyệt nhiều nhất cho thể loại đó). Ghi 1 dòng đầu báo cáo: "Dựng theo sở thích của Dũng: <tóm tắt>".
- Ghi `style.name`, `style.source`, `style.ref_videos` vào edit-state. Giai đoạn 2–6 bám theo style này (nhịp cắt, chữ, chuyển cảnh, MG, SFX role).
- **Chọn Bộ chữ** (luật 15): mỗi vai trò chữ (hook, giá, kết cấu, vị trí, pháp lý, cảm xúc, tóm tắt, CTA) chọn 1 Bộ chữ có video gốc; ghi mã vào `style.bo_chu[]`. Font qua `vn_font_gate.py`; được đọc (chỉ đọc) `C:\Windows\Fonts` và `%LOCALAPPDATA%\Microsoft\Windows\Fonts` để tìm font Việt hóa thay thế. Hiệu ứng [Chưa rõ tên] được dùng bằng mã thẻ khi bản dựng lại đạt điểm giống ≥80/100.
- Style không được vượt luật cứng (style mẫu có chữ nền hộp → bỏ nền; style có punch-in/rung → thay bằng chữ động; video mẫu dài → chỉ lấy cấu trúc, độ dài video của Dũng theo habit-memory).

**2. Chép lời + cắt dựng** (agent Dựng) — chính: `video-use`; người nói có overlay: `talking-head-recut`.
Chọn take tốt nhất, cắt ho/ậm ừ/khoảng lặng, chống trùng cảnh, giữ trọn ý lời thoại (H-dung-004), shot dài, giọng không cắt vụn. Nhịp cắt theo style đã chốt.
Nộp: transcript có timecode + danh sách cắt (timecode + lý do).

**2b. Che địa chỉ (tự động, mọi video, không cần Dũng nhắc)** (agent Dựng)
- Quét bản dựng sau giai đoạn 2: trích khung mỗi 0,5s + mọi điểm cắt, tìm số nhà, biển số nhà, bảng tên đường/hẻm, biển số xe, giấy tờ có địa chỉ.
- Mỗi vùng phát hiện: ghi `timecode bắt đầu–kết thúc + khung (x,y,w,h)`; cảnh có chuyển động → bám vùng theo từng khung (tracking), nới khung 15–20% để không lộ mép.
- Che bằng làm mờ: `crop` → `boxblur` (đủ mạnh để không đọc được) → `overlay` đúng chỗ. Không che mặt người dẫn, không che chi tiết đẹp của nhà ngoài vùng cần che.
- Không chắc một vùng có phải số nhà/địa chỉ → vẫn che, ghi vào báo cáo.
- Nộp: bảng `timecode → vùng → loại thông tin → cách che` + ảnh trước/sau 1 khung mỗi vùng; ghi `stages.2b` vào edit-state.

**3. Màu** (agent Dựng) — D-Log: LUT chính thức DJI V1.0 trước → chỉnh WB/tone riêng từng clip → bảng so khớp màu. Có video mẫu → hướng màu theo số đo `signalstats` trong bản đồ cấu trúc.
Công cụ: `video-use` (grade theo đoạn) hoặc `media-use resolve --type lut|grade`. Nộp ảnh trước/sau mỗi cảnh.

**4. Cảnh quan trọng** (agent Đồ họa lập bảng, agent Âm thanh nhận cột SFX) — đánh dấu: hook, giá/số liệu, diện tích, vị trí, điểm mạnh, tiện ích, pháp lý, chuyển ý, tâm sự, CTA (CTA chỉ lấy từ lời thoại).
Mỗi mốc gắn chữ/MG + SFX canh theo từng từ, theo style đã chốt và chuẩn phong cách (Thẻ phong cách / bản đồ cấu trúc của viral-style-scout; hệ chữ E1-E9 khi hợp).
Chữ hiệu ứng: đọc `references/text-fx/01-quy-tac-thep.md` + `02-phan-loai-truong-hop.md`, gán mỗi mốc một bậc T0–T3 và loại L1–L10.
SFX: `textfx-kit/references/08-sfx.md` (chọn SFX theo nghĩa/cảm xúc lời người dẫn đang nói). Bảng mốc ghi thêm `say` (câu đang nói) để chọn đúng nghĩa.
Nộp: bảng mốc `thời điểm → từ → say → Lớp (Effects/Animation/B-roll/MG) → MG/chữ (bậc, loại, mã E/MG, mã Bộ chữ) → SFX role`. Cột Lớp chọn theo bảng quyết định 4 lớp (luật 14).

**5. Phụ đề + MG + chuyển cảnh** (agent Đồ họa)
- Phụ đề: `embedded-captions` (HyperFrames) / `remotion-captions` (Remotion). Font Be Vietnam Pro (hoặc font đã chốt từ phiếu font), không khung chữ, không lặp chữ, vùng an toàn TikTok.
- MG/kinetic type: `motion-graphics`, `hyperframes-animation`; khối có sẵn: `hyperframes-registry` + registry blocks (C); recipe chữ `textfx-kit/recipes/*.html`.
- Chuyển cảnh/match cut: `hyperframes-keyframes` (chỉ chuyển cảnh, không punch-in/rung footage — luật 6); cut-the-curve, seam-craft (C).
- Chuẩn timing/easing: `motion-design` + motion-doctrine (C).
- Chữ hiệu ứng ở mốc quan trọng: theo `references/text-fx/04-quy-trinh.md`, chọn hiệu ứng ở `05-thu-vien-hieu-ung.md` (và E1-E9 của viral-style-scout), canh thời điểm theo `03-thoi-diem-su-dung.md`.
- Có font mới: chạy `textfx-kit/scripts/vn_font_gate.py` trước khi dùng.
Nộp: bảng đối chiếu chữ ↔ nguồn (dòng không có nguồn → xóa) + điểm `measure.py`/`score.py` khi có video mẫu.

**5b. MG bản đồ kiểu Google Maps (tự động khi lời thoại nói về vị trí/tiện ích/đường/khu vực)** (agent Đồ họa)
- Mặc định: bán kính 2km; 2–3 tiện ích, ưu tiên Bệnh viện → Siêu thị → Trường học (mỗi nhóm 1 điểm gần nhất trong 2km); 2–3 tuyến đường chính gần nhất. Lời thoại nêu tên tiện ích/đường cụ thể → bắt buộc dùng đúng tên đó.
- Dữ liệu thật: OpenStreetMap qua Overpass API (`amenity=hospital`, `shop=supermarket|mall`, `amenity=school`, `highway=trunk|primary|secondary`), chạy trên máy Dũng. Khoảng cách đường thẳng, làm tròn 0,1km, ghi "~1,2km"; không ghi số phút nếu không có dữ liệu. Điểm không có trong OSM → bỏ, không bịa.
- Tâm bản đồ: link Google Maps hoặc tên đường + phường do Dũng cung cấp; thiếu → hỏi 1 câu, không đoán.
- Bảo mật: pin dịch ngẫu nhiên 80–150m khỏi vị trí thật; không số nhà/tên hẻm.
- Phong cách giống Google Maps, tự vẽ bằng code (không logo/giao diện/ảnh chụp của Google): nền #F1F3F4; đường chính vàng #FDE293 viền #F9AB00; đường thường trắng viền #DADCE0; công viên #C8E6C9; sông #AADAFF; pin giọt nước đỏ #EA4335; icon tròn (bệnh viện dấu cộng, siêu thị giỏ hàng, trường mũ tốt nghiệp); font Roboto; nhãn không nền. Góc dưới ghi nhỏ `© OpenStreetMap` (bắt buộc).
- Chuyển động 4–6s, đặt đúng lúc lời thoại nói về vị trí, không cắt lời: zoom từ quận vào khu (whoosh) → pin rơi + nảy (pop) → vòng 2km lan ra (sonar ping) → đường vẽ nét sáng dần + tên đường (tick) → tiện ích bật lên theo lời thoại, đường nét đứt nối từ pin (mỗi tiện ích 1 SFX khác nhau) → giữ 0,5s rồi chuyển cảnh.
- Công cụ: `remotion-maps` (MapLibre, không cần key) hoặc vẽ SVG từ GeoJSON đã tải trong HyperFrames.

**6. Âm thanh** (agent Âm thanh) — cổng bắt buộc theo `references/audio-gate.md`. Không qua → KHÔNG render, báo lỗi ngay.
Nhạc nền: hook và gây cảm xúc từ giây đầu, dễ viral (H-am_thanh-001); chỉ xuất bản có nhạc.
SFX + trộn theo `textfx-kit/references/08-sfx.md` (Dũng duyệt 23/10/2026; THẮNG phần SFX/ducking trong audio-gate):
- `sfx_tag.py` (khi thư viện có file mới) → `sfx_plan.py` → `sfx_render.py --voice` → `sfx_mix.py --plan` → `sfx_audit.py --bgm` phải 100/100.
- SFX to rõ nhưng KHÔNG to hơn giọng (thấp hơn 1–6 dB); nhạc nền ĐỀU mức cũ (≈ −20 LUFS), chỉ hạ nhẹ ≤ 6 dB quanh mốc rồi lên lại; SFX nổi ≥ 4.5 dB trên nhạc.

**7. QA, render & giao bản cuối**
- Ghép 3 nhánh trên timeline Studio. Agent QA độc lập (chưa làm công đoạn nào) kiểm 1 lần trên bản dựng trong Studio: trích khung mỗi 1–2 giây + mọi điểm cắt + mọi lần chữ xuất hiện (ghép contact sheet), đối chiếu bảng chữ, danh sách cắt, bảng che địa chỉ, scorecard SFX, số đo ebur128. Còn 1 lỗi → trả đúng agent sửa, chỉ kiểm lại phần đã sửa (tối đa 3 lượt).
- Qua mọi cổng → render bản cuối 1 lần: H.265 theo thông số gốc (dưới 60fps thì lên 60fps, tăng bitrate phù hợp), lưu `C:\Users\Admin\HDRE AI\Videos`. Kiểm G6 trên file render (ffprobe, phát hết, có nhạc).
- Giao Dũng MỘT bản cuối, ghi rõ tên style đã dùng kèm 1–2 link video mẫu (hoặc video mẫu Dũng gửi) để Dũng so sánh.
- Dũng yêu cầu sửa → sửa đúng phần đó rồi render lại, làm tiếp từ chỗ dở, không làm lại từ đầu. Dũng không ưng style → quay lại 1b với gợi ý khác (vẫn kèm link mẫu) hoặc dựng theo sở thích của Dũng.
- Dũng duyệt → `hoang-dung-bds-habit-memory` ghi thói quen mới (gồm style được chọn cho thể loại đó); Thẻ phong cách ghi "đã dùng trên kênh Dũng".
- Sau render chỉ trả lời: `Thư mục chứa kết quả: <đường dẫn>` (kèm tên style + link mẫu).

## Cổng kiểm (orchestrator có quyền phủ quyết)
| # | Cổng | Đạt khi |
|---|------|---------|
| G0 | Style | edit-state có `style.name` + `style.source` + 1–2 `ref_videos` đã kiểm (hoặc video mẫu Dũng gửi); tên style có nguồn hoặc Dũng đã xác nhận |
| G1 | Lời | 100% lời thoại, đúng dấu, có timecode; số/giá khớp thông tin Dũng |
| G2 | Cắt | danh sách cắt đủ lý do; không mất chữ, không trùng cảnh; nhịp khớp style |
| G3 | Màu | đồng nhất giữa cảnh, da tự nhiên, có ảnh trước/sau |
| G4c | 4 lớp | mỗi 3–5s có lớp thay đổi; không quá 1 nền + 1 đè + 1 cụm chữ; mỗi chữ dùng đúng Bộ chữ có video gốc; tỷ lệ cảnh quay/animation đúng luật 14 |
| G4 | Chữ/MG | bảng đối chiếu đủ nguồn; không che nhà; không chữ tự thêm; mọi chữ có SFX; font qua cổng tiếng Việt; không punch-in/rung footage; map: mọi điểm có trong OSM, pin đã dịch, có `© OpenStreetMap` |
| G4b | Địa chỉ | 0 khung lộ số nhà/biển số/địa chỉ cụ thể (agent độc lập soát lại toàn video) |
| G5 | Âm thanh | qua toàn bộ audio-gate, `sfx_audit.py` 100/100, có số đo ebur128 thật, nhạc bắt tai từ giây đầu |
| G6 | Render | đúng khung, fps, thời lượng; có nhạc; phát hết file |
| G7 | QA tổng | agent độc lập, xem bằng mắt, 0 lỗi |

Chưa qua cổng → đoạn đó không sang giai đoạn sau (đoạn khác đã qua vẫn đi tiếp).

## Dừng và báo lỗi
- Một bước lỗi 2 lần liên tiếp, hoặc agent quá thời gian 2 lần → thay agent; vẫn lỗi → dừng, báo lỗi + đề xuất cách sửa.
- Nguồn trong routing.md hoặc `textfx-kit/` không có trên máy → ghi `missing` trong edit-state, dùng DỰ PHÒNG; không đoán nội dung.
- Không có `asset/phong_cach/` trên máy → vẫn hỏi Dũng ở 1b bằng các style đã có trong memory dự án, mỗi style vẫn phải có link mẫu đã kiểm; ghi `missing`.

## Tham chiếu (chỉ đọc khi tới giai đoạn cần)
- `references/routing.md` — chức năng → skill chính → dự phòng → điều kiện → credit
- `references/engines.md` — chọn HyperFrames / Remotion / Higgsfield
- `references/audio-gate.md` — cổng âm thanh bắt buộc
- `references/presets.md` — bds, promo, talking, explainer, music
- `references/repos.md` — 19 repo công nghệ (GSAP, anime, Lottie, three/R3F, Remotion, svgo, satori, howler, FFmpeg…): dùng khi nào, lệnh cài, giới hạn render
- `references/text-fx/00-README.md` — bộ quy tắc chữ hiệu ứng (quy tắc thép, phân loại, thời điểm, quy trình, thư viện hiệu ứng)
- `textfx-kit/` — SFX theo nghĩa lời, cổng font tiếng Việt, đo/chấm chữ, recipe chữ (gộp từ dung-text-fx)
- `hoang-dung-viral-style-scout` — phân tích video mẫu, phiếu font, prompt dựng lại, thư viện F1-F4 / E1-E9 / MG24-MG34 / TR01-TR06
- `asset/phong_cach/` — Từ điển hiệu ứng + Thẻ phong cách + video mẫu (giai đoạn 1b)
