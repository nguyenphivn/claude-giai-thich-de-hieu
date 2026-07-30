# claude-giai-thich-de-hieu

Một **output style** cho Claude Code: bắt Claude giải thích công việc bằng **tiếng Việt thường, cho
người không rành code** — dẫn bằng kết quả trước, không ký hiệu, không bảng dày, và chỉ giải thích khi
thật sự có gì để giải thích.

Viết cho người **vibecode**: bạn có ý tưởng, bạn để Claude viết code, và bạn cần hiểu **nó vừa làm gì
và vì sao** — chứ không phải học nghề lập trình để đọc nổi câu trả lời.

Mặc định, Claude trả lời như đang nói với một đồng nghiệp lập trình: nhãn `AF-0012`, bảng năm cột, "định
tuyến", "bề mặt", "over-scoped". Nó không sai, nhưng bạn đọc xong vẫn không biết **nó vừa làm gì cho
mình**. Style này bắt nó nói bằng tiếng của bạn — mà vẫn giữ nguyên phần giải thích **vì sao**, chứ
không rút gọn thành "đã xong".

|                    | Mặc định                                               | Với style này                                                                |
| :----------------- | :----------------------------------------------------- | :--------------------------------------------------------------------------- |
| Mở đầu câu trả lời | mô tả cách làm, kết quả nằm cuối                       | **kết quả trước**, giải thích sau                                            |
| Khi cần bạn quyết  | liệt kê phương án bằng thuật ngữ                       | mỗi cách được gì mất gì bằng lời, kèm đề xuất, rồi **một** câu hỏi           |
| Nhãn, bảng, khung  | `D1`, `AF-0012`, bảng năm cột, khung trang trí tự phát | cấm — trừ ô `★ Insight`                                                      |
| Ô `★ Insight`      | (style Explanatory) **mỗi lượt**                       | chỉ khi có đánh đổi thật, điều bất ngờ, hoặc số đo lật ngược điều tưởng đúng |

Dòng cuối là khác biệt đáng kể nhất. Chữ _"always"_ trong style Explanatory gốc chính là thứ đẻ ra
những ô Insight viết lấy lệ; ở đây nó được thay bằng điều kiện, và có câu **"không có gì đáng nói thì
bỏ trống"**.

## Cài

Hai bước. Không có bước ba.

**Bước 1 — tải file về:**

```bash
mkdir -p ~/.claude/output-styles && curl -fsSL -o ~/.claude/output-styles/giai-thich-de-hieu.md \
  https://raw.githubusercontent.com/nguyenphivn/claude-giai-thich-de-hieu/main/giai-thich-de-hieu.md
```

⚑ **Giữ nguyên tên file.** Tên file _chính là_ tên style. Xem cái bẫy đầu tiên bên dưới.

**Bước 2 — bật nó lên.** Chạy `/config` trong Claude Code, chọn **Output style** →
`giai-thich-de-hieu`. Có hiệu lực từ `/clear` hoặc phiên sau, vì output style là phần chỉ dẫn Claude
chỉ đọc một lần lúc mở phiên.

Muốn bật sẵn không qua menu thì thêm vào `~/.claude/settings.json`:

```json
{ "outputStyle": "giai-thich-de-hieu" }
```

### Cẩn thận: có HAI thứ cùng tên "Explanatory"

Đây là chỗ dễ lẫn nhất, kể cả với người đã dùng Claude Code lâu.

|                   | Style **Explanatory** có sẵn                                        | Plugin **`explanatory-output-style`**            |
| :---------------- | :------------------------------------------------------------------ | :----------------------------------------------- |
| Ở đâu ra          | dựng sẵn trong Claude Code, cài là có                               | phải tự vào `/plugin` cài mới có                 |
| Có phá style này? | **không** — các output style loại trừ nhau, chọn cái này là thay nó | **không**, đã đo — nhưng nó tốn chỗ mỗi phiên    |
| Cần làm gì        | không cần làm gì cả                                                 | tắt nếu bạn không dùng nó, cho gọn               |

Claude Code **tự thêm cửa hàng plugin chính thức** lúc khởi động, nên nhiều người tưởng plugin trong đó
cũng được cài sẵn. Tài liệu nói rõ là không: thêm cửa hàng chỉ để xem catalog, _"chưa có plugin nào được
cài"_. Ai đang có plugin đó là đã từng tự bấm cài.

### Có cần tắt plugin đó không

**Không bắt buộc.** Câu này đã đo, không phải suy luận — vì suy luận ban đầu đã sai.

Bật file style lên rồi chạy cùng một việc hai lần, một lần có plugin một lần không. Kết quả gần như y
hệt, và **cả hai lần đều không đẻ ra ô Insight thừa**. Thử cả hai loại việc: một câu cần khuyên chọn, và
một việc **viết code thật** — chỗ sau là chỗ plugin lẽ ra phải nổ, vì chữ của nó ghi rõ _"trước và sau
khi viết code, lúc nào cũng phải..."_.

Kèm đối chứng để phép thử không mù: hỏi thẳng Claude xem đoạn chữ tiếng Anh của plugin có nằm trong chỉ
dẫn không. Có cờ nạp plugin thì nó trả lời có, không cờ thì không. Nên plugin **thật sự đã nạp** lúc đo,
chứ không phải "không thấy tác dụng vì nó chưa bao giờ ở đó".

Đo hai đường độc lập, cùng một kết quả: một lần nạp plugin bằng cờ tạm cho đúng một lần chạy, một lần
bật nó thật trong cấu hình rồi mở phiên mới. Cả hai đều không thấy ô Insight thừa.

Vẫn có một lý do để tắt, nhưng là lý do **tốn kém**, không phải lý do sai: plugin đó bơm **1.192 byte
(140 từ)** vào mọi phiên, dặn những điều file này đã dặn — mà còn dặn ngược ở đúng một điểm ("lúc nào
cũng phải có ô Insight" so với "không có gì đáng nói thì bỏ trống"). Không dùng thì tắt cho gọn:

```bash
claude plugin disable explanatory-output-style@claude-plugins-official
```

Xem mình có nó hay không bằng `claude plugin list`.

## Hai dòng bạn cần sửa cho mình

Mở `~/.claude/output-styles/giai-thich-de-hieu.md`, mục **"Người đọc — sửa hai dòng này cho mình"**:

- **Người đọc là ai.** File điền mặc định là "chủ tiệm kiêm quản lý sản phẩm" — đổi thành đúng vai của
  bạn. Ví dụ: "người tự dựng sản phẩm bằng AI, hiểu ý tưởng nhưng không viết code", "nhà thiết kế đang
  tự làm app", "người làm marketing tự sửa website". Càng **cụ thể** càng tốt: đây là thứ điều khiển
  toàn bộ giọng điệu, để chung chung kiểu "người không biết code" là style yếu đi rõ rệt.
- **Xưng hô.** Mặc định gọi bạn là "anh", Claude tự xưng "em". Đổi thành "chị/em", "bạn/mình", hay bất
  cứ cặp nào bạn muốn.

Chỉ hai dòng đó. Phần còn lại dùng nguyên được.

## Những cái bẫy đã biết

**Đừng thêm `name:` vào frontmatter.** Bộ nạp so trường `name` với **tên file**, phân biệt hoa thường.
Lệch nhau một chữ thì nó **âm thầm bỏ toàn bộ nội dung** bên dưới — mà style vẫn hiện trong menu
`/config` và trên thanh trạng thái, nên trông y như đang chạy. Không khai `name` thì không có gì để
lệch. Lỗi này còn mở:
[anthropics/claude-code#47482](https://github.com/anthropics/claude-code/issues/47482).

**Subagent không theo style.** Output style chỉ áp dụng cho hội thoại chính; mỗi subagent chạy bằng
chỉ dẫn riêng của nó. Nên báo cáo một subagent gửi về vẫn có thể đầy thuật ngữ tiếng Anh. Đó không
phải style hỏng.

**Ghi chú dài trong file này bị tính tiền mỗi phiên.** Comment HTML trong output style **không** bị
lược bỏ — nó vào thẳng phần chỉ dẫn của Claude, nguyên văn, ở mọi phiên. Đo được: bản trước của file
này có một khối comment 6 dòng và nó xuất hiện đủ cả 6 dòng. Vì vậy mọi lời dặn cho người bảo trì nằm
ở README này, còn file style chỉ giữ đúng phần Claude cần đọc.

## Vài lựa chọn cố ý

**`keep-coding-instructions: true`.** Style này chỉ đổi **cách nói**, không đổi cách làm việc. Bật cờ
này để giữ nguyên toàn bộ chỉ dẫn lập trình sẵn có của Claude Code — cách khoanh phạm vi sửa, cách
viết comment, cách tự kiểm chứng. Bỏ cờ này đi là mất hết những thứ đó.

**Không đóng thành plugin.** Một output style vốn đã là một file văn bản; bọc thành plugin chỉ thêm
hai file JSON thủ tục và một bước cài, mà vẫn phải vào `/config` chọn. Tệ hơn: giá trị của nó **là**
đoạn chữ, và đoạn chữ là thứ bạn phải sửa cho mình — nên một cơ chế tự cập nhật sẽ ghi đè đúng phần
bạn vừa sửa.

**Dùng cho ngôn ngữ khác.** Dịch file, lưu bằng tên khác, tên file mới thành tên style mới. Cấu trúc —
kết quả trước, danh sách cấm, khối bốn bước khi cần người đọc quyết định, mục trung thực — không phụ
thuộc tiếng Việt.

## Có tác dụng thật không

Đo được, không phải tự khen. Cùng một câu hỏi — _"Tôi nên dùng PostgreSQL hay MySQL cho tiệm sửa điện
thoại của tôi?"_ — chạy hai lần, điều kiện giống nhau hoàn toàn, khác duy nhất ở chỗ có file này hay
không:

|                 | Không có file                                                                            | Có file                                                                                    |
| :-------------- | :--------------------------------------------------------------------------------------- | :----------------------------------------------------------------------------------------- |
| Xưng hô         | "bạn"                                                                                    | "anh" / "em"                                                                               |
| Thuật ngữ       | `NUMERIC`, "silent truncation", `CHECK constraint`, `EXCLUDE`, `JSONB`, "Prisma/Drizzle" | không một mã nào; `JSONB` được dịch thành "một ô dạng ghi chú có cấu trúc mà vẫn tìm được" |
| Ví dụ           | "lịch hẹn, tồn kho linh kiện"                                                            | "iPhone thì IMEI, laptop thì số serial, máy cũ thì ghi chú tình trạng vỏ"                  |
| Kết câu trả lời | thêm một đề xuất thứ ba không ai hỏi (SQLite)                                            | **một** câu hỏi rõ ràng để chốt                                                            |

Tự kiểm lại được. Đặt file vào `.claude/output-styles/` của một thư mục trống rồi chạy hai lệnh này —
cờ `--setting-sources project,local` cắt tầng cấu hình cá nhân, để kết quả không bị chính cấu hình của
bạn làm nhiễu:

```bash
claude -p "<câu hỏi>" --setting-sources project,local
claude -p "<câu hỏi>" --setting-sources project,local --settings '{"outputStyle":"giai-thich-de-hieu"}'
```

⚑ Bỏ cờ đó ra là phép thử **mất giá trị**: nếu máy bạn đang có một hook lúc-mở-phiên bơm giọng tương
tự, cả hai lần chạy sẽ giống nhau và bạn sẽ tưởng file này vô dụng. Lần đo đầu tiên khi làm repo này
đã dính đúng lỗi đó.

### Ô Insight vẫn chạy từ một file, không cần plugin

Bản trước của bộ này nằm ở **hai chỗ**: file style, cộng một thư mục plugin riêng trong
`~/.claude/skills/` chứa hook bơm phần ô Insight. Bản này gộp cả hai vào một file, nên câu hỏi hợp lý
là: gộp xong thì ô Insight còn hoạt động không?

Đã đo: đặt **một file này** vào một thư mục trống, cắt hết tầng cấu hình cá nhân, không plugin nào — rồi
giao một việc có bẫy thật (chọn kiểu số để tính tiền). Ô `★ Insight` xuất hiện đúng chỗ, nội dung giải
thích vì sao `0.1` không bao giờ đúng là một hào, bằng lời thường.

Ngược lại, mấy việc máy móc trong cùng đợt đo — tạo một file in ra "Xin chào" — thì **không** ra ô nào.
Đó là điều kiện "không có gì đáng nói thì bỏ trống" chạy đúng, chứ không phải ô bị hỏng.

Chọn câu hỏi cho đúng cũng quan trọng. Một câu quá dễ ("index là gì?") thì hai bên trả lời y hệt, vì
style không có chỗ nào để lộ ra. Cần câu **buộc phải xưng hô và buộc phải khuyên chọn**.

## Ghi công

Hình thức ô `★ Insight` lấy từ style **Explanatory** của Claude Code (Anthropic) và plugin
`explanatory-output-style`. Toàn bộ chữ trong repo này được viết lại, không sao chép.

MIT — xem [LICENSE](LICENSE).
