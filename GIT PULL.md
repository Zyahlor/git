# 3 - 10 - 2026
## 1. GIT PULL LÀ GÌ?
- láy thay đổi từ remote về và gộp vào nhánh hiện tại
```text
git pull = git fetch (down về) + git merge (gộp vào)
```

- remote (`origin`) là bản repo nằm ở chỗ khác, người khác push lên đó, bạn phải `pull` về để update bản của mình
## 2. TÙY CHỌN

| lệnh                  | tác dụng                                            |
| --------------------- | --------------------------------------------------- |
| `git pull`            | tải vaf gộp nhánh đang theo dõi vào nhánh hiện tại  |
| git pull origin main  | chỉ định rõ remote và nhánh                         |
| git pull --no-rebase  | gộp bằng merge ( cho cây rẽ + merge commit)         |
| git pull -ff-only     | chỉ cho fast-forward , không thì báo lỗi, không gộp |
| git fetch             | tải về, không đụng code                             |
| git merge origin/main | gộp thủ công thứ vừa fetch                          |
### Thông báo `divergent branches`

Khi 2 bên đã rẽ nhánh, git bản mới có thể đòi bạn chọn cách gộp:

```
fatal: Need to specify how to reconcile divergent branches.
```

Chọn 1 lần cho mọi repo:

bash

```bash
git config --global pull.rebase false   # pull = fetch + merge
```

Hoặc chọn riêng từng lần: `git pull --no-rebase`.

### ❌ Lưu ý

|❌|✅|
|---|---|
|`pull` chỉ tải về|Tải **và gộp**. Chỉ muốn tải thì `fetch`|
|`pull` luôn an toàn|Có thể conflict vì có bước merge|
|Pull khi đang sửa dở file|Commit hoặc `stash` trước, tránh vướng|
|Phải gõ `--no-rebase` mỗi lần|Đặt `pull.rebase false` 1 lần là xong|

### Quick reference

| Muốn                           | Lệnh                                  |
| ------------------------------ | ------------------------------------- |
| Xem remote có gì mới, chưa gộp | `git fetch` rồi `git log origin/main` |
| Tải + gộp                      | `git pull`                            |
| Gộp bằng merge, rõ ràng        | `git pull --no-rebase`                |
| Chỉ chấp nhận fast-forward     | `git pull --ff-only`                  |