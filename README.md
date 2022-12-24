# Review checklist
## UBot - 22/12/2022

[![Build Status](https://travis-ci.org/joemccann/dillinger.svg?branch=master)](https://travis-ci.org/joemccann/dillinger)

Review checklist deploy service UBot
- Checkout source code
- Develop & Fix bug
- Self Test
- Tạo merge request
- Review code
- Deploy dev
- Testing
- Deploy Live

## Checkout source code
- Luôn pull code mới nhất từ staging và develop hằng ngày và trước khi tạo merge request, đảm bảo code  người push luôn mới nhất so với nhánh merge vào, nếu có tự resolve conflig ở local trước.
- Checkout từ nhánh staging.
- Develop big feature gồm cả fontend và backend nên checkout ra nhánh riêng từ staging, fe và be có thể dev cùng trên nhánh đó, hoặc lấy nhánh đó là nhánh chung cho 2 bên tách ra tiếp.
- Branch name: 
    + New feature: dev-[<board-id>]-[<pule-id>]-[<description>]
    + Bug: hf-[<board-id>]-[<pule-id>]-[<description>]
    + board_id và pule_id lấy từ task monday.

## Develop & Fix bug
- Với task khó, cần new solution mới cần thảo luận  với  tối  thiểu ChungQD,  ChuyenNS  để xem hướng tiếp cận và đánh giá trước khi làm.
- Hạn chế commit code, và commit code cần có nghĩa, chấp nhận tiếng việt.
- Develop new feature:
    + Chốt lại yêu cầu phạm vi các task, khả năng ảnh hưởng tới các service khác  hoặc  chức năng khác đã chạy trước đó (Có thể hỏi ChuyenNS, ChungQD hoặc những người đã  care chính task liên quan trước đó).
    + Xem xét khả năng mở rộng và phát triển của task đó.
    + Nếu là feature to, tự break ra các task nhỏ bên trong và est lại time hợp lý với yêu cầu đã chốt trước đó.
- Fix bug:
    + Xác định nguyên nhân bug cụ thể, và cách khắc phục,  phạm vi ảnh hưởng, trong trường hợp  critial  cần đưa ra phương án khắc phục sớm tạm thời hiện trạng, sau đó viết code  cẩn thận update lại.,  tuỳ theo từng task, nếu không thể giải quyết ngay được thì hỏi các member khác trong team về cách giải quyết.

## Self test
- Tự test lại source code có TÂM trước khi tạo  merge request.
- Check lại source code đã follow các rule về clean code search google hoặc tham khảo link:            https://github.com/EwayJSC/Eway-Java-Coding-Guidelines
    + Tên hàm, biến
    + Độ dài các hàm
    + Cố gắng follow early return trong 1 method
    + Sử dụng stream, landa, Optional, viết gọn lại code
    + Mỗi hàm nên đảm bảo duy nhất 1 chức năng cụ thể 
    + Check duplicate code
    + Khởi tạo object quá nhiều, phân biệt default value kiểu wrapper và primative .
    + Để ý các alert của IDE, Cài plugin sonar hỗ trợ về  check  code.
    + ....
- Check lại code perfoment:
    + Chi phí về mặt I/O là lớn: hạn chế for-earch  vào database, với những dữ liệu ít thay đổi nên cache lại.
    + Với 1 số nghiệp vụ, nếu có thể  sử dụng multithreading  thì nên áp dụng vào , để tăng hiệu suất ứng dụng.
    + Review lại cách xử lý code, xem đã ổn chưa, có cách nào có thể làm tốt hơn không, bằng nhờ chính các thành viên khác góp ý.
    + ....

### Tạo merge request:
- Người accept merge request hiện tại: 
    + Assigneee: ChuyenNS, ChungQD
    + Reviewer: NhaLG, DungLV21(nếu change nhiều FE)
- Tạo merge request vào developer hoặc staging luôn với tuỳ độ khẩn cấp của task
- Chủ động push người review để kịp  tiến độ task của mình
- Description trong merge request cần có tối thiểu rõ: 5 mục 
    - Task gắn liền với merge request đó
    - Trong task monday đó cần ghi rõ, có thể clone sang merge request để đỡ review phải vào 2 link.
        - Nguyên nhân bug, nếu là feature to thì đã làm được đến đâu.
        - Phạm vi ảnh hưởng đến các chức năng nào, ảnh hưởng gì.
        - Các case cần lưu ý khi deploy, migrate data, config properties, các service cần deploy kèm theo, và thứ tự deploy nếu có., tài liệu nếu cần cài thêm  lib  bên ngoài..
        - Các case đã test và kết quả đã test là gì
- Tag đủ reviewer, và assign đúng member trên nhóm: gửi cả link merge request kèm.

### Review code:
- Logic code, convension coding, duplicate code, các vấn đề về performent code, khả năng mở rộng nghiệp vụ.
- Các case đã test.
- Review chịu trách nhiệm support nếu task member có bug.

### Deploy dev
- Merge request được approve thì người tạo mr tự build deploy lên môi trường dev. 
- Mỗi member có 1 foler deploy trên server dev, nếu có nhiều bản build khi deploy chỉ nên coppy  bản  deploy  vào tomcat để có thể giữ lại bản  code của mình khi người khác  deploy đè nên.
- Thực hiện backup data nếu cần trên dev trước khi deploy.
- Lưu ý lệnh: Server portal chạy Centos 7, Toàn bộ server còn lại chạy win 10 server và  Ubuntu  lớn hơn version 18.04.
    ```sh
    Xem log hiện tại: tail -f /opt/tomcat/logs/catalina.out
    Thư mục deploy: cd /opt/tomcat/webapps/
    Lệnh restart tomcat: sudo systemctl restart tomcat
    
    Backup database:
    mysqldump -u root -p ubotportal > DB_<enviroment: dev hoặc prod>_bk_ubotportal_<chuyenns>_dd_mm_yyyy.sql 
    >> Nhập passs
    Example: mysqldump -u root -p ubotportal > DB_prod_bk_ubotportal_chuyenns_20_12_2022.sql 
    
    Backup file war deploy:
    cp /opt/tomcat/webapps/ROOT.war ROOT_enviroment: dev hoặc prod_bk_<chuyenns>_21_12_2022_1.war 
    Example: cp /opt/tomcat/webapps/ROOT.war ROOT_prod_bk_21_12_2022_1.war 
    
    Deploy code mới: Đứng tại vị trí trên server: /home/centos
    cp <username>/ubotportal-1.0.0.war /opt/tomcat/webapps/ROOT.war
    Example: cp chuyenns/ubotportal-1.0.0.war /opt/tomcat/webapps/ROOT.war
    
    Backup lại code cũ trong trường hợp bản build mới lỗi:
    cp  <file backup trước đó>.war   /opt/tomcat/webapps/ROOT.war
    Example: cp  ROOT_prod_bk_07_12_2022.war   /opt/tomcat/webapps/ROOT.war
    Sau đó thực hiện restart lại tomcat: sudo systemctl restart tomcat
    ```

### Testing
- Không được tự ý connect vào database, insert data mẫu không đúng trước khi test, tất cả chạy qua liquibase nếu là UBot Portal.
- Báo chị Bình test và gửi lại task monday đi kèm.
- Nếu không pass test thì quay lại vòng fixbug.

## Deploy Live
- Pass hết test case. Qua vòng chị Bình thì được deploy
- Thông báo đến mọi người:
    - Với trường hợp deploy khẩn cấp,  thông báo  vào  nhóm CS -Marketing và nhóm All-Deploy  trước ít nhất  5 phút.
    - Các trường hợp deploy bình thường, báo trong nhóm All-Deploy
    - Cần nêu rõ trong nhóm All-Deploy: Cập nhật những chức năng gì,  fix bug nào, có những gì thay đổi.
- Với trường hợp hotfix critical có thể xem xét test local và lên live luôn với những các bug đã rõ lỗi.
- Thời gian deploy:
    - Task gấp: Tuỳ theo mức độ, có thể  ngay sau thời điểm  deploy xong hoặc 
    - Các future lớn: Deploy sau 8h tối và trước 7h sáng(Member làm task đó follow deploy cùng)
- Người deploy review kỹ lại quy trình cần deploy:  có cần miggrate data truoc hay không,  cần deploy thứ tự service như thế nào, nếu cài các lib và thư viện bên ngoài thì nên thực hiện ra sao.
- Backup lại database cũ, và bản deploy trước đó theo rule trong merge request
- Lưu ý lệnh: Server portal chạy Centos 7, Toàn bộ server còn lại chạy win 10 server và  Ubuntu  lớn hơn version 18.04.
    ```sh
    Xem log hiện tại: tail -f /opt/tomcat/logs/catalina.out
    Thư mục deploy: cd /opt/tomcat/webapps/
    Lệnh restart tomcat: sudo systemctl restart tomcat
    
    Backup database:
    mysqldump -u root -p ubotportal > DB_<enviroment: dev hoặc prod>_bk_ubotportal_<chuyenns>_dd_mm_yyyy.sql 
    >> Nhập passs
    Example: mysqldump -u root -p ubotportal > DB_prod_bk_ubotportal_chuyenns_20_12_2022.sql 
    
    Backup file war deploy:
    cp /opt/tomcat/webapps/ROOT.war ROOT_enviroment: dev hoặc prod_bk_<chuyenns>_21_12_2022_1.war 
    Example: cp /opt/tomcat/webapps/ROOT.war ROOT_prod_bk_21_12_2022_1.war 
    
    Deploy code mới: Đứng tại vị trí trên server: /home/centos
    cp <username>/ubotportal-1.0.0.war /opt/tomcat/webapps/ROOT.war
    Example: cp chuyenns/ubotportal-1.0.0.war /opt/tomcat/webapps/ROOT.war
    
    Backup lại code cũ trong trường hợp bản build mới lỗi:
    cp  <file backup trước đó>.war   /opt/tomcat/webapps/ROOT.war
    Example: cp  ROOT_prod_bk_07_12_2022.war   /opt/tomcat/webapps/ROOT.war
    Sau đó thực hiện restart lại tomcat: sudo systemctl restart tomcat
    ```

# Luồng xử lý:
- Invoice:
    + Listener(Mail-ID) => Handle(Sync Mail) => Download RPA(Download XML) => Extract(XML) => Lookup(Invoice) => Postback(Invoice)
    + Listener: Lấy mail id đẩy vào queue handle
    + Handle: Đọc id mail đi đồng bộ mail xuống bảng  emails, đẩy vào queue  extract và  database 
    + RPA download: download xml nếu email đồng bộ trước đó  không có  xml đẩy vào queue extract
    + Extract: đọc file xml đẩy invoice vào queue lookup
    + Lookup: đọc id invoice từ queue đi tra cứu xong đẩy id vào queuê postback
    + Postback: Đẩy lại luồng data cho khách hàng.

## Thông tin cấu hình server:
- Chỉ có thông tin IP server + Serivce và port đang chạy, cấu hình inbox member trong team.

| Ip |  Server | Service | Enviroment |
| ------ | ------ |  ------ | ------ |
| 103.160.84.151 | Ubuntu 18.04 | ActiveMQ-DEV(Port: 61616, 8161), Extract Service Dev(Port: 5555), AP Fontend Service  | Dev |
| 18.138.178.238 | Centos 7 | MySQL, Ubot Portal Dev, Elasticsearch Dev(9200) | Dev |
| 3.0.176.134 | Centos 7 | .... | Live |
| 192.168.30.50 | Ubuntu 18.04 | MongoDB, ActiveMQ | Live |
