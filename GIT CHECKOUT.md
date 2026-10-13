# 3 - 10 - 2026

> 🧭 **Điều hướng:** [[lộ trình]] • [[HỆ THỐNG NHANH]] | ⬅️ Trước: [[GIT BRANCH]] | ➡️ Tiếp theo: [[GIT STASH]]

## 1. GIT CHECKOUT LÀ GÌ?
- là lệnh cũ, hiện nay đã được đổi mới gần hết

|Việc|Lệnh cũ|Lệnh mới (nên dùng)|
|---|---|---|
|Chuyển nhánh|`git checkout main`|`git switch main`|
|Tạo + chuyển nhánh|`git checkout -b feature`|`git switch -c feature`|
|Hủy sửa file (lấy lại từ commit/staging)|`git checkout -- a.txt`|`git restore a.txt`|
|Lấy bản của 1 bên khi conflict|`git checkout --theirs a.txt`|(chưa có lệnh mới thay thế, vẫn dùng `checkout`)|
|Xem lại commit cũ|`git checkout 3f2a1b7`|`git switch --detach 3f2a1b7`|
- checkout chỉ cần biết để đọc tài liệu cũ và dùng `--ours/--theirs`
## demo output

```bash
echo "oops" >> greet.c # sua nham
git status -s

#output
M greet.c

git restore greet.c  # bo het thay doi, tro ve commit gan nhat
git status -s # sach

# tuong duong cach cu: git checkout -- greet.c
```

> xem commit cũ (detached HEAD)

```bash
git checkout [ma_hash] # vi du 3777737f

#output
Note: switching to '377737f'.

You are in 'detached HEAD' state...
```
**Detached HEAD =  HEAD** không trỏ vào nhánh nào, chỉ trỏ thẳng vào 1 commit, nếu commit ở trạng thái này, dễ bị mất,git log ko thấy

Vẫn cứu được bằng `git reflog` +  `git switch -c <tên> <hash> ` trong một thời gian (mặc định khoảng 30 ngày với commit không nhánh nào trỏ tới). Chỉ là **không hiện trong `git log`** nên dễ quên

### ❌ Lưu ý

|❌|✅|
|---|---|
|`git checkout -- file` an toàn|**Mất vĩnh viễn** thay đổi chưa commit của file đó. `git restore file` cũng vậy, cẩn thận|
|Detached HEAD là lỗi|Chỉ là chế độ xem. Muốn giữ việc làm ở đó thì `git switch -c <tên-nhánh>`|
|Cần học `checkout` kỹ như `switch`|Ưu tiên `switch` + `restore`, `checkout` biết để đọc tài liệu cũ|

### Quick reference

| Muốn                       | Dùng                                  |
| -------------------------- | ------------------------------------- |
| Chuyển nhánh               | `git switch <nhánh>`                  |
| Hủy sửa file               | `git restore <file>`                  |
| Bỏ khỏi staging            | `git restore --staged <file>`         |
| Lấy bản 1 bên khi conflict | `git checkout --ours/--theirs <file>` |
| Xem commit cũ              | `git switch --detach <hash>`          |