# Quy trình Spec-Driven Development (SDD)

[English](SDD-FLOW-en.md) · **Tiếng Việt**

Dự án này phát triển theo kiểu **viết đặc tả trước, làm sau**: mọi quyết định nằm trong tài liệu ở thư mục `spec/`, code đi theo tài liệu.

Tài liệu dành cho **AI agent** làm việc trong repo và cho **người dùng** (người chốt phạm vi, duyệt và cho phép merge). Agent: đọc hết tài liệu này trước khi làm bất cứ việc gì, rồi bắt đầu từ [mục 0](#0-agent-bắt-đầu-từ-đây).

## 0. Agent: bắt đầu từ đây

**Bước 1 — Xác định tình trạng repo** rồi làm theo dòng tương ứng:

| Tình trạng | Việc cần làm |
|---|---|
| Chưa có `spec/`, repo gần như trống | Khởi tạo dự án mới ([mục 8.1](#81-dự-án-mới)) |
| Chưa có `spec/`, đã có code | Khởi tạo cho dự án có sẵn ([mục 8.2](#82-dự-án-có-sẵn)) |
| Có `spec/` nhưng thiếu file constitution, hoặc tài liệu nằm ở chỗ khác (`specs/`, `docs/`…) | Báo người dùng những gì đang có, đề xuất cách chuẩn hoá theo tài liệu này. Không tự đổi tên hay di chuyển file |
| Có constitution, `roadmap.md` có giai đoạn 🔄 | Đọc thư mục spec của giai đoạn đó, kiểm tra đang ở đúng nhánh, làm tiếp từ nhóm đầu tiên còn `[ ]` trong `plan.md` |
| Có constitution, không có giai đoạn 🔄 | Báo người dùng giai đoạn ⬜ tiếp theo (và các giai đoạn ⏸ nếu có), đề nghị tạo spec hoặc làm tiếp giai đoạn tạm dừng ([mục 4](#4-vòng-đời-một-giai-đoạn)) |

**Bước 2 — Khi người dùng giao một việc cụ thể**, xếp nó vào một trong các loại:

| Loại việc | Xử lý |
|---|---|
| Thuộc giai đoạn đang 🔄 | Sửa spec trước, rồi làm ([mục 5](#5-khi-có-thay-đổi-giữa-chừng)) |
| Thay đổi nhỏ, không đụng constitution | Đường tắt ([mục 5.2](#52-đường-tắt-cho-thay-đổi-nhỏ)) |
| Đổi quy ước, công nghệ, mô hình dữ liệu… | Sửa constitution trước ([mục 5.1](#51-các-loại-thay-đổi)) |
| Tính năng mới chưa có trong roadmap | Đề xuất thêm vào `roadmap.md`, chờ đồng ý rồi mới tạo spec |

**Bước 3 — Nếu repo chưa có `CLAUDE.md` hoặc `AGENTS.md`**, đề nghị tạo (vài dòng, xem [mục 2](#2-cấu-trúc-thư-mục)) để phiên sau tự đọc tài liệu này.

**Không bao giờ:**

- bắt đầu code khi spec chưa được duyệt;
- tạo nhánh hoặc file spec trước khi hỏi xong;
- commit thẳng lên nhánh đích (trừ commit đầu tiên của repo mới, [mục 8.1](#81-dự-án-mới));
- merge khi người dùng chưa nói "merge";
- push, `push --force`, `reset --hard`, xoá nhánh chưa merge, hay viết lại lịch sử đã push khi người dùng chưa đồng ý;
- làm việc rủi ro ([quy tắc 15](#1-quy-tắc-làm-việc)) khi người dùng chưa xác nhận;
- đưa bí mật vào Git.

## 1. Quy tắc làm việc

1. **Constitution thắng spec.** Spec mâu thuẫn với constitution thì sửa constitution trước, ghi lý do.
2. **Hỏi trước, ghi sau.** Chưa hỏi xong thì chưa tạo nhánh hay file.
3. **Khuyến nghị rõ ràng.** Luôn nêu một phương án khuyến nghị kèm lý do ngắn, không liệt kê ngang hàng rồi để người dùng tự đoán.
4. **Ghi lại mọi quyết định**, kể cả quyết định agent tự chốt, vào spec hoặc constitution, không chỉ trong đoạn chat.
5. **Tài liệu không đi sau code.** Có thay đổi thì cập nhật spec trước hoặc trong cùng commit.
6. **Mỗi nhóm nhiệm vụ một commit;** chỉ commit khi trạng thái nhất quán (build được, test pass, cấu hình đã export…).
7. **Test là một phần của "xong".** Code mới đi kèm test theo công cụ ghi trong `tech-stack.md`.
8. **Kiểm chứng bằng bằng chứng.** Ghi kết quả lệnh đã chạy, không ghi "chắc là được".
9. **Không thêm thư viện/phụ thuộc** chưa có trong `tech-stack.md` khi người dùng chưa đồng ý.
10. **Bí mật không vào Git** (mật khẩu, khoá, file cấu hình riêng từng máy).
11. **Spec phải đủ để làm tiếp ở phiên mới.** Nếu phải dựa vào lịch sử chat mới hiểu được việc, spec còn thiếu: bổ sung.
12. **Tối đa một giai đoạn 🔄 tại một thời điểm.** Muốn bắt đầu giai đoạn khác thì giai đoạn đang làm phải được merge, hoặc tạm dừng/huỷ theo [mục 5.1](#51-các-loại-thay-đổi).
13. **Git sạch sẽ.** Chỉ tạo hoặc chuyển nhánh khi working tree sạch (còn thay đổi dở thì commit vào nhánh hiện tại hoặc hỏi người dùng). Nhánh mới luôn tạo từ nhánh đích đã cập nhật.
14. **Ngôn ngữ:** spec, constitution, CHANGELOG và commit viết bằng tiếng Việt, trừ khi `tech-stack.md` quy định khác. Tên file, nhánh, thư mục không dấu.
15. **Dừng hỏi trước việc rủi ro**, dù plan đã ghi: migration làm thay đổi hoặc xoá dữ liệu; đổi API/URL mà bên khác đang dùng; xác thực, phân quyền, thanh toán; mọi thao tác trên môi trường thật (production). Nói rõ sẽ làm gì, ảnh hưởng gì, cách quay lại; chờ người dùng đồng ý.

## 2. Cấu trúc thư mục

```
<dự án>/
├── SDD-FLOW.md                 # tài liệu này
├── CLAUDE.md / AGENTS.md       # chỉ dẫn ngắn cho agent (xem dưới)
├── README.md                   # cách dựng và chạy dự án; đầu vào từ các bên liên quan
├── CHANGELOG.md                # nhật ký thay đổi theo ngày (mục 7)
├── spec/
│   ├── mission.md              # ┐
│   ├── tech-stack.md           # ├ constitution
│   ├── roadmap.md              # ┘
│   └── YYYY-MM-DD-<ten>/       # một thư mục cho mỗi giai đoạn
│       ├── requirements.md
│       ├── plan.md
│       └── validation.md
├── backlog/                    # ghi chú nghiên cứu cho các điểm chưa chốt (tuỳ chọn)
│   └── YYYY-MM-DD-<chu-de>.md
└── .claude/skills/             # tuỳ chọn (mục 9)
```

Nội dung `CLAUDE.md` / `AGENTS.md` (chỉ trỏ đường, **không chép nội dung constitution vào đây**):

```markdown
Dự án theo Spec-Driven Development. Trước khi làm bất cứ việc gì:
1. Đọc `SDD-FLOW.md`.
2. Đọc `spec/mission.md`, `spec/tech-stack.md`, `spec/roadmap.md`.
3. Nếu roadmap có giai đoạn 🔄, đọc thư mục spec của giai đoạn đó.
```

## 3. Constitution: ba tài liệu nền

Constitution là căn cứ cao nhất của dự án. Spec giai đoạn không được đi ngược constitution.

| File | Trả lời câu hỏi | Nội dung |
|---|---|---|
| `mission.md` | Làm cái gì, cho ai, vì sao? | Sản phẩm; người dùng mục tiêu; yêu cầu của các bên liên quan; điểm khác biệt; nguyên tắc (nội dung, trải nghiệm, khả năng truy cập); thế nào là thành công (đo được: chỉ số, ngưỡng, cách đo); ngoài phạm vi |
| `tech-stack.md` | Làm bằng gì, theo quy ước nào? | Nền tảng, ngôn ngữ, phiên bản; mô hình dữ liệu; cấu trúc URL/API; kiến trúc front-end; thư viện; **kiểm thử** (công cụ, lệnh chạy); lệnh build/chạy; cấu hình và biến môi trường (tên, mặc định, ý nghĩa); vận hành; **khoảng trống / nợ kỹ thuật**; **bảng điểm chưa chốt** |
| `roadmap.md` | Làm theo thứ tự nào? | Các giai đoạn đánh số (có thể nhóm theo mốc), trạng thái ✅ xong · 🔄 đang làm · ⏸ tạm dừng · ⬜ chưa bắt đầu; mỗi giai đoạn có mục tiêu một dòng, danh sách việc `[ ]`, dòng nghiệm thu, "cần chốt trước"; mục "sau ra mắt" |

**Kích thước giai đoạn.** Mỗi giai đoạn làm xong được trong **một phiên làm việc tập trung**, và là **một lát cắt hoàn chỉnh**: merge xong thì dự án vẫn chạy được, giai đoạn đó kiểm thử và review được độc lập. Chia theo tính năng người dùng thấy được, không chia theo tầng kỹ thuật (không có giai đoạn "làm hết database" rồi giai đoạn "làm hết giao diện"); việc chia theo tầng chỉ nằm trong các nhóm nhiệm vụ của `plan.md`. Quá to thì tách (ví dụ "Trang liên hệ — nội dung" và "Trang liên hệ — bản đồ"). Nhiều giai đoạn nhỏ cùng khuôn mẫu đã quen có thể gom vào một spec, nhưng mỗi giai đoạn vẫn là một nhóm nhiệm vụ riêng trong `plan.md`.

**Mốc phát hành (tuỳ chọn).** Dự án cần mốc (MVP, v1.0…) thì nhóm các giai đoạn trong `roadmap.md` dưới tiêu đề mốc (`## Mốc 1 — MVP`); phạm vi mỗi mốc ghi trong `mission.md` thành hai danh sách "Mốc này gồm" và "Để sau". Mốc chỉ để nhóm: mỗi giai đoạn bên trong vẫn đi đủ vòng đời ở mục 4, giai đoạn đầu tiên vẫn nên là bộ khung chạy được. Khi giai đoạn cuối của một mốc được merge, agent đề nghị **nghiệm thu mốc**: người dùng dùng thử toàn bộ sản phẩm, không chỉ từng giai đoạn; lỗi phát hiện ra thành giai đoạn sửa trong chính mốc đó; roadmap ghi mốc ✅ khi người dùng xác nhận.

**Bảng điểm chưa chốt** trong `tech-stack.md`:

```markdown
| Điểm chưa chốt | Phương án | Chốt trước giai đoạn | Ghi chú |
|---|---|---|---|
| Hosting | A / B / C | Giai đoạn 5 | backlog/2026-10-01-hosting.md |
```

Khi người dùng chốt: chuyển nội dung vào phần chính của `tech-stack.md`, xoá dòng khỏi bảng. Nếu `/new-spec` gặp giai đoạn có điểm chưa chốt phụ thuộc, hỏi chốt điểm đó trước.

**Bản thiết kế giao diện.** Nếu dự án có giao diện và đã có bản thiết kế chuẩn (mockup, Figma…), `tech-stack.md` ghi đường dẫn tới nó và ghi rõ đó là nguồn sự thật về giao diện: muốn đổi giao diện thì sửa bản thiết kế trước, sau đó mới sửa code.

## 4. Vòng đời một giai đoạn

```
tạo spec ─► duyệt ─► làm theo plan ─► kiểm chứng ─► review ─► rút kinh nghiệm ─► changelog ─► "merge" ─► rà roadmap
(hỏi 3 vòng)         (mỗi nhóm 1 commit)    │ ❌                                                         (khi cần)
                          ▲                 │
                          └─────────────────┘ sửa rồi kiểm chứng lại toàn bộ
```

| Bước | Ai | Việc | Kết quả |
|---|---|---|---|
| 1. Tạo spec | Agent | Đọc constitution và các spec đã có; chọn giai đoạn ⬜ đầu tiên (hoặc giai đoạn người dùng chỉ định); **hỏi 3 vòng** (mục 4.2); sau đó mới tạo nhánh từ nhánh đích và tạo 3 file spec; đánh dấu roadmap 🔄 | Nhánh `spec/YYYY-MM-DD-<ten>`, 3 file spec. **Chưa commit, chưa code** |
| 2. Duyệt | Người dùng | Đọc 3 file, nhất là mục "Quyết định agent tự chốt"; sửa hoặc đồng ý | Agent commit spec |
| 3. Thực hiện | Agent | Làm lần lượt từng nhóm trong `plan.md`; viết test cùng code; đánh dấu `[x]`; phát sinh thì theo mục 5. Xong nhóm có 🔍 thì dừng, báo cáo, chờ người dùng nói "tiếp" | Mỗi nhóm một commit |
| 4. Kiểm chứng | Agent | Nếu nhánh đích đã có commit mới: merge nhánh đích vào nhánh spec (không rebase), xử lý xung đột. Sau đó chạy mọi mục trong `validation.md`, ghi ✅/❌ kèm bằng chứng vào cột Kết quả. Mục do người dùng kiểm: liệt kê cách kiểm, chờ người dùng báo kết quả | Bảng nghiệm thu có kết quả |
| 4b. Nếu có ❌ | Agent | Sửa, commit, chạy lại **toàn bộ** nghiệm thu. Nếu ❌ do spec sai hoặc thiếu: sửa spec, báo người dùng | Không còn ❌ |
| 5. Review | Agent | Xem lại toàn bộ diff của nhánh so với nhánh đích từ ba góc (dưới đây); sửa cái đáng sửa; cái để sau ghi vào `requirements.md` (mục Để sau) hoặc roadmap | Danh sách phát hiện đã xử lý |
| 6. Rút kinh nghiệm | Agent | Tự hỏi: "Qua giai đoạn này, chỗ nào trong spec/constitution cần làm rõ hoặc đã lỗi thời?"; cập nhật constitution; roadmap đánh dấu ✅ | Constitution khớp hiện trạng |
| 7. Changelog | Agent | Cập nhật `CHANGELOG.md` (mục 7) | Commit riêng |
| 8. Báo cáo, chờ merge | Agent → Người dùng | Báo cáo ngắn: đã làm gì, kết quả nghiệm thu, quyết định đã tự chốt, việc để sau. Chờ người dùng nói "merge" | — |
| 9. Merge | Agent | Làm theo "Cách merge" trong `validation.md`: merge local bằng `git merge --no-ff` rồi xoá nhánh spec, hoặc push nhánh và tạo PR. **Không squash, không rebase** (giữ nguyên SHA đã ghi trong CHANGELOG) | — |
| 10. Rà roadmap | Người dùng + Agent | Khi cần: tách, gộp, đổi thứ tự giai đoạn; cập nhật "cần chốt trước" | Sẵn sàng cho giai đoạn sau |

**Ba góc review (bước 5).** Nếu agent hỗ trợ subagent, chạy song song mỗi góc một subagent:

1. **Đúng spec:** có làm đủ, có làm thừa so với `requirements.md`, có đi ngược constitution không.
2. **Chất lượng:** code dễ hiểu, theo đúng quy ước trong `tech-stack.md`; bảo mật (dữ liệu đầu vào, phân quyền, bí mật); chỗ dễ vỡ.
3. **Kiểm thử và vận hành:** test có thật sự kiểm cái cần kiểm; dựng lại từ đầu có chạy không; phần nào còn thiếu test.

Giai đoạn rất nhỏ có thể bỏ review, nhưng ghi rõ trong mục "Không yêu cầu" của `validation.md`.

### 4.1. Ba file spec

| File | Mục đích | Phải có |
|---|---|---|
| `requirements.md` | **Làm gì, vì sao** | Bối cảnh; phạm vi đủ cụ thể để làm không phải đoán (tên, kiểu, bắt buộc, mặc định); **mẫu cần theo** (file/tính năng có sẵn để bắt chước); ngoài phạm vi; bảng quyết định có ngày; **quyết định agent tự chốt** |
| `plan.md` | **Làm theo thứ tự nào** | Các nhóm nhiệm vụ đánh số theo thứ tự phụ thuộc; mỗi nhóm một commit xem lại được riêng; test đi kèm nhóm tạo ra code đó; nhóm cần người dùng duyệt trước khi làm tiếp có 🔍 ở tiêu đề; nhóm cuối luôn là hoàn tất |
| `validation.md` | **Làm sao biết đã xong** | Lệnh chuẩn bị; kiểm tra tự động; kiểm tra thủ công; mục **không yêu cầu**; nhánh đích; **điều kiện merge** đánh số |

Quyết định nào trong spec có hiệu lực cho cả dự án (ví dụ "mọi trang phải responsive", "ghim phiên bản thư viện") thì đưa lên constitution ngay, spec chỉ dẫn tới đó. Spec gom nhiều giai đoạn thì `validation.md` chia mục kiểm tra theo từng giai đoạn.

Mẫu `requirements.md`:

```markdown
# Requirements — <tên giai đoạn>

## Bối cảnh
<Giai đoạn N trong roadmap; giải quyết vấn đề gì; phục vụ mục tiêu/người dùng nào trong mission.md; liên quan giai đoạn nào trước đó.>

## Phạm vi
| Thứ | Tên | Kiểu | Bắt buộc | Mặc định | Ghi chú |
|---|---|---|---|---|---|

## Mẫu cần theo
- <Theo cách làm ở `đường/dẫn/file`.>

## Ngoài phạm vi
- …

## Để sau
- <Việc phát hiện trong lúc làm/review, chưa làm ở giai đoạn này.>

## Quyết định
| Ngày | Quyết định | Lý do | Ai chốt |
|---|---|---|---|

## Quyết định agent tự chốt
- <Điều agent đã tự quyết khi không có chỉ dẫn, kèm lý do. Người dùng rà và bác bỏ nếu cần.>
```

Mẫu `plan.md`:

```markdown
# Plan — <tên giai đoạn>

## Nhóm 1 — <tên>
- [ ] 1.1 …
- [ ] 1.2 …
- [ ] 1.3 Test cho phần trên

## Nhóm 2 — <tên> 🔍
- [ ] 2.1 …

## Nhóm N — Hoàn tất
- [ ] Chạy toàn bộ validation.md, ghi kết quả
- [ ] Review ba góc
- [ ] Cập nhật constitution nếu có quyết định mới; roadmap ✅
- [ ] Cập nhật CHANGELOG.md
- [ ] Báo cáo, chờ "merge"
```

Mẫu `validation.md`:

````markdown
# Validation — <tên giai đoạn>

Nhánh đích: `main`
Cách merge: merge local `--no-ff` | tạo PR

## Chuẩn bị
```
<lệnh dựng môi trường, cài phụ thuộc, nạp dữ liệu>
```

## Kiểm tra tự động
| # | Lệnh | Mong đợi | Kết quả |
|---|---|---|---|
| A1 | <lệnh build/typecheck> | exit 0, không lỗi | |
| A2 | <lệnh test> | tất cả pass; có test cho: … | |

## Kiểm tra thủ công
| # | Thao tác | Mong đợi | Ai kiểm | Kết quả |
|---|---|---|---|---|
| M1 | … | … | Agent / Người dùng | |

## Kiểm tra nội dung (nếu có nội dung hiển thị cho người dùng)
- [ ] Đúng nguyên tắc nội dung trong mission.md (giọng văn, thuật ngữ)
- [ ] Không còn nội dung giữ chỗ (lorem ipsum, "TODO", "xxx")

## Không yêu cầu
- <Những gì cố ý chưa kiểm ở giai đoạn này.>

## Điều kiện merge
1. Mọi mục ở trên ✅.
2. Không còn code debug, log tạm, TODO tạm, file thừa.
3. CI (nếu dự án có) chạy xanh.
4. …
````

### 4.2. Ba vòng câu hỏi

Hỏi **trước khi ghi bất cứ thứ gì**, vì câu trả lời có thể đổi tên nhánh, tên thư mục, phạm vi. Dùng công cụ hỏi có lựa chọn của agent (trong Claude Code: `AskUserQuestion`); không có công cụ đó thì hỏi bằng danh sách đánh số, mỗi câu có lựa chọn a/b/c.

| Vòng | Cho file | Hỏi về |
|---|---|---|
| 1. Yêu cầu | `requirements.md` | Ranh giới phạm vi, lựa chọn constitution còn để mở, giá trị mặc định, việc để sau |
| 2. Kế hoạch | `plan.md` | Cách làm, làm gì ngay và gì để sau, chia nhóm, nhóm nào cần dừng duyệt (🔍), tên nhánh |
| 3. Nghiệm thu | `validation.md` | Kiểm chứng bằng gì (test tự động, thao tác tay, cả hai), mục nào người dùng tự kiểm, có dựng lại từ đầu không, nhánh đích, cách merge (local hay PR) |

- Mỗi câu 2–4 lựa chọn, **phương án khuyến nghị đứng đầu**, mỗi lựa chọn kèm một dòng đánh đổi.
- **Không hỏi lại điều constitution đã chốt**; ghi thẳng vào spec và dẫn tới chỗ đã chốt.
- Giai đoạn nhỏ mà constitution đã trả lời gần hết: **gộp 3 vòng thành một lượt** (mỗi file một câu).
- Người dùng muốn giám sát chặt (dự án lớn, hệ thống đang chạy thật): đánh 🔍 cho mọi nhóm và merge qua PR.
- Người dùng giao quyền "tự quyết đi": vẫn ghi mọi điều đã tự chốt vào mục "Quyết định agent tự chốt".

## 5. Khi có thay đổi giữa chừng

Nguyên tắc: **sửa tài liệu trước (hoặc cùng commit), code đi sau.** Mỗi lần người dùng thêm/bớt yêu cầu, agent tự hỏi "chỗ nào trong spec hoặc constitution phải ghi lại điều này?".

### 5.1. Các loại thay đổi

| Loại | Ví dụ | Xử lý |
|---|---|---|
| **Trong giai đoạn đang làm** | Tách một component ra file riêng; thêm một trang tối thiểu | Sửa `plan.md` (thêm/sửa nhóm), cập nhật `requirements.md` và `validation.md` cho khớp, thêm dòng vào bảng Quyết định, rồi mới làm |
| **Xuyên suốt dự án** | Đổi/thêm công cụ test; yêu cầu responsive; màu thương hiệu; quy ước đặt tên | Theo thứ tự: (1) sửa constitution; (2) rà **mọi spec còn liên quan và roadmap**, cập nhật cho khớp; (3) sửa code; (4) thêm/sửa test. Nếu phải sửa code nhiều nơi: thêm thành một giai đoạn trong roadmap và làm theo quy trình đầy đủ. Nếu chỉ sửa tài liệu: nhánh `chore/…` |
| **Sắp xếp lại roadmap** | Gộp nhiều giai đoạn; tách giai đoạn quá to; đổi thứ tự | Trên nhánh `chore/…`: sửa `roadmap.md`, ghi lý do trong commit, cập nhật "cần chốt trước", chờ "merge". Chỉ làm khi không có giai đoạn 🔄 |
| **Tạm dừng / huỷ giai đoạn** | Người dùng đổi ưu tiên giữa chừng | Ghi lý do và tình trạng vào `requirements.md` (mục Quyết định), commit trên nhánh spec. Tạm dừng: roadmap ghi ⏸ và giữ nhánh. Huỷ: roadmap trả về ⬜ hoặc bỏ giai đoạn; xoá nhánh chỉ khi người dùng đồng ý |
| **Cần nghiên cứu** | Chọn cơ sở dữ liệu, hosting, dịch vụ ngoài | Nghiên cứu, lưu vào `backlog/YYYY-MM-DD-<chu-de>.md` (bối cảnh, phương án, ưu/nhược, khuyến nghị), link từ bảng điểm chưa chốt. Chỉ đưa vào constitution khi người dùng chốt |

### 5.2. Đường tắt cho thay đổi nhỏ

Được bỏ qua thư mục spec khi thay đổi **thuộc loại** sửa lỗi rõ ràng, sửa chữ, nâng phiên bản phụ thuộc nhỏ, chỉnh giao diện theo đúng bản thiết kế đã có, **và** không đụng tới constitution.

Các bước: nhánh `fix/YYYY-MM-DD-<ten>` → sửa → thêm test nếu là lỗi → chạy build và toàn bộ test → cập nhật CHANGELOG → báo cáo, chờ "merge".

Trong lúc làm mà phát hiện phải đổi quy ước, thêm thư viện, đổi mô hình dữ liệu: **dừng lại, báo người dùng, chuyển sang quy trình đầy đủ**.

## 6. Đặt tên và ký hiệu

| Thứ | Quy ước | Ví dụ |
|---|---|---|
| Thư mục spec | `spec/YYYY-MM-DD-<ten-khong-dau>`, ngày tạo spec | `spec/2026-10-01-dang-nhap` |
| Nhánh spec | trùng tên thư mục spec | `spec/2026-10-01-dang-nhap` |
| Nhánh đường tắt | `fix/YYYY-MM-DD-<ten-khong-dau>` | `fix/2026-10-01-sua-link-footer` |
| Nhánh chỉ sửa tài liệu | `chore/YYYY-MM-DD-<ten-khong-dau>` (constitution, roadmap, backlog) | `chore/2026-10-01-gop-giai-doan` |
| Ghi chú nghiên cứu | `backlog/YYYY-MM-DD-<chu-de>.md` | `backlog/2026-10-01-hosting.md` |
| Trạng thái roadmap | ✅ xong · 🔄 đang làm · ⏸ tạm dừng · ⬜ chưa bắt đầu | |
| Kết quả nghiệm thu | ✅ đạt · ❌ không đạt, kèm ghi chú | |
| Điểm dừng duyệt | 🔍 ở tiêu đề nhóm trong `plan.md` | `## Nhóm 2 — Mô hình dữ liệu 🔍` |
| Commit spec | `Spec: <tên giai đoạn>` | `Spec: đăng nhập` |
| Commit nhóm nhiệm vụ | Ngắn, bắt đầu bằng động từ, ghi số nhóm | `Nhóm 2: thêm form đăng nhập và test` |

Tên không dấu, chữ thường, nối bằng `-`.

## 7. CHANGELOG

Định dạng:

```markdown
# Changelog

<!-- changelog-last: <SHA đầy đủ của commit cuối cùng đã ghi> -->

## 2026-10-02

- Thêm form đăng nhập và kiểm tra dữ liệu đầu vào (a1b2c3d)

## 2026-10-01

- Tạo constitution: mission, tech-stack, roadmap (9f8e7d6)
```

Cách cập nhật (trước mỗi lần merge):

1. Đọc SHA trong marker `changelog-last`. Chưa có file: lấy toàn bộ lịch sử.
2. Lấy các commit mới: `git log <SHA>..HEAD --no-merges --date=short --format='%ad %h %s'`.
3. Bỏ các commit chỉ sửa `CHANGELOG.md`. Gộp theo ngày, mới nhất ở trên; ngày đã có mục thì thêm vào mục đó. Mỗi dòng viết lại thành câu dễ hiểu cho người đọc (không chép nguyên văn commit nếu khó hiểu), cuối dòng có short SHA.
4. Cập nhật marker thành SHA của `HEAD` (trước commit changelog).
5. Commit riêng: `Cập nhật CHANGELOG`.

SHA trong CHANGELOG chỉ còn đúng nếu nhánh được merge bằng `--no-ff` (không squash, không rebase). Nếu merge bị xung đột ở `CHANGELOG.md`: giữ mục của cả hai bên, sắp lại theo ngày, marker lấy SHA mới hơn trong lịch sử của nhánh đích.

## 8. Khởi tạo

### 8.1. Dự án mới

1. Đọc `README.md` và các tài liệu đầu vào có sẵn (yêu cầu của các bên liên quan, danh sách tính năng, bản thiết kế). Thiếu đầu vào thì hỏi người dùng mô tả ngắn sản phẩm trước. Chưa có Git: hỏi người dùng rồi chạy `git init` với nhánh chính `main`.
2. Hỏi người dùng theo 3 nhóm, **trước khi ghi ra đĩa**: mission (sản phẩm, người dùng, thành công), tech stack (nền tảng, kiểm thử, vận hành), roadmap (thứ tự ưu tiên, phạm vi bản đầu tiên).
3. Tạo `spec/mission.md`, `spec/tech-stack.md`, `spec/roadmap.md` theo [mục 3](#3-constitution-ba-tài-liệu-nền). Roadmap chia giai đoạn nhỏ; giai đoạn 1 nên là bộ khung chạy được (dựng dự án, lệnh build, lệnh test chạy được dù chưa có test).
4. Tạo `CLAUDE.md` hoặc `AGENTS.md` theo [mục 2](#2-cấu-trúc-thư-mục), và `.gitignore` chặn sẵn file bí mật (`.env`, khoá…). Cần biến môi trường thì tạo `.env.example` chỉ chứa tên biến, không chứa giá trị.
5. Báo người dùng duyệt constitution. Duyệt xong thì commit lên `main` (`Tạo constitution`). Đây là commit duy nhất được làm thẳng trên nhánh chính. Sau đó đề nghị tạo spec cho giai đoạn 1.

### 8.2. Dự án có sẵn

Constitution được **dựng ngược từ hiện trạng**:

1. Đọc code, cấu hình, `README.md`, TODO/issue có sẵn, lịch sử git gần đây.
2. Hỏi người dùng về mission, người dùng mục tiêu, khoảng trống trong tech stack, và việc ưu tiên sắp tới, **trước khi ghi ra đĩa**.
3. Tạo constitution:
   - `tech-stack.md` mô tả **cái đang có** (kể cả nợ kỹ thuật, ghi ở mục Khoảng trống), không mô tả cái mong muốn như thể đã có.
   - `mission.md` từ README và câu trả lời của người dùng.
   - `roadmap.md` từ TODO/issue và mong muốn của người dùng, chia giai đoạn nhỏ. Phần đã làm xong không cần viết spec bù, chỉ ghi một dòng "Hiện trạng" ở đầu roadmap.
4. Nếu dự án chưa có test hoặc test không chạy được: đề xuất giai đoạn 1 là "dựng bộ test tối thiểu" trước khi sửa tính năng.
5. Làm toàn bộ việc trên trong nhánh `chore/YYYY-MM-DD-constitution`, kể cả tạo `CLAUDE.md`/`AGENTS.md`. Chờ người dùng duyệt, commit, rồi chờ "merge".

## 9. Skill (tuỳ chọn)

Nếu repo có `.claude/skills/`, ưu tiên dùng skill tương ứng; nếu không, làm theo tài liệu này là đủ.

| Skill | Tương ứng | Ghi chú |
|---|---|---|
| `/new-spec` | Mục 4 bước 1 + mục 4.2 | Đọc quy ước từ constitution mỗi lần chạy, không chép quy ước vào skill |
| `/changelog` | Mục 7 | |

Đặt `disable-model-invocation: true` trong frontmatter để skill chỉ chạy khi được gọi. Khi người dùng gõ lại cùng một yêu cầu dài lần thứ hai, đề nghị đóng nó thành skill.
