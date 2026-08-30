---
title: "Đề xuất"
date: 2026-08-24
weight: 2
chapter: false
pre: " <b> 2. </b> "
---

# Automated Threat Protection

## Automated Threat Protection với AWS WAF & Lambda

---

# 1. Tóm Tắt Dự Án

**Automated Threat Protection** là một giải pháp an ninh mạng dựa trên hạ tầng đám mây (Cloud-Native Security Solution), được thiết kế nhằm tự động hóa việc phát hiện, phản ứng và ngăn chặn các hành vi tấn công mạng (như DDoS, Brute Force, Web Scraping, SQL Injection, Cross-Site Scripting) hướng tới hệ thống ứng dụng web.

Hệ thống tận dụng tối đa các dịch vụ quản lý của AWS bao gồm **AWS WAF**, **Amazon CloudFront**, **Amazon CloudWatch**, **AWS Lambda** và **Amazon SNS**. Bằng cách liên tục phân tích log/metrics theo thời gian thực, hệ thống chủ động nhận diện các địa chỉ IP có dấu hiệu bất thường, tự động đưa các IP này vào danh sách chặn (**IP Set**) trên AWS WAF và gửi cảnh báo tức thì tới đội ngũ vận hành hệ thống thông qua Amazon SNS.

Giải pháp đảm bảo khả năng mở rộng vượt trội, độ trễ cực thấp trong việc ngăn chặn tấn công, tối ưu chi phí vận hành và loại bỏ tối đa sự can thiệp thủ công của con người trong quy trình ứng phó sự cố an ninh thông tin (Incident Response).

---

# 2. Đặt Vấn Đề

## Vấn đề hiện tại

Hiện nay, các ứng dụng web ngày càng trở thành mục tiêu hàng đầu của các cuộc tấn công mạng tự động. Các phương pháp phòng vệ và ứng phó sự cố truyền thống đang gặp nhiều hạn chế:

- **Phản ứng thủ công chậm trễ:** Khi xảy ra các cuộc tấn công Brute Force hoặc HTTP Flood, kỹ sư vận hành phải kiểm tra log thủ công, xác định IP độc hại và thêm IP vào firewall bằng tay. Quy trình này mất nhiều thời gian, dẫn đến thời gian gián đoạn dịch vụ (downtime) kéo dài.
- **Thiếu khả năng tự động hóa theo thời gian thực:** Các giải pháp tường lửa tĩnh không thể linh hoạt điều chỉnh quy tắc dựa trên lượng lưu lượng (traffic) biến động liên tục.
- **Tốn kém tài nguyên hệ thống:** Cuộc tấn công trôi qua mà không được chặn ngay tại cửa ngõ (Edge Location) sẽ đi sâu vào backend, làm quá tải máy chủ ứng dụng và cơ sở dữ liệu.
- **Thiếu cơ chế cảnh báo tập trung:** Đội ngũ an ninh thiếu thông tin tức thời về quy mô, tần suất và nguồn gốc của các đợt tấn công.

## Giải pháp

Giải pháp được đề xuất là xây dựng một hệ thống **Automated Threat Protection** tự động hóa hoàn toàn quy trình phát hiện và chặn IP tấn công trên AWS:

- **Amazon CloudFront** đóng vai trò là CDN phân phối nội dung và ngăn chặn tấn công ở vòng ngoài cùng (Edge Location).
- **AWS WAF** lọc lưu lượng HTTP/HTTPS, áp dụng các quy tắc bảo vệ cơ bản và thực thi chặn các IP trong **IP Set**.
- **Amazon CloudWatch** thu thập WAF Metrics và CloudFront Access Logs theo thời gian thực, phát hiện các ngưỡng lưu lượng bất thường (ví dụ: một IP gửi vượt quá 100 requests trong 5 phút).
- Khi có cảnh báo, **Amazon CloudWatch Alarm** kích hoạt **AWS Lambda Function**. Hàm Lambda ngay lập tức trích xuất IP vi phạm và cập nhật tự động vào **AWS WAF IP Set** để chặn truy cập.
- Đồng thời, **Amazon SNS** gửi cảnh báo chi tiết (thông qua Email/Telegram/Slack) đến quản trị viên hệ thống để theo dõi.

## Lợi ích

- **Tự động chặn tấn công gần như tức thì (Near Real-Time Blocking):** Giảm thời gian phản ứng từ vài giờ xuống còn vài giây.
- **Chặn ngay tại cửa ngõ (Edge Protection):** Ngăn chặn traffic độc hại trước khi nó tiếp cận máy chủ backend.
- **Tiết kiệm chi phí & tài nguyên:** Giảm tải cho các server nội bộ và giảm chi phí hạ tầng trong thời gian bị tấn công.
- **Vận hành Serverless:** Không cần quản lý hay bảo trì máy chủ phục vụ cho hệ thống giám sát an toàn thông tin.
- **Khả năng mở rộng cao:** Dễ dàng bổ sung các kịch bản phát hiện tấn công mới hoặc tích hợp thêm nhiều nguồn log.

---

# 3. Kiến Trúc Giải Pháp

Hệ thống tuân theo kiến trúc Cloud-Native Serverless Security trên hạ tầng AWS.

## Kiến trúc giải pháp

![System Architecture](/images/proposal/system_architecture.png)

## Các dịch vụ AWS sử dụng

- Amazon CloudFront
- AWS WAF (Web Application Firewall)
- Amazon CloudWatch (Metrics, Logs, Alarms)
- AWS Lambda
- Amazon SNS (Simple Notification Service)
- AWS IAM (Identity and Access Management)

## Thiết kế thành phần

### Edge & Traffic Filtering Layer

- **Amazon CloudFront:** Phân phối ứng dụng web, phân tải và tối ưu tốc độ truy cập.
- **AWS WAF:** Tích hợp trực tiếp tại CloudFront, quản lý các Managed Rule Sets và Custom Rules (bao gồm Blocked IP Set).

### Detection & Monitoring Layer

- **CloudWatch Logs:** Lưu trữ và phân tích log truy cập từ AWS WAF và CloudFront.
- **CloudWatch Alarms:** Giám sát các chỉ số WAF Metrics (ví dụ: `BlockedRequests`, `AllowedRequests` trên từng IP) và kích hoạt hành động khi vượt ngưỡng cấu hình.

### Automated Mitigation Layer

- **AWS Lambda:** Hàm tính toán Serverless (viết bằng Python/Node.js), chứa logic xử lý: trích xuất IP tấn công, gọi AWS WAF API để thêm IP vào danh sách đen, thiết lập thời gian hết hạn (TTL) cho IP.

### Notification Layer

- **Amazon SNS:** Đóng vai trò làm pub/sub broker để đẩy thông báo sự cố ngay lập tức tới các kênh của nhà phát triển/đội an ninh.

### Quy trình triển khai

Request từ người dùng

↓

Amazon CloudFront + AWS WAF _(Lọc traffic)_

↓

Amazon CloudWatch _(Ghi log & Kiểm tra ngưỡng cảnh báo)_

↓

AWS Lambda _(Tự động trích xuất IP & Cập nhật WAF IP Set)_

↓

Amazon SNS _(Gửi thông báo sự cố tới Đội ngũ Vận hành)_

---

# 4. Triển Khai Kỹ Thuật

## Các giai đoạn triển khai

Dự án được triển khai qua các giai đoạn cụ thể:

1. Nghiên cứu cơ chế tấn công Web (DDoS, HTTP Flood, Brute Force) và tính năng AWS WAF.
2. Thiết kế kiến trúc tổng quan hệ thống phản ứng tấn công tự động.
3. Cấu hình Amazon CloudFront làm điểm phân phối chính cho ứng dụng Web.
4. Tạo AWS WAF Web ACL, định nghĩa các WAF Rules cơ bản và khởi tạo IP Set trống.
5. Đổi nối WAF Logs / Metrics về Amazon CloudWatch.
6. Cấu hình CloudWatch Alarms phát hiện biến động lưu lượng bất thường từ các IP đơn lẻ.
7. Lập trình AWS Lambda Function (Python / `boto3`) thực hiện logic cập nhật WAF IP Set.
8. Thiết lập IAM Policy/Role đảm bảo nguyên tắc Least Privilege cho Lambda.
9. Cấu hình Amazon SNS Topic và Subscriber (Email/Webhook) để tiếp nhận cảnh báo.
10. Kiểm thử giả lập tấn công (Stress Test / HTTP Flood) để xác minh tính tự động của hệ thống.
11. Tối ưu hóa ngưỡng phát hiện (Threshold) và hoàn thiện tài liệu kỹ thuật.

## Yêu cầu kỹ thuật

### Ngôn ngữ lập trình & SDKs

- Python 3.x
- AWS SDK for Python (`boto3`) / AWS SDK for JavaScript

### Cloud Infrastructure & Tools

- AWS Management Console
- AWS CLI
- AWS CloudFormation / SAM (Serverless Application Model - _tùy chọn deployment_)

### Testing & Simulation Tools

- Apache JMeter / Locust / `artillery` (Giả lập traffic & tấn công HTTP Flood)
- Curl / Postman

---

# 5. Lộ trình triển khai

Dự án triển khai theo 8 giai đoạn công việc chính sau:

| Giai đoạn                                  | Nội Dung Công Việc                                                                                              |
| :----------------------------------------- | :-------------------------------------------------------------------------------------------------------------- |
| **Giai đoạn 1 – Lập kế hoạch dự án**       | Phân tích yêu cầu bài toán an ninh mạng; Thiết kế mô hình kiến trúc hạ tầng tự động hóa trên AWS.               |
| **Giai đoạn 2 – Bảo vệ vùng biên**         | Triển khai Amazon CloudFront distribution; Tạo Web ACL trên AWS WAF; Khởi tạo IP Set phục vụ danh sách chặn.    |
| **Giai đoạn 3 – Giám sát & Ghi log**       | Bật WAF Logging và chuyển giao log tới CloudWatch Logs; Thiết lập CloudWatch Alarms phát hiện vượt ngưỡng.      |
| **Giai đoạn 4 – Logic tự động hóa**        | Viết mã nguồn Python cho Lambda Function để thao tác với AWS WAF API (`GetIPSet`, `UpdateIPSet`); Xử lý TTL IP. |
| **Giai đoạn 5 – Tích hợp cảnh báo**        | Cấu hình Amazon SNS Topic; Đăng ký nhận thông báo qua Email / Slack Webhook / Telegram Bot.                     |
| **Giai đoạn 6 – Bảo mật & Phân quyền IAM** | Kiểm tra và siết chặt các IAM Roles, cấp quyền tối thiểu (Least Privilege) cho Lambda.                          |
| **Giai đoạn 7 – Kiểm thử hệ thống**        | Thực hiện kịch bản giả lập tấn công HTTP Flood / Brute Force; Đo đạc thời gian phản ứng và tinh chỉnh ngưỡng.   |
| **Giai đoạn 8 – Hoàn thành dự án**         | Tổng kết số liệu đánh giá hiệu năng; Hoàn thiện báo cáo thực tập và tài liệu demo giải pháp.                    |

---

# 6. Ước Tính Chi Phí

## Ước tính chi phí hạ tầng

Chi phí vận hành giải pháp tự động này cực kỳ tối ưu do áp dụng mô hình Serverless và Pay-as-you-go.

| Dịch vụ AWS              | Mô tả chi phí                         | Chi phí dự kiến      |
| :----------------------- | :------------------------------------ | :------------------- |
| **AWS WAF**              | Web ACL & Rule Groups                 | ~$5.00/tháng         |
| **Amazon CloudFront**    | Lưu lượng phân phối (Gói Free Tier)   | ~$0.50/tháng         |
| **Amazon CloudWatch**    | Metrics, Logs & Alarms                | ~$0.30/tháng         |
| **AWS Lambda**           | Số lượng yêu cầu & Thời gian thực thi | ~$0.05/tháng         |
| **Amazon SNS**           | Số lượng tin nhắn cảnh báo gửi đi     | ~$0.01/tháng         |
| **Tổng chi phí dự kiến** | **Chi phí vận hành hàng tháng**       | **~$5.86 USD/tháng** |

### Hướng dẫn tối ưu chi phí

- **AWS Budgets:** Cài đặt cảnh báo tự động khi tổng chi phí tài nguyên vượt quá **$10.00/tháng**.
- **CloudWatch Log Retention:** Cấu hình thời gian lưu trữ log ngắn hạn (3 - 7 ngày) trong quá trình thử nghiệm để tránh tăng chi phí lưu trữ Log Group.
- **Post-demo Cleanup:** Xóa các kịch bản test, hủy kích hoạt WAF Web ACL không dùng, hủy các CloudWatch Alarms và SNS Subscriptions sau khi kết thúc đợt thực tập để tránh phát sinh chi phí ngoài ý muốn.

---

# 7. Đánh Giá Rủi Ro

## Rủi ro & Giải pháp

- **Rủi ro 1 - Cấu hình sai ngưỡng gây chặn nhầm (False Positives):** Ngưỡng CloudWatch Alarm đặt quá thấp dẫn đến chặn nhầm người dùng hợp lệ.
  - _Giải pháp:_ Đặt ngưỡng thử nghiệm ở chế độ **Count** trước khi kích hoạt rule **Block** chính thức; Xây dựng quy trình bỏ chặn thủ công (Manual Override / Whitelist).
- **Rủi ro 2 - Lỗi phân quyền IAM hoặc API Throttling:** Hàm Lambda thực thi thất bại do thiếu quyền IAM hoặc dính giới hạn API từ AWS WAF.
  - _Giải pháp:_ Phân quyền IAM đúng chuẩn Least Privilege; Áp dụng kỹ thuật Backoff & Retry trong mã nguồn Lambda.
- **Rủi ro 3 - Chi phí ghi log tăng đột biến:** Tốc độ tấn công quá nhanh làm tăng chi phí lưu trữ CloudWatch Logs.
  - _Giải pháp:_ Giới hạn quy mô dữ liệu log thu thập; Sử dụng `Sampled Requests` của WAF thay vì lưu trữ toàn bộ raw log khi thử nghiệm.
- **Rủi ro 4 - Độ trễ xử lý log:** Độ trễ đẩy log từ CloudFront/WAF về CloudWatch gây chậm trễ trong quy trình phản ứng tự động.
  - _Giải pháp:_ Kết hợp WAF Rate-based Rules (xử lý trực tiếp tại WAF Engine) song song với quy trình Lambda Automation để tạo ra cơ chế bảo vệ 2 lớp.

---

# 8. Kết Quả Kỳ Vọng

## Kết quả kỹ thuật

Sau khi hoàn thành, dự án sẽ đem lại:

- Giải pháp bảo mật ứng dụng web tự động hóa 100% việc chặn địa chỉ IP bất thường.
- Hệ thống phát hiện và giảm thiểu tấn công với thời gian phản ứng tính bằng giây (Near Real-Time).
- Tích hợp hoàn chỉnh chuỗi dịch vụ AWS WAF – CloudFront – CloudWatch – Lambda – SNS theo đúng chuẩn Best Practices của AWS.
- Dashboard giám sát trực quan trên CloudWatch thể hiện lưu lượng truy cập, số lượng request bị chặn và danh sách các IP vi phạm.
- Hệ thống cảnh báo sự cố tức thời gửi tới quản trị viên qua SNS.

## Giá trị thực tiễn

Đề tài chứng minh tính thực tiễn cao của việc ứng dụng tư duy **DevSecOps** và kiến trúc **Serverless Security** vào bảo vệ hạ tầng doanh nghiệp:

- Tối ưu hóa chi phí nhân sự vận hành (Giảm thiểu đáng kể thao tác can thiệp thủ công từ đội ngũ SOC/SecOps).
- Nâng cao tính sẵn sàng và độ tin cậy cho các ứng dụng web trước các nguy cơ tấn công từ Internet.
- Đặt nền móng cho việc mở rộng thêm các kịch bản an toàn thông tin chuyên sâu khác như tích hợp AWS Shield, tự động cập nhật danh sách IP độc hại từ các nguồn Threat Intelligence bên ngoài.
