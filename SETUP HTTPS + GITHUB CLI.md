# 4 -10 - 2026

> 🧭 **Điều hướng:** [[lộ trình]] • [[HỆ THỐNG NHANH]] | ⬅️ Trước: [[REMOTE]] | ➡️ Đọc thêm: [[SSH KEY - ĐỌC THÊM]] | ➡️ Tiếp theo: [[GIT PULL]]

## 1. tải công cụ
- gh
- setup: `gh auth login` -> github.com -> https -> đăng nhập qua trình duyệt, browser
- tự cấu hình credential (chứng chỉ) cho git, sau đó push không hỏi gì
## 2. quy trình
tạọ sẵn repo trên github -  github hiện sẵn lệnh
với repo local:

```bash
cd git-notes
git remote add origin https://github.com/<username>/git-notes.git
git branch -M main
git push -u origin main
```

Lần đầu nếu chưa có `gh`, git sẽ hỏi:

```
Username: <username>
Password: <dán Personal Access Token, KHÔNG phải mật khẩu>
```

Tạo token: **Settings → Developer settings → Personal access tokens → Fine-grained tokens**, cho quyền `Contents: Read and write` cho đúng repo đó.

Muốn khỏi nhập lại token mỗi lần:

```bash
git config --global credential.helper store    # lưu token dạng text thô trong ~/.git-credentials
```

|                | HTTPS + `gh`                            | SSH                         |
| -------------- | --------------------------------------- | --------------------------- |
| Thiết lập      | Nhanh, 1 lệnh đăng nhập                 | Tạo key, dán lên GitHub     |
| Push hằng ngày | Không hỏi gì                            | Không hỏi gì                |
| An toàn        | Token có thể giới hạn quyền, thu hồi dễ | Private key nằm trên máy    |
| Hợp khi        | Mới học, cần nhanh                      | Dùng nhiều máy, làm lâu dài |
### ❌ Lưu ý

| ❌                                    | ✅                                                                                  |
| ------------------------------------ | ---------------------------------------------------------------------------------- |
| Nhập mật khẩu GitHub khi push        | Sẽ báo `Authentication failed`. Phải dùng **token**                                |
| `credential.helper store` là an toàn | Token nằm **text thô** trên đĩa. Dùng `gh auth login` hoặc `cache` thì an toàn hơn |
| Dán token vào code/commit            | Lộ là bị quét ngay. Lỡ lộ thì thu hồi token trên GitHub liền                       |
| Bỏ hẳn SSH                           | Với hướng pentest, SSH key vẫn nên biết (dùng lại ở lab, server, TryHackMe)        |