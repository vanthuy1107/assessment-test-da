# Data Analyst Assessment — Smartlog (customer: AcmeFoods)

Chào bạn, cảm ơn vì đã ứng tuyển vị trí **Data Analyst** tại **Smartlog**.

Smartlog cung cấp nền tảng **Control Tower** + đội DA hỗ trợ phân tích / build dashboard cho khách hàng logistics. Là DA Smartlog, bạn sẽ **làm việc trên dữ liệu vận hành của khách**, output chính là **dashboard + insight report** cho stakeholder phía khách (thường là Supply Chain Manager). Bài test này mô phỏng đúng tình huống đó — bạn vừa được assign vào project của 1 khách giả định, **AcmeFoods**.

Đây là bài take-home để bọn mình hiểu cách bạn **tiếp cận dữ liệu khách mới**, **đặt câu hỏi đúng**, và **kể câu chuyện cho stakeholder business (không phải dev nội bộ)**. KHÔNG có "đáp án đúng" duy nhất — quan trọng là cách bạn tư duy.

---

## Thông tin chung

- **Thời gian làm bài**: ~3 giờ target, hard-cap 4 giờ (timeboxed — đừng làm quá 4 giờ, bọn mình ưu tiên cách bạn quản lý thời gian hơn là làm hết mọi thứ).
- **Hạn nộp**: 48 giờ sau khi nhận đề.
- **Tool**: tự do chọn — SQL (DuckDB/SQLite/Postgres tuỳ bạn), Python (pandas/polars), R, Excel/Google Sheets, Power BI, Tableau, Metabase, Looker Studio... bất cứ gì bạn quen tay.
- **Ngôn ngữ**: tiếng Việt hoặc tiếng Anh đều được, đồng nhất 1 thứ là OK.

---

## Bối cảnh

**AcmeFoods Vietnam** (khách hàng giả định của Smartlog) là công ty FMCG bánh kẹo, phân phối qua nhiều kênh (siêu thị, tạp hoá, e-com, horeca). Họ **thuê ngoài toàn bộ vận chuyển** qua một nhóm nhỏ nhà vận tải đối tác. Mỗi tháng đội logistics của AcmeFoods có cuộc họp với **Supply Chain Manager (SC Manager)** — đây là stakeholder phía khách mà bạn (DA Smartlog) sẽ build dashboard và gửi insight.

Bạn vừa được assign vào project AcmeFoods và nhận dataset **3 tháng vận hành Feb-Apr 2026 (01/02 → 30/04/2026)** từ hệ thống Smartlog Control Tower. Đọc kĩ `dataset/README.md` trước khi bắt đầu — file đó mô tả 5 CSV (shipments, trips, carriers, locations, products) và quan hệ giữa chúng.

Bọn mình **KHÔNG** đưa sẵn danh sách KPI cần tính, **KHÔNG** đưa câu hỏi cụ thể cần trả lời. Khách hàng thật cũng thường không biết họ muốn dashboard hiển thị cái gì — phần lớn giá trị của 1 DA Smartlog giỏi nằm ở chỗ **biết phải hỏi câu gì** trước khi viết SQL và build widget.

---

## Yêu cầu

Bài chia làm 4 phần. **Bạn không bắt buộc làm hết** — nếu thời gian eo hẹp, ưu tiên Phần 1 và 3 (bắt buộc), Phần 2 và 4 chọn 1.

### Phần 1 (BẮT BUỘC) — Data Profiling

Trước khi phân tích bất cứ gì, hãy **khám phá dữ liệu**. Viết ngắn (~300-500 từ) trả lời:

1. Dataset có bao nhiêu row mỗi file? Date range thực tế là gì?
2. Có **data quality issue** nào bạn phát hiện được? (vd NULL, duplicate, outlier, value lạ, inconsistent...) — list càng nhiều càng tốt.
3. **3-5 quan sát đầu tiên** bạn thấy thú vị / đáng đào sâu. Quan sát không nhất thiết phải là KPI — có thể là pattern, anomaly, phân bố lạ, mối quan hệ giữa cột.
4. Dựa trên những gì bạn thấy, **đề xuất 2-3 câu hỏi business** mà SC Manager có thể quan tâm.

> Tip: chạy `.describe()` / `COUNT(*)` / `GROUP BY` đơn giản trước khi nhảy vào tính KPI phức tạp. Profile là nền tảng — làm cẩn thận bước này thường tiết kiệm thời gian sau.

---

### Phần 2 (CHỌN 1 với Phần 4) — Đào sâu 1 KPI bạn tự định nghĩa

Chọn **1 KPI quan trọng** mà bạn thấy có thể tính từ dataset này. Bạn tự định nghĩa metric, công thức, và lý do tại sao nó quan trọng.

Một số ý tưởng (không exhaustive, không bắt buộc theo): giao đúng giờ, giao đủ hàng, tỉ lệ tận dụng xe, lead time, hiệu suất nhà vận tải, chi phí ngầm (vd over-delivery), ... — hoặc một metric khác bạn nghĩ ra.

Yêu cầu:
1. **Định nghĩa rõ ràng**: tên metric, công thức (text hoặc SQL), tại sao nó quan trọng cho business. Nếu gặp NULL hoặc edge case, bạn xử lý thế nào? Giải thích trade-off.
2. **Tính theo ≥ 2 chiều slice**: tháng, kênh bán, kho, khu vực giao, carrier, vehicle type, cargo group... tuỳ bạn chọn.
3. **Bảng + ít nhất 1 chart** trình bày kết quả.
4. **Nhận xét 3-5 câu**: số nói gì? Có gì bất thường?

---

### Phần 3 (BẮT BUỘC) — Email từ SC Manager

SC Manager gửi bạn 1 email ngắn:

> *"Hi bạn,*
> 
> *Bạn vừa nhận data 3 tháng vận hành. Cho mình hỏi nhanh:*
> 
> *(a) Trong 3 tháng này có **gì đáng lưu ý** không? Pattern gì lạ, anomaly gì cần để ý, hay carrier/region/vehicle nào đang gây vấn đề?*
> 
> *(b) Nếu phải chọn **1 vấn đề ưu tiên xử lý** trong tuần tới, bạn pick cái gì? Tại sao là cái đó chứ không phải cái khác?*
> 
> *(c) Bạn đề xuất action gì? Cụ thể: ai cần làm gì, đo bằng metric nào, mục tiêu trong bao lâu?"*

Trả lời trong ~400-600 từ, kèm 1-2 chart/bảng để support.

**Lưu ý**:
- Bạn tự chọn **cái gì là "đáng lưu ý"** — không có guideline trước. Đây là phần test khả năng đặt câu hỏi.
- Nếu data **không** support kết luận, hãy nói thẳng "không có evidence" thay vì bịa.
- Đề xuất hành động phải **cụ thể**: làm gì, ai làm, đo bằng gì để biết thành công.
- Phân biệt rõ trong câu trả lời: cái nào là **fact** (số nói), cái nào là **hypothesis** (bạn đoán).

---

### Phần 4 (CHỌN 1 với Phần 2) — Đề xuất 1 widget dashboard

> Phần này phản ánh sát công việc thực tế của DA Smartlog — bạn không bắt buộc chọn, nhưng nếu chọn thì đây là cơ hội thể hiện tư duy "build cho khách dùng" (chứ không chỉ phân tích nội bộ).

Nếu được build **1 widget duy nhất** trên dashboard Control Tower của SC Manager AcmeFoods, bạn chọn gì?

1. **Câu hỏi widget trả lời**: 1 câu rõ ràng.
2. **Visualization**: dạng chart gì? (bar / line / heatmap / KPI card / table / ...)
3. **Mockup**: vẽ tay / dùng tool / mô tả bằng chữ — không cần đẹp, cần truyền đạt.
4. **Refresh frequency**: real-time / hourly / daily / weekly?
5. **Edge case**: empty state hiển thị gì? Lỗi load data thì sao?
6. **Lý do chọn**: tại sao widget này thay vì widget khác? (3-5 câu)

---

## Deliverable

Submit 1 file zip hoặc link Google Drive với:

1. **Báo cáo chính** (`report.pdf` hoặc `report.md` hoặc notebook `.ipynb`).
2. **Code/query** bạn dùng (nếu có): file `.sql`, `.py`, `.xlsx`... đặt trong folder `code/`.
3. **Chart export** (nếu chart không nhúng được trong báo cáo).
4. **File `notes.md`** (1 trang) — phần "behind-the-scenes":
   - Bạn dành nhiều thời gian nhất ở phần nào? Tại sao?
   - Có giả định/cách tiếp cận nào bạn cân nhắc rồi bỏ? Ngắn gọn 1-2 câu cho mỗi.
   - Nếu có thêm 2 giờ nữa, bạn sẽ làm gì tiếp?

---

## Tiêu chí chấm

Bài được chấm theo 4 trục (chi tiết bọn mình sẽ chia sẻ sau buổi discuss):

| # | Trục | Trọng số | Bạn được đánh giá dựa trên |
|---|---|---|---|
| 1 | **Curiosity & Framing** | 30% | Bạn có **đặt được câu hỏi đúng** trước khi tính không? Profile có sâu không? Phân biệt fact/hypothesis có rõ không? |
| 2 | **Execution** | 25% | Cách bạn viết query / pandas / Excel — đúng, hiệu quả, repro được? Định nghĩa metric có defensible không? |
| 3 | **Insight quality** | 25% | Bạn quan sát được pattern gì? Decompose root cause không? Recommendation có actionable không? |
| 4 | **Communication** | 20% | Stakeholder business (KHÔNG biết code) đọc có hiểu không? Chart có rõ không? |

---

## Một vài lưu ý

- **Không cần làm "hoàn hảo"** — bọn mình quan tâm **cách bạn nghĩ** hơn là số lượng output. 1 phân tích sâu thường giá trị hơn 5 phân tích nông.
- **Không có KPI nào là "đáp án đúng"** trong bài này. Bạn chọn OTIF, On-time, In-full, VFR, Lead time, Carrier perf, hay metric nào khác — đều OK miễn là bạn defend được lựa chọn.
- **Đừng AI-wash**: nếu bạn dùng AI hỗ trợ, OK — nhưng phải hiểu output. Trong buổi discuss sẽ có câu hỏi đào sâu, không hiểu sẽ lộ ngay.
- **Note lại assumption**: nếu có chỗ trong đề bạn thấy chưa rõ, **đừng hỏi lại** — hãy ghi giả định của bạn vào báo cáo (vd "tôi assume X vì Y") rồi tiếp tục làm. Khả năng tự nêu giả định và defense được nó là 1 phần bài test.

Chúc bạn làm bài vui.

— Hiring Team, Smartlog
