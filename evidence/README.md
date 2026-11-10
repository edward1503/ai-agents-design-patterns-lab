# Minh chứng hoàn thành lab

## Kết quả cuối

- Python 3.12.16, chỉ dùng thư viện chuẩn.
- Lệnh `test_lab.py all`: 12 tests, OK; exit code 0.
- `../test_results.txt`: stdout/stderr thực tế của lần chạy toàn bộ test.
- `all_final.txt`: cùng kết quả, thêm thời gian, lệnh đầy đủ và exit code.
- `test_pass.jpg`: ảnh chụp nguyên bản từ Notepad đang hiển thị `all_final.txt`.
- Dòng warning `notification failed for task t1` được tạo có chủ đích trong test lỗi thông báo; bộ test vẫn kết thúc OK.

## Quá trình sửa

| Phần | Trước sửa | Sau sửa | File được sửa |
| --- | --- | --- | --- |
| Lab 2 | 1 test đạt, 1 failure, 1 error | 3/3 đạt | `push_channel.py` |
| Lab 3 | 2 test đạt, 1 failure | 3/3 đạt | `service.py` |
| Lab 4 | 3 errors | 3/3 đạt | `audit_channel.py` |

Các file `lab2_before.txt` đến `lab4_after.txt` lưu kết quả thực tế từng bước. Mỗi log chứa thời gian với múi giờ +07:00, lệnh test, stdout/stderr và exit code. `runs.json` liệt kê các lần chạy được ghi lại.

Lab 1 đã hoàn thành ở lượt trước. Các bản chụp nguồn và mã băm baseline trong thư mục này bắt đầu **sau Lab 1**; log tổng cuối xác nhận cả ba test Lab 1 vẫn đạt.

## Kiểm tra thay đổi và thiết kế

- `baseline_hashes.json` và các file `*_hashes.json`: mã băm SHA-256 trước/sau.
- `integrity_review.txt`: kiểm tra file được phép thay đổi, file được bảo toàn và đối chiếu code/test/sơ đồ.
- `implementation_changes.diff`: thay đổi triển khai từ baseline sau Lab 1.
- `*.py.before.txt`: nguồn trước khi bắt đầu Lab 2.
- `../sequence.mmd`: luồng thành công, lỗi lưu không gửi, lỗi thông báo trả task bình thường và lỗi bất ngờ truyền ra.
- `../DECISIONS.md`: 197 từ, có khai báo AI hỗ trợ; giải thích kiến trúc, patterns và đề xuất outbox cho R6.

## Chạy lại trên máy này

Mở PowerShell tại thư mục lab:

```powershell
& 'C:\Users\VINUNI\AppData\Roaming\uv\python\cpython-3.12.16-windows-x86_64-none\python.exe' test_lab.py all
```

Đây là đường dẫn Python thực tế đã dùng vì lệnh `py -3` chưa tìm được runtime mặc định.

## Phạm vi minh chứng

Tests kiểm tra hành vi với các adapter giả lập. Chúng không xác nhận độ bền của database thật, khôi phục sau crash hay gửi tới nhà cung cấp thật. Outbox chỉ được trình bày trong quyết định thiết kế; audit events chỉ lưu trong bộ nhớ.

File ZIP bài nộp chứa sáu file yêu cầu trong README và thư mục minh chứng này. Khi chạy lại từ ZIP, cần đặt các file triển khai cạnh `test_lab.py`, `models.py` và `adapters.py` của bộ starter gốc.
