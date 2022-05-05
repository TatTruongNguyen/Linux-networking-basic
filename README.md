# Linux-networking-basic
Linux networking basic

## 1. Network Sharing
 - File Sharing Overview
   - Truyền dữ liệu là cách chia sẻ tệp khi muốn chia sẻ với các máy trong cùng một mạng
   - Lệnh ``` scp ``` là công cụ chia sẻ tệp, viết tắt của sao chép an toàn và hoạt động như lệnh ``` cp ``` nhưng cho phép sao chép từ máy chủ này sang máy chủ khác trên cùng một mạng.
   - Hoạt động thông qua ssh nên tất cả các hành động đang sử dụng xác thực và bảo mật như ssh
   - Sao chép tệp từ máy chủ cục bộ sang máy chủ từ xa: 
     ```
     $ scp myfile.txt username@remotehost.com:/remote/directory
     ```
   - Sao chép tệp từ máy chủ từ xa sang máy chủ cục bộ:
     ```
     $ scp username@remotehost.com:/remote/directory/myfile.txt /local/directory
     ```
   - Sao chép qua một thư mục từ máy chủ cục bộ sang máy chủ từ xa:
     ```
     $ scp -r mydir username@remotehost.com:/remote/directory
     ```
 
 - rsync
   - Một công cụ khác được sử dụng để sao chép dữ liệu từ các máy chủ khác nhau là rsync (viết tắt của từ xa đồng bộ hóa).
   - Rsync rất giống với scp, nhưng nó có một sự khác biệt lớn. 
   - Rsync sử dụng một thuật toán đặc biệt để kiểm tra nâng cao nếu đã có dữ liệu mà bạn đang sao chép và sẽ chỉ sao chép những điểm khác biệt.
   - Rsync sẽ chỉ sao chép những phần chưa được sao chép.
   - Nó cũng xác minh tính toàn vẹn của tệp đang sao chép bằng tổng kiểm tra. 
   - Những tối ưu hóa nhỏ này cho phép khả năng truyền tệp linh hoạt hơn và làm cho rsync trở nên lý tưởng để đồng bộ hóa thư mục từ xa và cục bộ, sao lưu dữ liệu, truyền dữ liệu lớn và hơn thế nữa.
   - Một số tùy chọn rsync thường được sử dụng:
     - v - đầu ra đầy đủ
     - r - đệ quy vào thư mục
     - h - đầu ra con người có thể đọc được
     - z - nén để truyền dễ dàng hơn, thích hợp cho các kết nối chậm
   - Sao chép/ đồng bộ hóa các tệp trên cùng một máy chủ:
     ```
     $ rsync -zvr /my/local/directory/one /my/local/directory/two
     ```
   - Sao chép/ đồng bộ hóa các tệp vào máy chủ cục bộ từ máy chủ từ xa:
     ```
     $ rsync /local/directory username@remotehost.com:/remote/directory
     ```
   - Sao chép/ đồng bộ hóa tệp vào máy chủ từ xa từ máy chủ cục bộ:\
     ```
     $ rsync username@remotehost.com:/remote/directory /local/directory
     ```
 
 - Simple HTTP Server
    - Python có một công cụ siêu hữu ích để phân phát tệp qua HTTP. 
    - Điều này thật tuyệt nếu chỉ muốn tạo chia sẻ mạng nhanh chóng mà các máy khác trong mạng của có thể truy cập. 
    - Để làm điều đó, chỉ cần chuyển đến thư mục mong muốn chia sẻ và chạy:
      ```
      $ python -m SimpleHTTPServer
      ```
    - Điều này thiết lập một máy chủ web cơ bản mà ta có thể truy cập thông qua địa chỉ localhost. Vì vậy, hãy lấy địa chỉ IP của máy hiện tại đã chạy nó và sau đó trên một máy khác truy cập nó trong trình duyệt với: http://IP_ADDRESS:8000. 
    - Trên máy của riêng ta, có thể xem các tệp có sẵn bằng cách nhập: http://localhost:8000 trong trình duyệt web. 
    
  - NFS
    - Chia sẻ tệp mạng tiêu chuẩn nhất cho Linux là NFS (Hệ thống tệp mạng), NFS cho phép máy chủ chia sẻ thư mục và tệp với một hoặc nhiều máy khách qua mạng.
    - Thiết lập ứng dụng khách NFS
      ```
      $ sudo service nfsclient start
      $ sudo mount server:/directory /mount_directory
      ```
    - Tự động đếm
      - Giả sử ta sử dụng máy chủ NFS khá thường xuyên và muốn giữ nó được gắn kết vĩnh viễn, thông thường ta nghĩ rằng sẽ chỉnh sửa tệp /etc/fstab, nhưng không phải lúc nào cũng có được kết nối với máy chủ và điều đó có thể gây ra sự cố khi khởi động. 
      - Thay vào đó, những gì ta muốn làm là thiết lập tự động đếm để có thể kết nối với máy chủ NFS khi cần. 
      - Điều này được thực hiện bằng công cụ tự động hoặc trong các phiên bản gần đây của Linux amd. 
      - Khi một tệp được truy cập trong một thư mục được chỉ định, automount sẽ tra cứu máy chủ từ xa và tự động gắn kết tệp đó.
  
  - Samba 
    - Trong những ngày đầu của máy tính, các máy Windows phải chia sẻ tệp với các máy Linux, do đó giao thức Server Message Block (SMB) đã ra đời. 
    - SMB được sử dụng để chia sẻ tệp giữa các hệ điều hành Windows (Mac cũng có chia sẻ tệp với SMB) và sau đó nó đã được dọn dẹp và tối ưu hóa dưới dạng giao thức Hệ thống tệp Internet chung (CIFS). 
    - Tạo mạng chia sẻ với Samba:
      - Cài đặt Samba: 
        ```
        $ sudo apt update
        $ sudo apt install samba
        ```
      - Cài đặt smb.conf
        ```
        $ sudo vi /etc/samba/smb.conf
        ```
      - Cài đặt mật khẩu cho Samba
        ```
        $ sudo smbpasswd -a [username]
        ```
      - Tạo một thư mục chia sẻ
        ```
        $ mkdir /my/directory/to/share
        ```
      - Khởi động lại dịch vụ Samba
        ```
        $ sudo service smbd restart
        ```
      - Truy cập chia sẻ Samba qua Windows
        ```
        Trong Windows, chỉ cần nhập kết nối mạng trong lời nhắc chạy: \\HOST\sharename.
        ```
      - Truy cập chia sẻ Samba/Windows qua Linux
        ```
        $ smbclient //HOST/directory -U user
        ```
      - Đính kèm một chia sẻ Samba vào hệ thống
        ```
        $ sudo mount -t cifs servername:directory mountpount -o user=username,pass=password
        ```
  
## 2. Network Basics  
 - Network Basics
   - Một mạng gia đình sẽ có các thành phần khác nhau:
     -   
 - OSI Model
 - TCP/IP Model
 - Network Addressing
 - Application Layer
 - Transport Layer
 - Network Layer
 - Link Layver
 - DHCP Overview

     
