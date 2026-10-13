# 1 -  10 - 2026
## 1. GIT STATUS LÀ GÌ?
- hiển thị trạng thái cây làm việc - working tree và staging area - vùng đệm giữa folder làm việc và commit
```text
working tree -> git add -> staging area -> git commit -> repository
(file ban sua)           (danh sach cho)                 (lich su commit)
```

### cho biết:
- `staged` (changes to be committed): đã git add, sẵn sàng commit
- `modified`(changes not staged for commit):file đã track - theo dõi những sửa chưa add
- `untracked`: file mới, git chưa theo dõi

| option                                 | ý nghĩa                                                              |
| -------------------------------------- | -------------------------------------------------------------------- |
| `-s`,`--short`                         | output dạng  ngắn                                                    |
| `-b`,`--branch`                        | hiển thị thông tin nhánh(kể cả ở dạng short)<br>                     |
| `--show-stash`                         | hiện số lượng stash - ngăn kéo tạm, các thay đổi đang dở chưa commit |
| `--porcelain[=ver]`                    | <br>output ổn định cho máy - script đọc - ver = phiên bản            |
| `-u[mode]`, `--untracked-files[=mode]` | cách hiển thị file `untracked`                                       |
| `--ignored`                            | hiện cả file bị .gitignore                                           |
### 2. TEST
> `git init`

```text
Initialized empty Git repository in /home/Zyahlor/demo/.git/ 

lưu ý: /home/Zyahlor/demo/.git/  là path demo ko phải mỗi lần làm mặc định là như vậy
```

> `git status`

```text
On branch main
No commits yet
nothing to commit (create/copy files and use "git add" to track)
```

> `file untracked`

```bash
# tao file cai da
echo "hello,github" > a.txt

-- git status
On branch main

No commits yet

Untracked files:
  (use "git add <file>..." to include in what will be committed)
	a.txt

nothing added to commit but untracked files present (use "git add" to track)
```

> `staged`

```bash
# add file
git add a.txt

-- git status
On branch main

No commits yet

Changes to be committed:
  (use "git rm --cached <file>..." to unstage)
	new file:   a.txt
```

> `commit`

```bash
git commit -m "add a.txt"

[main (root-commit) b0c9f5f] add a.txt
 1 file changed, 1 insertion(+)
 create mode 100644 a.txt
 
git status
On branch main
nothing to commit, working tree clean
```

> `modified`

```bash
# modify
echo "my name's Zyahlor" >> a.txt

-- git status
On branch main
Changes not staged for commit:
  (use "git add <file>..." to update what will be committed)
  (use "git restore <file>..." to discard changes in working directory)
	modified:   a.txt

no changes added to commit (use "git add" and/or "git commit -a")
```

> `-s` & `-sb`

```text
git status -s -- dang short
 M a.txt
 
git status -sb -- short + branch = ngan va show nhanh
## main
 M a.txt
```

> `-u` với untrack folder

```bash
# tao thu muc con
mkdir src && touch src/x.txt src/y.txt

# -u - hien thi file untrack
git status -s -uno # an file untracked
M a.txt 
# vi sao lai a.txt? vi no da tracked roi - folder src va con la file x.txt,y.txt chua duoc tracked nen ko hien

# -u+all
git status -s -uall 
 M a.txt
?? src/x.txt
?? src/y.txt
# liet ke tung file ben trong folder untracked

# -u+normal = status mac dinh
git status -s # vi normal la mac dinh roi ko can them tham so
 M a.txt
?? src/
```

> `--ignored`

```bash
# them file .gitignore da 
echo "*.log" > .gitignore

# tao 1 file log
touch debug.log

# check
git status -s --ignored
 M a.txt             # m = file da tracked va sua, chua staged 
?? .gitignore        # ?? =  chua track
?? src/
!! debug.log         # !! file bi ignore - ko ghi nhan de day len git
```

> `--show-stash`

```bash
git add -A # dua full thay doi vao staging (ke ca file moi)
git commit -m "second commit" #commit - ghi chu , working tree sach

echo "x" >> a.txt # them x vao file => thay doi lan 1
git stash         # cat thay doi vao stash,a.txt ve trang thai commit -  tuc ve lai nhu ban dau truoc khi them x
Saved working directory and index state WIP on main: 417b2c1 second commit

echo "y" >> a.txt # y nhu tren
git stash         # cat tiep => thay doi lan 2
Saved working directory and index state WIP on main: 417b2c1 second commit

git stash list # xem danh sach cac muc dang cat
stash@{0}: WIP on main: 417b2c1 second commit
stash@{1}: WIP on main: 417b2c1 second commit

git status --show-stash # status kem so luong stash
On branch main
nothing to commit, working tree clean
Your stash currently has 2 entries
```

> `STASH POP` - lấy ra  stash mới nhất ra working tree và xóa nó

```bash
git stash pop 
Changes not staged for commit:
	modified:   a.txt
Dropped refs/stashes/stash@{0} (a1b2c3d...)

git stash list # kiem tra chi con 1 muc
stash@{0}: WIP on main: 5c9d2e1 save all
```

> `--porcelain`

```bash
git status --porcelain # giong -s nhuwng format dam bao ko doi,dung cho script
M a.txt
```