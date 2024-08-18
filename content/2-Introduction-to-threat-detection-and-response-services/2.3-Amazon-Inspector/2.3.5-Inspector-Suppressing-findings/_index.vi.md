---
title : "Inspector - Ẩn phát hiện"
date : "`r Sys.Date()`"
weight : 5
chapter : false
pre : " <b> 2.3.5 </b> "
---

#### Cấu hình một rule để ẩn các phát hiện có mức độ nguy hiểm thấp trong môi trường dev



1. Nhấp vào **Suppression rules** từ thanh điều hướng bên trái trong Inspector.


2. Nhấp vào **Create Rule**.


3. Nhập "**Low Severity - Dev Environment**" vào trường **Name**.



4. Nhập "**Suppress all findings with a low severity score for resources tagged with Development**" vào trường **Description**.


5. Trong phần **Suppression rule filters** chọn **Severity** và đánh dấu vào ô **Low**. Nhấp vào **Apply**.



6. Sau đó, nhấp để thêm một bộ lọc khác và chọn **Resource tag** dưới EC2. Nhập "**Environment**" cho key và "**Development**" cho **value**. Nhấp vào **Apply**.


7. Nhấp vào **Save**.


8. Bạn có thể xem các phát hiện đã bị ẩn trong trang **All Findings** bằng cách chuyển từ **Active** sang **Suppressed** trong menu thả xuống.