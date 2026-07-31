# CLAUDE.md

Repo này chứa **một output style** cho Claude Code: `giai-thich-de-hieu.md`. Lý do tồn tại, cách cài,
và bằng chứng đo được nằm ở [README](README.md) — đừng lặp lại ở đây.

File này chỉ giữ những thứ cần biết **trước khi sửa**.

## ⚑ File này có HAI bản — sửa một bên là lệch

| Ở đâu                                           | Vai gì                          |
| :---------------------------------------------- | :------------------------------ |
| `./giai-thich-de-hieu.md`                       | bản gốc, cái người khác tải về  |
| `~/.claude/output-styles/giai-thich-de-hieu.md` | bản đang chạy trên máy chủ repo |

Sửa xong phải đồng bộ, và **kiểm bằng `diff`** chứ không tin là đã copy:

```bash
cp giai-thich-de-hieu.md ~/.claude/output-styles/giai-thich-de-hieu.md
diff -q giai-thich-de-hieu.md ~/.claude/output-styles/giai-thich-de-hieu.md && echo KHỚP
```

⚑ Đổi **tên file** là đổi luôn tên style: người dùng phải chọn lại trong `/config`, và dòng
`"outputStyle"` trong `settings.json` của họ chết. Coi tên file là một phần giao diện công khai.

## Sửa xong phải đo, không được suy luận

Bốn phép đo dưới đây đã bắt được lỗi thật ít nhất một lần mỗi cái.

**1. Style có tác dụng không** — chạy hai lần, khác duy nhất ở chỗ có file hay không:

```bash
D=$(mktemp -d); mkdir -p "$D/.claude/output-styles"
cp giai-thich-de-hieu.md "$D/.claude/output-styles/"; cd "$D"
claude -p "<câu hỏi>" --setting-sources project,local
claude -p "<câu hỏi>" --setting-sources project,local --settings '{"outputStyle":"giai-thich-de-hieu"}'
```

⚑⚑ **Bỏ `--setting-sources project,local` là phép đo mất giá trị.** Nó cắt cấu hình cá nhân ra. Nếu
máy có hook lúc-mở-phiên bơm giọng tương tự, hai nhánh giống nhau và ta kết luận sai theo cả hai
hướng. Đã dính đúng lỗi này lần đo đầu tiên.

⚑ **Chọn câu hỏi đủ khó.** "index là gì" thì hai nhánh trả lời y hệt — style không có chỗ lộ ra. Cần
câu **buộc xưng hô và buộc khuyên chọn**.

**2. Ô `★ Insight` còn chạy** — giao một việc **có bẫy thật** (ví dụ: chọn kiểu số để tính tiền) thì
phải ra ô; giao việc máy móc (in "Xin chào") thì **không** được ra ô. Thiếu nhánh thứ hai là không
chứng minh được gì, vì "không ra ô" cũng là kết quả đúng.

**3. Gộp/sửa nội dung không rơi luật** — lấy danh sách cụm từ khoá của bản cũ, `grep -qF` từng cụm
trên bản mới, đếm số cụm mất. Kèm một cụm **không tồn tại** để chứng minh dụng cụ báo được khi thiếu.

**4. Con trỏ chết** — mọi con trỏ dạng `§ <tên mục>` trong file style phải khớp một heading thật trong README. Đổi tên
mục README là phải sửa comment trong file style.

## Cái bẫy đắt nhất, đọc trước khi sửa frontmatter

**Đừng thêm `name:`.** Bộ nạp so `name` với tên file, phân biệt hoa thường. Lệch một chữ ⇒ **âm thầm bỏ
toàn bộ nội dung**, mà style vẫn hiện trong `/config` và trên thanh trạng thái nên trông y như đang
chạy. Không khai thì không có gì để lệch.
Issue còn mở: [anthropics/claude-code#47482](https://github.com/anthropics/claude-code/issues/47482).

**Comment `<!-- -->` trong file style bị tính tiền mỗi phiên** — nó KHÔNG bị lược, vào thẳng chỉ dẫn của
Claude, nguyên văn. Đo được: một khối 6 dòng xuất hiện đủ 6 dòng. Nên lời dặn dài để ở README hoặc file
này, không để trong file style.

## Đừng đề xuất lại — đã quyết, có lý do

- **Đóng thành plugin.** Thêm hai file JSON thủ tục, thêm một bước cài, mà vẫn phải vào `/config` chọn.
- **Cơ chế tự cập nhật.** Giá trị của repo **là** đoạn chữ, mà đoạn chữ là thứ người dùng phải sửa cho
  mình ⇒ tự cập nhật = ghi đè đúng phần họ vừa sửa.
- **Bắt người dùng tắt plugin `explanatory-output-style`.** Đã đo hai đường độc lập: bật hay tắt đều
  không đổi kết quả. Nó chỉ tốn 1.192 byte/phiên, không phá. Đây là câu **đã sai một lần rồi mới đo**.

## Nếu có issue báo "cài rồi mà không thấy gì"

Hỏi theo thứ tự này, đây là bốn nguyên nhân đã gặp thật:

1. **Chưa mở phiên mới.** Output style chỉ được đọc **một lần lúc mở phiên**. Thêm `outputStyle` giữa
   phiên thì phiên đó không có style nào cả. Đây là nguyên nhân từng bị chẩn đoán sai thành "plugin đè
   lên style".
2. **Có khai `name:`** hoặc **tên file bị đổi** — xem bẫy ở trên.
3. **Đang xem báo cáo của subagent.** Output style chỉ áp cho hội thoại chính; mỗi subagent chạy chỉ
   dẫn riêng nên báo cáo nó gửi về vẫn đầy thuật ngữ. Không phải style hỏng.
4. **Việc quá máy móc nên không có ô Insight.** Đúng thiết kế, không phải lỗi.
