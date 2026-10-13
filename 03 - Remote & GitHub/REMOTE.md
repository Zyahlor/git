# 3 - 10 - 2026

> 🧭 **Điều hướng:** [[lộ trình]] • [[HỆ THỐNG NHANH]] | ⬅️ Trước: [[GIT CLEAN]] | ➡️ Tiếp theo: [[SETUP HTTPS + GITHUB CLI]]

## 1. REMOTE LÀ GÌ?
- `remote` - bản sao của repo nằm ở chỗ khác (`github` , `gitlaab` , `server` )
- local của bạn chỉ là 1 bản, remote dùng để sao lưu + chia sẻ

```text
Local repo ──git push──► Remote (origin) 
Local repo ◄──git pull── Remote (origin)
```

| lệnh                         | tác dụng                                               |
| ---------------------------- | ------------------------------------------------------ |
| git clone `url`              | tải cả repo về (kèm lịch sử,) tự đặt remote tên origin |
| git remote -v                | xem các remote đang kết nối                            |
| git remote add  origin `url` | nối repo local với remote                              |
| git push                     | đẩy commit local lên remote                            |
| git fetch                    | tải commit mới từ remote về, chưa gộp (merge)          |
| git pull                     | `fetch` + `merge` (tải rồi gộp)                        |
## demo output

```bash
cd ~
git init --bare -b main server.git # tạo "remote" giả để thực hành, nhánh main
git clone server.git alice   # alice = máy bạn
git clone server.git bob     # máy đồng nghiệp

#output
Cloning into 'alice'...
warning: You appear to have cloned an empty repository.
(Cảnh báo bình thường, remote còn trống.)
```

```bash
cd alice
echo "hello" > a.txt
git add a.txt
git commit -m "add a.txt"
git remove -v            # xem remote: origin trỏ tới server.git
git push -u origin main  # đẩy lần đầu; -u = nhớ origin/main làm mặc định

#output
To /home/bao/server.git
 * [new branch]      main -> main
branch 'main' set up to track 'origin/main'.

-u chỉ cần lần đầu
```

> bob lấy về

```bash
cd ../bob
git pull             # tải commit của alice về
ls                   # có a.txt
git log --oneline

#output
3f2a1b7 add a.txt
```

>**Bob sửa, push; Alice pull lại:**

```bash
echo "from bob" >> a.txt
git commit -am "bob edit"
git push

cd ../alice
git fetch                               # tải về nhưng CHƯA gộp
git status

#output
Your branch is behind 'origin/main' by 1 commit, and can be fast-forwarded.

git pull                                # giờ mới gộp
```

### `fetch` vs `pull`

|                        | `git fetch`<br> | `git pull`               |
| ---------------------- | --------------- | ------------------------ |
| Tải commit mới         | Có              | Có                       |
| Gộp vào nhánh hiện tại | **Không**       | Có (`fetch` + `merge`)   |
| An toàn để xem trước   | Có              | Có thể gây conflict ngay |

Muốn xem remote có gì mới trước khi gộp thì `fetch` rồi `git log origin/main`.

### Lỗi hay gặp: push bị từ chối

Alice và Bob **cùng commit** rồi Bob push trước, Alice push sau:

```
! [rejected]        main -> main (fetch first)
error: failed to push some refs
```

Cách xử lý: `git pull` (gộp, có thể gặp conflict, bạn đã biết giải) rồi `git push` lại.

### ❌ Lưu ý

| ❌                                      | ✅                                                                                |
| -------------------------------------- | -------------------------------------------------------------------------------- |
| `git push -f` để "ép" cho qua          | **Ghi đè lịch sử remote**, có thể xóa commit của người khác. Người mới tránh hẳn |
| `origin` là từ khóa đặc biệt           | Chỉ là **tên mặc định**, đặt tên khác được                                       |
| `clone` xong phải `git init`           | `clone` đã tự init + nối remote                                                  |
| `pull` luôn an toàn                    | Có thể conflict vì nó có bước merge                                              |
| Push là xong, remote tự thấy mọi nhánh | Nhánh mới phải `git push -u origin <nhánh>`                                      |
| Hash/đường dẫn giống máy mình          | Máy bạn sẽ khác                                                                  |
### Quick reference

| Muốn            | Lệnh                          |
| --------------- | ----------------------------- |
| Tải repo về     | `git clone <url>`             |
| Xem remote      | `git remote -v`               |
| Nối remote      | `git remote add origin <url>` |
| Đẩy lần đầu     | `git push -u origin main`     |
| Đẩy các lần sau | `git push`                    |
| Tải xem trước   | `git fetch`                   |
| Tải + gộp       | `git pull`                    |
i1. Cho **alice và bob cùng sửa dòng 1 của `a.txt`**, bob push trước, alice `pull` → conflict → tự giải → push
2. Dán `git log --graph --oneline` của alice lên mình xem