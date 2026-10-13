# 3 - 10 - 2026
## 1. SSH key là gì?
- thay vì gõ pass cho mỗi lần push , bạn tạo 1 cặp khóa:

| Khóa                            | File             | Để đâu           | Chia sẻ?            |
| ------------------------------- | ---------------- | ---------------- | ------------------- |
| **Private key** (khóa riêng)    | `id_ed25519`     | Chỉ trên máy bạn | ❌ **Không bao giờ** |
| **Public key** (khóa công khai) | `id_ed25519.pub` | Dán lên GitHub   | ✅ Thoải mái         |
```
Máy bạn (private key) ──"tôi chứng minh tôi có khóa riêng"──► GitHub (public key)
```

GitHub dùng public key để kiểm tra bạn có đúng private key không, mà private key không bao giờ rời máy.

## 2. các bước thực hiện:

### 1. khai báo danh tính git (nếu chưa)

```bash
git config --global user.name "Bao"               # tên hiện trong commit
git config --global user.email "ban@email.com"    # nên dùng email đăng ký GitHub
git config --global init.defaultBranch main       # nhánh mặc định main
git config --global pull.rebase false             # pull = fetch + merge
git config --global --list                        # kiểm tra lại
```

### Bước 2: Tạo SSH key

```bash
ssh-keygen -t ed25519 -C "ban@email.com"   # -t: thuật toán, -C: ghi chú nhận diện key
```

```
Enter file in which to save the key (/home/bao/.ssh/id_ed25519):    ← Enter (mặc định)
Enter passphrase (empty for no passphrase):                         ← nên đặt passphrase
```

Hai file được tạo trong `~/.ssh/`:

```bash
ls ~/.ssh
```

```
id_ed25519  id_ed25519.pub
```

|Option|Nghĩa|
|---|---|
|`-t ed25519`|Thuật toán hiện đại, ngắn, nhanh, an toàn (nên dùng thay RSA)|
|`-C "..."`|Chỉ là nhãn để phân biệt nhiều key|
|passphrase|Mật khẩu bảo vệ private key, lỡ lộ file cũng chưa dùng được ngay|
### Bước 3: Nạp key vào ssh-agent

`ssh-agent` nhớ key đã mở khóa, khỏi gõ passphrase mỗi lần push.


```bash
eval (ssh-agent -c)          # fish dùng cú pháp này (bash là eval "$(ssh-agent -s)")
ssh-add ~/.ssh/id_ed25519    # nạp private key vào agent
ssh-add -l                   # liệt kê key đang nạp
```

### Bước 4: Đưa public key lên GitHub


```bash
cat ~/.ssh/id_ed25519.pub    # in ra public key (1 dòng, bắt đầu bằng ssh-ed25519)
```

Copy **cả dòng** đó, rồi trên GitHub:

```
Avatar → Settings → SSH and GPG keys → New SSH key → dán vào → Add SSH key
```

### Bước 5: Kiểm tra kết nối


```bash
ssh -T git@github.com
```

```
The authenticity of host 'github.com (...)' can't be established.
ED25519 key fingerprint is SHA256:+DiY3wvvV6TuJJhbpZisF/zLDA0zPMSvHdkr4UvCOqU.
Are you sure you want to continue connecting (yes/no/[fingerprint])?
```

Lần đầu git hỏi tin server này không. Đối chiếu fingerprint với trang chính thức của GitHub rồi gõ `yes`. Thành công sẽ ra:

```
Hi <username>! You've successfully authenticated, but GitHub does not provide shell access.
```

Dòng "does not provide shell access" là **bình thường**, không phải lỗi.

Với hướng pentest, bước hỏi fingerprint là kiến thức thật: gõ `yes` bừa mà không kiểm tra là mở cửa cho tấn công **MITM** (kẻ giữa đứng giả server).

### Bước 6: Tạo repo trên GitHub và đẩy lên

Trên GitHub bấm **New repository**, đặt tên (vd `git-notes`), **không tick** "Add README" (để repo trống), rồi:


```bash
mkdir git-notes && cd git-notes
git init -b main
echo "# Git notes" > README.md
git add README.md
git commit -m "init"

git remote add origin git@github.com:<username>/git-notes.git   # nối local với GitHub
git remote -v                                                    # kiểm tra
git push -u origin main                                          # đẩy lần đầu
```

```
origin  git@github.com:<username>/git-notes.git (fetch)
origin  git@github.com:<username>/git-notes.git (push)
```

```
To github.com:<username>/git-notes.git
 * [new branch]      main -> main
branch 'main' set up to track 'origin/main'.
```

Reload trang repo trên GitHub là thấy file.

### SSH vs HTTPS

||SSH|HTTPS|
|---|---|---|
|URL|`git@github.com:user/repo.git`|`https://github.com/user/repo.git`|
|Xác thực|Cặp khóa|Token (GitHub không còn nhận mật khẩu)|
|Tiện|Thiết lập 1 lần, push không hỏi|Phải nhập/lưu token|

Đổi URL của remote đã có:


```bash
git remote set-url origin git@github.com:<username>/git-notes.git
```

### ❌ Lỗi hay gặp

|❌ Lỗi|✅ Cách xử lý|
|---|---|
|`Permission denied (publickey)`|Chưa dán public key lên GitHub, hoặc chưa `ssh-add`. Chạy `ssh -T git@github.com` để test|
|Dán nhầm file `id_ed25519` (không `.pub`)|**Lộ private key!** Xóa key trên GitHub, tạo cặp mới. Chỉ dán file `.pub`|
|`Repository not found`|Sai username/tên repo trong URL, kiểm tra `git remote -v`|
|`rejected ... fetch first`|Repo GitHub đã có commit (vd tick README). `git pull --no-rebase origin main` rồi push|
|`WARNING: UNPROTECTED PRIVATE KEY FILE`|Quyền file quá rộng: `chmod 600 ~/.ssh/id_ed25519`|
|Commit private key / `.env` lên GitHub|Coi như **đã lộ**, đổi key ngay (xóa file không đủ, lịch sử vẫn còn)|

### Quick reference

| Muốn           | Lệnh                               |
| -------------- | ---------------------------------- |
| Tạo key        | `ssh-keygen -t ed25519 -C "email"` |
| Xem public key | `cat ~/.ssh/id_ed25519.pub`        |
| Nạp key        | `ssh-add ~/.ssh/id_ed25519`        |
| Test GitHub    | `ssh -T git@github.com`            |
| Nối remote     | `git remote add origin <url>`      |
| Đổi URL        | `git remote set-url origin <url>`  |
| Đẩy lần đầu    | `git push -u origin main`          |
