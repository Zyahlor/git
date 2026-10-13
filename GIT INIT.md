# 2 - 10 -2026

> 🧭 **Điều hướng:** [[lộ trình]] • [[HỆ THỐNG NHANH]] | ➡️ Tiếp theo: [[GIT STATUS]]

## 1. GIT INIT là gì?
- biến folder thành repo git - làm 1 việc duy nhất tạo folder ẩn `.git` bên trong
```bash
mkdir demo && cd demo # tao folder
git init # khoi tao repo
ls -a # liet ke tat ca ben trong folder Zyahlor gom ca folder/file an
```

```text
. .. .git 
# la bo nao cua repo - lich su commit,staging area,bracnh - nhanh,config
```

## 2. TUỲ CHỌN
| lệnh             | tác dụng                                                                 |
| ---------------- | ------------------------------------------------------------------------ |
| git init         | khởi taoj repo                                                           |
| git init `name`  | tạo folder `name` rồi init luôn trong đó                                 |
| git init -b main | đặt tên nhánh đầu tiên là main - khỏi phải đổi tên nhánh - git branch -m |
| git init --bare  | repo "trần", không có working tree (dùng cho server)                     |

> git init -b main myproject

```bash
git init -b main myproject # tao folder + repo + branch voi 1 lenh duy nhat

Initialized empty Git repository in /home/Zyahlor/myproject/.git/

ls .git # check ben trong co gi
HEAD  branches  config  description  hooks  info  objects  refs
```

| thành phần | chúa gì                         |
| ---------- | ------------------------------- |
| `HEAD`     | đang đứng ở nhánh nào           |
| `config`   | cấu hình riêng của repo         |
| `objects`  | nơi lưu nội dung file và commit |
| `refs`     | con trỏ tới các nhánh, tag      |

> **LƯU Ý:**

| ❌                                          | ✅                                                                              |
| ------------------------------------------ | ------------------------------------------------------------------------------ |
| `git init` ở thư mục đã có repo sẽ xóa hết | An toàn: chỉ báo `Reinitialized existing Git repository`, không mất dữ liệu    |
| Phải `git init` mỗi lần làm việc           | Chỉ làm **1 lần** cho mỗi project. Nếu lấy project có sẵn thì dùng `git clone` |
| Init lồng repo trong repo                  | Dễ rối, tránh `git init` bên trong thư mục đã là repo                          |
```text
git init → git status → git add → git commit → git log → git branch → git remote/push
```

