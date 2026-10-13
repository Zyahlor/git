# 2 - 10 -2026

> 🧭 **Điều hướng:** [[lộ trình]] • [[HỆ THỐNG NHANH]] | ⬅️ Trước: [[GIT STATUS]] | ➡️ Tiếp theo: [[GIT COMMIT]]

## 1. GIT ADD
- đưa thay đổi vào staging - chọn những thay đổi nào sẽ vào commit tiếp theo

```text
working tree -> git add -> staging area -> git commit -> repository
(file ban sua)           (danh sach cho)                 (lich su commit)
```

## 2. TÙY CHỌN
| lệnh                          | ý nghĩa                                    |
| ----------------------------- | ------------------------------------------ |
| git add name_file             | add 1 file                                 |
| git add name_file1 name_file2 | add nhiều file                             |
| git add src/                  | add cả folder                              |
| git add .                     | add tất cả thay đôi từ folder hiện tại     |
| git add -p                    | add chọn từng đoạn thay đổi (patch) để add |
| git  add -A                   | add tất cả thay đổi trong toàn repo        |
| git add -u                    | add file đã tracked, bỏ qua file mới       |

### demo output

```bash
mkdir demo2 && cd demo2       # tạo thư mục và di chuyển vào
git init -b main              # tạo repo + tên nhấnh
echo "hello" > a.txt          # tạo file a.txt
echo "world" > b.txt          # tạo file b.txt
git status -s                 # xem trạng thái dạng ngắn

?? a.txt
?? b.txt
```

```bash
git add . # add hết phần còn lại
git status -s

A a.txt # A = đã staged
A b.txt
```

> sửa file đã add,phải add lại
```bash
git commit -m "init" # commit để có mốc 
echo "more" >> a.txt # sửa a.txt 
git add a.txt # đưa bản sửa vào staging 
git status -s

M a.txt
```
> hủy add

```bash
git restore --staged a.txt # bỏ a.txt khỏi staging , nội dung sửa vẫn còn đó
git status -s

M a.txt
```

### ❌ Lưu ý

|❌|✅|
|---|---|
|`git add` là lưu vào lịch sử|Chỉ vào **staging**, `git commit` mới lưu|
|`git add .` luôn add toàn repo|Chỉ từ **thư mục hiện tại** trở xuống|
|Add xong sửa tiếp thì bản sửa cũng được commit|Staging là **snapshot lúc add**, sửa thêm phải add lại (như `MM` bạn đã gặp)|
|`git add .` an toàn tuyệt đối|Có thể add nhầm file nhạy cảm (`.env`, key). Luôn `git status` kiểm tra trước khi commit|

### Quick reference

| Muốn                | Lệnh                        |
| ------------------- | --------------------------- |
| Add 1 file          | `git add file`              |
| Add tất cả          | `git add -A`                |
| Chỉ file đã tracked | `git add -u`                |
| Chọn từng đoạn      | `git add -p`                |
| Hủy add             | `git restore --staged file` |

