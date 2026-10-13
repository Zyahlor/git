# 3 - 10 - 2026
## 1. BRANCH LÀ GÌ?
-  là nhánh = 1 dòng phát triển riêng, bạn tách ra làm tính năng mới mà ko đụng vào code chính (main), làm xong thì merge (gộp) lại
```text
main:     A --- B --- C
                 \
feature:          D --- E
```

nhánh chỉ là 1 con trỏ và trỏ tới 1 commit, nên tạo nhánh cực nhanh, không copy file

## 2. Tùy Chọn

| lệnh                     | tác dụng                               |
| ------------------------ | -------------------------------------- |
| git branch               | liệt kê nhánh ( `*` là nhánh hiện tại) |
| git branch `name`        | tạo nhánh, **chưa** chuyển sang        |
| git switch -c `name`     | tạo và chuyển sang nhánh đó luôn       |
| git switch `name`        | chuyển nhánh                           |
| git merge `name`         | gộp nhánh `name` vào nhánh hiện tại    |
| git branch -d `name`     | xóa nhánh đã merge                     |
| git branch -D `name`     | xóa ép (kể cả chưa merge)              |
| git branch -m `name_moi` | đổi tên nhánh hiện tại                 |

## demo output

```bash
mkdir demo4 && cd demo4
git init -b main                  # tạo repo, nhánh main
echo "hello" > a.txt              # tạo file
git add a.txt                     # đưa vào staging
git commit -m "add a.txt"         # commit đầu tiên

git branch                        # xem danh sách nhánh

# output
* main
```
> chuyển nhánh
```bash
git switch -c feature  # tạo nhánh feature và chuyển sang

#output
switched to a new branch 'feature'

git branch                        # giờ có 2 nhánh, * ở feature

#output
* feature
  main
```

> commit trên nhánh riêng

```bash
echo "new feature" > b.txt # tạo file mới ( đang ở nhánh feature)
git add b.txt 
git commit -m "add b.txt"  # commit này nằm trên feature

ls # xem file hiện có
```

```text
a.txt  b.txt
```

```bash
git switch main # quay về main
ls              # b.txt biến mất ( main chưa có commit đó)

Switched to branch 'main'
a.txt
```

`b.txt` không mất, nó nằm an toàn trên `feature`. Git đổi file trong thư mục theo nhánh bạn đứng.

> merge - kiểu fast-forward

```bash
git merge feature                 # đang ở main, gộp feature vào

Updating 3f2a1b7..9c1d4e8
Fast-forward
 b.txt | 1 +
 1 file changed, 1 insertion(+)
 create mode 100644 b.txt
 
 git branch -d feature             # xong việc, xóa nhánh feature
 
 Deleted branch feature (was 9c1d4e8).
```

**Fast-forward** = `main` chưa có commit mới nào, nên git chỉ việc **đẩy con trỏ `main` tiến lên** tới commit của `feature`. Không tạo commit mới.

> merge khi cả 2 nhánh đều có commit mới

```bash
git switch -c feature2    # tạo nhánh feature2
echo "c" > c.txt
git add c.txt
git commit -m "add c.txt"  # commit trên feature2

git switch main            # về main
echo "d" > d.txt
git add d.txt
git commit -m "add d.txt"  # commit trên main (2 nhánh đã tách nhau )

git merge feature2 -m "merge feature2"   # gộp, -m khỏi mở editor

Merge made by the 'ort' strategy.
 c.txt | 1 +
 1 file changed, 1 insertion(+)
 create mode 100644 c.txt
```

### 3. MERGE CONFLICT
- `COnflict` = 2 nhánh sửa cùng 1 chỗ  ( cùng 1 dòng,cùng  file) , git không biết chọn bên nào -> dưng flaij, bát user tự quyết định

| Tình huống                                | Kết quả       |
| ----------------------------------------- | ------------- |
| 2 nhánh sửa **file khác nhau**            | Merge tự động |
| 2 nhánh sửa **cùng file, dòng khác nhau** | Merge tự động |
| 2 nhánh sửa **cùng dòng**                 | ⚠️ CONFLICT   |
### Demo gây conflict
> main branch
```bash
mkdir conflict-demo && cd conflict-demo
git init -b main # khởi tạo repo + nhánh 
printf 'line1\nline2\nline3' > a.txt # tao file vs 3 dong
git add a.txt or git add . # add . vi co 1 file duy nhat hoac nhieu file muon add
git commit -m "add a.txt" # commit de staged
```
> feature branch

```bash
git switch -c feature # tao nhanh moi ten feature
printf 'line1\nFEATURE line2\nline3\n' # sua dong 2 o feature 
git commit -am "feature:edit line2" # co -a vi a.txt da tracked
```

> switch main

```bash
git switch main
printf 'line1\nMAIN line2\nline3\n' > a.txt # sua cung dong 2 o main
git commit -am "main : edit line2" 

git merge feature # gay conflict

#output
Auto-merging a.txt
CONFLICT (content): Merge conflict in a.txt
Automatic merge failed; fix conflicts and then commit the result.
```

> xem trạng thái

```bash
git status

#output
On branch main
You have unmerged paths.
  (fix conflicts and run "git commit")

Unmerged paths:
  (use "git add <file>..." to mark resolution)
	both modified:   a.txt
	
# both modified =  2 nhanh deu sua file nay
```

> đọc conflict

```bash
cat a.txt

#output
line1
<<<<<<< HEAD
MAIN line2
=======
FEATURE line2
>>>>>>> feature
line3
```

| Dấu               | Nghĩa                                                |
| ----------------- | ---------------------------------------------------- |
| `<<<<<<< HEAD`    | Bắt đầu phần của **nhánh hiện tại** (main)           |
| `=======`         | Ranh giới giữa 2 bên                                 |
| `>>>>>>> feature` | Kết thúc phần của **nhánh đang merge vào** (feature) |
### GIẢI QUYẾT

1. sửa bằng tay - giữ bên nào/ghép cả 2, xóa hết dòng đấu `<<<`, `===` , `>>>`
```bash
printf 'line1\nMAIN + FEATURE line2\nline3\n' > a.txt   # ví dụ: ghép cả hai
```

2. báo git là xử lý xong
```bash
git add a.txt
```

3. commit để hoàn thành merge
```bash
git commit -m "merge feature, resolve conflict"
git log --graph --oneline

#ouput
*   c3d4e5f (HEAD -> main) merge feature, resolve conflict
|\
| * 9b8a7c6 (feature) feature: edit line2
* | 2e1d0c9 main: edit line2
|/
* 5f4e3d2 add a.txt
```

- `--abort` : thoát giữa chừng - quay về trc merge
```bash
git merge --abort
```

### Lệnh hỗ trợ

| Lệnh                          | Tác dụng                                |
| ----------------------------- | --------------------------------------- |
| `git status`                  | Xem file nào đang conflict              |
| `git diff`                    | Xem chi tiết phần xung đột              |
| `git merge --abort`           | Hủy merge, về như cũ                    |
| `git checkout --ours a.txt`   | Lấy nguyên bản của nhánh hiện tại       |
| `git checkout --theirs a.txt` | Lấy nguyên bản của nhánh đang merge vào |
### ❌ Lưu ý

| ❌                                 | ✅                                                                     |
| --------------------------------- | --------------------------------------------------------------------- |
| Commit khi file còn dấu `<<<<<<<` | Phải xóa **hết** dấu, không thì code hỏng (với `.c` là lỗi biên dịch) |
| Sửa xong là xong                  | Phải `git add` rồi `git commit` mới kết thúc merge                    |
| Conflict là lỗi/hỏng repo         | Bình thường, git chỉ hỏi ý bạn. `--abort` luôn cứu được               |
| Hash giống y hệt                  | Hash máy bạn sẽ khác                                                  |
### Quick reference

| Bước              | Lệnh                |
| ----------------- | ------------------- |
| Gây merge         | `git merge <nhánh>` |
| Tìm file conflict | `git status`        |
| Sửa tay, xóa dấu  | editor              |
| Đánh dấu xong     | `git add <file>`    |
| Hoàn tất          | `git commit`        |
| Bỏ cuộc           | `git merge --abort` |