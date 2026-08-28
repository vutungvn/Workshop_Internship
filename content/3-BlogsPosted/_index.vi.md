---
title: "Các bài blogs đã đăng"
date: 2026-08-01
weight: 3
chapter: false
pre: " <b> 3. </b> "
---

Trong suốt 15 tuần thực tập, tôi đã xuất bản **3 bài blog kỹ thuật chuyên sâu** trên [AWS Study Group](https://www.facebook.com/groups/awsstudygroupfcj) — phân tích các tình huống thực tế trên AWS, giới thiệu các tính năng dịch vụ mới, tối ưu hóa chi phí bằng IaC và ứng dụng các dịch vụ AI để giải quyết bài toán doanh nghiệp cụ thể.

| #      | Chủ đề                                                                                | Danh mục (Theo yêu cầu FCAJ)                       | Ngày đăng  |
| ------ | ------------------------------------------------------------------------------------- | -------------------------------------------------- | ---------- |
| Blog 1 | **Tối ưu chi phí EC2 & RDS tự động bằng AWS Lambda và EventBridge**                   | Tối ưu chi phí & Tự động hóa FinOps với Serverless | 01/08/2026 |
| Blog 2 | **Tự động hóa số hóa hồ sơ y tế với Amazon Bedrock Data Automation + AWS HealthLake** | Sử dụng dịch vụ AI giải quyết bài toán ngành Y tế  | 03/05/2026 |
| Blog 3 | **Giải bài toán phân tích hợp đồng thông minh với Doczy.ai™ trên AWS**                | Tính năng mới: Textract + Bedrock + Smart Chunking | 07/06/2026 |

---

### [3.1. Blog 1](3.1-Blog1/)

Vận hành môi trường không phải sản xuất (Dev, Test, Staging) 24/7 tạo ra lãng phí chi phí đám mây vô cùng lớn cho doanh nghiệp. Bài viết này giới thiệu một giải pháp FinOps tự động kết hợp **AWS Lambda**, **Amazon EventBridge** và **Resource Tagging** để tự động tắt các máy chủ EC2 và cơ sở dữ liệu RDS nhàn rỗi ngoài giờ làm việc. Bằng cách lên lịch tự động khởi động/dừng và xác thực thẻ tài nguyên, giải pháp này giúp các đội ngũ DevOps giảm tới **60-70%** chi phí điện toán không cần thiết mà không làm ảnh hưởng đến tiến độ công việc của lập trình viên.

---

### [3.2. Blog 2](3.2-Blog2/)

Hàng triệu hồ sơ y tế bằng giấy tại các bệnh viện vẫn đang được nhập liệu thủ công với chi phí lên tới **hàng triệu USD/năm** và tỷ lệ lỗi từ 5-15%. Bài viết phân tích kiến trúc **serverless + event-driven** kết hợp **Amazon Bedrock Data Automation** (trích xuất hơn 50 trường dữ liệu lâm sàng bằng AI mà không cần dữ liệu huấn luyện) và **AWS HealthLake** (kho dữ liệu chuẩn FHIR R4 đạt chuẩn HIPAA) để chuyển đổi PDF quét thành dữ liệu y tế chuẩn hóa chỉ trong **~30 phút cho mỗi 1.000 hồ sơ** thay vì 2-3 tuần, với chi phí **<$0.50 mỗi hồ sơ** và tỷ lệ lỗi **<1%**.

---

### [3.3. Blog 3](3.3-Blog3/)

AArete đã xây dựng **Doczy.ai™** — một hệ thống phân tích hợp đồng thông minh chạy trên AWS đã xử lý **2.5 triệu hợp đồng (~50 triệu trang)** trong 22 tháng, đạt độ chính xác **99%** (so với 55% của các hệ thống dựa trên quy tắc cũ), thực hiện **137 triệu lượt gọi API Bedrock** và tiết kiệm cho khách hàng **~$330 triệu USD**. Bài viết đi sâu vào kiến trúc **Textract + Bedrock + Smart Chunking** (bằng sáng chế cốt lõi của AArete) và **phân cụm kép (dual clustering)** — một mô hình mạnh mẽ cho tài liệu pháp lý mà bất kỳ đội ngũ nào đang xây dựng hệ thống RAG đều có thể học hỏi.
