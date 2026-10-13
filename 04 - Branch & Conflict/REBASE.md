# 4 - 10 -2026

> 🧭 **Điều hướng:** [[lộ trình]] • [[HỆ THỐNG NHANH]] | ⬅️ Trước: [[GIT CHERRY-PICK]] | ➡️ Tiếp theo: [[GITHUB PULL REQUEST]] | 🚑 Cứu nguy: [[RESTORE]]

## 1. rebase là gif?
- dời các commit của nhánh bạn sang đứng sau đầu cảu nhánh khác, như bắt đầu làm từ commit mới nhất

Cả `merge` và `rebase` đều gộp công việc 2 nhánh. Khác nhau ở **lịch sử** để lại.

```
Trước:
main:     A ── B ── C
               \
feature:        D ── E

merge:    A ── B ── C ───── M      (M = merge commit, cây rẽ rồi gộp)
               \          /
                D ── E ──┘

rebase:   A ── B ── C ── D' ── E'  (1 đường thẳng, D' E' là commit MỚI)
```

|                    | `merge`                      | `rebase`                            |
| ------------------ | ---------------------------- | ----------------------------------- |
| Lịch sử            | Có rẽ nhánh, có merge commit | Thẳng 1 đường                       |
| Commit cũ          | Giữ nguyên                   | **Viết lại** (hash đổi: `D` → `D'`) |
| Giữ bối cảnh nhánh | Có                           | Không                               |
| Rủi ro             | Thấp                         | Cao nếu commit đã push              |
## demo output

```bash
mkdir rebase-demo && cd rebase-demo
git init -b main
echo "base" > base.txt && git add base.txt && git commit -m "base"

git switch -c feature
echo "f1" > f1.txt && git add f1.txt && git commit -m "feature: f1"
echo "f2" > f2.txt && git add f2.txt && git commit -m "feature: f2"

git switch main
echo "m1" > m1.txt && git add m1.txt && git commit -m "main: m1"

git log --graph --oneline --all

#output
* 5c6d7e8 (HEAD -> main) main: m1
| * 3a4b5c6 (feature) feature: f2
| * 1f2a3b4 feature: f1
|/
* 9e8d7c6 base
```

Giờ **rebase `feature` lên `main`**: đứng ở `feature`, rebase vào `main`:

```bash
git switch feature
git rebase main
```

```
Successfully rebased and updated refs/heads/feature.
```

```bash
git log --graph --oneline --all
```

```
* 8b9c0d1 (HEAD -> feature) feature: f2
* 7a8b9c0 feature: f1
* 5c6d7e8 (main) main: m1
* 9e8d7c6 base
```

Thẳng 1 đường. Hash `f1`, `f2` đã **đổi** (`1f2a3b4` → `7a8b9c0`): chúng là commit mới. Giờ gộp về main sẽ là fast-forward:

```bash
git switch main
git merge feature
```

```
Fast-forward
```

### Rebase gặp conflict

Giống merge, nhưng giải **từng commit một**:
- rebase = dời commit nhánh mình lên đầu nhánh khác (hash đổi)
- mục đích: lịch sử thẳng, ít merge commit rác
- quy tắc vàng: KHÔNG rebase commit đã push