# Bài 2: Khởi tạo User thường và thiết lập đặc quyền quản trị (Sudoers Configuration)

- **Học viên:** Đỗ Gia Hưng
- **Mục tiêu:** Áp dụng nguyên tắc đặc quyền tối thiểu (Least Privilege), tạo user `devops`, cấp quyền `sudo`, sao chép khóa SSH và kiểm tra thực thi đặc quyền.

---

## 1. Các lệnh Linux đã thực hiện

### Bước 1: Tạo user và cấp quyền nhóm sudo
Đăng nhập qua tài khoản `root` và khởi tạo người dùng:
```bash
# Tạo user mới với thư mục home
adduser devops

# Cấp đặc quyền quản trị bằng cách thêm vào group sudo
usermod -aG sudo devops
Bước 2: Đồng bộ SSH Keypair và phân quyền
Sử dụng công cụ rsync để đồng bộ thư mục .ssh và cập nhật quyền sở hữu cho devops:

Bash
# Copy thư mục .ssh và gán ownership devops:devops
rsync --archive --chown=devops:devops ~/.ssh /home/devops/

# Đảm bảo phân quyền bảo mật OpenSSH
chmod 700 /home/devops/.ssh
chmod 600 /home/devops/.ssh/authorized_keys
2. Kiểm tra và xác thực
Log Terminal kết nối và xác minh sudo
Chạy từ Terminal máy cá nhân:

Bash
$ ssh -i ~/.ssh/id_ed25519 devops@<IP_ADDRESS_DROPLET>
Welcome to Ubuntu 22.04.4 LTS (GNU/Linux 5.15.0-xx-generic x86_64)

devops@devops-lab-ubuntu:~$ id
uid=1001(devops) gid=1001(devops) groups=1001(devops),27(sudo)

devops@devops-lab-ubuntu:~$ sudo whoami
[sudo] password for devops: 
root