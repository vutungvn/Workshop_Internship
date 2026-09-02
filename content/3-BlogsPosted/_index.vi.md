---
title: "Các bài blogs đã đăng"
date: 2026-08-01
weight: 3
chapter: false
pre: " <b> 3. </b> "
---

Trong suốt 15 tuần thực tập, tôi đã xuất bản **3 bài blog kỹ thuật chuyên sâu** trên [AWS Study Group](https://www.facebook.com/groups/awsstudygroupfcj) — phân tích các tình huống thực tế trên AWS, giới thiệu các tính năng dịch vụ mới, tối ưu hóa chi phí bằng IaC và ứng dụng các dịch vụ AI để giải quyết bài toán doanh nghiệp cụ thể.

| #      | Chủ đề                                                                           | Danh mục (Theo yêu cầu FCAJ)                       | Ngày đăng  |
| ------ | -------------------------------------------------------------------------------- | -------------------------------------------------- | ---------- |
| Blog 1 | **Tối ưu chi phí EC2 & RDS tự động bằng AWS Lambda và EventBridge**              | Tối ưu chi phí & Tự động hóa FinOps với Serverless | 01/08/2026 |
| Blog 2 | **Triển khai AWS Verified Access trong kiến trúc bảo mật TIC 3.0 không cần VPN** | Mạng nâng cao & Bảo mật Zero-Trust Access          | 15/08/2026 |
| Blog 3 | **Giải bài toán phân tích hợp đồng thông minh với Doczy.ai™ trên AWS**           | Tính năng mới: Textract + Bedrock + Smart Chunking | 07/06/2026 |

---

### [3.1. Blog 1](3.1-Blog1/)

Vận hành môi trường không phải sản xuất (Dev, Test, Staging) 24/7 tạo ra lãng phí chi phí đám mây vô cùng lớn cho doanh nghiệp. Bài viết này giới thiệu một giải pháp FinOps tự động kết hợp **AWS Lambda**, **Amazon EventBridge** và **Resource Tagging** để tự động tắt các máy chủ EC2 và cơ sở dữ liệu RDS nhàn rỗi ngoài giờ làm việc. Bằng cách lên lịch tự động khởi động/dừng và xác thực thẻ tài nguyên, giải pháp này giúp các đội ngũ DevOps giảm tới **60-70%** chi phí điện toán không cần thiết mà không làm ảnh hưởng đến tiến độ công việc của lập trình viên.

---

### [3.2. Blog 2](3.2-Blog2/)

Triển khai bảo mật Zero-Trust cho khối cơ quan chính phủ và doanh nghiệp theo tiêu chuẩn TIC 3.0 (Trusted Internet Connections) thường gặp trở ngại do sự phụ thuộc vào VPN truyền thống. Bài viết này phân tích phương pháp ứng dụng **AWS Verified Access (AVA)** kết hợp với **AWS Network Firewall** và **Amazon Route 53 Resolver DNS Firewall** để xây dựng kiến trúc truy cập an toàn không cần VPN (VPN-less). Giải pháp giúp xác thực liên tục dựa trên nhận dạng người dùng (Identity) và trạng thái thiết bị (Device Posture), đáp ứng toàn bộ các yêu cầu khắt khe của khung bảo mật TIC 3.0 mà vẫn tối ưu hóa trải nghiệm làm việc từ xa.

---

### [3.3. Blog 3](3.3-Blog3/)

AArete đã xây dựng **Doczy.ai™** — một hệ thống phân tích hợp đồng thông minh chạy trên AWS đã xử lý **2.5 triệu hợp đồng (~50 triệu trang)** trong 22 tháng, đạt độ chính xác **99%** (so với 55% của các hệ thống dựa trên quy tắc cũ), thực hiện **137 triệu lượt gọi API Bedrock** và tiết kiệm cho khách hàng **~$330 triệu USD**. Bài viết đi sâu vào kiến trúc **Textract + Bedrock + Smart Chunking** (bằng sáng chế cốt lõi của AArete) và **phân cụm kép (dual clustering)** — một mô hình mạnh mẽ cho tài liệu pháp lý mà bất kỳ đội ngũ nào đang xây dựng hệ thống RAG đều có thể học hỏi.
