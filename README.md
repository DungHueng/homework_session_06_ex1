# BÀI 1: KHẢO SÁT FHS VÀ PHÂN QUYỀN FILE/FOLDER NÂNG CAO

## 1. Mục tiêu

* Tạo cấu trúc thư mục theo chuẩn FHS tại `/var/www/my-app`.
* Tạo thư mục `public` chứa nội dung web tĩnh.
* Tạo thư mục `logs` chứa nhật ký.
* Thực hiện phân quyền bằng octal với `chmod`.
* Thay đổi Owner và Group bằng `chown`.
* Kiểm tra lại quyền và quyền sở hữu bằng `ls -la`.

---

## 2. Môi trường thực hiện

* Hệ điều hành: Ubuntu Server
* Máy chủ: AZVPS
* User sử dụng: `devops`
* Group sử dụng: `www-data`

---

## 3. Tạo cấu trúc thư mục

Tạo thư mục `public` và `logs`:

```bash
mkdir -p /var/www/my-app/public
mkdir -p /var/www/my-app/logs
```

Kiểm tra:

```bash
ls -la /var/www/my-app
```

---

## 4. Tạo file HTML mẫu

Tạo file `index.html` trong thư mục `public`:

```bash
echo "<h1>My App</h1>" > /var/www/my-app/public/index.html
```

Kiểm tra:

```bash
ls -la /var/www/my-app/public
```

---

## 5. Phân quyền thư mục public

Yêu cầu:

* Owner: đọc, ghi, thực thi/truy cập.
* Group: đọc, thực thi/truy cập.
* Others: không có quyền.

Sử dụng quyền `750`:

```bash
chmod 750 /var/www/my-app/public
```

Kiểm tra:

```bash
ls -ld /var/www/my-app/public
```

Kết quả:

```text
drwxr-x--- ... public
```

Giải thích:

```text
750
7 = Owner  = rwx
5 = Group  = r-x
0 = Others = ---
```

---

## 6. Phân quyền thư mục logs

Yêu cầu:

* Owner: đọc, ghi, thực thi/truy cập.
* Group: đọc, ghi, thực thi/truy cập.
* Others: không có quyền.

Sử dụng quyền `770`:

```bash
chmod 770 /var/www/my-app/logs
```

Kiểm tra:

```bash
ls -ld /var/www/my-app/logs
```

Kết quả:

```text
drwxrwx--- ... logs
```

Giải thích:

```text
770
7 = Owner  = rwx
7 = Group  = rwx
0 = Others = ---
```

---

## 7. Thay đổi Owner và Group

Gán:

* Owner: `devops`
* Group: `www-data`

Thực hiện:

```bash
chown -R devops:www-data /var/www/my-app
```

Kiểm tra:

```bash
ls -la /var/www/my-app
```

---

## 8. Kết quả kiểm tra

Lệnh kiểm tra chính:

```bash
ls -la /var/www/my-app
```

Kết quả mong đợi:
```text
drwxr-x--- 2 devops www-data ... public
drwxrwx--- 2 devops www-data ... logs
```

Kết quả thực tế:
![Kết quả kiểm tra ls -la](./Screenshot%202026-10-06%20195708.png)

---

## 9. Kiểm tra quyền chi tiết

Thực hiện:

```bash
stat -c "%A %a %U %G %n" /var/www/my-app/public /var/www/my-app/logs
```

Kết quả mong đợi:
```text
drwxr-x--- 750 devops www-data /var/www/my-app/public
drwxrwx--- 770 devops www-data /var/www/my-app/logs
```

Kết quả thực tế:
![Kết quả kiểm tra quyền chi tiết](./Screenshot%202026-10-06%20195246.png)
