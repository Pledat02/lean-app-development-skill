# Lean App Development Skill

Skill Codex gọn nhẹ dành cho ứng dụng production quy mô nhỏ, thường khoảng 50-1000 người dùng. Skill chọn độ sâu công việc theo rủi ro thực tế thay vì áp dụng một quy trình enterprise cố định.

Skill kết hợp bốn góc nhìn trong một luồng làm việc và có chế độ viết specification:

- Product: kết quả, phạm vi và acceptance criteria.
- Architecture: ranh giới, dữ liệu, failure mode và trade-off.
- Development: đọc code trước khi sửa, theo convention và thay đổi tối thiểu.
- Quality: kiểm thử dựa trên yêu cầu và rủi ro.

Không phụ thuộc AWS, Amazon Bedrock, binary riêng, hooks, MCP server hoặc workflow engine. Skill sử dụng model/provider đang được Codex cấu hình.

## Cấu trúc

```text
lean-app-development-skill/
├── README.md
└── skills/
    └── lean-app-development/
        ├── SKILL.md
        ├── agents/
        │   └── openai.yaml
        └── references/
            ├── workflow.md
            ├── engineering-guardrails.md
            ├── specification.md
            └── verification.md
```

## Cài đặt từ repository private

Yêu cầu Git hoặc GitHub CLI đã được xác thực bằng tài khoản có quyền đọc repository. Cài vào thư mục skill cá nhân trên Windows:

```powershell
gh auth login
python "$env:USERPROFILE\.codex\skills\.system\skill-installer\scripts\install-skill-from-github.py" `
  --repo Pledat02/lean-app-development-skill `
  --path skills/lean-app-development `
  --method git
```

Trên macOS hoặc Linux:

```bash
gh auth login
python3 "$HOME/.codex/skills/.system/skill-installer/scripts/install-skill-from-github.py" \
  --repo Pledat02/lean-app-development-skill \
  --path skills/lean-app-development \
  --method git
```

Khởi động lại Codex sau khi cài để danh sách skill được nạp lại.

Để vendor skill vào một project và commit cùng source code, chạy từ root project:

```powershell
python "$env:USERPROFILE\.codex\skills\.system\skill-installer\scripts\install-skill-from-github.py" `
  --repo Pledat02/lean-app-development-skill `
  --path skills/lean-app-development `
  --dest .agents/skills `
  --method git
```

Bạn cũng có thể yêu cầu Codex dùng skill installer:

```text
Use $skill-installer to install the skill from the private GitHub repository
https://github.com/Pledat02/lean-app-development-skill/tree/main/skills/lean-app-development
```

Codex hoặc GitHub CLI phải được xác thực với tài khoản có quyền truy cập repository private.

## Cập nhật

PowerShell:

```powershell
git -C "$env:USERPROFILE\.codex\skills\lean-app-development" pull --ff-only
```

macOS/Linux:

```bash
git -C "$HOME/.codex/skills/lean-app-development" pull --ff-only
```

## Sử dụng

Gọi trực tiếp:

```text
$lean-app-development triển khai chức năng đặt lịch và ngăn hai người đặt trùng cùng một khung giờ
```

```text
$lean-app-development sửa lỗi form đăng nhập nhưng giữ nguyên API hiện tại
```

```text
$lean-app-development review migration này và đề xuất cách rollback an toàn
```

Viết specification mà chưa triển khai:

```text
$lean-app-development viết spec cho chức năng đặt sân, lưu tại docs/specs/booking/spec.md
```

Spec mode tạo tài liệu có problem, scope, user flow, requirements, business rules, data/API impact, security, failure cases, acceptance criteria, verification strategy và open decisions. Một yêu cầu chỉ viết spec không tự động cho phép triển khai code.

Skill cũng cho phép Codex tự kích hoạt khi yêu cầu phù hợp với mô tả trong `SKILL.md`.

## Ba mức công việc

### Quick

Dành cho tài liệu, nội dung, styling, cấu hình nhỏ hoặc bug cô lập:

```text
Inspect → Change → Focused verification → Handoff
```

### Standard

Dành cho feature/refactor thông thường:

```text
Context → Acceptance criteria → Impact/design
→ Implement vertical slice → Verify → Handoff
```

### Critical

Dành cho authentication, authorization, payment, migration, concurrency, dữ liệu cá nhân, security hoặc production deployment:

```text
Requirements → Risks and rollback → Approval when needed
→ Implement → Broader verification → Deployment readiness
```

Số lượng người dùng chỉ là tín hiệu về capacity. Một hệ thống 50 người dùng vẫn thuộc mức Critical nếu lỗi có thể làm mất tiền, lộ dữ liệu hoặc tạo booking trùng.

## Tùy biến theo project

Đặt các quyết định riêng của project trong `AGENTS.md`, project memory hoặc một domain skill riêng. Ví dụ:

- Framework và package manager bắt buộc.
- Kiến trúc module hiện tại.
- Database, ORM và identity provider.
- Quy tắc migration, authorization và logging.
- Lệnh lint, build và test chính thức.
- Tester agent bắt buộc sau khi sửa frontend/backend.

Project instructions được ưu tiên hơn các mặc định chung của skill này.

## Phạm vi không phù hợp

Skill không thay thế quy trình chuyên biệt cho:

- Hệ thống hyperscale hoặc distributed platform phức tạp.
- Phần mềm chịu quy định pháp lý yêu cầu lifecycle/audit chính thức.
- Safety-critical software.
- Incident response production đang diễn ra.
- Nghiên cứu thị trường hoặc quản lý portfolio sản phẩm.

## Nguồn ý tưởng

Workflow được viết lại theo hướng tối giản, lấy cảm hứng từ các nguyên tắc Product, Architecture, Developer, Quality, brownfield safeguards và risk-based scope của `awslabs/aidlc-workflows`. Repository này không yêu cầu runtime AI-DLC và không sao chép state engine, hooks, AWS configuration hoặc agent orchestration của AI-DLC.
