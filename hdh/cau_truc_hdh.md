## Monolithic

Toàn bộ hệ điều hành chạy như 1 chương trình duy nhất ở chế độ kernel. Hệ điều hành được tổ chức thành 1 tập các thủ tục, liên kết với nhau tạo thành 1 file thực thi nhị phân duy nhất. Mỗi thủ tục trong hệ thống đều có thể thoải mái gọi tới các thủ tục khác. Việc có thể gọi tới bất kì thủ tục nào mà ta muốn là rất hiệu quả nhưng việc có hàng nghìn các thủ tục gọi lẫn nhau mà không có giới hạn nào có thể làm cho hệ thông khó sử dụng và khó hiểu.

Process ở user mode gọi các lời gọi hệ thống ở kernel mode. Việc chuyển chế độ thực thi từ user-kernel thông qua hàm TRAP.

## Microkernel

Chỉ giữ lại các phần quan trọng của hệ điều hành chạy ở kernel mode, các phần còn lại như driver thiết bị chạy như các tiến trình ở user mode. Việc 1 modul bị crash chỉ ảnh hưởng đến các modul khác mà phần nhân hệ điều hành không bị ảnh hưởng tạo độ tin cậy cao.


