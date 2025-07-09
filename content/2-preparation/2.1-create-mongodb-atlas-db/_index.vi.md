---
title : "Tạo Database MongoDB Atlas cho Development"
date: 2025-07-07
weight : 1
chapter : false
pre : " <b> 2.1 </b> "
---

# Tạo Database MongoDB Atlas

Hướng dẫn từng bước để tạo một database mới trên **MongoDB Atlas** cho ứng dụng MERN stack của bạn.

---

### 🧭 Các bước thực hiện:

1. Đăng nhập vào [MongoDB Atlas](https://www.mongodb.com/cloud/atlas).  

{{< figure src="./../../images/2-preparation/020-mongoDB.png" title="Đăng nhập vào MongoDB Atlas Dashboard" >}}

2. Tạo một **Project mới** (nếu chưa có).

{{< figure src="./../../images/2-preparation/021-mongoDB.png" title="Đăng nhập vào MongoDB Atlas Dashboard" >}}

- Đặt tên project: (ví dụ: `Xray`)

{{< figure src="./../../images/2-preparation/022-mongoDB.png" title="Đăng nhập vào MongoDB Atlas Dashboard" >}}

- Nhấp **Next** và **Create project**
{{< figure src="./../../images/2-preparation/023-mongoDB.png" title="Đăng nhập vào MongoDB Atlas Dashboard" >}}
3. Tạo một **Cluster mới**


{{< figure src="./../../images/2-preparation/024-mongoDB.png" title="Nhấp Build a Database trong MongoDB Atlas" >}}

- Chọn **Cloud Provider**, **Cluster Tier/Template** và **Region** ưa thích của bạn

{{< figure src="./../../images/2-preparation/025-mongoDB.png" title="Chọn Cloud Provider, Template và Region" >}}

- Sau khi chọn các tùy chọn, nhấp **Create Deployment** để bắt đầu cung cấp cluster

{{< figure src="./../../images/2-preparation/026-mongoDB.png" title="Tạo MongoDB Cluster Deployment" >}}

4. Thiết lập **Network Access** và **Tạo Database User**

Sau khi cluster được triển khai, MongoDB Atlas sẽ hướng dẫn bạn qua quá trình thiết lập kết nối.

---

- **Bước 1: Thêm IP address và Tạo user**

IP address hiện tại của bạn sẽ được tự động thêm vào whitelist.  
Một database user mặc định cũng được đề xuất.



---

- **Bước 2: Đặt Username và Password**

Bạn có thể sử dụng credentials được tạo tự động hoặc tạo riêng.  
Nhấp **Copy** (1) để lưu password, sau đó nhấp **Create Database User** (2).
{{< figure src="./../../images/2-preparation/027-mongoDB.png" title="Thêm IP address và chuẩn bị tạo database user" >}}


---

- **Bước 3: Chọn phương thức kết nối**

Sau khi user được tạo, nhấp **Choose a connection method**.
{{< figure src="./../../images/2-preparation/028-mongoDB.png" title="Tạo MongoDB database user và copy credentials" >}}


---

- **Bước 4: Chọn 'Drivers' và chọn Node.js**

Trong màn hình tiếp theo, chọn **Drivers**, sau đó chọn **Node.js** làm driver và version.
{{< figure src="./../../images/2-preparation/029-mongoDB.png" title="Chọn phương thức kết nối cho MongoDB Atlas" >}}


---

💡 Connection string hiển thị ở đây sẽ chứa username và password của bạn.  
Nhấp nút **Copy** và lưu string — bạn sẽ sử dụng nó trong ứng dụng backend.

{{% notice note %}} 
Đây là **lần duy nhất password sẽ hiển thị**, vì vậy hãy lưu trữ an toàn hoặc ngay lập tức thêm vào AWS Secrets Manager.
{{% /notice %}}
{{< figure src="./../../images/2-preparation/030-mongoDB.png" title="Lấy connection string với Node.js driver" >}}

---

Sau khi hoàn thành, bạn sẽ nhận được **MongoDB connection URI**, sẽ được sử dụng bởi backend Node.js (thường được lưu trong Secrets Manager).

## 🔐 Quản lý Database Access (Username & Password)

Để cập nhật hoặc reset username/password MongoDB sau khi thiết lập ban đầu:

1. Trong menu bên trái của [MongoDB Atlas Console](https://www.mongodb.com/cloud/atlas), nhấp **Database Access** dưới **Security**.

2. Bạn sẽ thấy danh sách các user hiện có.  
   Nhấp nút **Edit** bên cạnh user bạn muốn chỉnh sửa.

{{< figure src="./../../images/2-preparation/031-mongoDB.png" title="Chỉnh sửa MongoDB database user" >}}

3. Trong modal:
   - Để **thay đổi password**, nhập password mới
   - Để **thay đổi roles**, chọn các mức truy cập khác nhau (ví dụ: `readWrite`, `atlasAdmin`)
   - Để **xóa user**, nhấp **Delete**

4. Nhấp **Update User** để lưu thay đổi.

{{% notice warning %}}
Đừng quên cập nhật connection string hoặc secret trong AWS Secrets Manager nếu password đã thay đổi.
{{% /notice %}}

---

## 🌐 Quản lý Network Access (IP Whitelist)

Để thay đổi IP nào có thể truy cập MongoDB cluster:

1. Trong menu bên trái, nhấp **Network Access** dưới **Security**.

2. Bạn sẽ thấy **IP Access List**.

{{< figure src="./../../images/2-preparation/032-mongoDB.png" title="Quản lý IP Whitelist MongoDB Atlas" >}}

3. Để **thêm IP mới**:
   - Nhấp **Add IP Address**
   - Bạn có thể chọn:
     - **Current IP** (tự động phát hiện)
     - Một **IP cụ thể** (ví dụ: `203.0.113.4`)
     - Hoặc Allow Access from Anywhere (`0.0.0.0/0`) để cho phép tất cả (không khuyến nghị cho production)

4. Để **xóa hoặc chỉnh sửa** IP hiện có, nhấp options (ba chấm `...`) bên cạnh entry.

{{% notice note %}}
Thay đổi IP có hiệu lực trong vài giây, không cần restart cluster.
{{% /notice %}}

---

Bây giờ bạn đã biết cách quản lý truy cập và bảo mật cho môi trường MongoDB Atlas. 