#1. Nhóm lệnh Kiểm tra cấu hình mạng của ubuntu
>sudo ip -4 a

>sudo ifconfig -a

#2. Nhóm lệnh hiển thị

Hiển thị các định tuyến đi qua các card mạng
>route -n

#3. Nhóm lệnh cấu hình

##3.1 Gán địa chỉ IP tức thời
Sẽ mất sau khi reboot máy
>sudo ifconfig ethX IP-address netmask subnetmask

vd: 
>sudo ifconfig eth0 192.168.1.2 netmask 255.255.255.0

##3.1 Gán địa chỉ cố định và lưu vào file cấu hình
File cấu hình có đường dẫn là: */etc/network/interfaces*
>sudo vim /etc/network/interfaces

Nội dung file:
* auto lo
* iface lo inet loopack
* auto eth0
* iface eth0 inet static
* address 192.168.1.2
* netmask 255.255.255.0
* gateway 192.168.1.1

Sau đó cần khởi động lại dịch vụ mạng. 

**Note:** hai dòng đầu tiên không nên thay đổi.
>sudo reboot

>sudo /etc/init.d/networking restart

##3.2 Nhận IP từ DHCP Server
>auto eth0

>iface eth0 inet dhcp

Sau đó cũng restart dịch vụ như trên.

##3.3 Đặt địa chỉ DNS Server 
>sudo vim /etc/resolv

Sửa nội dung file như sau:
*	nameserver 8.8.8.8
*	namewerver 8.8.4.4

Dòng trên là DNS của goole, dòng sau là DNS phụ của google. Thay thế hai dòng này bằng địc chỉ DNS riêng của bạn

##3.4 Nhóm lệnh với routing
Đặt default gateway:

>route add default gw ip-gateway

hoặc
>route add -net 0.0.0.0 mask 0.0.0.0 dev {interface-name}

Ví dụ:
* route add default gw 192.168.1.1*
* route add -nt 0.0.0.0 mask 0.0.0.0 dev eth0*

Để thêm một routing tĩnh:
>route add -net x.x.x.x mask y.y.y.y dev {interface-name}

Ví dụ:
* route add -net 192.168.5.0 mask 255.255.255.0 dev eth0

Để gỡ bỏ routing tĩnh hay một default gateway
>route delete -net 192.168.5.0 mask 255.255.255.0 dev eth0

>route delete default gateway 192.168.1.1

##3.5 Nhóm câu lệnh với card mạng
Để tạm ngưng (disable) một card mạng:
>sudo ifconfig eth0 down

>sudo ifconfig eth0 up *enable card mạng*

