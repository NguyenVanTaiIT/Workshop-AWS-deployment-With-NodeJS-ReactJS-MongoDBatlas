---
title : "Trace Requests từ Frontend đến Database"
date: 2025-07-07
weight : 6
chapter : false
pre : " <b> 6. </b> "
---

Trong phần này, bạn sẽ thực hiện **end-to-end tracing** của user actions từ React frontend, qua backend, xuống MongoDB, tất cả hiển thị trong **AWS X-Ray**.

### 🧪 Thử điều này

Mở frontend của bạn (hosted trên S3 hoặc Beanstalk)

Đăng ký tài khoản mới  
{{< figure src="../../images/6-end-to-end-tracing/001-EndToEndTracing.png" title="Đăng ký tài khoản user mới" >}}

Đăng nhập sử dụng credentials vừa tạo  
{{< figure src="../../images/6-end-to-end-tracing/002-EndToEndTracing.png" title="Đăng nhập vào tài khoản" >}}

Duyệt đến một sản phẩm  
{{< figure src="../../images/6-end-to-end-tracing/003-EndToEndTracing.png" title="Xem chi tiết sản phẩm" >}}

Nhấp "Add to cart", sau đó tiến hành checkout  
{{< figure src="../../images/6-end-to-end-tracing/004-EndToEndTracing.png" title="Thêm sản phẩm vào giỏ hàng và tiến hành checkout" >}}

Đặt hàng  
{{< figure src="../../images/6-end-to-end-tracing/005-EndToEndTracing.png" title="Đặt hàng" >}}

{{< figure src="../../images/6-end-to-end-tracing/006-EndToEndTracing.png" title="Trace xuất hiện trong AWS X-Ray sau khi đặt hàng" >}}

### 🔍 Bây giờ kiểm tra AWS X-Ray:

- Truy cập CloudWatch Console → Trace Map
- Bạn sẽ thấy:
  - Frontend traced request → Backend service (`EcommerceApp`)

{{< figure src="../../images/6-end-to-end-tracing/007-EndToEndTracing.png" title="Trace xuất hiện trong AWS X-Ray sau khi đặt hàng" >}}


{{< figure src="../../images/6-end-to-end-tracing/008-EndToEndTracing.png" title="Trace xuất hiện trong AWS X-Ray sau khi đặt hàng" >}}

- Backend → MongoDB operation

{{< figure src="../../images/6-end-to-end-tracing/009-EndToEndTracing.png" title="Trace xuất hiện trong AWS X-Ray sau khi đặt hàng" >}}

- Nhấp vào bất kỳ trace nào để xem Segment details.

### ✅ Điều này chứng minh:
- Frontend gửi trace headers (`X-Amzn-Trace-Id`)
- Backend capture và log segments
- Database activity cũng được ghi lại
