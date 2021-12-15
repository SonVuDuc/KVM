# Kernel-based Virtual Machine

## ***Mục lục***

[1. Ảo hóa KVM/QEMU](#1)

- [1.1. Giới thiệu về ảo hoá](#1.1)

- [1.2. KVM](#1.2)

- [1.3. QEMU](#1.3)

- [1.4. Ảo hoá KVM kết hợp QEMU](#1.4)


[2. Libvirt](#2)

[3. Cài đặt KVM](#3)

[4. Quản lý KVM](#4)

- [4.1. Công cụ Virt Manager](#4.1)

- [4.2. Tạo máy ảo](#4.2)


[5. Network và Storage](#5)

- [5.1. Virtual Network](#5.1)

- [5.2. Storage](#5.2)

[6. Templates và Snapshots](#6)

- [6.1. Templates](#6.1)

- [6.2. Snapshots](#6.2)

  - [6.2.1. Internal snapshots](#6.2.1)
  
  - [6.2.2. External snapshots](#6.2.2)   

---

<a name = "1"></a>
# 1. Ảo hóa KVM/QEMU

<a name = "1.1"></a>
## 1.1. Giới thiệu về ảo hoá

Theo định nghĩa trong CNTT, thì ảo hoá là công nghệ được thiết kế để tạo ra tầng trung gian giữa hệ thống phần cứng (hardware) và phần mềm (OS). 

Ý tưởng của công nghệ này là từ một máy chủ vật lý có thể tạo ra nhiều máy ảo riêng biệt. Máy chủ vật lý dùng để chạy phần mềm ảo hoá được gọi là Host, còn các máy ảo được gọi là guests. 

Mỗi máy ảo đều hoạt động độc lập với những máy ảo khác. Chúng nó OS riêng, ứng dụng riêng, storage riêng.

![image](https://user-images.githubusercontent.com/32956424/142969846-3c956d72-dbd6-4b2a-8d04-d356e3e4b4f9.png)


### Các thành phần của một hệ thống ảo hoá

- **Tài nguyên phần cứng**: CPU, RAM, ổ cứng lưu trữ... ở trên máy chủ vật lý. Cung cấp tài nguyên cho các máy ảo.
- **Phần mềm ảo hoá**: còn được gọi là Hypervisor. Là nền tảng của môi trường ảo hóa, cho phép tạo ra các máy ảo, quản lý và cung cấp tài nguyên đến các máy ảo.
- **Máy ảo**: Virtual Machine, được tạo ra từ Hypervisor và phân bổ tài nguyên phần cứng. Có vai trò như một máy vật lý thông thường.
- **Hệ điều hành**: Hệ điều hành, ứng dụng được cài đặt trên máy ảo.

![image](https://user-images.githubusercontent.com/32956424/142970616-8123ce81-f7ca-4348-972f-0bdba5fb51c9.png)

### Phân loại Hypervisor

![image](https://user-images.githubusercontent.com/32956424/142970797-6a92407d-6865-4be3-9496-96fc881e11ed.png)

- **Type 1: Bare-metal**

Là loại hypervisor chạy trực tiếp trên phần cứng máy chủ vật lý như một OS (VD: VMware Esxi,...). 

Sử dụng 100% tài nguyên hệ thống nên có hiệu năng và tính bảo mật cao. Nhưng phải bỏ tiền ra mua license.

![image](https://user-images.githubusercontent.com/32956424/142971064-9c9ab2e6-2020-4bf5-a2b3-f2caae9b85a3.png)


- **Type 2: Host-based**

Là loại hypervisor được cài đặt trên hệ điều hành như một ứng dụng bình thường (VD: VMware Workstation, Oracle VirtualBox,...)

Ăn tranh tài nguyên hệ thống với hệ điều hành và các ứng dụng khác, hiệu năng không cao nhưng đa số là hàng FREE.

![image](https://user-images.githubusercontent.com/32956424/142971119-dfc60da4-99c5-4de4-a69b-aa603554427e.png)

### Lợi ích khi sử dụng ảo hoá

- Tiết kiệm, giảm chi phí duy trì server
- Giảm thiểu số lượng thiết bị vật lý 
- Quản lý tập trung
- Khả năng mở rộng dễ dàng  


<a name = "1.2"></a>
## 1.2. KVM

**KVM** - **Kernel-based Virtual Machine**: máy ảo dựa trên nhân, là một module ảo hoá nằm trong nhân Linux, cho phép nhân Linux thực hiện các chức năng như một hypervisor

### Tính năng của KVM

- Security – Bảo mật:
- Memory management – Quản lý bộ nhớ:
- Storage – Lưu trữ:
- Live migration:

<a name = "1.3"></a>
## 1.3. QEMU


<a name = "1.4"></a>
## 1.4. Ảo hoá KVM kết hợp QEMU

<a name = "2"></a>
# 2. Libvirt

Để thuận tiện cho việc quản lý các máy ảo và các chức năng ảo hoá, thư viện **libvirt** đã được phát triển. 

Libvirt là một công cụ quản lý các nền tảng ảo hoá. Cung cấp API cho cho nhiều hypervisor như KVM, XEN, ESXi,... Đồng thời được sủ dụng bởi nhiều công nghệ cloud như Openstack, oVirt,...

Mục đích của libvirt là cung cấp 1 phương pháp duy nhất để quản lý ảo hoá từ nhiều loại hypervisor khác nhau.

![image](https://user-images.githubusercontent.com/32956424/144171957-0573732d-99b6-49e6-808b-1205851e9db8.png)

## Các chức năng chính của Libvirt

- **VM management**: cho phép quản lý máy ảo, như start, stop, pause, save, restore, migrate,... 
- **Remote machine support**: Hỗ trợ kết nối từ xa, có thể dùng SSH để truy cập vào nhiều Host chạy daemon libvirt. Nếu máy remote host từ xa đang chạy libvirty và SSH được cho phép, câu lệnh sau sẽ cung cấp khả năng truy cập virsh cho tất cả máy ảo KVM/QEMU trên remote host đó.

``` virsh --connect qemu+ssh://root@example.com/system```

- **Storage management**: quản lý, lưu trữ image máy ảo với nhiều định dạng: qcow2, img,...  Cho phép liệt kê LVM, tạo LVM mới.
- **Network interface management**: quản lý các interface network logic và vật lý. Liệt kê, cấu hình các interface logic, bridge, VLAN,...
- **Virtual NAT and Route based networking**: tạo và quản lý các mạng ảo.


Tóm lại là: QEMU là mức thấp nhất mô phỏng bộ xử lý và thiết bị ngoại vi. KVM là tăng tốc nếu CPU được bật VT. Libvirt cung cấp trình nền và ứng dụng khách để thao tác với VM cho thuận tiện.


<a name = "3"></a>
# 3. Cài đặt KVM

Để cài được KVM thì cần phải được CPU hỗ trợ, kiểm tra xem CPU có hỗ trợ hay không bằng cách sử dụng lệnh:

``` egrep -c "svm|vmx" /proc/cpuinfo ```

Nếu kết quả trả về lớn hơn 0 thì tức là CPU có hỗ trợ

![image](https://user-images.githubusercontent.com/32956424/144169623-9d8d8f74-898d-4e2d-861f-770d394aba55.png)

Chạy lệnh sau để cài đặt KVM và các package liên quan:

```  yum install qemu-kvm libvirt bridge-utils virt-install virt-manager virt-install -y ```

Sau khi quá trình cài đặt hoàn tất, kiểm tra KVM đã được cài đặt chưa bằng lệnh:

``` lsmod | grep kvm ``` 

![image](https://user-images.githubusercontent.com/32956424/144170292-14e6bea8-0616-4f4d-88a4-06b27526af0d.png)

Khởi động service libvirtd:

``` systemctl enable libvirtd && systemctl start libvirtd ``` 

Kiểm tra service libvirtd

``` systemctl status libvirtd ```

![image](https://user-images.githubusercontent.com/32956424/144170588-ea40a593-f79a-43d6-af1e-f265392b6c7d.png)


<a name = "4"></a>
# 4. Quản lý KVM

<a name = "4.1"></a>
## 4.1. Công cụ Virt Manager

Virt Manager là một ứng dụng giao diện đồ hoạ dùng để quản lý các máy ảo thông qua API của libvirt.

Virt Manager có thể được dùng để quản lý các máy ảo KVM. Cho phép hiển thị thông tin, cấu hình máy ảo, quản lý các mạng ảo thông qua giao diện GUI.

Có thể dùng Virt Manager để quản lý máy ảo từ xa trên các máy khác.

Chọn **File** > **Add Connection**

![image](https://user-images.githubusercontent.com/32956424/144355684-35c0bf07-3f3c-4d8e-be7e-f16c4c65ed9b.png)

Nhập thông tin của remote host,

![image](https://user-images.githubusercontent.com/32956424/144355780-c4cc3585-2935-4f52-9927-fff6bba63bdf.png)

Kết nối remote host thành công

![image](https://user-images.githubusercontent.com/32956424/144355829-f3397bf5-6684-45f8-8621-fb27f75f1b0c.png)



<a name = "4.2"></a>
## 4.2. Tạo máy ảo

Tại giao diện chính của Virt Manager, chọn Create a new Virtual Machine.

![image](https://user-images.githubusercontent.com/32956424/144356143-936cb819-058d-4599-ab74-b9d022e96081.png)

Chọn **Local install media** rồi chọn **Forward**.

![image](https://user-images.githubusercontent.com/32956424/144356228-cdb4b6c1-23c6-403a-9f73-068717705ad9.png)

Chọn **Browse** để cài đặt bằng file ISO.

![image](https://user-images.githubusercontent.com/32956424/144356359-cbc65d84-8265-4d20-b7aa-12e796746b76.png)

Chọn đường dẫn đến file ISO, rồi chọn **Forward**.

![image](https://user-images.githubusercontent.com/32956424/144356490-e57fb963-a99d-4264-8766-42ef63b38805.png)

![image](https://user-images.githubusercontent.com/32956424/144356611-00ef312d-5a93-49f3-a37e-8bf18a17db06.png)

Tuỳ chỉnh thông số RAM và CPU cho máy ảo.

![image](https://user-images.githubusercontent.com/32956424/144356667-d1729dad-d0a3-4500-a483-5f4f67285e3b.png)

Chọn Storage để lưu trữ image của máy ảo.

![image](https://user-images.githubusercontent.com/32956424/144357122-e79535ef-2d70-48f2-860c-478dc27ad173.png)

- **Create a disk image for virtual machine** - mặc định sẽ được lưu ở thư mục /lib/libvirt/images/<Tên VM>.qcow2
- **Select or create a custom storage** - cho phép tạo hoặc chỉ định storage lưu trữ image

Đặt tên cho VM. Ngoài ra, tuỳ chọn **Network selection** cho phép người dùng cấu hình loại mạng ảo cho VM.

![image](https://user-images.githubusercontent.com/32956424/144372638-6596f86f-0b5e-47a5-a6d3-af6f32edb662.png)

Cuối cùng chọn **Finish** để toàn tất 

![image](https://user-images.githubusercontent.com/32956424/144357718-85bd1a99-43b7-4507-9ace-d61a50a2f460.png)

Máy ảo đã được tạo và boot bằng file ISO đã chọn trước đó. Tiến hành cài đặt OS như bình thường.

![image](https://user-images.githubusercontent.com/32956424/144357777-cdca87ad-80f9-4d70-8fae-d893c23055be.png)

Trong KVM, mỗi VM được tạo nên bởi 2 thành phần chính là VM defination được lưu ở dưới dạng  file XML và VM storage được lưu ở dạng virtual disk.

Mặc định các file XML của VM được lưu ở  **```/etc/libvirt/qemu```**.

File XML sẽ chứa những thông tin về các thành phần của VM, như số CPU, RAM, các thiết lập I/O, OS,...

![image](https://user-images.githubusercontent.com/32956424/145966579-3b32829b-6812-4f2d-983b-36698d6f01a7.png)

Từ file XML trên, libvirt sử dụng những thông tin đó để tiến hành khởi chạy tiến trình QEMU-KVM để tạo máy ảo.


<a name = "5"></a>
# 5 . Network và Storage

Bên cạnh những file XML chứa thông tin của VM, KVM cũng có những file XML khác để lưu những thông tin liên quan đến Network và Storage.

<a name = "5.1"></a>
## 5.1. Virtual Network

Thành phần chính của Network trong KVM là **bridge**. Có thể hiểu đơn giản nó là một switch ảo (virtual switch), với chức năng và hoạt động như một switch layer 3 thông thường.

Các máy ảo (guest) sẽ kết nối vào virtual switch đó. Virtual switch có nhiều mode khác nhau, tuỳ thuộc theo mục đích sử dụng.

![image](https://user-images.githubusercontent.com/32956424/144544999-6f78a1b2-69f9-413f-88bd-817d5cac0bbd.png)


### Isolated Virtual Network

Khi sử dụng Isolated Mode, các máy ảo (guest) có thể tương tác với nhau nếu được kết nối vào cùng virtual switch. Nhưng không thể kết nối ra mạng ngoài, không thể nhận traffic từ bên ngoài HOST vật lý.

![image](https://user-images.githubusercontent.com/32956424/144536469-d482b0c2-9f15-44dc-a2fc-33e36feed6b8.png)

### Routed Virtual Network

Trong Routed Mode, virtual switch sẽ kết nối với mạng LAN vật lý của HOST, đồng thời đóng vai trò chuyển tiếp traffic cho các máy guest mà không cần sử dụng NAT.

Tất cả các máy guest có cùng subnet đều được định tuyến qua virtual switch. Tuy nhiên, các máy HOST vật lý khác không hề biết đến sự tồn tại của subnet này, cũng như không thể định tuyến tới nó. Do đó cần phải cấu hình định tuyến tĩnh trên router vật lý mới có thể kết nối được

![image](https://user-images.githubusercontent.com/32956424/144536490-5425863c-0cd5-4ca3-918d-3af5e9e3ad35.png)

### NATed Virtual Network

Mặc định, virtual switch sẽ hoạt động ở chế độ NAT mode.

Các máy guest sẽ sử dụng IP của máy HOST để kết nối với mạng ngoài. Máy ngoài HOST không thể khởi tạo kết nối tới những máy guest này.

![image](https://user-images.githubusercontent.com/32956424/144536502-492855cb-fabb-4133-89ac-78ae5ee568d4.png)

<a name = "5.2"></a>
## 5.2. Storage

Trong KVM, khi tạo ra các máy ảo, cần phải có một khu vực để lưu trữ virtual disk của chúng. Được gọi là Storage pool.

Storage pool là nơi lưu trữ các virtual disk của máy ảo, có thể tạo nhiều storage pool tuỳ theo mục đích sử dụng. Nơi lưu trữ mặc định là thư mục **``` /var/lib/libvirt/images ```**.

KVM cung cấp đa dạng các storage type khác nhau để tạo storage pool cho việc lưu trữ máy ảo:

![image](https://user-images.githubusercontent.com/32956424/145926022-7dbfb53b-ea7d-4779-a2c9-ded1fb1e7f2f.png)

- **dir**: Sử dụng filesystem directory để lưu trữ virtual disks
- **disk**: Sử dụng ổ đĩa vật lý để tạo ra virtual disk
- **fs**: Chỉ định một phân vùng của ổ đĩa rồi dùng nó như một directory (không cần mount trước)
- **gluster**: Cho phép sử dụng gluster filesystem
- **iscsi**: Dùng network-shared iSCSI, volume phải được cấp phát trước trên iSCSI server. Không thể tạo qua Libvirt API
- **logical**: Sử dụng LVM để lưu virtual disk, đồng thời hỗ trợ tính năng tạo LVM từ disk.
- **mpath**: Pool này sử dụng tất cả multipath từ thiết bị khác kết nối tới host. Do đó mỗi host chỉ có thể cấu hình duy nhất một Multipath pool
- **netfs**: Dùng network-shared filesystem như NFS, yêu cầu tên host và path của exported directory
- **rbd**:  Sử dụng Ceph storage
- **scsi**: Dùng local SCSI 
- **sheepdog**: Pool được tạo từ Sheepdog cluster
- **zfs**: Dùng ZFS filesystem, tính năng này chỉ có trên UNIX

Trong đó, những storage type thường được dùng nhất là: **File System Directory**, **LVM**, **NFS storage** và **iSCSI**

Storage volume là đơn vị lưu trữ dữ liệu ở trong storage pool, có thể hiểu chúng là virtual disk của máy ảo. Khi cần thêm disk lưu trữ cho VM, cần phải tạo storage volume cho nó.

![image](https://user-images.githubusercontent.com/32956424/145962347-9223226d-8696-4122-ab6b-15f4c04b138b.png)


<a name = "6"></a>
# 6. Templates và Snapshots

<a name = "6.1"></a>
## 6.1. Templates

Template là một image OS của VM chưa được cấu hình hoàn chỉnh, được dùng để deloy các VM một cách nhanh chóng.

Sử dụng template sẽ tiết kiệm được thời gian, bỏ qua những thao tác install và cấu hình ban đầu. 

VD về việc không sử dụng template, khi cần nhiều VM chạy dịch vụ, sẽ phải tạo ngần ấy VM, khai báo hardware và cấu hình từng VM.

![image](https://user-images.githubusercontent.com/32956424/144776990-f0a08bd7-bd98-453f-9fa5-b97470c02637.png)

VD về việc sử dụng template, chỉ cần tạo 1 VM template, khi cần VM có cấu hình tương tự, chỉ cần deploy từ template 

![image](https://user-images.githubusercontent.com/32956424/144777431-cc904b84-889f-4821-948f-73994ed92894.png)

Thông thường khi muốn tạo 1 VM dựa trên một VM khác, chúng ta hay sử dụng chức năng **Clone**. Tính năng clone không hẳn là tạo hay deploy template.

Thay vào đó, tính năng **``` virt-sysprep ```** mới thực sự là tạo template. Clone đơn giản chỉ là một phiên bản copy của VM, còn template mới thực sự dùng cho việc tạo ra nhiều clone. 

Template được tạo ra bằng cách convert một VM thành template, quá trình này bao gồm 3 bước:

- Build một VM để làm template, cấu hình và cài đặt các gói phần mềm cần thiết.
- Gỡ bỏ nhưng thông số đặc thù của VM đó, VD: SSH key, địa chỉ MAC, user account,... để đảm bảo những thông tin đó không tồn tại trong template.
- Đánh dấu VM đó là template bằng cách đặt tên cho nó. Khi cần deploy VM nhanh chỉ cần clone từ nó.

Liệt kê danh sách các VM. Trước khi tạo template, VM template phải được shutdown

![image](https://user-images.githubusercontent.com/32956424/144819694-4b981ad3-a63a-4c95-8262-104a4f0f068d.png)

Remove các thông số của VM bằng lệnh ``` virt-sysprep -d Centos7-1 ```

![image](https://user-images.githubusercontent.com/32956424/144819973-c861133c-55b0-4b1c-a3ef-e6106e9dd2bc.png)

Đổi tên VM bằng cách chỉnh sửa file xml

``` virsh dumpxml Centos7-1 > /root/Template_Centos7.xml ```

``` vi /root/Template_Centos7.xml ```

Thay đổi trường **``` <name> </name> ```** trong file xml thành **``` <name> Template_Centos7 </name> ```**

![image](https://user-images.githubusercontent.com/32956424/144824905-761dcf64-1d0f-4061-9218-310126f20733.png)

Template đã được tạo.

![image](https://user-images.githubusercontent.com/32956424/144825188-c8a691e8-bc0a-4fe1-bcba-59d888059917.png)

Deploy VM nhanh chóng bằng cách clone từ VM template.

![image](https://user-images.githubusercontent.com/32956424/144825335-fc636d79-ab17-48d3-b9a9-ccae74359104.png)

Đặt tên cho VM và cấu hình hardware. Chọn **Clone**.

![image](https://user-images.githubusercontent.com/32956424/144825394-d0cb069b-1334-4801-8f5f-d083c41c3465.png)

Template phải luôn luôn trong trạng thái shutdown và không được bật. Nếu không sẽ ảnh hưởng đến những VM được clone từ nó.

<a name = "6.2"></a>
## 6.2. Snapshots

Snapshot là một image của VM tại một thời điểm nào đó, bản chất là một file lưu thông tin cấu hình của VM. 

Snapshot có thể được dùng để khôi phục trạng thái trước đó của VM trong trường hợp VM bị lỗi, hoặc trước khi thực hiện những tác động đến VM. 

Snapshot trong KVM có thể thực hiện ngay cả khi VM đang chạy.

Trong KVM, kỹ thuật snapshot được chia làm 2 loại:

- Internal snapshot
- External snapshot

<a name = "6.2.1"></a>
### Internal snapshot 

Thông tin dữ liệu của các phiên bản snapshot được lưu trong một file qcow2 duy nhất - là file qcow2 của VM luôn, dễ dàng trong việc quản lý. 

Virt-Manager cũng cung cấp giao diện với internal snapshot, dễ thao tác và sử dụng.

![Screenshot (15)](https://user-images.githubusercontent.com/32956424/145975993-a23e4af0-de55-4115-9eb2-e7c16de33ba4.png)

Tuy nhiên nhược điểm là: khi tiến hành snapshot, VM sẽ bị paused, đồng nghĩa với việc không thể truy cập bất cứ dịch vụ gì. Internal snapshot cũng không hoat động ở LVM storage pool. Và nó chỉ support duy nhất định dạng file qcow2.

<a name = "6.2.2"></a>
### External snapshot

Nguyên lý hoạt động của External snapshot khác với Internal snapshot. Thay vì lưu trữ toàn bộ thông tin phiên bản snapshot trong một file qcow2 duy nhất, External snapshot sẽ tạo ra **overlay image** mỗi khi tiến hành snapshot.

Dựa trên cơ chế **copy-on-write**. File image gốc sẽ đưa vào trạng thái **read-only**, những file **read-only** được gọi là **backing file**.

![image](https://user-images.githubusercontent.com/32956424/145980717-c4412be6-51f5-4b64-b315-3c6721b8ed3f.png)

Mỗi khi tiến hành snapshot, sẽ tạo ra một file overlay image mới và trở đến file image hiện tại, đồng thời file image hiện tại sẽ chuyển sang trạng thái read-only. Tạo thành một chuỗi gọi là **backing chain**.

![image](https://user-images.githubusercontent.com/32956424/145981629-ebfce69b-a16c-4c9c-a31a-32e49c7cb732.png)


Trong quá trình snapshot, VM không bị paused, đồng thời có tốc độ nhanh hơn.

Khác với Internal snapshot chỉ hỗ trợ định dạng qcow2, External snapshot hỗ trợ nhiều định dạng file.

Tuy nhiên nhược điểm là External snapshot không có giao diện người dùng, việc revert lại cần phải thực hiện khá phức tạp do KVM hiện tại chưa hỗ trợ đầy đủ. Không thể live restore mà cần phải shut down VM mới restore được.

Khi những file overlay image quá nhiều, cần phải xoá bớt để giải phóng dung lượng. Tuy nhiên các file overlay image không thể bị xoá một cách trực tiếp. Mà chúng phải được merge với file base image rồi mới có thể xoá được.

Có 2 phương thức để merge data file image:

- **Blockcommit**: merge data từ overlay image với backing file image, tốc độ nhanh vì file overlay image thường nhỏ hơn base image.
- **Blockpull**: merge data theo hướng từ backing file image đến current image. File current image sẽ không còn phụ thuộc vào base image (bản thân nó sẽ trở thành base image mới) và chứa toàn bộ data từ base image. File sẽ luôn là qcow2.

Sau khi merge data xong, các overlay image đã trở nên vô dụng và có thể xoá chúng đi.

### Lưu ý
- Bản chất snapshot không phải là giải pháp backup cho VM, nó chỉ là trạng thái của VM ở một thời điểm nhất định, cho phép VM restore về thời điểm đó.
- Không nên giữ những file snapshot quá lâu, chúng sẽ làm tốn dung lượng lưu trữ và ảnh hưởng đến hiệu suất của VM, có thể gây ra lỗi cho VM.
- Nên sử dụng External snapshot, tỉ lệ lỗi sẽ ít hơn so với Internal snapshot (hãng recommend thế).


























