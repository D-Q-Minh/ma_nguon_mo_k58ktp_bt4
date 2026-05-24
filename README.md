# Môn: Phát triển ứng dụng với mã nguồn mở-TEE0421
### Họ tên: Dương Quang Minh
### MSSV: K225480106047
### Lớp: K58KTP

### Bài 4: KHAI THÁC N8N ĐỂ TỰ ĐỘNG ĐĂNG BÀI LÊN WORDPRESS
### 1.
##### file docker-compose.yml (cập nhật từ bài 3)
```
services:
  db:
    image: mariadb:latest
    container_name: mariadb_db
    restart: always
    environment:
      TZ: "Asia/Ho_Chi_Minh"
      MARIADB_ROOT_PASSWORD: "123456_db_root_pass"
      MARIADB_DATABASE: "wordpress_db"
      MARIADB_USER: "wp_user"
      MARIADB_PASSWORD: "123456_wp_password"
    volumes:
      - db_data:/var/lib/mysql

  phpmyadmin:
    image: phpmyadmin:latest
    container_name: phpmyadmin_ui
    restart: always
    ports:
      - "8080:80"
    environment:
      PMA_HOST: db
      PMA_ARBITRARY: 1
    depends_on:
      - db

  wordpress:
    image: wordpress:latest
    container_name: wordpress_app
    restart: always
    ports:
      - "8000:80"
    environment:
      WORDPRESS_DB_HOST: db
      WORDPRESS_DB_NAME: "wordpress_db"
      WORDPRESS_DB_USER: "wp_user"
      WORDPRESS_DB_PASSWORD: "123456_wp_password"
    depends_on:
      - db
    volumes:
      - wp_data:/var/www/html

  n8n:
    image: n8nio/n8n:latest
    container_name: n8n_automation
    restart: always
    ports:
      - "5678:5678"
    environment:
      - TZ=Asia/Ho_Chi_Minh
      - WEBHOOK_URL=https://n8n.tnut58ktp047.id.vn/
    volumes:
      - n8n_data:/home/node/.n8n
    depends_on:
      - db

  tunnel:
    image: cloudflare/cloudflared:latest
    container_name: cloudflare_tunnel
    restart: always
    command: tunnel --no-autoupdate run
    environment:
      - TUNNEL_TOKEN=eyJhIjoiMzljZDJiMmQyNDNhZDU3MzA2ZjFjMGY0OTA5ZTZlYWIiLCJ0IjoiNmY0M2Q4NjktNzBhYy00NjY1LWI2OWYtOTlkYjUwNjI4ODkwIiwicyI6IlpXSTFNbVV4TkRjdFptTTNOeTAwTURJeExUZzFObUl0WWpaak9EUTVOVFU0T0RnMiJ9
    depends_on:
      - wordpress
      - n8n
      - phpmyadmin

volumes:
  db_data:
  wp_data:
  n8n_data:
```
###### docker compose down
###### docker compose up -d
<img width="368" height="136" alt="image" src="https://github.com/user-attachments/assets/b6026835-656d-4ee4-8280-b2a32a6e8898" />
<img width="652" height="237" alt="image" src="https://github.com/user-attachments/assets/d4189a3d-9e7b-4439-8060-90d88d9202d2" />

### 2.
