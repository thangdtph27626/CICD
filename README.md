# Khái niệm về CI/CD

## CI/CD là gì?

CI là viết tắt của Continuous Integration (tích hợp liên tục), CD là viết tắt của Continuous Delivery (chuyển giao liên tục) hoặc Continuous Deployment (triển khai liên tục).

Khái niệm CI/CD thường đề cập đến việc tự động hóa trong quy trình phát triển phần mềm và chuyển giao sản phẩm, giúp cho việc tích hợp diễn ra nhanh hơn và sản phẩm hoàn thiện được chuyển đến người dùng trong thời gian ngắn nhất.

Hiện nay, CI/CD đã được áp dụng rộng rãi vào quy trình làm việc của các doanh nghiệp làm trong lĩnh vực IT, song hành cùng với DevOps và Agile. Một quy trình CI/CD hoàn chỉnh có thể được hình dung như sau: 

- Developer commit code (đẩy code lên server).
- Quy trình CI/CD sẽ tự động chạy build, chạy test và deploy sản phẩm.
- Tiếp tục chuyển giao sản phẩm đến người dùng.

![image](https://github.com/thangdtph27626/CICD/assets/109157942/d103e064-b69d-438f-9bdd-881c2d07018a)

## Quy trình CI/CD tham khảo

Có rất nhiều quy trình, công cụ khác nhau để ứng dụng CI/CD vào dự án. Nội dung của bài viết này dựa trên kinh nghiệm cho các dự án của mình triển khai cũng như đặc thù là các hệ thống web, và viết bằng PHP (PHP hoặc Python hoặc abc…không quá quan trọng trong ngữ cảnh chia sẻ về CI/CD).

Dưới đây là các bước thông thường của quá trình release tính năng trong một dự án thuộc hệ thống Teamcrop.

- Bước 1: [Manual] Khởi tạo repository và có branch default là master và dev. Cài đặt trên Gitlab 9.
- Bước 2: [Manual] Trừ owner ra, thì các coder sẽ push code tính năng lên branch dev
- Bước 3: [Auto] Hệ thống tự động thực hiện test source code, nếu PASS thì sẽ deploy tự động (rsync) code lên server beta.
- Bước 4: [Manual] Tester/QA sẽ vào hệ thống beta để làm UAT (User Acceptance Testing) và confirm là mọi thứ OK.
- Bước 5: [Manual] Coder hoặc owner sẽ vào tạo Merge Request, và merge từ branch dev sang branch master.
- Bước 6: [Manual] Owner sẽ accept merge request.
- Bước 7: [Auto] Hệ thống sẽ tự động thực hiện test source code, nếu PASS sẽ enable tính năng cho phép deploy lên production server.
- Bước 8: [Manual] Owner review là merge request OK, test OK. Tiến hành nhấn nút để deploy các thay đổi lên môi trường production.
- Bước 9: [Manual] Tester/QA sẽ vào hệ thống production để làm UAT và confirm mọi thứ OK. Nếu không OK, Owner có thể nhấn nút Deploy phiên bản master trước đó để rollback hệ thống về trạng thái stable trước đó.
- Bước 10: [Manual] Thắp nhang và hy vọng khách hàng không chửi rủa hoặc email complain.


## Ưu điểm và nhược điểm của CI/CD là gì?

### Ưu điểm 

- Tránh được những lỗi không đáng có: chẳng hạn như lỗi compile (khi đẩy code lên) hoặc các lỗi phát sinh liên quan đến môi trường build sản phẩm. Ví dụ: khi làm thủ công, cùng 1 source code nhưng sẽ có sự khác biệt khi bạn A build trên máy bạn A, bạn B build trên máy bạn B.
- Đảm bảo logic (vì quy trình CI/CD có phần automation test), khi Developer xây dựng tính năng mới sẽ không gây ảnh hưởng đến tính năng cũ.
- Giúp tập trung vào công việc bởi quy trình CI/CD mang tính tự động cao nên Developer không cần phải thực hiện việc build và deploy phần mềm/ứng dụng trên máy cá nhân nữa.
- Nâng cao chất lượng code thông qua quy trình, Developer có thể cài đặt những ràng buộc ngay từ đầu. Ví dụ: pull request khi được tạo ra thì không được quá lớn, không được vượt quá X thay đổi…, điều này góp phần giúp chất lượng pull request ngày càng tốt hơn.
- Phát triển kỹ năng unit test cho Developer thông qua các chỉ số ràng buộc về code coverage (% code đã được cover) được cài đặt trong quy trình CI/CD. Nghĩa là khi phát triển tính năng mới, để không làm giảm chỉ số code coverage, - Developer phải ý thức được tầm quan trọng của unit test và chủ động học hỏi, nâng cao các kỹ năng liên quan.
- Tối ưu tốc độ phát triển của sản phẩm thông qua việc theo dõi thời gian build pipeline (các bước chạy test, build, chạy static code analytics (lint check)).

### Hạn chế

- Trong một dự án nếu có quá nhiều Developer cùng tham gia phát triển sản phẩm, sẽ có thời điểm phát sinh nhiều pull request cần được merge vào branch. Lúc này, các thành viên phải chờ pull request của người trước được merge hoàn tất, - sau đó thực hiện update (cập nhật) lại source code (trong trường hợp có thông báo conflict từ Git repository) và phải trải qua các bước test lại từ đầu. Hệ quả là làm gián đoạn thời gian phát triển sản phẩm.
- Vì sử dụng dịch vụ CI/CD của bên service thứ 3 nên nếu service đó gặp vấn đề và bị crash, bị khai tử thì những dự án áp dụng CI/CD cũng bị ảnh hưởng khá nghiêm trọng.

## Khi nào nên dùng và khi nào không nên dùng quy trình CI/CD?

Quy trình CI/CD có thể yêu cầu một số đầu tư ban đầu, bao gồm thời gian, công sức và tài nguyên. Do đó, quy trình này không nên được sử dụng trong các tổ chức:

- Không có nhu cầu phát triển và triển khai phần mềm một cách nhanh chóng và hiệu quả.
- Không quan tâm đến việc cải thiện chất lượng phần mềm.
- Không có đủ nguồn lực để đầu tư vào quy trình CI/CD.
  
Dưới đây là một số trường hợp cụ thể mà quy trình CI/CD không nên được sử dụng:

- Các dự án nhỏ: Đối với các dự án nhỏ, quy trình CI/CD có thể không cần thiết vì việc phát triển và triển khai phần mềm có thể được thực hiện thủ công một cách hiệu quả.
- Các dự án không có nhiều thay đổi: Đối với các dự án không có nhiều thay đổi, quy trình CI/CD có thể không cần thiết vì việc phát triển và triển khai phần mềm có thể được thực hiện theo cách thủ công một cách hiệu quả.
- Các dự án không có nhiều người tham gia: Đối với các dự án không có nhiều người tham gia, quy trình CI/CD có thể không cần thiết vì việc phát triển và triển khai phần mềm có thể được thực hiện theo cách thủ công một cách hiệu quả.

## Mối liên hệ giữa CI/CD, Agile và DevOps là gì?


CI/CD, Agile và DevOps có mối liên hệ tương đối chặt chẽ. Chúng đều nằm trong top 3 công cụ hỗ trợ xây dựng ứng dụng tuyệt vời cho doanh nghiệp. Mỗi công nghệ đảm nhiệm vai trò, chức năng riêng và có thể tạo nên sự khác biệt vượt trội nếu như kết hợp cùng nhau. Cụ thể như sau:

- Agile – Công nghệ vận hành dựa trên tiến trình lặp lại theo chu kỳ. Mỗi vòng quay lặp lại có thể giải quyết tốt các tồn đọng trong chương trình, từ đó loại bỏ các rào cản, kích thích tăng trưởng nhanh, và bảo mật khá tốt.
- CI/CD: Công nghệ hỗ trợ kiểm tra thường xuyên và liên hoàn. Quy trình hoạt động của CI/CD khá tốn kém, bù lại hiệu suất hoạt động rất tốt, đảm bảo đáp ứng công việc của doanh nghiệp một cách tự động hóa theo vòng tuần hoàn. Bên cạnh đó, CI/CD cũng có tính năng chia sẻ đến các thành viên, giúp mọi người luôn cập nhật công việc nhanh chóng.
- DevOps – Công nghệ tập trung giải quyết các hạn chế của tài nguyên. Trách nhiệm chính của DevOps là giảm thiểu nhiều nhất tác động tiêu cực trong quá trình sản xuất, đồng thời mang lại hiệu năng và chất lượng công việc.

![image](https://github.com/thangdtph27626/CICD/assets/109157942/7822de4f-d1b9-49e6-b811-a25ffe87f17f)
