## Các khái niệm cơ bản
### User vs Kernel

Kernel là 1 phần của hệ điều hành được thực hiện với đặc quyền cao hơn trong khi đó user(space) thường có nghĩa là các ứng dụng chạy với đặc quyền thấp hơn.

Đối với viêc thực thi của vi xử lí, mã nguồn chạy ở kernel mode có thể kiểm soát hoàn toàn CPU trong khai đó khi chạy ở user sẽ có 1 vài giới hạn. Ví dụ ngắt CPU có thể được bật hoặc tắt khi đang chạy ở kernel mode, nếu hành động này được thực hiện khi đang ở user mode sẽ gây ra exception và kernel được gọi để xử lí.

Kernel và user có vùng nhớ riêng, vùng nhớ của kernel thì được bảo vệ để cho các ứng dụng ở user không thể truy cập được còn vùng nhớ của user có thể truy cập trực tiếp từ mã nguồn chạy trong kernel mode.

### Cấu trúc hệ điều hành điển hình

Nhân hệ điều hành chịu trách nhiệm truy cập và chia sẻ phần cứng 1 cách an toàn và công bằng cho các ứng dụng.

![](/img/tq-kernel.png)

Kernel cung cấp 1 tập các API để các ứng dụng có thể sử dụng thường được nhắc tới với tên "System Calls". Các API này khác với các thư viện API thông thường bởi vì chúng là ranh giời giữa việc chuyển chế độ thực thi từ user sang kernel

Để cung cấp khả năng tương thích, system calls thường ít thay đổi. Linux bắt buộc điều này ( phân biệt với kernel API có thể thay đổi khi cần)

Kernel code có thể tự phân biệt logic thành core kernel và device driver code. Device chịu trách nhiệm truy cập 1 thiết bị cụ thể nào đó trong khi core kernel chịu trách nhiệm chung. Core vì thế có thể chua thành các hệ thống logic nhỏ hơn(truy xuất file, mạng, quản lí tiến trình...)

### Monolithic kernel(nhân khối)

Trong monolithic, không có bảo vệ truy cập giữa các hệ thống con, các hàm public có thể được gọi trực tiếp giữa các hệ thống con.

![](/img/monolithic.png)

Tuy nhiên, hầu hết các monolithic kernel đều bắt buộc chia logic giữa các subsystem đặc biệt là core kernel và device drive. Việc truy cập các dịch vụ được cung cấp bỏi từng subsystem thông qua các strict API.

### Vi nhân

Trong vi nhân, các phần của nhân được bảo vệ với các phần khác, thường được chạy trong user mode. Bởi vì các phần quan trọng được chạy trong user space, phần còn lại nhỏ 1 cách đáng kể chạy trong kernel mode, được gọi là vi nhân.

![](/img/micro-k.png)

Trong kiến trúc vi nhân, phần nhân chỉ chứ đủ code để cho phép thông điệp truyền qua lại giữa các tiến trình. Trong thực tế, điều này có nghĩa là thực thi scheduler và IPC trong kernel cũng như quản lí bộ nhớ để thiết lập bảo vệ giữa các ứng dụng và dịch vụ.

1 lời gọi hàm đơn giản giữa 2 dịch vụ trong monolithic giờ cần đi qua IPC và scheduling sẽ làm giảm hiệu năng.

### Không gian địa chỉ 

- Không gian vật lí: cách RAM và các thiết bị nhớ được nhìn thấy trong memory bus. Ví dụ, trong kiến trúc Intel 32 bit, thông thường RAM được ánh xạ tới vùng nhớ vật lí thấp còn bộ nhớ của card đồ họa được ánh xạ vào vùng nhớ cao hơn.

- Không gian bộ nhớ ảo: cách CPU nhìn thấy bộ nhớ khi mô đun bộ nhớ ảo được kích hoạt. Kernel chịu trách nhiệm cho thiết lập 1 ánh từ bộ nhớ ảo vào trong vùng nhớ vật lí.

Liên quan tới vùng nhớ ảo có 2 thuật ngữ thường được dùng: process space và kernel space.

Process space là 1 phần của bộ nhớ ảo có liên kết với 1 process. Là 1 vùng nhớ liên tục bắt đầu từ 0. 

Kernel sapce là "memory view" của code chạy ở kernel mode.

### Chia sẻ không gian địa chỉ ảo giữa user và kernel

Trong tình huống này, kernel được cấp vùng ở trên đỉnh của không gian địa chỉ, ở dưới là user. Để ngăn không cho user truy cập vào không gian của kernel, nhân hđh tạo ra các ánh xạ để tránh truy cập tới kernel từ user space.

### Đa nhiệm

Đa nhiệm là khả năng chạy các "song song" nhiều chương trình cùng lúc. Việc này đươc thực hiện bằng việc chuyển qua lại giữa các tiến trình đang chạy.


