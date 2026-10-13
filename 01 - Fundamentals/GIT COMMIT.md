# 2 -10 - 2026

> 🧭 **Điều hướng:** [[lộ trình]] • [[HỆ THỐNG NHANH]] | ⬅️ Trước: [[GIT ADD]] | ➡️ Tiếp theo: [[GIT LOG]]

## 1. GIT COMMIT LÀ GÌ?
- lưu staging thành 1 mốc trong lịch sử

```text
Working tree --git add--> Staging area --git commit--> Repository
```

`commit` = chụp snapshot những gì đã trong staging,kèm lời nhắn, tên người làm, thời gian

## 2. TÙY CHỌN

| lệnh                      | ý nghĩa                                              |
| ------------------------- | ---------------------------------------------------- |
| git commit -m "add a.txt" | commit kèm lời nhắn                                  |
| git commit                | mở ediitor để viết lời nhắn                          |
| git commit -am "fix bug"  | tự add file đã tracked rồi commit ( bỏ qua bước add) |
| git commit --amend        | sửa commit gần nhất                                  |

## demo ouput

```bash
mkdir demo3 && cd demo3          # tạo folder và di chuyển vào
git init -b main                 # tạo repo + đổi tên nhánh master -> main
echo "hello" > a.txt             # tạo file
git add a.txt                    # đưa vào staging
git commit -m "add a.txt"        # lưu vào lịch sử

[main (root-commit) 3f2a1b7] add a.txt
 1 file changed, 1 insertion(+)
 create mode 100644 a.txt
```

| Phần output                      | Nghĩa                                                       |
| -------------------------------- | ----------------------------------------------------------- |
| `main`                           | Nhánh đang commit                                           |
| `root-commit`                    | Commit **đầu tiên** của repo (các lần sau không có chữ này) |
| `3f2a1b7`                        | Mã hash rút gọn của commit (máy bạn sẽ khác)                |
| `1 file changed, 1 insertion(+)` | Thống kê: 1 file đổi, thêm 1 dòng                           |
| `create mode 100644`             | File mới, quyền thường                                      |
> commit lần 2

```bash
echo "world" >> a.txt            # sửa file
git commit -am "update a.txt"    # add + commit 1 lệnh (a.txt đã tracked nên được)

[main 8d4e9c2] update a.txt
 1 file changed, 1 insertion(+)
```

### Lần đầu: phải khai báo tên

Nếu git báo lỗi `Please tell me who you are`:

bash

```bash
git config --global user.name "Zyahlor"
git config --global user.email "Zyahlor@email.com"
```

### Viết lời nhắn tốt

|❌|✅|
|---|---|
|`git commit -m "fix"`|`git commit -m "fix: login lỗi khi để trống mật khẩu"`|
|`git commit -m "aaa"`|`git commit -m "add: trang đăng ký"`|

Ngắn gọn, nói **làm gì**, không phải "sửa lung tung".

### ❌ Lưu ý

| ❌                                 | ✅                                                                   |
| --------------------------------- | ------------------------------------------------------------------- |
| `git commit` lưu cả file chưa add | Chỉ lưu những gì **trong staging**                                  |
| `git commit -a` add cả file mới   | `-a` chỉ add file **đã tracked**, file untracked vẫn phải `git add` |
| Commit xong là mất file sửa       | Không, commit chỉ chụp lại, working tree vẫn nguyên                 |
### Quick reference

| Muốn                      | Lệnh                   |
| ------------------------- | ---------------------- |
| Commit nhanh              | `git commit -m "msg"`  |
| Bỏ qua add (file tracked) | `git commit -am "msg"` |
| Sửa commit gần nhất       | `git commit --amend`   |
