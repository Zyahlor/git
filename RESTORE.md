# 4 - 10 - 2026

Lỡ tay là chuyện chắc chắn xảy ra. Bài này là bộ "cứu hộ". Trước tiên phải biết **sai ở khu vực nào** thì dùng lệnh đó:

```
Working tree  --add-->  Staging  --commit-->  Repository
   (sửa file)          (đã add)              (đã commit)
```

| Sai ở đâu                       | Lệnh cứu                              |
| ------------------------------- | ------------------------------------- |
| Sửa file nhầm, chưa add         | `git restore <file>`                  |
| Add nhầm, chưa commit           | `git restore --staged <file>`         |
| Commit nhầm, **chưa push**      | `git commit --amend` hoặc `git reset` |
| Commit nhầm, **đã push**        | `git revert`                          |
| Lỡ `reset --hard`, lỡ xóa nhánh | `git reflog`                          |
## 1. git restore: hủy thay đổi chưa commit

```bash
mkdir undo-demo && cd undo-demo
git init -b main
echo "v1" > a.txt
git add a.txt && git commit -m "v1"

echo "oops" >> a.txt            # sửa nhầm
git restore a.txt               # bỏ sửa, về như commit cuối
cat a.txt             # output v1
```

```bash
echo "oops" >> a.txt
git add a.txt                   # lỡ add
git restore --staged a.txt      # bỏ khỏi staging (nội dung sửa VẪN CÒN)
git status -s           #output  M a.txt
```

## 2. git commit --amend: sửa commit gần nhất

```bash
git commit -amend -m "v1 da fix"
```

Quên 1 file? Add rồi amend:

```bash
git add quen.txt
git commit --amend --no-edit    # gộp vào commit cũ, giữ nguyên lời nhắn
```

Amend **tạo commit mới thay commit cũ** (hash đổi). Chỉ dùng khi **chưa push**.

### 3. `git reset`: lùi nhánh về commit cũ (3 chế độ)

Chuẩn bị 3 commit:

```bash
echo "v2" > a.txt && git commit -am "v2"
echo "v3" > a.txt && git commit -am "v3"
git log --oneline
```

```
c3c3c3c v3
b2b2b2b v2
a1a1a1a v1
```

Giờ muốn bỏ commit `v3`:

| Lệnh                                  | Con trỏ nhánh | Staging          | Working tree (file) |
| ------------------------------------- | ------------- | ---------------- | ------------------- |
| `git reset --soft HEAD~1`             | Lùi           | **Giữ** thay đổi | Giữ                 |
| `git reset --mixed HEAD~1` (mặc định) | Lùi           | Bỏ               | **Giữ**             |
| `git reset --hard HEAD~1`             | Lùi           | Bỏ               | **Bỏ luôn** ⚠️      |
`HEAD~1` = "lùi 1 commit so với hiện tại".

```bash
git reset --soft HEAD~1
git status -s
```

```
M  a.txt          ← thay đổi của v3 vẫn nằm trong staging, commit lại được
```

```bash
git reset --hard HEAD~1       # ⚠️ mất cả thay đổi chưa commit
cat a.txt
```

```
v1
```

### 4. `git revert`: hoàn tác bằng commit mới (an toàn khi đã push)

`reset` **xóa** commit khỏi lịch sử. `revert` **thêm 1 commit đảo ngược** thay đổi, lịch sử giữ nguyên.

```bash
git revert HEAD               # đảo ngược commit gần nhất
```

```
[main 9d8e7f6] Revert "v3"
 1 file changed, 1 insertion(+), 1 deletion(-)
```

```
A ── B ── C ── C'    (C' = commit đảo ngược C)
```

### 5. `git reflog`: tấm lưới an toàn

Git ghi lại **mọi nơi HEAD từng đứng**, kể cả commit đã bị reset hay nhánh đã xóa.

```bash
git reflog
```

```
9d8e7f6 HEAD@{0}: revert: Revert "v3"
c3c3c3c HEAD@{1}: commit: v3
b2b2b2b HEAD@{2}: commit: v2
```

Cứu sau khi lỡ `reset --hard`:

```bash
git reset --hard HEAD~2            # lỡ tay mất v2, v3
git reflog                         # tìm hash c3c3c3c
git reset --hard c3c3c3c           # quay lại như chưa có chuyện gì
```

Cứu nhánh lỡ xóa (và commit mồ côi ở detached HEAD, bài trước):

```bash
git switch -c khoi-phuc c3c3c3c
```

### Nhìn theo 3 khu vực

```
Repository (commit)  ←  Staging  ←  Working tree (file)
```

|Chế độ|Dời con trỏ nhánh|Reset staging|Reset file|
|---|---|---|---|
|`--soft`|✅|❌ giữ|❌ giữ|
|`--mixed` (mặc định)|✅|✅|❌ giữ|
|`--hard`|✅|✅|✅ **mất file**|

Mỗi chế độ "đi sâu" hơn 1 khu vực: soft chỉ đụng commit, mixed đụng thêm staging, hard đụng cả file.

### Demo để tự thấy khác biệt

Chuẩn bị 3 commit (nhớ `git add` lần đầu):

bash

```bash
mkdir reset-demo && cd reset-demo
git init -b main
echo "v1" > a.txt && git add a.txt && git commit -m "v1"
echo "v2" > a.txt && git commit -am "v2"
echo "v3" > a.txt && git commit -am "v3"
git log --oneline
```

```
c3c3c3c v3
b2b2b2b v2
a1a1a1a v1
```

#### `--soft`: bỏ commit, giữ nguyên mọi thứ

bash

```bash
git reset --soft HEAD~1
git log --oneline          # chỉ còn v2, v1
git status -s              # M  a.txt  (đã staged, chờ commit)
cat a.txt                  # v3
```

Commit `v3` biến mất khỏi log, nhưng nội dung `v3` vẫn nằm **trong staging**. Gõ `git commit` là có lại.

Quay lại để thử tiếp:

```bash
git commit -m "v3"
```

#### `--mixed`: bỏ commit + bỏ staging, giữ file

bash

```bash
git reset HEAD~1           # không ghi gì = --mixed
git status -s              #  M a.txt  (sửa, CHƯA staged)
cat a.txt                  # v3
```

File vẫn là `v3`, nhưng git coi là "sửa chưa add". Muốn commit lại phải `git add`.

```bash
git commit -am "v3"
```

#### `--hard`: bỏ hết

bash

```bash
git reset --hard HEAD~1
git status -s              # (trống, sạch)
cat a.txt                  # v2
```

Commit, staging **và nội dung file** đều về `v2`. `v3` mất khỏi mọi nơi nhìn thấy được.

### Cứu lại sau `--hard`

Commit chưa bị xóa thật, reflog còn:

bash

```bash
git reflog                 # tìm hash của v3
git reset --hard c3c3c3c   # quay lại v3
```

### `HEAD~1` là gì

|Viết|Nghĩa|
|---|---|
|`HEAD~1`|Lùi 1 commit|
|`HEAD~2`|Lùi 2 commit|
|`<hash>`|Về thẳng commit đó|

### Chọn chế độ nào

| Tình huống                              | Dùng        |
| --------------------------------------- | ----------- |
| Commit vội, muốn gộp/sửa lại commit     | `--soft`    |
| Commit nhầm file, muốn add lại cho đúng | `--mixed`   |
| Bỏ hẳn, làm lại từ đầu                  | `--hard` ⚠️ |

### So sánh reset vs revert

|          | `reset`                        | `revert`                                      |
| -------- | ------------------------------ | --------------------------------------------- |
| Lịch sử  | **Viết lại** (commit biến mất) | Giữ nguyên, thêm commit mới                   |
| Dùng khi | Commit **chưa push**           | Commit **đã push**, người khác có thể đã pull |
| Rủi ro   | Cao (`--hard` mất file)        | Thấp                                          |

### ❌ Lưu ý

|❌|✅|
|---|---|
|`reset --hard` rồi vẫn lấy lại file chưa commit|**Không lấy lại được.** Reflog chỉ cứu **commit**, không cứu thay đổi chưa commit|
|Reset/amend commit đã push|Phải `push -f`, làm hỏng lịch sử người khác. Dùng `revert`|
|`restore file` có thể hoàn tác|Thay đổi chưa commit bị hủy **vĩnh viễn**|
|Reflog giữ mãi mãi|Chỉ khoảng 30-90 ngày, và chỉ ở **máy local**|
|`HEAD~1` = `HEAD-1`|Viết đúng `HEAD~1` (dấu ngã)|

### Quick reference

| Muốn                               | Lệnh                          |
| ---------------------------------- | ----------------------------- |
| Hủy sửa file                       | `git restore <file>`          |
| Bỏ khỏi staging                    | `git restore --staged <file>` |
| Sửa commit cuối                    | `git commit --amend`          |
| Bỏ commit, giữ code ở staging      | `git reset --soft HEAD~1`     |
| Bỏ commit, giữ code ở working tree | `git reset HEAD~1`            |
| Bỏ commit + code                   | `git reset --hard HEAD~1` ⚠️  |
| Đảo ngược commit đã push           | `git revert <hash>`           |
| Xem lịch sử HEAD để cứu            | `git reflog`                  |