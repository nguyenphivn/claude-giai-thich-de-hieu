# claude-giai-thich-de-hieu

Một file để Claude Code nói chuyện với mình bằng tiếng người.

## Chuyện là thế này

Mình không phải lập trình viên. Mình có ý tưởng, mình để Claude viết code — cái mà bây giờ người ta gọi
là vibecode.

Rồi vấn đề đến: Claude làm xong, báo lại, mà mình đọc không hiểu gì.

Nó viết đại khái kiểu này:

> Đã refactor `RepairOrderService` để inject `PricingStrategy` qua constructor, loại bỏ coupling với
> concrete implementation. Bổ sung guard `assertSameBranch` trước mutation. AF-0012 resolved.
>
> | Trục | Trước | Sau | Delta | Rủi ro còn lại |
> | ---- | ----- | --- | ----- | -------------- |
> | D1   | ...   | ... | ...   | ...            |

Nó không sai một chữ nào. Nó đang nói chuyện với một đồng nghiệp lập trình. Chỉ có điều mình không phải
đồng nghiệp đó.

Mình thử nhắc: "giải thích dễ hiểu hơn đi". Được đúng một lượt. Lượt sau nó quay về như cũ. Hôm sau mở
máy lên lại phải nhắc lại từ đầu.

Cái mất mát thật không phải là đọc mệt. Là mình **không biết nó vừa làm gì cho sản phẩm của mình**, nên
không biết cái nào nên đồng ý và cái nào nên chặn lại.

## Nên mình làm cái này

Claude Code có một thứ gọi là **output style** — một file văn bản mà nó đọc mỗi lần mở phiên, để biết
nên nói chuyện theo kiểu nào. Nhắc bằng miệng thì trôi; ghi vào file thì nó nhớ mãi.

Mình viết một file như vậy. Nội dung, nói gọn:

- Kể **kết quả trước**, giải thích sau. Đừng bắt mình đọc hết mới biết chuyện gì xảy ra.
- Từ chuyên ngành nào buộc phải dùng thì **dịch ngay trong cùng câu**.
- Cấm mấy thứ làm mình choáng: nhãn `AF-0012`, bảng năm cột, "định tuyến", "bề mặt", "over-scoped".
- Nhưng **vẫn phải giải thích vì sao**. Đây là chỗ mình cẩn thận nhất — mình muốn dễ hiểu, không muốn bị
  rút gọn thành "đã xong nhé".
- Khi cần mình quyết: nói có mấy cách, mỗi cách được gì mất gì, **nghiêng về cách nào**, rồi hỏi đúng
  một câu. Việc nào xoá dữ liệu hay mất tiền thì nói thẳng ra trước.
- Chưa kiểm chứng được thì nói là chưa kiểm chứng được. Đừng đoán rồi nói như thật.

Kết quả là Claude trả lời kiểu này:

> **PostgreSQL.** Với tiệm sửa điện thoại thì chọn nó, không cần phân vân lâu.
>
> **Tiền phải chính xác.** MySQL ở cấu hình mặc định có thể âm thầm làm tròn hoặc cắt bớt số sai kiểu —
> nhập số tiền quá dài thì nó tự cắt rồi vẫn báo "lưu xong". PostgreSQL báo lỗi ngay. Với sổ tiền công,
> tiền linh kiện, thà bị chặn còn hơn sai ngầm.

Cùng một câu hỏi, cùng một cái máy. Chỉ khác một file.

## Cài

Hai bước. Không có bước ba.

**Bước 1 — tải file về:**

```bash
mkdir -p ~/.claude/output-styles && curl -fsSL -o ~/.claude/output-styles/giai-thich-de-hieu.md \
  https://raw.githubusercontent.com/nguyenphivn/claude-giai-thich-de-hieu/main/giai-thich-de-hieu.md
```

**Bước 2 — bật lên.** Trong Claude Code chạy `/config`, chọn **Output style** → `giai-thich-de-hieu`.

Có hiệu lực từ `/clear` hoặc lần mở tiếp theo, vì file này chỉ được đọc một lần lúc mở phiên. Đừng thấy
nó nói y như cũ mà tưởng hỏng.

Muốn khỏi vào menu thì thêm dòng này vào `~/.claude/settings.json`:

```json
{ "outputStyle": "giai-thich-de-hieu" }
```

⚑ **Giữ nguyên tên file.** Tên file chính là tên style. Đổi tên là đổi luôn thứ phải chọn trong
`/config`.

## Hai dòng bạn nên sửa cho mình

Mở file vừa tải, ngay đầu có mục **"Người đọc — sửa hai dòng này cho mình"**.

**Dòng thứ nhất: bạn là ai.** Mình để mặc định là "chủ tiệm kiêm quản lý sản phẩm" vì đó là mình. Bạn
đổi thành vai của bạn — "người tự dựng sản phẩm bằng AI, hiểu ý tưởng nhưng không viết code", "nhà thiết
kế đang tự làm app", "người làm marketing tự sửa website".

Càng cụ thể càng tốt. Đây là dòng điều khiển cả cái giọng, và mình đã thử: viết chung chung kiểu "người
không biết code" thì style yếu đi rõ rệt, vì Claude không hình dung được đang nói với ai.

**Dòng thứ hai: xưng hô.** Mặc định gọi bạn là "anh", Claude tự xưng "em". Đổi thành "chị/em",
"bạn/mình", hay bất cứ cặp nào bạn thấy thuận.

Xong. Hai dòng đó thôi, phần còn lại dùng nguyên được.

## Nó có tác dụng thật không

Mình không muốn tự khen, nên mình đo.

Cùng một câu hỏi — _"Tôi nên dùng PostgreSQL hay MySQL cho tiệm sửa điện thoại của tôi?"_ — chạy hai
lần. Cùng máy, cùng điều kiện. Khác duy nhất: một lần có file, một lần không.

|                  | Không có file                                                                            | Có file                                                                                    |
| :--------------- | :--------------------------------------------------------------------------------------- | :----------------------------------------------------------------------------------------- |
| Gọi mình là      | "bạn"                                                                                    | "anh", tự xưng "em"                                                                        |
| Thuật ngữ        | `NUMERIC`, "silent truncation", `CHECK constraint`, `EXCLUDE`, `JSONB`, "Prisma/Drizzle" | không một mã nào; `JSONB` được dịch thành "một ô dạng ghi chú có cấu trúc mà vẫn tìm được" |
| Ví dụ nó lấy     | "lịch hẹn, tồn kho linh kiện"                                                            | "iPhone thì IMEI, laptop thì số serial, máy cũ thì ghi chú tình trạng vỏ"                  |
| Kết thúc thế nào | thêm một đề xuất thứ ba không ai hỏi                                                     | **một** câu hỏi rõ ràng để mình chốt                                                       |

Bạn tự kiểm lại được. Đặt file vào `.claude/output-styles/` của một thư mục trống rồi chạy hai lệnh:

```bash
claude -p "<câu hỏi của bạn>" --setting-sources project,local
claude -p "<câu hỏi của bạn>" --setting-sources project,local --settings '{"outputStyle":"giai-thich-de-hieu"}'
```

⚑ **Đừng bỏ cái cờ `--setting-sources project,local`.** Nó cắt tầng cấu hình cá nhân của bạn ra khỏi
phép thử. Nếu máy bạn đang có thứ gì khác cũng nhắc Claude nói dễ hiểu, hai lần chạy sẽ giống nhau và
bạn kết luận file này vô dụng. Lần đo đầu tiên khi làm repo này mình dính đúng cái bẫy đó.

Chọn câu hỏi cũng phải khéo. Câu quá dễ như "index là gì" thì hai bên trả lời y hệt, vì style không có
chỗ nào để lộ ra. Cần câu **buộc phải xưng hô và buộc phải khuyên chọn**.

## Cái ô `★ Insight`

Đôi khi Claude sẽ chèn một ô như thế này:

```
★ Insight ─────────────────────────────────────
Máy tính lưu số lẻ theo hệ nhị phân, nên 0.1 không bao giờ đúng là một hào.
Cộng vài trăm dòng hoá đơn thì mấy chút xíu đó dồn lại thành lệch một xu.
─────────────────────────────────────────────────
```

Hình thức ô này mình lấy từ style **Explanatory** có sẵn của Claude Code. Nhưng bản gốc dặn _"lúc nào
cũng phải có"_, nên nó đẻ ra rất nhiều ô viết cho có, nói mấy điều ai học nghề cũng biết.

Mình đổi thành **có điều kiện**: chỉ chèn khi có một đánh đổi thật, một điều bất ngờ, hoặc một con số đo
được lật ngược điều tưởng đúng. Kèm một câu quan trọng — **không có gì đáng nói thì bỏ trống**.

Mình đã đo cái này. Giao một việc có bẫy thật (chọn kiểu số để tính tiền) thì ô xuất hiện, giải thích
đúng chỗ đáng giải thích. Giao một việc máy móc (tạo file in ra "Xin chào") thì **không** ra ô nào. Đó
là điều kiện chạy đúng, không phải ô bị hỏng.

## Mấy chỗ mình đã dập đầu

Đọc trước khi tự sửa file, đỡ mất buổi chiều như mình.

**Đừng thêm dòng `name:` vào đầu file.** Claude Code so trường `name` với **tên file**, phân biệt chữ
hoa chữ thường. Lệch một chữ là nó **âm thầm bỏ hết** nội dung bên dưới — mà style vẫn hiện trong menu
`/config` và vẫn hiện trên thanh trạng thái, nên trông y như đang chạy. Mình mất khá lâu mới hiểu vì sao
"đã bật rồi mà không thấy gì".

File này cố ý không khai `name:` — không khai thì không có gì để lệch. Lỗi vẫn còn mở:
[anthropics/claude-code#47482](https://github.com/anthropics/claude-code/issues/47482).

**Subagent không theo style.** Khi Claude sai một trợ lý con đi làm việc riêng, trợ lý đó chạy bằng chỉ
dẫn của nó, không đọc file này. Nên báo cáo nó gửi về vẫn có thể đầy thuật ngữ tiếng Anh. Không phải
style hỏng.

**Ghi chú dài trong file bị tính tiền mỗi phiên.** Mình tưởng comment `<!-- -->` trong file này là ghi
cho người đọc, Claude sẽ bỏ qua. Không phải: nó vào thẳng chỉ dẫn của Claude, nguyên văn, ở mọi phiên.
Bản trước có một khối comment 6 dòng và nó xuất hiện đủ cả 6 dòng. Nên mọi lời dặn dài nằm ở README
này, còn file kia chỉ giữ đúng phần Claude cần đọc.

## Vài chỗ mình cố ý làm như vậy

**Giữ nguyên phần lập trình của Claude.** File có dòng `keep-coding-instructions: true`. Nó chỉ đổi
**cách nói**, không đổi cách làm việc — Claude vẫn khoanh phạm vi sửa, vẫn tự kiểm chứng, vẫn cẩn thận
như trước. Bỏ dòng đó đi là mất hết mấy thứ ấy, chỉ còn cái giọng.

**Không đóng thành plugin.** Bọc nó thành plugin thì thêm hai file thủ tục và thêm một bước cài, mà vẫn
phải vào `/config` chọn. Tệ hơn: giá trị của nó **là** đoạn chữ, mà đoạn chữ là thứ bạn phải sửa cho
mình. Có cơ chế tự cập nhật thì mỗi lần cập nhật là một lần ghi đè đúng phần bạn vừa sửa.

**Dùng cho ngôn ngữ khác được.** Dịch file, lưu tên khác, tên file mới thành tên style mới. Cái xương —
kết quả trước, danh sách cấm, khối bốn bước khi cần bạn quyết, mục trung thực — không phụ thuộc tiếng
Việt.

## Ghi công

Hình thức ô `★ Insight` lấy từ style **Explanatory** của Claude Code (Anthropic) và plugin
`explanatory-output-style`. Chữ trong repo này mình viết lại hết, không sao chép.

MIT — xem [LICENSE](LICENSE).
