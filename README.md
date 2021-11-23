# Kernel-based Virtual Machine

## ***Mục lục***

[1. Ảo hóa KVM/QEMU](#1)

- [1.1. Giới thiệu về ảo hoá](#1.1)

- [1.2. Ảo hóa KVM](#1.2)

- [1.3. Các tính năng của ảo hóa KVM](#1.3)

- [1.4. Kiến trúc KVM kết hợp QEMU](#1.4)

[2. Cài đặt KVM](#2)

- [2.1. Kiểm tra cấu hình hệ thống và cài đặt](#2.1)

- [2.2. Cấu hình](#2.2)

[3. Quản lý KVM](#3)

- [3.1. Công cụ Virt-Manager](#3.1)

- [3.2. Các chức năng chính](#3.2)


[4. Network và Storage](#3)

- [3.1. Công cụ Virt-Manager](#4.1)

- [3.2. Các chức năng chính](#4.2)


[5. Network và Storage](#5)

[6. Libvirt](#6)

---

<a name = "1"></a>
# 1. Ảo hóa KVM/QEMU

<a name = "1.1"></a>
## 1.1. Giới thiệu về ảo hoá

Theo định nghĩa trong CNTT, thì ảo hoá là công nghệ được thiết kế để tạo ra tầng trung gian giữa hệ thống phần cứng (hardware) và phần mềm (OS). 

![image](https://user-images.githubusercontent.com/32956424/142969846-3c956d72-dbd6-4b2a-8d04-d356e3e4b4f9.png)

Ý tưởng của công nghệ này là từ một máy chủ vật lý có thể tạo ra nhiều máy ảo riêng biệt. Máy chủ vật lý dùng để chạy phần mềm ảo hoá được gọi là Host, còn các máy ảo được gọi là guests. 

Mỗi máy ảo đều hoạt động độc lập với những máy ảo khác. Chúng nó OS riêng, ứng dụng riêng, storage riêng.

Các thành phần của một hệ thống ảo hoá bao gồm:

- Tài nguyên phần cứng: CPU, RAM, ổ cứng lưu trữ... ở trên máy chủ vật lý. Cung cấp tài nguyên cho các máy ảo.
- Phần mềm ảo hoá: còn được gọi là Hypervisor. Lền tảng của môi trường ảo hóa, cho phép tạo ra các máy ảo, quản lý và cung cấp tài nguyên đến các máy ảo.
- Máy ảo: Virtual Machine, được tạo ra từ Hypervisor và phân bổ tài nguyên phần cứng. Có vai trò như một máy vật lý thông thường.
- Hệ điều hành: Hệ điều hành, ứng dụng được cài đặt trên máy ảo.

![image](https://user-images.githubusercontent.com/32956424/142970501-e244c38b-6085-4f8e-b5fa-06cb2ff96fd7.png)









