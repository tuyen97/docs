### Định danh

Định danh theo địa chỉ MAC: Device được định danh theo địa chỉ MAC của board ESP 8266, Gateway được định danh theo địa MAC của board Intel Galielo.

### Định dạng message và MQTT topic

- Khi 1 device mới được kết nối với gateway, nó phải đăng kí topic ```esp/dang_kí``` với QoS=2 và  chỉ định LWT là:

```
lastWillTopic: "esp/disconnect"
lastWillQoS: 2
lastWillMessage: <MAC Addr> + "disconnected"
lastWillRetain: false
```

 và sau đó publish 1 thông điệp với định dạng:

```
{
"MAC":,
"type":,
"location":,
"sensors":[
        {"name":"DHT-11", "unit":"C"},
        {"name":"DHT-11", "unit":"%"}
]
}
```

- Sau khi đã đăng kí và publish lên ```esp/dang_ki``` thì device có thể đăng kí và publish lên topic ```esp/data``` với định dạng payload:

```
{
"MAC":,
"data":[
     {"temp":,}
     {"humi":,}
]
}
```
- Khi device kết nối tới gateway(Intel galileo), gateway lưu lại thông tin và trạng thái của device này vào csdl, khi device mất kết nối will message được phát đi, gateway ghi lại trạng thái là offline cho device trong csdl của nó.

- Dữ liệu về nhiệt độ và độ ẩm được kiểm tra hợp lệ ( ví dụ: >200, hoặc NaN...) sẽ được gắn thêm định danh của gateway(địa chỉ MAC) và timestamp để chuyển lên server.

-  Trên Socket.io server mở 3 event, ```new_gw``` để gateway mới có thể đăng kí với server, ```new_device``` để gateway gửi thông tin device mới kết nối tới server, ```data``` để nhận dữ liệu nhiệt độ độ ẩm từ gateway.

- Trạng thái online/offline của gateway được socket.io tự nhận biết, sẽ được log vào csdl. Khi gateway mất kết nối thì toàn bộ các device cũng được coi là offline. 

- CSDL tại server cần lưu thông tin về gateway, device trên gateway, trạng thái của device và gateway, dữ liệu nhiệt độ độ ẩm theo thời gian.
 
