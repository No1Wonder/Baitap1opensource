# Baitap1opensource

# đăng ký tên miền trên mắt bão
<img width="1920" height="1080" alt="Screenshot (124)" src="https://github.com/user-attachments/assets/1e10bf09-cbd3-446b-a6cd-fcb97ad30108" />

# Liên kết tên miền với clound

<img width="1920" height="1080" alt="Screenshot (125)" src="https://github.com/user-attachments/assets/7b32facd-e5cb-45bd-848a-c45bb5db6b83" />
<img width="1920" height="1080" alt="Screenshot (126)" src="https://github.com/user-attachments/assets/1f554dcc-a69a-4fdb-bfa9-cabafb90be3f" />

# cài đặt máy ảo và cấu hình Docker

<img width="1920" height="1080" alt="Screenshot (127)" src="https://github.com/user-attachments/assets/e2be1dee-00ef-486b-8451-917062a7a8d7" />
<img width="1920" height="1080" alt="Screenshot (128)" src="https://github.com/user-attachments/assets/1116ba7b-3352-48b5-be57-b08d60f59bbd" />
<img width="1920" height="1080" alt="Screenshot (129)" src="https://github.com/user-attachments/assets/272b952a-5b05-45ae-af78-ab124677559b" />

# trả lời câu hỏi

1. Vì sao dùng Nginx Reverse Proxy thay vì trỏ thẳng Tunnel vào Node-RED?
+	Nginx đóng vai trò “cổng” trung gian: quản lý SSL/TLS, cân bằng tải, rewrite URL, bảo mật (rate limiting, firewall rule).
+	Nếu trỏ thẳng Tunnel vào Node-RED, bạn mất đi lớp kiểm soát này. Node-RED không được thiết kế để xử lý trực tiếp nhiều kết nối phức tạp từ Internet.
+	Reverse Proxy giúp hệ thống an toàn, linh hoạt và dễ mở rộng.
2. Khác biệt giữa mount file và mount thư mục trong Docker
+	Mount file: chỉ ánh xạ một file cụ thể từ host vào container (ví dụ: nginx.conf).
+	Mount thư mục: ánh xạ toàn bộ thư mục, container sẽ thấy tất cả file trong đó.
+	Mount file phù hợp cho cấu hình đơn lẻ, mount thư mục tiện cho nhiều file hoặc dữ liệu.
3. Nếu thay đổi index.html trên Ubuntu, web có đổi ngay không?
+	Nếu container/nginx đang đọc trực tiếp file đó (được mount từ host), thì thay đổi sẽ hiển thị ngay khi refresh.
+	Nếu file nằm trong image (không mount), thì nội dung web không đổi vì container chạy theo image build sẵn.
+	Muốn thay đổi tức thì, cần mount file/thư mục từ host.
4. Ý nghĩa của restart: always và restart: unless-stopped
+	always: container sẽ tự khởi động lại bất kể lý do, kể cả khi Docker daemon restart.
+	unless-stopped: container tự restart trừ khi bạn chủ động dừng nó. 
+	Giúp dịch vụ duy trì liên tục, tránh downtime
5. Khai báo để tất cả services dùng chung 1 network
Trong docker-compose.yml:
networks:
  mynetwork:
    driver: bridge
services:
  service1:
    image: ...
    networks:
      - mynetwork
  service2:
    image: ...
    networks:
      - mynetwork

Lợi ích: các container giao tiếp với nhau bằng tên service, không cần IP. Dễ quản lý, bảo mật nội bộ.
6. Đưa Cloudflare Token vào .env và thêm .env vào .gitignore
+	.env chứa biến môi trường, ví dụ:
CLOUDFLARE_TOKEN=abc123
+	Trong docker-compose.yml gọi bằng ${CLOUDFLARE_TOKEN}.
+	Thêm .env vào .gitignore để không push token lên GitHub. 
+	 Đây là quan trọng về bảo mật vì token là chìa khóa truy cập dịch vụ. Nếu lộ, người khác có thể chiếm quyền.
7. Vì sao nên thêm hậu tố :ro khi mount file cấu hình Nginx?
+	:ro = read-only. Container chỉ đọc, không ghi đè file cấu hình. 
+	 Tránh việc container vô tình hoặc bị tấn công sửa file cấu hình trên host.
8. Khi dùng Cloudflare Tunnel có cần mở cổng cho service nữa không?
+	Không cần. Tunnel tạo kết nối outbound từ server tới Cloudflare, rồi Cloudflare chuyển tiếp traffic. 
+	Không phải mở port trên firewall, an toàn hơn vì dịch vụ không lộ trực tiếp ra Internet.
