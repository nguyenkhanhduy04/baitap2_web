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
=======================================================================

# Bài giải:

- 2.1. Cài đặt Apache web server:<br>
- Vô hiệu hoá IIS: nếu iis đang chạy thì mở cmd quyền admin để chạy lệnh: iisreset /stop:<br>
  <img width="1105" height="630" alt="image" src="https://github.com/user-attachments/assets/05946bed-23a8-415b-be13-49a69e37d2d2" /><br>

- Download apache server, giải nén ra ổ G<br>
  <img width="943" height="467" alt="image" src="https://github.com/user-attachments/assets/a85714ee-81d3-4add-86fb-ec6e84a8f8d2" /><br>

-  cấu hình các file:<br>
  + G:\Apache24\conf\httpd.conf:<br>
    <img width="511" height="133" alt="image" src="https://github.com/user-attachments/assets/5bf3bbe8-079b-4af9-a78b-56640c49d7ce" /><br>
    <img width="872" height="184" alt="image" src="https://github.com/user-attachments/assets/3e22c34c-1508-4542-a22a-3c0c03df4f19" /><br>


  + G:Apache24\conf\extra\httpd-vhosts.conf:<br>
    <img width="925" height="446" alt="image" src="https://github.com/user-attachments/assets/18a0820d-511c-4831-8211-e7a81b6a1420" /><br>
  để tạo website với domain: nguyenkhanhduy.com<br>
- code web sẽ đặt tại thư mục: `G:\Apache24\nguyenkhanhduy`:<br>
  <img width="892" height="623" alt="image" src="https://github.com/user-attachments/assets/8ee9b36f-91a0-4a49-96ee-e4d15ca0fc27" /><br>
- - sử dụng file `c:\WINDOWS\SYSTEM32\Drivers\etc\hosts` để fake ip 127.0.0.1 cho domain này:<br>
    <img width="842" height="680" alt="image" src="https://github.com/user-attachments/assets/17632dfd-474a-4021-b764-9bd4e2222119" /><br>

- thao tác dòng lệnh trên file `G:\Apache24\bin\httpd.exe` với các tham số `-k install` và `-k start` để cài đặt và khởi động web server apache.<br>
  <img width="722" height="275" alt="image" src="https://github.com/user-attachments/assets/592a8f2b-1559-44a4-be87-a037a52cb29f" /><br>
  <img width="1890" height="607" alt="image" src="https://github.com/user-attachments/assets/8c134f42-d776-4df3-9fed-cbe14800c766" /><br>


2.2. Cài đặt nodejs và nodered => Dùng làm backend:<br>

  + download file `https://nodejs.org/dist/v20.19.5/node-v20.19.5-x64.msi`  (đây ko phải bản mới nhất, nhưng ổn định)<br>
  + cài đặt vào thư mục `G:\nodejs`:<br>

    <img width="1141" height="495" alt="image" src="https://github.com/user-attachments/assets/ffd68f0e-20f5-4c05-b6eb-2bd4caa00526" /><br>
- Cài đặt nodered:<br>
  + chạy cmd, vào thư mục `G:\nodejs`, chạy lệnh `npm install -g --unsafe-perm node-red --prefix "G:\nodejs\nodered"`<br>
    <img width="922" height="538" alt="image" src="https://github.com/user-attachments/assets/f7c881f7-c9f6-481a-93fc-5e510fb0c168" /><br>

  + download file: https://nssm.cc/release/nssm-2.24.zip, giải nén được file nssm.exe<br>
    copy nssm.exe vào thư mục `G:\nodejs\nodered\`:<br>
    <img width="942" height="384" alt="image" src="https://github.com/user-attachments/assets/16694418-8b84-44ac-b42e-1166ffbee78b" /><br>
  + tạo file "D:\nodejs\nodered\run-nodered.cmd" với nội dung (5 dòng sau):<br>
    <img width="1654" height="957" alt="image" src="https://github.com/user-attachments/assets/72da0d0b-f757-4d6f-af0b-e26e48ab3623" /><br>

`+ mở cmd, chuyển đến thư mục: `D:\nodejs\nodered`:<br>
<img width="467" height="156" alt="image" src="https://github.com/user-attachments/assets/922f5d3d-54aa-4f44-9fbd-adb4ea92f9e8" /><br>

  + cài đặt service `a1-nodered` bằng lệnh: nssm.exe install a1-nodered "G:\nodejs\nodered\run-nodered.cmd"<br>
  + chạy service `a1-nodered` bằng lệnh: `nssm start a1-nodered`<br>
    <img width="816" height="174" alt="image" src="https://github.com/user-attachments/assets/2e424400-b57d-403a-bbe0-c77f0556629c" /><br>

2.3. Tạo csdl tuỳ ý trên mssql (sql server 2022), nhớ các thông số kết nối: ip, port, username, password, db_name, table_name:<br>

<img width="1249" height="515" alt="image" src="https://github.com/user-attachments/assets/3a91da6c-cea2-4ad8-89d1-88f17b1b03d6" /><br>

2.4. Cài đặt thư viện trên nodered:<br>
- truy cập giao diện nodered bằng url: http://localhost:1880<br>
- cài đặt các thư viện:<br>
  <img width="666" height="391" alt="image" src="https://github.com/user-attachments/assets/6c167df3-0906-4446-923a-b959f86365ae" /><br>
<img width="656" height="439" alt="image" src="https://github.com/user-attachments/assets/5dd8b752-2368-4c5f-91b6-b0626399c595" /><br>
<img width="666" height="406" alt="image" src="https://github.com/user-attachments/assets/3a7a9e76-bc20-4426-999b-bff640bf9fd1" /><br>
<img width="671" height="538" alt="image" src="https://github.com/user-attachments/assets/f421091c-722a-4fb5-a3c6-9d9df869a7fb" /><br>
<img width="667" height="500" alt="image" src="https://github.com/user-attachments/assets/f321f70b-7623-4925-9242-324a9246c95b" /><br>
<img width="664" height="441" alt="image" src="https://github.com/user-attachments/assets/47a70d99-68bb-4a65-83b0-34ca17b5cef0" /><br>


- Sửa file `G:\nodejs\nodered\work\settings.js` :<br>
  <img width="913" height="268" alt="image" src="https://github.com/user-attachments/assets/ba1df420-a973-460e-9050-eeda72f3d3de" /><br>
- Chạy lại nodered bằng cmd và truy cập lại url để đăng nhập bằng tài khoản admin:<br>
  <img width="562" height="248" alt="image" src="https://github.com/user-attachments/assets/e8659f9e-3e27-45b8-83dd-211de11faf85" /><br>
  <img width="955" height="617" alt="image" src="https://github.com/user-attachments/assets/5f646ab7-0bf8-44d1-a8f2-5aede4cbe8cb" /><br>


2.5. tạo api back-end bằng nodered:<br>
<img width="820" height="306" alt="image" src="https://github.com/user-attachments/assets/0efea7e9-d3e8-4216-a987-b3435f609323" /><br>

2.6. Tạo giao diện front-end liên kết với back end qua js:<br>

<img width="1900" height="987" alt="image" src="https://github.com/user-attachments/assets/92dc4739-49a3-4310-baf4-fdce4b9ca760" /><br>


2.7. Nhận xét bài làm của mình:<br>

- Đã hiểu rõ nguyên lí hoạt động của Appache (cần vô hiệu hóa IIS trước)<br>
- Biết cách cài và sử dung nodejs và nodered để liên kết frontend(web) với backend(database)<br>
- Nguyên lí tương tác giữa frontend và backend:   frontend gửi yêu cầu → Node-RED xử lý → truy vấn SQL → trả kết quả → frontend hiển thị.<br>






    


    

    


  
