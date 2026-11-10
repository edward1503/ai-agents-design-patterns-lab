# Tổng kết bài thực hành

Đã hoàn thành cả bốn phần của **Design, Architecture and Patterns with AI Agents**. Bộ test được cung cấp đạt **12/12 tests, OK, exit code 0** trên Python 3.12.16 ngày 10/11/2026.

## Nội dung đã làm

| Phần | Thay đổi | Kết quả kiểm tra |
| --- | --- | --- |
| Lab 1 — SRP, DIP | Trong `service.py`, lưu assignment trước khi gửi thông báo. `SaveError` được truyền ra; lưu thất bại thì không thử gửi. | Ba test Lab 1 đạt trong lần chạy tổng. |
| Lab 2 — Strategy, OCP | `PushChannel.send` ghi từng tuple `(recipient, message)` vào `self.sent`. Kênh mới được đăng ký qua interface có sẵn. | 3/3 test đạt; bước này chỉ sửa `push_channel.py`. |
| Lab 3 — Ranh giới lỗi | Chỉ bắt `NotificationError`; warning chứa task ID, không chứa thông tin liên hệ. Giữ assignment đã lưu, trả task bình thường, gửi một lần. Lỗi bất ngờ như `RuntimeError` vẫn truyền ra. | 3/3 test đạt; bước này chỉ sửa `service.py`. |
| Lab 4 — Decorator | `AuditedChannel` gọi kênh bên trong một lần, rồi ghi `notification.accepted` sau thành công. Khi gửi lỗi, truyền lỗi ra và không ghi sự kiện. | 3/3 test đạt; bước này chỉ sửa `audit_channel.py`. |

Các interface, giá trị domain, adapters được cung cấp và `test_lab.py` được bảo toàn. Không thêm thư viện phụ thuộc, network call hay database code vào triển khai lab.

## Thiết kế

- [sequence.mmd](sequence.mmd): thứ tự gọi khớp code; có nhánh thành công, `SaveError` không gửi, `NotificationError` trả task bình thường và lỗi bất ngờ truyền ra. `save` thành công được chú thích là mô hình của một commit đã hoàn tất.
- [DECISIONS.md](DECISIONS.md): 197 từ, so sánh modular monolith với microservices, phân biệt Strategy/Decorator/Observer, khai báo AI hỗ trợ và trình bày phương án outbox cho R6.
- Phương án R6 ghi assignment và notification intent trong cùng transaction, dùng relay gửi lại các intent còn chờ; nêu rủi ro gửi trùng nếu relay crash sau khi provider chấp nhận nhưng trước khi đánh dấu hoàn tất.

## Minh chứng

- [Kết quả 12 test](test_results.txt).
- [Log tổng có thời gian, lệnh và exit code](evidence/all_final.txt).
- [Ảnh chụp nguyên bản từ Notepad](evidence/test_pass.jpg).
- [Hướng dẫn đọc minh chứng và kết quả trước/sau](evidence/README.md).
- [Kiểm tra SHA-256 và đối chiếu thiết kế](evidence/integrity_review.txt).
- [Diff triển khai từ baseline sau Lab 1](evidence/implementation_changes.diff).
- [ZIP bài nộp: sáu file yêu cầu và minh chứng](lab_submission.zip).

Việc ghi log theo từng bước bắt đầu sau khi Lab 1 đã hoàn thành. Lần chạy tổng cuối xác nhận cả bốn nhóm test đều đạt. Warning `notification failed for task t1` là đầu ra dự kiến khi test giả lập lỗi thông báo.

## Chạy lại

Tại thư mục repo, dùng Python 3.11 trở lên; không cần cài package hay cấu hình API key:

```sh
python test_lab.py all
```

Trên Windows, có thể dùng `py -3 test_lab.py all` nếu launcher đã nhận Python. Đường dẫn Python thực tế dùng cho lần kiểm tra này được lưu trong [log tổng](evidence/all_final.txt).

## Giới hạn

Kết quả xác nhận hành vi với adapter giả lập. Tests không chứng minh chất lượng kiến trúc, độ bền database thật, khôi phục sau crash hay việc provider thật gửi thông báo. Outbox chỉ là đề xuất thiết kế; audit events hiện lưu trong bộ nhớ.

[README gốc của bài thực hành](README.md) được giữ nguyên để đối chiếu yêu cầu.
