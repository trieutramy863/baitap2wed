# baitap2wed
Bài tập 02: Lập trình web.
==============================
NGÀY GIAO: 19/10/2025
==============================
DEADLINE: 26/10/2025
==============================
1. Sử dụng github để ghi lại quá trình làm, tạo repo mới, để truy cập public, edit file `readme.md`:
   chụp ảnh màn hình (CTRL+Prtsc) lúc đang làm, paste vào file `readme.md`, thêm mô tả cho ảnh.
2. NỘI DUNG BÀI TẬP:
2.1. Cài đặt Apache web server:
- Vô hiệu hoá IIS: nếu iis đang chạy thì mở cmd quyền admin để chạy lệnh: iisreset /stop
- Download apache server, giải nén ra ổ D, cấu hình các file:
  + D:\Apache24\conf\httpd.conf
  + D:Apache24\conf\extra\httpd-vhosts.conf
  để tạo website với domain: fullname.com
  code web sẽ đặt tại thư mục: `D:\Apache24\fullname` (fullname ko dấu, liền nhau)
- sử dụng file `c:\WINDOWS\SYSTEM32\Drivers\etc\hosts` để fake ip 127.0.0.1 cho domain này
  ví dụ sv tên là: `Đỗ Duy Cốp` thì tạo website với domain là fullname ko dấu, liền nhau: `doduycop.com`
- thao tác dòng lệnh trên file `D:\Apache24\bin\httpd.exe` với các tham số `-k install` và `-k start` để cài đặt và khởi động web server apache.
2.2. Cài đặt nodejs và nodered => Dùng làm backend:
- Cài đặt nodejs:
  + download file `https://nodejs.org/dist/v20.19.5/node-v20.19.5-x64.msi`  (đây ko phải bản mới nhất, nhưng ổn định)
  + cài đặt vào thư mục `D:\nodejs`
- Cài đặt nodered:
  + chạy cmd, vào thư mục `D:\nodejs`, chạy lệnh `npm install -g --unsafe-perm node-red --prefix "D:\nodejs\nodered"`
  + download file: https://nssm.cc/release/nssm-2.24.zip
    giải nén được file nssm.exe
    copy nssm.exe vào thư mục `D:\nodejs\nodered\`
  + tạo file "D:\nodejs\nodered\run-nodered.cmd" với nội dung (5 dòng sau):
@echo off
REM fix path
set PATH=D:\nodejs;%PATH%
REM Run Node-RED
node "D:\nodejs\nodered\node_modules\node-red\red.js" -u "D:\nodejs\nodered\work" %*
  + mở cmd, chuyển đến thư mục: `D:\nodejs\nodered`
  + cài đặt service `a1-nodered` bằng lệnh: nssm.exe install a1-nodered "D:\nodejs\nodered\run-nodered.cmd"
  + chạy service `a1-nodered` bằng lệnh: `nssm start a1-nodered`
2.3. Tạo csdl tuỳ ý trên mssql (sql server 2022), nhớ các thông số kết nối: ip, port, username, password, db_name, table_name
2.4. Cài đặt thư viện trên nodered:
- truy cập giao diện nodered bằng url: http://localhost:1880
- cài đặt các thư viện: node-red-contrib-mssql-plus, node-red-node-mysql, node-red-contrib-telegrambot, node-red-contrib-moment, node-red-contrib-influxdb, node-red-contrib-duckdns, node-red-contrib-cron-plus
- Sửa file `D:\nodejs\nodered\work\settings.js` : 
  tìm đến chỗ adminAuth, bỏ comment # ở đầu dòng (8 dòng), thay chuỗi mã hoá mật khẩu bằng chuỗi mới
    adminAuth: {
        type: "credentials",
        users: [{
            username: "admin",
            password: "chuỗi_mã_hoá_mật_khẩu",
            permissions: "*"
        }]
    },   
   với mã hoá mật khẩu có thể thiết lập bằng tool: https://tms.tnut.edu.vn/pw.php
- chạy lại nodered bằng cách: mở cmd, vào thư mục `D:\nodejs\nodered` và chạy lệnh `nssm restart a1-nodered`
  khi đó nodered sẽ yêu cầu nhập mật khẩu mới vào được giao diện cho admin tại: http://localhost:1880
2.5. tạo api back-end bằng nodered:
- tại flow1 trên nodered, sử dụng node `http in` và `http response` để tạo api
- thêm node `MSSQL` để truy vấn tới cơ sở dữ liệu
- logic flow sẽ gồm 4 node theo thứ tự sau (thứ tự nối dây): 
  1. http in  : dùng GET cho đơn giản, URL đặt tuỳ ý, ví dụ: /timkiem
  2. function : để tiền xử lý dữ liệu gửi đến
  3. MSSQL: để truy vấn dữ liệu tới CSDL, nhận tham số từ node tiền xử lý
  4. http response: để phản hồi dữ liệu về client: Status Code=200, Header add : Content-Type = application/json
  có thể thêm node `debug` để quan sát giá trị trung gian.
- test api thông qua trình duyệt, ví dụ: http://localhost:1880/timkiem?q=thị
2.6. Tạo giao diện front-end:
- html form gồm các file : index.html, fullname.js, fullname.css
  cả 3 file này đặt trong thư mục: `D:\Apache24\fullname`
  nhớ thay fullname là tên của bạn, viết liền, ko dấu, chữ thường, vd tên là Đỗ Duy Cốp thì fullname là `doduycop`
  khi đó 3 file sẽ là: index.html, doduycop.js và doduycop.css
- index.html và fullname.css: trang trí tuỳ ý, có dấu ấn cá nhân, có form nhập được thông tin.
- fullname.js: lấy dữ liệu trên form, gửi đến api nodered đã làm ở bước 2.5, nhận về json, dùng json trả về để tạo giao diện phù hợp với kết quả truy vấn của bạn.
2.7. Nhận xét bài làm của mình:
- đã hiểu quá trình cài đặt các phần mềm và các thư viện như nào?
- đã hiểu cách sử dụng nodered để tạo api back-end như nào?
- đã hiểu cách frond-end tương tác với back-end ra sao?
==============================
TIÊU CHÍ CHẤM ĐIỂM:
1. y/c bắt buộc về thời gian: ko quá Deadline, quá: 0 điểm (ko có ngoại lệ)
2. cài đặt được apache và nodejs và nodered: 1đ
3. cài đặt được các thư viện của nodered: 1đ
4. nhập dữ liệu demo vào sql-server: 1đ
5. tạo được back-end api trên nodered, test qua url thành công: 1đ
6. tạo được front-end html css js, gọi được api, hiển thị kq: 1đ
7. trình bày độ hiểu về toàn bộ quá trình (mục 2.7): 5đ
# Bài Làm 
# 2.1. Cài đặt Apache web server:
# Vô hiệu hoá IIS: nếu iis đang chạy thì mở cmd quyền admin để chạy lệnh: iisreset /stop
Download apache server, giải nén ra ổ D, cấu hình các file:
D:\Apache24\conf\httpd.conf
D:Apache24\conf\extra\httpd-vhosts.conf để tạo website với domain: fullname.com code web sẽ đặt tại thư mục: D:\Apache24\fullname (fullname ko dấu, liền nhau)
sử dụng file c:\WINDOWS\SYSTEM32\Drivers\etc\hosts để fake ip 127.0.0.1 cho domain này ví dụ sv tên là: Đỗ Duy Cốp thì tạo website với domain là fullname ko dấu, liền nhau: doduycop.com
thao tác dòng lệnh trên file D:\Apache24\bin\httpd.exe với các tham số -k install và -k start để cài đặt và khởi động web server apache.
Đâu tiên là lên gg viết apache ra chọn cái có link như ảnh để tải
# <img width="960" height="600" alt="image" src="https://github.com/user-attachments/assets/7406684d-3b0e-4650-ab9f-7cb6d71b0238" />
# Tiếp theo là chọn dowload và chọn vào chữ a number of third party vendors
# <img width="1920" height="1200" alt="Screenshot 2025-10-26 175937" src="https://github.com/user-attachments/assets/305daad2-1a27-4580-9aa7-b09bff2e31bf" />
# Sau đó chọn Apache Louge
# <img width="1920" height="1200" alt="Screenshot 2025-10-26 175953" src="https://github.com/user-attachments/assets/9b0950c1-35de-4f01-9f80-05b2b2aadd58" />
# chọn win 64
# <img width="1920" height="1200" alt="Screenshot 2025-10-26 180005" src="https://github.com/user-attachments/assets/62040cbb-71cb-407c-ba65-b27b53b86476" />
# Cấu hình D:\Apache24\conf\httpd.conf
# <img width="1920" height="1200" alt="Screenshot 2025-10-26 180254" src="https://github.com/user-attachments/assets/d73854a6-13b0-4023-bcb1-f381bc784cd0" />
# Cấu hình D:Apache24\conf\extra\httpd-vhosts.conf
# <img width="1920" height="1200" alt="Screenshot 2025-10-26 180352" src="https://github.com/user-attachments/assets/a0cddf2b-d097-4928-8181-d2505011bd46" />
# Code web sẽ đặt tại thư mục: D:\Apache24\fullname (fullname ko dấu, liền nhau)
# <img width="1920" height="1200" alt="Screenshot 2025-10-26 180517" src="https://github.com/user-attachments/assets/b698d6ba-1450-4f7b-9cee-e5b34b56787f" />
# sử dụng file c:\WINDOWS\SYSTEM32\Drivers\etc\hosts để fake ip 127.0.0.1 cho domain này ví dụ sv tên là: Đỗ Duy Cốp thì tạo website với domain là fullname ko dấu, liền nhau: `doduycop.com
# <img width="1920" height="1200" alt="Screenshot 2025-10-26 180621" src="https://github.com/user-attachments/assets/28b54642-55e3-47ce-8566-a0cba108dbc2" />
# thao tác dòng lệnh trên file D:\Apache24\bin\httpd.exe với các tham số -k install và -k start để cài đặt và khởi động web server apache.
k install
# Kết quả
# <img width="1310" height="1141" alt="Screenshot 2025-10-24 213328" src="https://github.com/user-attachments/assets/12fc356c-fbbe-403a-845b-0d55d76a9529" />
# 2.2. Cài đặt nodejs và nodered => Dùng làm backend:
# Cài đặt nodejs:
# download file https://nodejs.org/dist/v20.19.5/node-v20.19.5-x64.msi (đây ko phải bản mới nhất, nhưng ổn định)
# cài đặt vào thư mục D:\nodejs
# <img width="1920" height="1200" alt="Screenshot 2025-10-24 184845" src="https://github.com/user-attachments/assets/7cfd083a-b0c1-4c1a-bd7b-5f626dccded1" />
# <img width="1920" height="1200" alt="Screenshot 2025-10-24 184741" src="https://github.com/user-attachments/assets/eea8fc34-5229-456c-a9c6-9fa58cc0bf82" />
# <img width="1920" height="1200" alt="Screenshot 2025-10-24 184655" src="https://github.com/user-attachments/assets/66f852ea-5539-4dcd-bd98-fa65d24f920e" />
# <img width="1920" height="1200" alt="Screenshot 2025-10-24 184708" src="https://github.com/user-attachments/assets/bfb70903-16b0-4d3d-a419-457a670f2ac4" />
# <img width="1920" height="1200" alt="Screenshot 2025-10-24 184719" src="https://github.com/user-attachments/assets/310c8c4a-fdac-4536-a263-37f893a39974" />
# <img width="786" height="624" alt="Screenshot 2025-10-24 190946" src="https://github.com/user-attachments/assets/57adf8a4-e4cb-4589-91db-5029e6520f9f" />
# <img width="1920" height="1200" alt="Screenshot 2025-10-24 184820" src="https://github.com/user-attachments/assets/fc8374fd-5d82-42f5-9b47-c65870ea58d1" />
# <img width="1920" height="1200" alt="Screenshot 2025-10-24 184845" src="https://github.com/user-attachments/assets/c16353e6-2165-4abe-8ac8-5d83de977d03" />
# <img width="1920" height="1200" alt="Screenshot 2025-10-26 181524" src="https://github.com/user-attachments/assets/a9a5a8bc-2ff8-4823-bd2a-43c42b98b5cd" />
# Cài đặt nodered:
# chạy cmd, vào thư mục D:\nodejs, chạy lệnh npm install -g --unsafe-perm node-red --prefix "D:\nodejs\nodered"
# download file: https://nssm.cc/release/nssm-2.24.zip giải nén được file nssm.exe copy nssm.exe vào thư mục D:\nodejs\nodered\ 
# <img width="1920" height="1200" alt="image" src="https://github.com/user-attachments/assets/48b3786f-00af-441e-acb3-becf811252c0" />
# tạo file "D:\nodejs\nodered\run-nodered.cmd" với nội dung (5 dòng sau): @echo off REM fix path set PATH=D:\nodejs;%PATH% REM Run Node-RED node "D:\nodejs\nodered\node_modules\node-red\red.js" -u "D:\nodejs\nodered\work" %*
# <img width="1920" height="1200" alt="Screenshot 2025-10-26 181939" src="https://github.com/user-attachments/assets/b40fa24e-0ebb-4be9-bff4-c61a5c405c3a" />
# mở cmd, chuyển đến thư mục: D:\nodejs\nodered
# cài đặt service a1-nodered bằng lệnh: nssm.exe install a1-nodered "D:\nodejs\nodered\run-nodered.cmd"
# chạy service a1-nodered bằng lệnh: nssm start a1-nodered
# <img width="1102" height="470" alt="Screenshot 2025-10-26 182037 (2)" src="https://github.com/user-attachments/assets/ad9cdd81-4855-4f2f-abc7-36d984ab4ab8" />
# 2.3. Tạo csdl tuỳ ý trên mssql (sql server 2022), nhớ các thông số kết nối: ip, port, username, password, db_name, table_name
# <img width="1920" height="1200" alt="Screenshot 2025-10-26 182222" src="https://github.com/user-attachments/assets/c882fe0e-653c-445c-82f3-75582205716e" />
# <img width="1920" height="1200" alt="Screenshot 2025-10-26 182326" src="https://github.com/user-attachments/assets/f6148f20-5569-41f7-ae40-ed08136b5557" />
# 2.4. Cài đặt thư viện trên nodered:
# truy cập giao diện nodered bằng url: http://localhost:1880
# <img width="1920" height="1200" alt="Screenshot 2025-10-24 200420" src="https://github.com/user-attachments/assets/818f0fd4-08ec-488c-8a93-8cd71f1f8e07" />
# cài đặt các thư viện: node-red-contrib-mssql-plus, node-red-node-mysql, node-red-contrib-telegrambot, node-red-contrib-moment, node-red-contrib-influxdb, node-red-contrib-duckdns, node-red-contrib-cron-plus
# <img width="1231" height="664" alt="Screenshot 2025-10-26 182729 (2)" src="https://github.com/user-attachments/assets/7d688ea4-9961-424a-996d-0b1beeef8c2d" />
# <img width="1152" height="523" alt="Screenshot 2025-10-26 182912" src="https://github.com/user-attachments/assets/33ab74cf-a506-44d1-b43d-7e33cdb6ffdf" />
# Sửa file D:\nodejs\nodered\work\settings.js : tìm đến chỗ adminAuth, bỏ comment # ở đầu dòng (8 dòng), thay chuỗi mã hoá mật khẩu bằng chuỗi mới adminAuth: { type: "credentials", users: [{ username: "admin", password: "chuỗi_mã_hoá_mật_khẩu", permissions: "*" }] },
# với mã hoá mật khẩu có thể thiết lập bằng tool: https://tms.tnut.edu.vn/pw.php
# <img width="1920" height="1200" alt="Screenshot 2025-10-26 183110" src="https://github.com/user-attachments/assets/f97b5a83-d8f6-4cdd-a844-0c6e6aa8bd4c" />
# <img width="1920" height="1200" alt="Screenshot 2025-10-26 183152" src="https://github.com/user-attachments/assets/428a5377-dc46-4473-8234-9ed10301e629" />
# chạy lại nodered bằng cách: mở cmd, vào thư mục D:\nodejs\nodered và chạy lệnh nssm restart a1-nodered khi đó nodered sẽ yêu cầu nhập mật khẩu mới vào được giao diện cho admin tại: http://localhost:1880
# <img width="1920" height="1200" alt="Screenshot 2025-10-26 183323" src="https://github.com/user-attachments/assets/a5027726-aa0f-48da-8876-21604553b96c" />
# 2.5. tạo api back-end bằng nodered:
tại flow1 trên nodered, sử dụng node http in và http response để tạo api
thêm node MSSQL để truy vấn tới cơ sở dữ liệu
logic flow sẽ gồm 4 node theo thứ tự sau (thứ tự nối dây):
http in : dùng GET cho đơn giản, URL đặt tuỳ ý, ví dụ: /timkiem
function : để tiền xử lý dữ liệu gửi đến
MSSQL: để truy vấn dữ liệu tới CSDL, nhận tham số từ node tiền xử lý
http response: để phản hồi dữ liệu về client: Status Code=200, Header add : Content-Type = application/json
# <img width="1920" height="1128" alt="Screenshot 2025-10-25 140247" src="https://github.com/user-attachments/assets/9ca56a1e-b86f-46e3-abad-628532c4a9c6" />
# <img width="1920" height="1200" alt="Screenshot 2025-10-25 131247" src="https://github.com/user-attachments/assets/58f77c81-9e58-41c2-8187-c8c70fa65262" />
# <img width="1920" height="1200" alt="Screenshot 2025-10-25 131423" src="https://github.com/user-attachments/assets/500cbb40-3586-4b71-b1c2-808c2d12a555" />
# <img width="1920" height="1200" alt="Screenshot 2025-10-25 131449" src="https://github.com/user-attachments/assets/b2c10430-235b-4f95-b3b0-527fe0913337" />
# Kết Quả
# <img width="1920" height="1128" alt="Screenshot 2025-10-25 135323" src="https://github.com/user-attachments/assets/ab0658ac-6e5a-4dd7-93d8-41c8291044c2" />
# 2.6. Tạo giao diện front-end:
html form gồm các file : index.html, fullname.js, fullname.css cả 3 file này đặt trong thư mục: D:\Apache24\fullname nhớ thay fullname là tên của bạn, viết liền, ko dấu, chữ thường, vd tên là Đỗ Duy Cốp thì fullname là doduycop khi đó 3 file sẽ là: index.html, doduycop.js và doduycop.css
index.html và fullname.css: trang trí tuỳ ý, có dấu ấn cá nhân, có form nhập được thông tin.
fullname.js: lấy dữ liệu trên form, gửi đến api nodered đã làm ở bước 2.5, nhận về json, dùng json trả về để tạo giao diện phù hợp với kết quả truy vấn của bạn.
# <img width="960" height="1128" alt="Screenshot 2025-10-25 135106" src="https://github.com/user-attachments/assets/307d30e5-64a9-4341-a118-80a8b0d6a36f" />
# <img width="1920" height="1200" alt="Screenshot 2025-10-25 143355" src="https://github.com/user-attachments/assets/1e440a3c-d5da-4aa7-955c-a1f67c40a5cb" />
# <img width="1218" height="1088" alt="Screenshot 2025-10-25 143402" src="https://github.com/user-attachments/assets/467bc9e3-5980-4367-a55b-7556d26f9cd0" />
# <img width="1920" height="1200" alt="Screenshot 2025-10-26 184048" src="https://github.com/user-attachments/assets/e9ea8fcb-ead6-43cf-b76d-a28cbd951007" />
# Kết Quả
# <img width="1920" height="1200" alt="Screenshot 2025-10-25 140953" src="https://github.com/user-attachments/assets/9ae861f8-0b2f-4a99-9d02-27cf4cbf0e55" />
# 2.7. Nhận xét bài làm của mình:
Trong quá trình thực hiện bài thực hành, em đã tiến hành cài đặt và cấu hình các phần mềm cần thiết để xây dựng một hệ thống web có khả năng kết nối cơ sở dữ liệu và tạo API. Qua các bước triển khai, em đã hiểu rõ hơn quy trình thiết lập môi trường làm việc, cơ chế hoạt động của từng thành phần trong hệ thống, cũng như mối quan hệ giữa frontend và backend trong mô hình ứng dụng web.

Hiểu biết về quá trình cài đặt và cấu hình phần mềm

Thông qua bài làm, em đã nắm được cách cài đặt và cấu hình các công cụ quan trọng như:

Apache Web Server: Em biết cách cài đặt và chỉnh sửa file cấu hình httpd.conf, thiết lập đường dẫn tài nguyên, thay đổi cổng truy cập khi bị trùng (ví dụ cổng 80), đồng thời khắc phục lỗi do xung đột với IIS.

Node.js và Node-RED: Em đã hiểu cách cài đặt Node.js để chạy môi trường JavaScript phía server và cài Node-RED để xây dựng luồng xử lý dữ liệu trực quan.

SQL Server: Em biết cách cài đặt, kích hoạt tài khoản sa, tạo cơ sở dữ liệu và các bảng cần thiết, đồng thời mở quyền truy cập để Node-RED có thể kết nối đến SQL Server.

Ngoài ra, em cũng đã thực hành cài đặt các thư viện mở rộng trong Node-RED như node-red-contrib-mssql-plus, node-red-node-mysql, node-red-dashboard,… để mở rộng chức năng và tăng khả năng kết nối dữ liệu.

Hiểu cách sử dụng Node-RED để xây dựng API Backend

Em đã nắm được cách sử dụng Node-RED để tạo API backend thông qua mô hình flow (luồng dữ liệu) gồm nhiều node liên kết. Cụ thể:

Node http in dùng để định nghĩa endpoint cho API.

Node function xử lý logic, đọc tham số từ request và tạo truy vấn SQL.

Node MSSQL đảm nhiệm việc kết nối và thực hiện truy vấn với SQL Server.

Node http response trả kết quả về cho client dưới dạng JSON.

Qua thực hành, em hiểu rõ cách dữ liệu di chuyển qua từng node, cách debug lỗi, xem log và kiểm tra phản hồi từ API. Em cũng đã biết cách tạo và triển khai nhiều API khác nhau trong cùng một flow như API đăng nhập, tìm kiếm hay thêm mới dữ liệu.

Hiểu mối quan hệ giữa Frontend và Backend

Em đã nắm rõ cơ chế tương tác giữa frontend và backend trong hệ thống web. Khi người dùng thao tác trên giao diện (frontend), yêu cầu sẽ được gửi đến API (backend) do Node-RED cung cấp. Backend tiếp nhận, xử lý logic, truy vấn dữ liệu từ SQL Server và gửi phản hồi (response) dạng JSON về cho frontend để hiển thị kết quả.
Qua đó, em hiểu được vai trò trung tâm của backend trong việc xử lý dữ liệu và logic, trong khi frontend là cầu nối tương tác trực tiếp với người dùng. Nắm vững mối quan hệ này giúp em tự tin hơn khi muốn phát triển hoặc mở rộng các chức năng của ứng dụng web.
Bài thực hành giúp em hiểu được quy trình thiết lập và vận hành một hệ thống web hoàn chỉnh, từ backend đến frontend. Em đã biết cách phát hiện và khắc phục lỗi như sai thông tin đăng nhập, lỗi kết nối cơ sở dữ liệu, hay lỗi định nghĩa biến trong Node-RED.
Tuy nhiên, trong quá trình làm việc em vẫn gặp một số lỗi cú pháp (ví dụ: “Invalid property expression”) và phải tra cứu tài liệu, thử nghiệm nhiều lần để tìm ra nguyên nhân. Nhờ đó, em rút ra được kinh nghiệm quý báu trong việc kiểm tra luồng dữ liệu, theo dõi log, và xử lý lỗi cẩn thận hơn.

Tổng kết lại, bài thực hành không chỉ giúp em củng cố kiến thức về lập trình backend mà còn mở rộng hiểu biết về hệ thống, mạng, cơ sở dữ liệu và bảo mật. Đây là nền tảng quan trọng để em có thể phát triển các ứng dụng web phức tạp hơn trong tương lai.
