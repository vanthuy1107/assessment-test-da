# Data Analyst Assessment — Smartlog (customer: AcmeFoods)

Repo này chứa **bài test Data Analyst (1-2 năm kinh nghiệm)** cho vị trí **DA tại Smartlog** + **dataset mẫu** để bạn làm tại nhà.

> **Về vai trò**: Smartlog cung cấp nền tảng **Control Tower** + đội ngũ DA hỗ trợ phân tích / build dashboard cho khách hàng logistics. DA Smartlog làm việc trên dữ liệu vận hành của khách, output chính là **dashboard + insight report** cho stakeholder phía khách (vd Supply Chain Manager).

> **Về dataset**: Đây là **dữ liệu hư cấu** mô phỏng hoạt động vận chuyển FMCG của 1 khách hàng giả định ("AcmeFoods"). Tên công ty, nhà vận tải, kho, brand đều là tên giả; số liệu giữ phân phối thực tế để bài phân tích có ý nghĩa.

---

## Bắt đầu từ đâu

1. Đọc **[`test/assessment.md`](test/assessment.md)** — đề bài, yêu cầu, deliverable.
2. Đọc **[`dataset/README.md`](dataset/README.md)** — schema 5 file CSV, quan hệ join, KPI cần biết (OTIF, On-Time, In-Full, VFR).
3. Profile dataset bằng tool bạn quen (SQL / Python / Excel / BI tool — tự do chọn).
4. Làm bài theo hướng dẫn trong `assessment.md`.

---

## Cấu trúc repo

```
.
├── README.md                  # File này
├── dataset/
│   ├── README.md              # Schema + business context — ĐỌC TRƯỚC
│   ├── shipments.csv          # Fact: đơn hàng giao (OTIF)
│   ├── trips.csv              # Fact: chuyến xe (VFR)
│   ├── carriers.csv           # Dim: nhà vận tải
│   ├── locations.csv          # Dim: kho + khu vực giao
│   └── products.csv           # Dim: brand + cargo group
└── test/
    └── assessment.md          # Đề bài
```

---

## Thời gian & nộp bài

- **Timebox**: ~3 giờ target, hard-cap 4 giờ (đừng làm quá 4 giờ — bọn mình ưu tiên cách bạn quản lý thời gian hơn là làm hết).
- **Hạn**: 48 giờ sau khi nhận đề.
- **Submit**: gửi link Google Drive (hoặc file zip qua email) chứa file phân tích (notebook / Excel / PDF / BI export) + ít nhất 1 chart + narrative ngắn. Gửi về email recruiter với subject `[DA Assessment] <Tên ứng viên>`. Chi tiết deliverable xem `test/assessment.md`.

---

## Câu hỏi

Nếu có gì chưa rõ về dataset hoặc business context, cứ ghi vào phần "Assumptions" trong bài nộp — bọn mình muốn xem cách bạn xử lý ambiguity hơn là việc bạn đoán đúng 100%.

Chúc bạn làm bài vui :)
