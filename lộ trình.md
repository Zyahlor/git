# Git Learning Order

> Mục tiêu: Nắm vững Git từ cơ bản đến làm việc với nhánh, xử lý xung đột, cộng tác trên GitHub và các kỹ thuật nâng cao.
> 
> 🗺️ **Bản đồ tra cứu nhanh & Công dụng từng file:** [[HỆ THỐNG NHANH]]
> 
> Quy ước:
> 
> - `[x]`: Đã có tài liệu trong kho ghi chú.
>     
> - `[ ]`: Chủ đề kế hoạch cần học hoặc bổ sung tài liệu.
>     
> - Thứ tự bên dưới là thứ tự học đề xuất, không phải thứ tự tên file hiện tại.
> 
> 📊 **Tiến độ hiện tại:** 15/32 File đã có (`[x]`) | 17 File kế hoạch (`[ ]`)

Repository: `https://github.com/Zyahlor/git`

---

## Giai đoạn 1: Git Fundamentals

**Mục tiêu:** Hiểu Git quản lý file như thế nào, tạo repository và lưu lại lịch sử thay đổi.

### 1. Khởi tạo repository

- [x] [[GIT INIT]]
    
    - `git init`
        
    - `git clone`
        
    - Khởi tạo repository mới và sao chép repository có sẵn.
        
    

### 2. Kiểm tra trạng thái

- [x] [[GIT STATUS]]
    
    - `git status`
        
    - Phân biệt `untracked`, `modified`, `staged`.
        
    - Hiểu trạng thái của file trong working tree và staging area.
        
    

### 3. Đưa file vào staging area

- [x] [[GIT ADD]]
    
    - `git add <file>`
        
    - `git add .`
        
    - `git add -A`
        
    - `git add -u`
        
    - `git add -p`
        
    - `git restore --staged <file>` (tham khảo thêm [[RESTORE]])
        
    

### 4. Tạo commit

- [x] [[GIT COMMIT]]
    
    - `git commit`
        
    - `git commit -m`
        
    - `git commit --amend` (tham khảo thêm [[RESTORE]])
        
    - Viết commit message có ý nghĩa.
        
    

### 5. Xem lịch sử commit

- [x] [[GIT LOG]]
    
    - `git log`
        
    - `git log --oneline`
        
    - `git log -n`
        
    - `git log --stat`
        
    - `git log -p`
        
    - `git log --graph --oneline --all`
        
    

### 6. So sánh thay đổi

- [x] [[GIT DIFF]]
    
    - `git diff`
        
    - `git diff --staged`
        
    - `git diff HEAD`
        
    - `git diff --stat`
        
    

### 7. Xem chi tiết một commit

- [ ] [[GIT SHOW]] *(Kế hoạch tạo)*
    
    - `git show`
        
    - `git show <commit>`
        
    - `git show --stat <commit>`
        
    - So sánh chức năng của `git show` với `git log -p`.
        
    

### 8. Ôn tập giai đoạn 1

- Tạo repository mới ([[GIT INIT]]).
    
- Thêm nhiều file và kiểm tra `git status` ([[GIT STATUS]]).
    
- Stage một file, sau đó stage toàn bộ file còn lại ([[GIT ADD]]).
    
- Tạo ít nhất ba commit ([[GIT COMMIT]]).
    
- Dùng `git diff` ([[GIT DIFF]]), `git log` ([[GIT LOG]]) và `git show` ([[GIT SHOW]]) để kiểm tra lịch sử.
    
- Tự giải thích được đường đi của một thay đổi:
    

`Working tree → Staging area → Commit`

---

## Giai đoạn 2: Quản lý file và cấu hình

**Mục tiêu:** Biết quản lý file trong repository, tránh đưa file không cần thiết vào Git và cấu hình môi trường làm việc.

### 9. Cấu hình Git

- [ ] [[GIT CONFIG]] *(Kế hoạch tạo)*
    
    - `git config --list`
        
    - `git config --global`
        
    - `git config --local`
        
    - `git config --unset`
        
    - Cấu hình `user.name`, `user.email`.
        
    - Phân biệt cấu hình toàn máy và cấu hình riêng repository.
        
    

### 10. Di chuyển và xóa file

- [ ] [[GIT FILE OPERATIONS]] *(Kế hoạch tạo)*
    
    - `git rm <file>`
        
    - `git rm --cached <file>`
        
    - `git mv <old> <new>`
        
    - Phân biệt xóa file khỏi Git và xóa file khỏi ổ đĩa.
        
    - Hiểu khi nào cần dùng `git rm --cached`.
        
    

### 11. Bỏ qua file không cần theo dõi

- [x] [[gitignore]]
    
    - Quy tắc trong `.gitignore`.
        
    - Các mẫu đường dẫn và ký tự wildcard.
        
    - Kiểm tra file bị bỏ qua bằng `git status --ignored`.
        
    
- Thực hành thêm
    
    - Bỏ qua file biên dịch, file cấu hình cá nhân và thư mục cache.
        
    - Xử lý file đã được Git theo dõi trước khi thêm quy tắc `.gitignore`.
        
    

### 12. Khôi phục thay đổi

- [x] [[RESTORE]]
    
    - `git restore <file>`
        
    - `git restore --staged <file>`
        
    
- Liên kết: [[RESTORE]] và [[REBASE]] cùng các ghi chú liên quan
    
    - Phân biệt khôi phục file với khôi phục trạng thái commit.
        
    
- Ôn kỹ sự khác nhau giữa:
    
    - `git restore`
        
    - `git reset`
        
    - `git revert`
        
    - `git reflog`
        
    

### 13. Làm sạch file chưa được theo dõi

- [ ] [[GIT CLEAN]] *(Kế hoạch tạo)*
    
    - `git clean -n`
        
    - `git clean -f`
        
    - `git clean -fd`
        
    - Hiểu nguy cơ xóa file chưa được Git theo dõi.
        
    - Luôn xem trước bằng `-n` trước khi xóa.
        
    

### 14. Ôn tập giai đoạn 2

- Đổi tên và xóa file bằng Git ([[GIT FILE OPERATIONS]]).
    
- Tạo `.gitignore` cho một project C hoặc Python ([[gitignore]]).
    
- Thử xử lý một file đã được theo dõi nhưng sau đó muốn bỏ theo dõi.
    
- Thực hành khôi phục file đã sửa và file đã stage ([[RESTORE]]).
    
- Hiểu rõ lệnh nào có thể làm mất dữ liệu.
    

---

## Giai đoạn 3: Remote và GitHub

**Mục tiêu:** Đưa repository lên GitHub và đồng bộ giữa máy cá nhân với repository từ xa.

### 15. Kết nối remote

- [x] [[REMOTE]]
    
    - `git remote -v`
        
    - `git remote add`
        
    - `git remote remove`
        
    - `git remote rename`
        
    - Hiểu `origin` là gì.
        
    

### 16. Xác thực với GitHub

- [x] [[SETUP HTTPS + GITHUB CLI]]
    
    - Kết nối bằng HTTPS.
        
    - Xác thực bằng GitHub CLI.
        
    
- [x] [[SSH KEY - ĐỌC THÊM]]
    
    - SSH key là gì.
        
    - Tạo và sử dụng SSH key để xác thực.
        
    

### 17. Đẩy commit lên GitHub

- [ ] [[GIT PUSH]] *(Kế hoạch tạo - hiện có trong [[REMOTE]])*
    
    - `git push`
        
    - `git push -u origin main`
        
    - Tracking branch và upstream.
        
    - Ý nghĩa của `-u`.
        
    - Phân biệt commit ở máy cá nhân và commit đã được đẩy lên remote.
        
    

### 18. Tải thay đổi từ remote

- [x] [[GIT PULL]]
    
    - `git fetch`
        
    - `git pull`
        
    - Phân biệt tải thông tin từ remote và tích hợp thay đổi vào nhánh hiện tại.
        
    

### 19. Đồng bộ khi lịch sử bị lệch

- [ ] [[GIT SYNC]] *(Kế hoạch tạo - hiện có trong [[GIT PULL]])*
    
    - Local branch và remote branch khác nhau.
        
    - `git branch -vv`
        
    - `git fetch --prune`
        
    - Non-fast-forward.
        
    - Divergent branches.
        
    - Khi nào chọn merge hoặc rebase.
        
    

### 20. Ôn tập giai đoạn 3

- Tạo repository local và kết nối với GitHub ([[REMOTE]]).
    
- Push commit từ máy cá nhân ([[GIT PUSH]] / [[REMOTE]]).
    
- Sửa file trực tiếp trên GitHub rồi pull về ([[GIT PULL]]).
    
- Tạo tình huống local và remote có commit riêng.
    
- Tự xử lý lỗi non-fast-forward mà không xóa lịch sử một cách tùy tiện.
    

---

## Giai đoạn 4: Branch và xử lý xung đột

**Mục tiêu:** Làm việc với nhiều nhánh, tích hợp code và giải quyết conflict.

### 21. Quản lý branch

- [x] [[GIT BRANCH]]
    
    - `git branch`
        
    - Tạo, xem và xóa branch.
        
    - Hiểu vai trò của branch trong quá trình phát triển.
        
    

### 22. Chuyển nhánh và khôi phục trạng thái

- [x] [[GIT CHECKOUT]]
    
    - `git checkout`
        
    - Chuyển nhánh và checkout commit.
        
    
- Bổ sung vào [[GIT CHECKOUT]]:
    
    - `git switch`
        
    - `git switch -c`
        
    - Khi nào dùng `switch`, khi nào dùng `restore` (tham khảo [[RESTORE]]).
        
    

### 23. Merge branch

- [x] [[GIT BRANCH]] *(Phần Merge)*
    
    - `git merge`
        
    - Fast-forward merge.
        
    - Merge commit.
        
    - Merge conflict.
        
    - `git merge --abort`.
        
    

### 24. Giải quyết merge conflict

- [x] [[GIT BRANCH]] và [[REBASE]] *(Phần giải quyết xung đột)*
    
    - Nhận biết conflict.
        
    - Đọc các marker `<<<<<<<`, `=======`, `>>>>>>>`.
        
    - Sửa nội dung và đánh dấu đã giải quyết bằng `git add` ([[GIT ADD]]).
        
    - Hoàn thành hoặc hủy thao tác đang thực hiện.
        
    
- Thực hành thêm
    
    - Conflict khi hai nhánh sửa cùng một dòng.
        
    - Conflict khi hai nhánh thêm cùng một file.
        
    - Phân biệt `--ours` và `--theirs` trong đúng ngữ cảnh ([[GIT CHECKOUT]]).
        
    

### 25. Stash thay đổi tạm thời

- [ ] [[GIT STASH]] *(Kế hoạch tạo)*
    
    - `git stash`
        
    - `git stash list`
        
    - `git stash apply`
        
    - `git stash pop`
        
    - `git stash drop`
        
    - Khi nào cần cất thay đổi chưa commit để chuyển nhánh.
        
    

### 26. Cherry-pick commit

- [ ] [[GIT CHERRY-PICK]] *(Kế hoạch tạo)*
    
    - `git cherry-pick <commit>`
        
    - Đưa một commit cụ thể sang nhánh khác.
        
    - Xử lý conflict và hủy cherry-pick.
        
    

### 27. Rebase

- [x] [[REBASE]]
    
    - `git rebase`
        
    - Xử lý conflict trong quá trình rebase.
        
    - `git rebase --continue`
        
    - `git rebase --abort`
        
    

### 28. Interactive rebase

- Bổ sung vào [[REBASE]]:
    
    - `git rebase -i`
        
    - `pick`
        
    - `reword`
        
    - `edit`
        
    - `squash`
        
    - `fixup`
        
    - Sắp xếp và gộp commit trước khi chia sẻ lịch sử.
        
    

> Lưu ý: Không tùy tiện rebase các commit đã được chia sẻ và người khác đang dựa vào.

### 29. Reset, revert và reflog

- [x] [[RESTORE]] *(Mục Reset, Revert, Reflog)*
    
    - `git reset`
        
    - `git revert`
        
    - `git reflog`
        
    - `HEAD~1`
        
    - Phục hồi khi di chuyển nhầm HEAD hoặc làm mất dấu một commit.
        
    

### 30. Ôn tập giai đoạn 4

- Tạo hai nhánh và merge chúng ([[GIT BRANCH]]).
    
- Tự tạo và giải quyết ít nhất hai loại conflict.
    
- Dùng stash để cất thay đổi rồi quay lại làm việc ([[GIT STASH]]).
    
- Dùng cherry-pick đưa một commit sang nhánh khác ([[GIT CHERRY-PICK]]).
    
- Dùng interactive rebase để gộp các commit thử nghiệm ([[REBASE]]).
    
- Phân biệt trường hợp dùng merge ([[GIT BRANCH]]), rebase ([[REBASE]]), reset và revert ([[RESTORE]]).
    

---

## Giai đoạn 5: Cộng tác trên GitHub

**Mục tiêu:** Hiểu quy trình làm việc nhóm thay vì chỉ push code lên repository cá nhân.

### 31. Pull Request và code review

- [ ] [[GITHUB PULL REQUEST]] *(Kế hoạch tạo)*
    
    - Pull Request (PR) là gì.
        
    - Tạo branch cho một tính năng hoặc bản sửa lỗi.
        
    - Push branch lên GitHub.
        
    - Tạo PR và mô tả thay đổi.
        
    - Review code, nhận góp ý và cập nhật PR.
        
    - Merge PR.
        
    

### 32. Fork và upstream

- [ ] [[GITHUB FORK UPSTREAM]] *(Kế hoạch tạo)*
    
    - Fork repository.
        
    - Phân biệt `origin` và `upstream`.
        
    - Đồng bộ fork với repository gốc.
        
    - Đóng góp thay đổi vào repository của người khác.
        
    

### 33. Quy trình làm việc nhóm

- [ ] [[GIT TEAM WORKFLOW]] *(Kế hoạch tạo)*
    
    - Issue → branch → commit → push → PR → review → merge.
        
    - Đặt tên branch.
        
    - Viết commit message.
        
    - Đồng bộ nhánh trước khi tạo PR.
        
    - Xử lý conflict trong quá trình review.
        
    

### 34. Ôn tập giai đoạn 5

- Tạo một repository thử nghiệm.
    
- Tạo branch và thực hiện một thay đổi.
    
- Tạo Pull Request ([[GITHUB PULL REQUEST]]).
    
- Thử cập nhật PR sau khi nhận góp ý.
    
- Thực hành fork và đồng bộ với upstream ([[GITHUB FORK UPSTREAM]]).
    

---

## Giai đoạn 6: Tags, tìm kiếm và điều tra lịch sử

**Mục tiêu:** Quản lý phiên bản phát hành và truy tìm nguyên nhân của lỗi.

### 35. Tags và releases

- [ ] [[GIT TAGS RELEASES]] *(Kế hoạch tạo)*
    
    - `git tag`
        
    - Tạo lightweight tag và annotated tag.
        
    - `git show <tag>`
        
    - `git push origin <tag>`
        
    - Xóa tag local và remote.
        
    - Phân biệt branch và tag.
        
    - GitHub Releases và Semantic Versioning.
        
    

### 36. Truy tìm thay đổi theo dòng code

- [ ] [[GIT BLAME GREP]] *(Kế hoạch tạo)*
    
    - `git blame <file>`
        
    - `git grep <pattern>`
        
    - Tìm commit và tác giả liên quan đến một đoạn code.
        
    - Tìm chuỗi hoặc tên hàm trong repository.
        
    

### 37. Tìm commit gây lỗi

- [ ] [[GIT BISECT]] *(Kế hoạch tạo)*
    
    - `git bisect start`
        
    - `git bisect bad`
        
    - `git bisect good`
        
    - `git bisect reset`
        
    - Dùng tìm kiếm nhị phân trên lịch sử commit để xác định commit gây lỗi.
        
    

### 38. Ôn tập giai đoạn 6

- Tạo tag cho một phiên bản thử nghiệm ([[GIT TAGS RELEASES]]).
    
- Dùng `git blame` để kiểm tra lịch sử một đoạn code ([[GIT BLAME GREP]]).
    
- Dùng `git grep` để tìm hàm hoặc chuỗi ([[GIT BLAME GREP]]).
    
- Tạo một lỗi có chủ đích rồi dùng `git bisect` tìm commit gây lỗi ([[GIT BISECT]]).
    

---

## Giai đoạn 7: Git nâng cao

**Mục tiêu:** Hiểu sâu hơn về cách Git lưu trữ dữ liệu và các công cụ phục vụ workflow phức tạp.

### 39. Cấu trúc bên trong Git

- [ ] [[GIT INTERNALS]] *(Kế hoạch tạo)*
    
    - Blob, tree, commit và tag object.
        
    - HEAD, index và refs.
        
    - `git ls-files`
        
    - `git cat-file`
        
    - Quan hệ giữa file, staging area và commit.
        
    

### 40. Worktree

- [ ] [[GIT WORKTREE]] *(Kế hoạch tạo)*
    
    - `git worktree list`
        
    - `git worktree add`
        
    - `git worktree remove`
        
    - Làm việc trên nhiều branch bằng các thư mục làm việc riêng.
        
    

### 41. Submodule

- [ ] [[GIT SUBMODULE]] *(Kế hoạch tạo)*
    
    - `git submodule add`
        
    - `git submodule update --init --recursive`
        
    - Cập nhật và quản lý repository lồng nhau.
        
    - Phân biệt submodule với việc sao chép thư mục code thông thường.
        
    

### 42. Hooks và bare repository

- [ ] [[GIT HOOKS SERVER]] *(Kế hoạch tạo)*
    
    - Git hooks và thời điểm chúng chạy.
        
    - Các hook phổ biến như `pre-commit` và `pre-push`.
        
    - Bare repository.
        
    - Vai trò của Git server trong workflow tự quản lý.
        
    

### 43. Ôn tập giai đoạn 7

- Dùng `git cat-file` xem thông tin một object ([[GIT INTERNALS]]).
    
- Tạo hai worktree từ một repository ([[GIT WORKTREE]]).
    
- Thử thêm submodule vào project thử nghiệm ([[GIT SUBMODULE]]).
    
- Tạo một hook đơn giản để kiểm tra điều kiện trước khi commit ([[GIT HOOKS SERVER]]).
    

---

## Giai đoạn 8: Dự án tổng hợp

**Mục tiêu:** Dùng Git trong một quy trình gần với công việc thực tế.

### Project 1: Quản lý project cá nhân

- Tạo repository cho một project C hoặc Python ([[GIT INIT]]).
    
- Cấu hình `.gitignore` ([[gitignore]]).
    
- Tạo commit theo từng tính năng ([[GIT ADD]], [[GIT COMMIT]]).
    
- Sử dụng branch cho tính năng mới ([[GIT BRANCH]]).
    
- Tạo tag cho phiên bản đầu tiên ([[GIT TAGS RELEASES]]).
    

### Project 2: Mô phỏng làm việc nhóm

- Tạo repository remote ([[REMOTE]]).
    
- Mô phỏng hai nhánh phát triển song song ([[GIT BRANCH]]).
    
- Tạo merge conflict và giải quyết ([[GIT BRANCH]], [[REBASE]]).
    
- Thử stash, cherry-pick và rebase ([[GIT STASH]], [[GIT CHERRY-PICK]], [[REBASE]]).
    
- Tạo Pull Request và hoàn thành quy trình merge ([[GITHUB PULL REQUEST]], [[GIT TEAM WORKFLOW]]).
    

### Project 3: Điều tra một lỗi trong lịch sử

- Tạo nhiều commit, trong đó một commit gây lỗi.
    
- Dùng `git log`, `git show` và `git blame` ([[GIT LOG]], [[GIT SHOW]], [[GIT BLAME GREP]]).
    
- Dùng `git bisect` xác định commit gây lỗi ([[GIT BISECT]]).
    
- Viết báo cáo ngắn giải thích nguyên nhân và cách sửa (tham khảo [[RESTORE]]).
    

---

## Thứ tự ưu tiên

|Ưu tiên|Nội dung|Bài học & File trọng tâm|
|---|---|---|
|1|Giai đoạn 1: Fundamentals|[[GIT INIT]], [[GIT STATUS]], [[GIT ADD]], [[GIT COMMIT]], [[GIT LOG]], [[GIT DIFF]]|
|2|Giai đoạn 2: Quản lý file và cấu hình|[[gitignore]], [[RESTORE]], [[GIT CONFIG]], [[GIT CLEAN]]|
|3|Giai đoạn 3: Remote và GitHub|[[REMOTE]], [[SETUP HTTPS + GITHUB CLI]], [[SSH KEY - ĐỌC THÊM]], [[GIT PULL]]|
|4|Giai đoạn 4: Branch, conflict, stash, rebase|[[GIT BRANCH]], [[GIT CHECKOUT]], [[REBASE]], [[RESTORE]]|
|5|Giai đoạn 5: Pull Request và workflow nhóm|[[GITHUB PULL REQUEST]], [[GIT TEAM WORKFLOW]]|
|6|Giai đoạn 6: Tags, blame, grep, bisect|[[GIT TAGS RELEASES]], [[GIT BLAME GREP]], [[GIT BISECT]]|
|7|Giai đoạn 7: Internals, worktree, submodule, hooks|[[GIT INTERNALS]], [[GIT WORKTREE]], [[GIT SUBMODULE]]|
|8|Giai đoạn 8: Project tổng hợp|Thực hành tích hợp toàn diện|

## Nguyên tắc học

1. Đọc file hiện có trước khi viết thêm file mới (tra cứu tại [[HỆ THỐNG NHANH]]).
    
2. Mỗi lệnh cần có cú pháp, ví dụ thực tế, giải thích kết quả và lỗi thường gặp.
    
3. Sau mỗi chủ đề, tự làm bài thực hành mà không nhìn lời giải.
    
4. Không học thuộc tất cả tham số. Học cách đọc `git help <command>` khi cần.
    
5. Ưu tiên hiểu trạng thái repository và hậu quả của lệnh trước khi chạy lệnh có khả năng ghi đè hoặc xóa dữ liệu.
    
6. Chỉ chuyển sang giai đoạn tiếp theo khi có thể tự thực hành các thao tác chính của giai đoạn hiện tại.
    

**Đích đến:** Có thể tự quản lý lịch sử code, đồng bộ GitHub, giải quyết conflict và tham gia workflow Pull Request mà không phải mò lệnh trong hoảng loạn.