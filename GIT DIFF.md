# 2 - 10 - 2026

> 🧭 **Điều hướng:** [[lộ trình]] • [[HỆ THỐNG NHANH]] | ⬅️ Trước: [[GIT LOG]] | ➡️ Tiếp theo: [[gitignore]]

## 1. GIT DIFF LÀ GÌ?
 - `git status` cho biết file nào đổi  thì `git diff` cho biết đổi gì bên trong bao gồm dòng nào, thêm ,sửa gì
### 3 kiểu so sánh hay dùng

```text
Working tree  <--git diff-->  Staging  <--git diff --staged-->  Commit cuối (HEAD)
```

## 2. TÙY CHỌN

| lệnh                       | so sánh                                        |
| -------------------------- | ---------------------------------------------- |
| git diff                   | working tree  vs staging ( thay đổi chưa add ) |
| git diff --staged          | staging vs commit cuối ( thay đổi sắp commit)  |
| git diff HEAD              | working tree vs commit cuối ( cả hai gộp lại)  |
| git diff `<hash1> <hash2>` | so sánh 2 commit bất kỳ                        |
## demo output

| Phần output                   | Nghĩa                                                 |
| ----------------------------- | ----------------------------------------------------- |
| `a/a.txt` → `b/a.txt`         | `a/` = bản cũ, `b/` = bản mới                         |
| `--- a/a.txt` / `+++ b/a.txt` | Dấu `-` = bản cũ, `+` = bản mới                       |
| `@@ -1,2 +1,3 @@`             | Bản cũ: từ dòng 1, 2 dòng. Bản mới: từ dòng 1, 3 dòng |
| Dòng bắt đầu bằng `+`         | Dòng **thêm mới** (màu xanh)                          |
| Dòng bắt đầu bằng `-`         | Dòng **bị xóa** (màu đỏ)                              |
| Dòng không có dấu             | Dòng giữ nguyên (để làm ngữ cảnh)                     |

```bash
cd demo3
echo "git" >> a.txt        # sửa a.txt (thêm dòng "git")
git diff                   # xem thay đổi chưa add
```

```text
diff --git a/a.txt b/a.txt
index 8b1a2c3..4d5e6f7 100644
--- a/a.txt
+++ b/a.txt
@@ -1,2 +1,3 @@
 hello
 world
+git
```

> add rồi thì git diff trống

```bash
git add a.txt
git diff            # không hiện gì (đã vào staging rồi )
git diff --staged   # hiện thay đổi đang chờ commit
```

```text
diff --git a/a.txt b/a.txt
index 8b1a2c3..4d5e6f7 100644
--- a/a.txt
+++ b/a.txt
@@ -1,2 +1,3 @@
 hello
 world
+git
```

> sửa dòng (xóa+thêm)

```text
đôi từ hello -> hihi

-hello
+hi
world
```

> git không có sửa - chỉ có xóa cũ thêm mới

### option hay dùng

```bash
git diff --stat  # chỉ thống kê,không hiện nội dung
git diff a.txt   # chỉ xem 1 file
git diff --name-only  # chỉ liệt kê tên file đổi
```

### ❌ Lưu ý

| ❌                              | ✅                                                                        |
| ------------------------------ | ------------------------------------------------------------------------ |
| `git diff` hiện hết thay đổi   | Chỉ hiện thay đổi **chưa staged**. Đã add rồi phải dùng `--staged`       |
| `git diff` thấy file untracked | **Không**, file mới chưa add thì diff không hiện (xem bằng `git status`) |
| Bị kẹt khi diff dài            | Bấm `q` để thoát                                                         |
### Quick reference

| Muốn xem                  | Lệnh                |
| ------------------------- | ------------------- |
| Chưa add                  | `git diff`          |
| Đã add, chưa commit       | `git diff --staged` |
| Tất cả so với commit cuối | `git diff HEAD`     |
| Chỉ thống kê              | `git diff --stat`   |