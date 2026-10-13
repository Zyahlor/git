# 3 - 10 - 2026

> 🧭 **Điều hướng:** [[lộ trình]] • [[HỆ THỐNG NHANH]] | ⬅️ Trước: [[GIT FILE OPERATIONS]] | ➡️ Tiếp theo: [[RESTORE]]

## 1. `.gitignore` là gì?
- bỏ qua các tên file trong  nội dung của `.gitignore` 
- là 1 file text nằm trong repo, liệt kê file/folder git không theo dõi, không hiện ở git status, không bị git add đưa vào
### Dùng để làm gì

| Loại file                    | Ví dụ                                    |
| ---------------------------- | ---------------------------------------- |
| File nhạy cảm                | `.env`, `*.key`, `secrets.json`          |
| File sinh ra tự động         | `*.log`, `node_modules/`, `__pycache__/` |
| File của hệ điều hành/editor | `.DS_Store`, `.vscode/`                  |
## 2. Tùy chọn

| Mẫu     | Ý nghĩa                 |
| ------- | ----------------------- |
| `*`     | Khớp bất kỳ chuỗi nào   |
| `?`     | Khớp đúng 1 ký tự       |
| `dir/`  | Chỉ khớp thư mục        |
| `/name` | Chỉ khớp ở thư mục gốc  |
| `!name` | Ngoại lệ (không ignore) |
| `**/`   | Khớp thư mục ở mọi cấp  |
```bash
*.log            # mọi file đuôi .log
debug.txt        # đúng file tên debug.txt
node_modules/    # cả thư mục (dấu / cuối)
/build           # chỉ thư mục build ở gốc repo
!important.log   # ! = ngoại lệ, KHÔNG ignore file này
# dòng này là comment
```

## demo output

```bash
cd demo3
echo "*.log" > .gitignore     # ignore mọi file .log
touch debug.log app.txt       # tạo 1 file log, 1 file thường
git status -s
```

```text
?? .gitignore
?? app.txt
```
> debug.log không hiện vì đã vì có tên trong file `.gitignore` muốn thấy sử dụng git status -s --ignored

```bash
git status -s --ignored
```

```text
?? .gitignore
?? app.txt
!! debug.log
```
> Nhớ `git add .gitignore` và commit file này luôn, để cả team dùng chung quy tắc.

### ⚠️ Cái bẫy lớn nhất

`.gitignore` **chỉ có tác dụng với file chưa tracked**. File đã commit rồi thì thêm vào `.gitignore` cũng vô ích.

```bash
git rm --cached secret.txt    # bỏ file khỏi git nhưng GIỮ file trên đĩa
echo "secret.txt" >> .gitignore
git commit -m "stop tracking secret.txt"
```


### ❌ Lưu ý

| ❌                                                | ✅                                                                   |
| ------------------------------------------------ | ------------------------------------------------------------------- |
| Thêm vào `.gitignore` là file đã commit biến mất | Phải `git rm --cached` để git thôi theo dõi                         |
| `git rm` xóa luôn file trên đĩa                  | `git rm --cached` chỉ gỡ khỏi git, file vẫn còn                     |
| Lỡ commit key rồi `git rm --cached` là xong      | Key **vẫn nằm trong lịch sử**. Phải coi như đã lộ, **đổi key ngay** |
| `.gitignore` lưu trên máy mình thôi              | Nó là file thường, commit lên repo được                             |
### Quick reference

| Muốn               | Cách                   |
| ------------------ | ---------------------- |
| Ignore đuôi file   | `*.log`                |
| Ignore thư mục     | `node_modules/`        |
| Ngoại lệ           | `!keep.log`            |
| Gỡ file đã tracked | `git rm --cached file` |
| Xem file bị ignore | `git status --ignored` |