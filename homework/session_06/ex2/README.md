\# Báo cáo Bài 2: Cấu hình phân quyền Nhóm và sudoers bằng visudo



\## 1. Dòng cấu hình sudoers đã thêm vào hệ thống



Dòng cấu hình được thêm qua `visudo`:

```text

%devops-admin ALL=(ALL) NOPASSWD: /usr/bin/systemctl start \*, /usr/bin/systemctl stop \*, /usr/bin/systemctl restart \*, /usr/bin/systemctl status \*



%devops-admin: Áp dụng cho tất cả thành viên thuộc nhóm devops-admin.



ALL=(ALL): Có thể thực thi trên mọi máy chủ dưới danh nghĩa mọi user.



NOPASSWD: Không yêu cầu nhập mật khẩu xác thực.



/usr/bin/systemctl ...: Giới hạn chỉ được phép thao tác các lệnh start, stop, restart, status trên các service.



2\. Nhật ký các lệnh đã thực hiện

Bash

\# 1. Tạo nhóm devops-admin và gán user deployer vào nhóm

groupadd devops-admin

usermod -aG devops-admin deployer



\# 2. Cấu hình sudoers an toàn

visudo



\# 3. Chuyển sang user deployer để kiểm tra

su - deployer

3\. Kết quả kiểm tra xác thực

Kiểm tra quyền hạn với sudo -l

Plaintext

Matching Defaults entries for deployer on lab-practice:

&#x20;   env\_reset, mail\_badpass, secure\_path=/usr/local/sbin\\:/usr/local/bin\\:/usr/sbin\\:/usr/bin\\:/sbin\\:/bin\\:/snap/bin, use\_pty



User deployer may run the following commands on lab-practice:

&#x20;   (ALL) NOPASSWD: /usr/bin/systemctl start \*, /usr/bin/systemctl stop \*, /usr/bin/systemctl restart \*, /usr/bin/systemctl status \*

Chạy thử lệnh restart service

Bash

deployer@lab-practice:\~$ sudo systemctl restart cron

deployer@lab-practice:\~$ sudo systemctl status cron

● cron.service - Regular background program processing daemon

&#x20;    Loaded: loaded (/usr/lib/systemd/system/cron.service; enabled; preset: enabled)

&#x20;    Active: active (running) since Tue 2026-10-06 11:15:00 UTC; 5s ago

...

