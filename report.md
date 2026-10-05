# Báo cáo Session 04 - Bài 2: Quản lý nhánh & Giải quyết xung đột (Merge Conflict)

- **Đường dẫn nộp:** `homework/session_04/ex2/`

---

## 1. Mục tiêu
- Tạo và chuyển đổi giữa các nhánh cục bộ.
- Cố ý tạo xung đột gộp nhánh và xử lý thủ công các ký hiệu `<<<<<<<`, `=======`, `>>>>>>>`.
- Hoàn tất merge commit (gộp 3 vùng - 3-Way Merge).

---

## 2. Các bước thực hiện (chạy trong repo trên máy cá nhân)

### 2.1. Khởi tạo repo + commit nền trên main
```bash
git init
echo "# My Project" > README.md
git add README.md && git commit -m "docs: initial README"
```

### 2.2. Tạo nhánh feature-update và chỉnh sửa cùng 1 dòng
```bash
git checkout -b feature-update
echo "# My Project - Feature Update (Beta)" > README.md
git add README.md && git commit -m "feat: update README on feature branch"
```

### 2.3. Quay lại main và sửa KHÁC trên cùng dòng đó (tạo mầm xung đột)
```bash
git checkout main
echo "# My Project - Stable Release v1.0" > README.md
git add README.md && git commit -m "docs: update README on main"
```

### 2.4. Gộp feature-update vào main → phát sinh xung đột
```bash
git merge feature-update
```
Git báo:
```
Auto-merging README.md
CONFLICT (content): Merge conflict in README.md
Automatic merge failed; fix conflicts and then commit the result.
```

### 2.5. Giải quyết xung đột THỦ CÔNG
Mở `README.md`, sẽ thấy các ký hiệu đánh dấu:
```
<<<<<<< HEAD
# My Project - Stable Release v1.0
=======
# My Project - Feature Update (Beta)
>>>>>>> feature-update
```
Xóa các ký hiệu `<<<<<<<`, `=======`, `>>>>>>>` và chọn nội dung cuối cùng.
Ở đây **giữ cả hai** bằng cách gộp lại thành:
```
# My Project - Stable Release v1.0 + Feature Update (Beta)
```

### 2.6. Hoàn tất merge commit
```bash
git add README.md
git commit -m "merge: resolve conflict between main and feature-update"
```

---

## 3. Kiểm tra (Verification)
```bash
git log --graph --oneline
```
**Kết quả mong đợi** (dán output / chụp ảnh để nộp):
```
*   e1a2b3c (HEAD -> main) merge: resolve conflict between main and feature-update
|\
| * 4d5e6f7 (feature-update) feat: update README on feature branch
* | 7g8h9i0 docs: update README on main
|/
* 1a2b3c4 docs: initial README
```
→ Đồ thị cho thấy nhánh `feature-update` tách ra từ `main`, rồi gộp lại bằng **merge commit có 2 nhánh tổ tiên** (chỗ `|\` và `|/`).

---

## 4. Giải thích 3-Way Merge
Git so sánh 3 phiên bản: tổ tiên chung (`initial README`), đầu nhánh `main`, và đầu nhánh `feature-update`. Khi cùng một dòng bị sửa khác nhau ở 2 nhánh, Git không tự quyết được nên báo xung đột để người dùng xử lý thủ công. Merge commit tạo ra sau đó có **2 commit cha** (từ cả 2 nhánh).

---
