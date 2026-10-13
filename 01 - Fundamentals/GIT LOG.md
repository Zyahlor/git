# 2 - 10 - 2026

> 🧭 **Điều hướng:** [[lộ trình]] • [[HỆ THỐNG NHANH]] | ⬅️ Trước: [[GIT COMMIT]] | ➡️ Tiếp theo: [[GIT DIFF]]

## 1. GIT LOG LÀ GÌ?
- liệt kê các commit từ mới -> cũ nhất

## 2. tùy chọn

| Lệnh             | Tác dụng                                              |
| ---------------- | ----------------------------------------------------- |
| `--oneline`      | Hash rút gọn + lời nhắn, 1 dòng/commit                |
| `-n <số>`        | Giới hạn số commit hiện                               |
| `--stat`         | Thêm thống kê số dòng thay đổi mỗi file               |
| `-p`             | Thêm diff (xem chính xác dòng nào thay đổi ) chi tiết |
| `--graph`        | Vẽ đồ thị nhánh                                       |
| `--author="Bao"` | Lọc theo người commit                                 |
## demo output
- dùng lại repo của [[GIT COMMIT]]

bash

```bash
git log                   # xem đầy đủ
```

```
commit 8d4e9c2a1f3b7e6d5c4b3a29187f6e5d4c3b2a10 (HEAD -> main)
Author: Zyahlor <Zyahlor@email.com>
Date:   Thu Oct 1 23:40:12 2026 +0700

    update a.txt

commit 3f2a1b7c9d8e6f5a4b3c2d1e0f9a8b7c6d5e4f3a
Author: Zyahlor <Zyahlor@email.com>
Date:   Thu Oct 1 23:35:48 2026 +0700

    add a.txt
```

| Phần output         | Nghĩa                                           |
| ------------------- | ----------------------------------------------- |
| `commit 8d4e9c2...` | Mã hash đầy đủ (40 ký tự) của commit            |
| `(HEAD -> main)`    | Bạn đang đứng ở commit này, trên nhánh `main`   |
| `Author`            | Người commit (lấy từ `user.name`, `user.email`) |
| `Date`              | Thời gian commit                                |
| Dòng thụt vào       | Lời nhắn `-m`                                   |
> --oneline

```bash
git log --oneline

8d4e9c2 (HEAD -> main) update a.txt
3f2a1b7 add a.txt
```

> --stat & -n

```bash
git log --stat -n 1 # thêm thống kê số dòng thay đổi mỗi file + giới hạn số commit hiện
# --stat
commit 8d4e9c2a1f3b7e6d5c4b3a29187f6e5d4c3b2a10 (HEAD -> main)
Author: Bao <ban@email.com>
Date:   Thu Oct 1 23:40:12 2026 +0700

    update a.txt
# -n 1
 a.txt | 1 +
 1 file changed, 1 insertion(+)
```

### Thoát khi log dài

Log dài sẽ mở trình xem (pager). Bấm **`q`** để thoát, **mũi tên / space** để cuộn.

### ❌ Lưu ý

| ❌                                       | ✅                                                                                   |
| --------------------------------------- | ----------------------------------------------------------------------------------- |
| `git log` xem được thay đổi chưa commit | Chỉ hiện commit **đã lưu**. Thay đổi chưa commit xem bằng `git status` / `git diff` |
| Phải gõ hash đầy đủ 40 ký tự            | Chỉ cần 7 ký tự đầu (như `8d4e9c2`)                                                 |
| Bị kẹt không thoát được                 | Bấm `q`                                                                             |
### Quick reference

| Muốn                  | Lệnh                              |
| --------------------- | --------------------------------- |
| Xem gọn               | `git log --oneline`               |
| Xem 3 commit gần nhất | `git log -n 3`                    |
| Xem file nào đổi      | `git log --stat`                  |
| Xem diff từng commit  | `git log -p`                      |
| Xem cây nhánh         | `git log --graph --oneline --all` |