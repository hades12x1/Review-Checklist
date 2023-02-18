# Review checklist
## UBot - 22/12/2022

[![Build Status](https://travis-ci.org/joemccann/dillinger.svg?branch=master)](https://travis-ci.org/joemccann/dillinger)

## Review checklist deploy service UBot: 
- Detail coding-guideline Java: https://bom.so/yb1Fvv
- Chọn 1 trong 3 loại merge request template: Bug, Refactor, Feature trong khi tạo merge request.

## Checkout source code
- Luôn pull code mới nhất từ staging và develop hằng ngày và trước khi tạo merge request, đảm bảo code  người push luôn mới nhất so với nhánh merge vào, nếu có tự resolve conflig ở local trước.
- Checkout từ nhánh staging.
- Develop big feature gồm cả fontend và backend nên checkout ra nhánh riêng từ staging, fe và be có thể dev cùng trên nhánh đó, hoặc lấy nhánh đó là nhánh chung cho 2 bên tách ra tiếp.
- Branch name: 
    + New feature: dev-[tên_chức_năng]
    + Bug: hf-[tên_chức_năng]

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
- Check lại source code đã follow các rule về clean code search google hoặc tham khảo link: https://bom.so/yb1Fvv
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
- Folder: .gitlab/merge_request_templates tại git hiện tại chứa các merge request template.
- Nếu project chưa có merge template mẫu: thực hiện coppy folder ".gitlab" vào root project.
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
