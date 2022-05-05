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
     - ISP - Nhà cung cấp dịch vụ Internet của bạn, công ty bạn trả tiền để có Internet tại nhà của bạn.
     - Router - Bộ định tuyến cho phép mỗi máy trong mạng của bạn kết nối với Internet. Trong hầu hết các bộ định tuyến hiện đại, bạn có thể kết nối qua không dây hoặc cáp Ethernet.
     - WAN - Mạng diện rộng, đây là cái mà chúng tôi gọi là mạng bao gồm mọi thứ giữa bộ định tuyến của bạn và một mạng rộng hơn, chẳng hạn như Internet.
     - WLAN - Mạng cục bộ không dây, đây là mạng giữa bộ định tuyến của bạn và bất kỳ thiết bị không dây nào bạn có, chẳng hạn như máy tính xách tay.
     - LAN - Mạng cục bộ, đây là mạng giữa bộ định tuyến của bạn và bất kỳ thiết bị có dây nào như Máy tính để bàn.
     - Hosts - Mỗi máy trên mạng được gọi là máy chủ lưu trữ.
   - Dữ liệu và thông tin được truyền qua các mạng được gọi là các gói và ở phần cuối của phần Networking Nomad.     
 
 - OSI Model
   - Mô hình OSI (Open Systems Interconnection) là một mô hình lý thuyết về mạng. 
   - Mô hình này cho chúng ta thấy cách một gói tin truyền qua một mạng ở bảy lớp khác nhau. 
   - Đóng một phần lớn trong TCP/IP mà chúng ta sử dụng ngày nay.  
 
 - TCP/IP Model
   - Mô hình OSI đã khai sinh ra mô hình cuối cùng trở thành mô hình TCP/IP và mô hình này thực sự là nền tảng của Internet. 
   - Đó là việc triển khai thực tế của mạng. Mô hình TCP/IP sử dụng bộ giao thức TCP/IP, mà chúng ta thường gọi là TCP/IP. 
   - Các giao thức này làm việc cùng nhau để chỉ định cách dữ liệu nên được thu thập, định địa chỉ, truyền và định tuyến thông qua một mạng. Sử dụng mô hình TCP/IP, chúng ta có thể thấy cách các giao thức này được sử dụng để chỉ ra sự phân tích về cách một gói tin truyền qua mạng.
   - Các lớp trong mô hình:
     - Lớp ứng dụng:
       - Lớp trên cùng của mô hình TCP/IP. 
       - Nó xác định cách các chương trình máy tính của bạn (chẳng hạn như trình duyệt web của bạn) giao diện với các dịch vụ lớp truyền tải để xem dữ liệu được gửi hoặc nhận.
       - Lớp này sử dụng:
         - HTTP (Giao thức truyền siêu văn bản) - được sử dụng cho các trang web trên Internet.
         - SMTP (Giao thức truyền thư đơn giản) - truyền thư điện tử (email)
     - Lớp vận chuyển:
       - Dữ liệu sẽ được truyền như thế nào, bao gồm việc kiểm tra các cổng chính xác, tính toàn vẹn của dữ liệu và về cơ bản việc phân phối các gói.
       - Lớp này sử dụng:
         - TCP (Giao thức điều khiển truyền) - cung cấp dữ liệu đáng tin cậy
         - UDP (Giao thức dữ liệu người dùng) - cung cấp dữ liệu không đáng tin cậy
     - Lớp mạng:
       - Lớp này chỉ định cách di chuyển các gói giữa các máy chủ và giữa các mạng.
       - Lớp này sử dụng:
         - IP (Giao thức Internet) - Giúp định tuyến các gói từ máy này sang máy khác.
         - ICMP (Internet Control Message Protocol) - Giúp cho chúng tôi biết điều gì đang xảy ra, chẳng hạn như thông báo lỗi và thông tin gỡ lỗi.
     - Lớp liên kết:
       - Lớp này chỉ định cách gửi dữ liệu qua một phần cứng vật lý. Chẳng hạn như dữ liệu truyền qua Ethernet, cáp quang, v.v.                  
     
 - Network Addressing
   - Các gói cần thông tin giống nhau, máy chủ và các máy khác được xác định bằng địa chỉ MAC (điều khiển truy cập phương tiện) và địa chỉ IP, để giúp con người chúng ta dễ dàng sử dụng tên máy để xác định máy chủ hơn. 
   - Địa chỉ MAC
     - Địa chỉ MAC là một định danh duy nhất được sử dụng làm địa chỉ phần cứng. 
     - Địa chỉ này sẽ không bao giờ thay đổi. Khi muốn truy cập Internet, máy cần phải có một thiết bị gọi là card giao diện mạng. 
     - Bộ điều hợp mạng này có địa chỉ phần cứng riêng được sử dụng để xác định máy của bạn. Địa chỉ MAC cho thiết bị Ethernet trông giống như sau 00: C4: B5: 45: B2: 43. 
     - Địa chỉ MAC được cấp cho bộ điều hợp mạng khi chúng được sản xuất. Mỗi nhà sản xuất có một mã định danh duy nhất về mặt tổ chức (OUI) để xác định họ là nhà sản xuất. OUI này được biểu thị bằng 3 byte đầu tiên của địa chỉ MAC. 
   - Các địa chỉ IP
     - Địa chỉ IP được sử dụng để xác định một thiết bị trên mạng, chúng độc lập về phần cứng và có thể khác nhau về cú pháp tùy thuộc vào việc đang sử dụng IPv4 hay IPv6. 
     - Địa chỉ IP điển hình sẽ giống như sau: 10.24.12.4. Địa chỉ IP được sử dụng với phần mềm của mạng. Bất cứ khi nào một hệ thống được kết nối với Internet, nó phải có một địa chỉ IP. Chúng cũng có thể thay đổi nếu mạng thay đổi và là duy nhất cho toàn bộ Internet.
   - Tên máy chủ 
     - Một cách cuối cùng để xác định máy là thông qua tên máy chủ. Tên máy chủ lưu trữ địa chỉ IP và cho phép gắn địa chỉ đó với một tên có thể đọc được của con người. Thay vì nhớ 192.12.41.4, thì có thể nhớ myhost.com.  
 
 - Application Layer
   - Đầu tiên, chúng ta bắt đầu trong lớp ứng dụng. 
   - Khi gửi email thông qua ứng dụng email, lớp ứng dụng sẽ đóng gói dữ liệu này. 
   - Lớp ứng dụng nói chuyện với lớp truyền tải thông qua một cổng được chỉ định và thông qua cổng này, nó sẽ gửi dữ liệu của nó. 
   - Chúng ta muốn gửi một email thông qua giao thức lớp ứng dụng SMTP (giao thức truyền thư đơn giản). 
   - Dữ liệu được gửi thông qua giao thức truyền tải, điều này sẽ mở ra một kết nối đến cổng này (cổng 25 được sử dụng cho SMTP), vì vậy chúng ta nhận được dữ liệu này được gửi qua cổng này và dữ liệu đó được gửi đến lớp Truyền tải để được đóng gói thành các phân đoạn.  
 
 - Transport Layer
   - Lớp truyền tải giúp chúng ta truyền dữ liệu của mình theo cách mà mạng có thể đọc được. 
   - Nó chia nhỏ dữ liệu của chúng ta thành nhiều phần sẽ được vận chuyển và tập hợp lại với nhau theo đúng thứ tự. 
   - Những phần này được gọi là phân đoạn. Phân đoạn giúp truyền dữ liệu qua mạng dễ dàng hơn.
   - Ports:
     - Các dịch vụ như HTTP sử dụng một kênh giao tiếp thông qua các cổng. Nếu chúng ta muốn gửi dữ liệu trang web, chúng ta cần gửi nó qua cổng HTTP (cổng 80). 
     - Ngoài việc hình thành các phân đoạn, lớp truyền tải cũng sẽ gắn các cổng nguồn và cổng đích vào phân đoạn, vì vậy khi người nhận nhận được gói tin cuối cùng, nó sẽ biết cổng nào sẽ sử dụng.  
   - UDP:
     - UDP không phải là một phương pháp vận chuyển dữ liệu đáng tin cậy, trên thực tế, nó không thực sự quan tâm đến việc có lấy được tất cả dữ liệu gốc của mình hay không. những công dụng của nó, chẳng hạn như để phát trực tuyến phương tiện, sẽ không sao nếu bạn mất một số khung hình, đổi lại bạn nhận được dữ liệu của mình nhanh hơn một chút   
   - TCP:  
 - Network Layer
 - Link Layver
 - DHCP Overview

     
