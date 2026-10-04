# SDD-FLOW

**English** · [Tiếng Việt](#tiếng-việt)

A drop-in **Spec-Driven Development** workflow for projects built with AI coding agents (Claude Code, Codex, Cursor…).

Put one file in your project root and the agent knows how to work: write the spec first, ask before deciding, implement in reviewable steps, verify with evidence, and never merge without your say-so.

## What you get

- **Constitution** — three foundation documents (`mission.md`, `tech-stack.md`, `roadmap.md`) that every later decision must respect.
- **Per-phase specs** — `requirements.md`, `plan.md`, `validation.md` for each roadmap phase, with ready-to-use templates.
- **A full phase lifecycle** — question rounds → approval → implementation → validation → review → retrospective → changelog → merge.
- **Rules for change** — mid-phase changes, project-wide changes, roadmap reshuffles, pausing or cancelling a phase, and a fast track for small fixes.
- **Bootstrapping** for both new and existing (legacy) projects.
- **Agent-agnostic** — plain Markdown in the repo; skills are optional.

## Usage

1. Copy one file into your project root and rename it to `SDD-FLOW.md`:
   - [`SDD-FLOW-en.md`](SDD-FLOW-en.md) — English (specs and commits in English)
   - [`SDD-FLOW-vi.md`](SDD-FLOW-vi.md) — Vietnamese (specs and commits in Vietnamese)

   Then delete the language switcher line under the title, since the other file won't be in your project.
2. Open your agent in the project and say:

   > Read `SDD-FLOW.md` and bootstrap SDD for this project.

3. The agent inspects the repo, interviews you, and drafts the constitution. From then on, just say what you want in plain words (e.g. "start the next phase", "merge"); the agent maps it onto the workflow.

The workflow assumes Git. The agent creates `CLAUDE.md` or `AGENTS.md` so future sessions read `SDD-FLOW.md` automatically.

## Resulting project layout

```
<project>/
├── SDD-FLOW.md
├── CLAUDE.md / AGENTS.md
├── CHANGELOG.md
├── spec/
│   ├── mission.md
│   ├── tech-stack.md
│   ├── roadmap.md
│   └── YYYY-MM-DD-<phase>/
│       ├── requirements.md
│       ├── plan.md
│       └── validation.md
└── backlog/            # optional research notes
```

## License

[MIT](LICENSE)

---

## Tiếng Việt

Quy trình **Spec-Driven Development** dùng ngay cho dự án làm cùng AI agent (Claude Code, Codex, Cursor…).

Đặt một file vào thư mục gốc dự án là agent biết cách làm việc: viết spec trước, hỏi trước khi quyết, làm từng bước xem lại được, kiểm chứng bằng bằng chứng, và không bao giờ merge khi bạn chưa đồng ý.

### Có gì trong đó

- **Constitution**: ba tài liệu nền (`mission.md`, `tech-stack.md`, `roadmap.md`), mọi quyết định sau phải tuân theo.
- **Spec cho từng giai đoạn**: `requirements.md`, `plan.md`, `validation.md` cho mỗi giai đoạn trong roadmap, kèm mẫu dùng ngay.
- **Vòng đời đầy đủ**: hỏi → duyệt → làm → kiểm chứng → review → rút kinh nghiệm → changelog → merge.
- **Quy tắc khi có thay đổi**: thay đổi trong giai đoạn, thay đổi xuyên suốt, sắp xếp lại roadmap, tạm dừng hoặc huỷ giai đoạn, đường tắt cho sửa lỗi nhỏ.
- **Khởi tạo** cho cả dự án mới và dự án đang chạy.
- **Không phụ thuộc agent**: markdown thuần trong repo, skill là tuỳ chọn.

### Cách dùng

1. Chép một file vào thư mục gốc dự án và đổi tên thành `SDD-FLOW.md`:
   - [`SDD-FLOW-vi.md`](SDD-FLOW-vi.md): tiếng Việt (spec và commit bằng tiếng Việt)
   - [`SDD-FLOW-en.md`](SDD-FLOW-en.md): tiếng Anh (spec và commit bằng tiếng Anh)

   Sau đó xoá dòng chuyển ngôn ngữ dưới tiêu đề, vì file kia không có trong dự án.
2. Mở agent trong dự án và nói:

   > Đọc `SDD-FLOW.md` và khởi tạo SDD cho dự án này.

3. Agent xem repo, phỏng vấn bạn và soạn constitution. Từ đó cứ nói việc bạn muốn bằng lời thường (ví dụ "làm giai đoạn tiếp theo", "merge"), agent sẽ tự đối chiếu với quy trình.

Quy trình cần Git. Agent sẽ tạo `CLAUDE.md` hoặc `AGENTS.md` để các phiên sau tự đọc `SDD-FLOW.md`.

### Giấy phép

[MIT](LICENSE)
