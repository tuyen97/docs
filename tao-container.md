Để chạy container thực hiện lệnh:

``` 
docker run -it -p 8000:8000 trongtuyen/simple_web_app:lastest
```

Bật trình duyệt và mở địa chỉ:  [localhost:8000](localhost:8000)

Dưới đây là cách tạo image trên: 

Tạo 1 thư mục tên là django và bỏ project vào. Tạo file Dockerfile với nội dung như sau: 

Bắt đầu với base-image là python:3.7.0-alpine3.8 với dung lượng rất nhẹ :

```
FROM python:3.7.0-alpine3.8

```
Cài đặt django:

```
RUN pip3 install Django
```

Sau đó ta copy toàn bộ mã nguồn trong thư mục ```mysite``` vào trong thư mục ```/app``` của container

```
COPY /mysite /app
```

Chuyển thư mục làm việc của container về ```/app```:

``` 
WORKDIR /app
```

Webserver trong container sẽ lắng nghe ở cổng 8000:

```
EXPOSE 8000
```

Đặt lệnh khời động server khi khởi chạy container:

```
ENTRYPOINT python3 manage.py runserver 0.0.0.0:8000
```

Lưu ý cần phải sử file ```settings.py``` trong project ở dòng:

```
ALLOWED_HOSTS = ['*']

```

Build image với lệnh:

```
docker build -t <image_name> .
```


