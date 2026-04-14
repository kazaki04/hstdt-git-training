### BT1: Khởi tạo & Thao tác cơ bản
1. Lên GitLab, tạo một project trống có tên `hstdt-git-training`.
2. Mở Terminal, clone project về máy: `git clone <URL_repo_của_bạn>`.
3. Di chuyển vào thư mục dự án: `cd hstdt-git-training`.
4. Tạo file `.gitignore` và copy nội dung cấu hình cho PHP vào file này.
5. Tạo thư mục `src` (`mkdir src`) và tạo file `index.php` bên trong với đoạn code: `<?php echo "Xin chào, tôi là [Tên bạn] - Học viên HSTDT";`
6. Lưu thay đổi lần 1:
   * `git add .`
   * `git commit -m "init: khởi tạo project training"`
   * `git push -u origin main`
7. Mở file `src/index.php`, thêm một đoạn code để hiển thị ngày tháng hiện tại.
8. Lưu thay đổi lần 2:
   * `git add .`
   * `git commit -m "feat: hiển thị ngày hiện tại trên trang chủ"`
   * `git push origin main`

### BT2: Merge Remote (Cập nhật code từ gốc)
1. Lên GitLab, vào repo `hstdt-upstream-demo` của Mentor và bấm nút **Fork** để tạo một bản sao về tài khoản của bạn.
2. Clone bản fork đó về máy: `git clone <URL_repo_fork_của_bạn>`.
3. Thêm link repo của Mentor làm gốc (upstream): `git remote add upstream <URL_repo_của_mentor>`. Kiểm tra lại bằng lệnh `git remote -v`.
4. Tạo nhánh mới để làm việc: `git checkout -b feature/add-info`.
5. Tạo file `info.md`, ghi thông tin cá nhân của bạn vào đó.
6. Commit và đẩy lên kho của bạn:
   * `git add info.md`
   * `git commit -m "feat: thêm thông tin cá nhân"`
   * `git push origin feature/add-info`
7. Chờ Mentor báo đã thêm commit mới, tiến hành cập nhật:
   * `git fetch upstream`
   * `git checkout main`
   * `git merge upstream/main`
8. Đẩy bản cập nhật mới nhất lên repo fork của bạn: `git push origin main`.

### BT3: Merge Request & Conflict (Làm theo cặp A & B)
**Phần 1: Tạo nhánh và sửa chung 1 dòng**
1. A và B cùng clone repo chung do Mentor tạo.
2. A tạo nhánh `feature/header-a`. B tạo nhánh `feature/header-b`.
3. Cả A và B cùng mở file `header.php` và cùng sửa tại đúng một dòng code (nhưng nội dung sửa khác nhau).
4. Cả A và B chạy chuỗi lệnh `add`, `commit`, và `push` nhánh của mình lên GitLab.
5. Cả hai lên GitLab tạo Merge Request (MR) trỏ vào nhánh `main`. Vào MR của đối phương để lại comment review có tiền tố `[must]` hoặc `[nit]`.

**Phần 2: Xử lý xung đột**
6. Mentor (hoặc Team) tiến hành Merge MR của A vào `main` trước. Lúc này MR của B sẽ bị báo Conflict (Xung đột).
7. B quay lại máy của mình, chuyển sang nhánh của B và lấy code mới nhất từ main về để xử lý:
   * `git checkout feature/header-b`
   * `git pull origin main` (hoặc `git fetch` rồi `git merge origin/main`)
8. B mở file `header.php` lên, xóa các dòng có dấu `<<<<<<<`, `=======`, `>>>>>>>` và tự gõ/chỉnh lại dòng code sao cho hợp lý nhất.
9. B lưu lại file đã sửa, sau đó:
   * `git add .`
   * `git commit -m "merge: giải quyết conflict header.php"`
   * `git push origin feature/header-b`
10. Trên GitLab, MR của B đã xanh (hết conflict) và có thể bấm Merge.

### BT4: Diff, Cherry-pick, Reset, Tag
**Phần 1: Cherry-pick**
1. Tạo nhánh mới: `git checkout -b develop`.
2. Thực hiện 3 lần sửa file và commit riêng biệt:
   * Commit 1: `feat: thêm function tính tổng`
   * Commit 2: `feat: thêm function tính trung bình`
   * Commit 3: `fix: sửa lỗi chia cho 0`
3. Xem lại lịch sử và copy **mã hash** (7 chữ số) của commit số 3 (fix lỗi): `git log develop --oneline`.
4. Về nhánh chính: `git checkout main`.
5. Bốc riêng commit sửa lỗi sang nhánh chính: `git cherry-pick <mã_hash_đã_copy>`. Kiểm tra lại bằng `git log --oneline`.

**Phần 2: Reset (Thử nghiệm trên nhánh develop)**
6. Quay lại nhánh develop: `git checkout develop`.
7. Gõ `git reset --soft HEAD~1` (kiểm tra `git status` sẽ thấy file code vẫn còn ở trạng thái màu xanh/Staging).
8. Commit lại file đó. Sau đó gõ `git reset --mixed HEAD~1` (kiểm tra `git status` sẽ thấy file code màu đỏ/Working Directory).
9. Commit lại lần nữa. Gõ `git reset --hard HEAD~1` (kiểm tra file code sẽ thấy đoạn code vừa viết đã bốc hơi hoàn toàn).

**Phần 3: Dọn dẹp và gắn Tag**
10. Trở về nhánh chính: `git checkout main`.
11. Gắn phiên bản: `git tag -a v1.0.0 -m "Hoàn thành bài tập Git training"`.
12. Đẩy Tag lên GitLab: `git push origin v1.0.0`.
13. Xóa nhánh `develop` ở máy tính: `git branch -d develop`.
14. Xóa nhánh `develop` trên GitLab: `git push origin --delete develop`.

---

## 3. Câu hỏi vấn đáp (Q&A)

### Phần 1: Lý thuyết lõi (Hiểu bản chất)

**Câu 1: `git fetch` khác `git pull` như thế nào?**
* **`git fetch`**: Chỉ tải dữ liệu mới từ kho gốc (Remote) về máy (Local) để xem trước, **không** làm thay đổi code hiện tại của bạn.
* **`git pull`**: Là sự kết hợp của `git fetch` + `git merge`. Nó tải dữ liệu mới về và **trộn ngay lập tức** vào nhánh đang đứng. Dễ sinh ra xung đột nếu không kiểm tra trước.

**Câu 2: Phân biệt 3 mức độ của `git reset` (`--soft`, `--mixed`, `--hard`)?**
* **`--soft`**: Lùi lại commit, **giữ nguyên code** và giữ các file đang ở vùng chờ (Staging Area). Dùng khi muốn gom/sửa lại commit message.
* **`--mixed`** (Mặc định): Lùi lại commit, **giữ nguyên code** nhưng đẩy các file ra khỏi vùng chờ. Dùng khi muốn chọn lại từng file để commit.
* **`--hard`**: Lùi lại commit và **XÓA SẠCH** mọi code đã viết sau mốc đó. Chỉ dùng khi muốn làm lại từ đầu.

**Câu 3: Tại sao không nên commit trực tiếp vào nhánh `main`?**
* `main` là nhánh chứa code ổn định để đưa lên môi trường thật. 
* Commit trực tiếp sẽ bỏ qua quy trình Review Code, dễ vô tình đưa lỗi làm sập hệ thống. Luôn phải rẽ branch -> code -> tạo Merge Request -> Review -> Merge.

**Câu 4: Lệnh `git cherry-pick` dùng trong trường hợp nào?**
* Khi chỉ muốn **nhặt đúng 1 commit** cụ thể từ nhánh khác mang về nhánh hiện tại, thay vì phải merge toàn bộ cả một nhánh. Thường dùng để nhặt các đoạn code sửa lỗi khẩn cấp.

**Câu 5: Tại sao lệnh push `--force` (ép đẩy) lại nguy hiểm?**
* Lệnh này sẽ **ghi đè/xóa bỏ** lịch sử commit trên server bằng lịch sử trên máy cá nhân. Nếu dùng trên nhánh chung (như `main`, `develop`), có thể xóa sạch code của người khác. Chỉ dùng trên nhánh cá nhân.

### Phần 2: 

**Câu 6: Bạn vừa commit xong thì phát hiện mình gõ sai lỗi chính tả trong "commit message". Bạn sửa thế nào?**
* **Cách giải quyết:** Gõ lệnh `git commit --amend -m "Message mới"`. Nếu lỡ push lên nhánh cá nhân trên server rồi thì cần ép đẩy lại: `git push --force origin <tên-nhánh>`.

**Câu 7: Bạn đang code dở tính năng A thì sếp yêu cầu chuyển nhánh gấp để sửa một lỗi nghiêm trọng. Code dở chưa xong nên bạn không muốn commit. Làm sao đây?**
* **Cách giải quyết:** Dùng lệnh `git stash` để tạm thời lưu code dang dở vào kho. Sau đó chuyển nhánh đi fix lỗi. Sử dụng `git stash pop` để lấy ra code đang viết dở ra làm tiếp.

**Câu 8: Bạn lỡ tay gõ `git reset --hard` và mất sạch code quan trọng. Có cứu được không?**
* **Cách giải quyết:** Có. Dùng lệnh `git reflog` để xem lại toàn bộ lịch sử thao tác. Tìm mã băm của thời điểm trước khi thực hiện thao tác, sau đó gõ `git reset --hard <mã-hash>` để quay lại lúc trước khi thực hiện thao tác.

**Câu 9: Trong team có 5 người, ai cũng thích đẩy code thẳng lên `main` gây loạn. Bạn sẽ đề xuất quy trình nào?**
* **Cách giải quyết:** Bật tính năng **Protected Branch** trên GitLab cho nhánh `main` để cấm push trực tiếp. Yêu cầu mọi người rẽ nhánh riêng `feature/tên-tính-năng`. Xong việc phải tạo Merge Request, cần ít nhất 1 người khác vào đọc code và Approve (duyệt) thì mới được vào `main`.

**Câu 10: Code đã được merge vào `main` nhưng ngay sau đó phát hiện có lỗi nghiêm trọng làm sập web. Phải gỡ gấp, bạn làm thế nào?**
* **Cách giải quyết:** Không dùng `reset --hard` vì sẽ làm hỏng lịch sử chung của cả team. Dùng `git revert <mã-hash-của-commit-merge>`. Lệnh này sẽ tạo ra một commit *mới* có tác dụng xóa toàn bộ code của commit bị lỗi.
