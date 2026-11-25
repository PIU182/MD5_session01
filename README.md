# BÀI TẬP: KIẾN TRÚC PHẦN MỀM, MICROSERVICES VÀ DOMAIN-DRIVEN DESIGN

> **Nội dung:** Bài tập 1 (Khá), Bài tập 2 (Khá), Bài tập 3 (Giỏi), Bài tập 4 (Giỏi).  
> **Mục tiêu:** Hiểu cách lựa chọn kiến trúc phần mềm, phân tích sự đánh đổi khi sử dụng Microservices, tư duy thiết kế theo miền nghiệp vụ (DDD) và phân chia hệ thống thương mại điện tử thành các service.

## Mục lục

1. [Bài tập 1: Tầm quan trọng của kiến trúc và thuộc tính chất lượng](#bài-tập-1-tầm-quan-trọng-của-kiến-trúc-và-thuộc-tính-chất-lượng)
2. [Bài tập 2: Cân nhắc bài toán đánh đổi với Microservices](#bài-tập-2-cân-nhắc-bài-toán-đánh-đổi-với-microservices)
3. [Bài tập 3: Tư duy thiết kế theo Domain (DDD)](#bài-tập-3-tư-duy-thiết-kế-theo-domain-ddd)
4. [Bài tập 4: Phân rã Shopee theo Business Capability](#bài-tập-4-phân-rã-shopee-theo-business-capability)
5. [Kết luận](#kết-luận)

---

## Bài tập 1: Tầm quan trọng của kiến trúc và thuộc tính chất lượng

### Câu 1. Tại sao lựa chọn kiến trúc trước khi bắt đầu dự án lại quan trọng?

**Kiến trúc phần mềm** là cách tổ chức các thành phần của hệ thống, quy định trách nhiệm của từng thành phần, cách chúng giao tiếp và cách hệ thống được triển khai, vận hành.

Việc lựa chọn kiến trúc ngay từ đầu rất quan trọng vì:

1. **Định hướng phát triển:** Giúp nhóm lập trình thống nhất cách tổ chức code, chia module và phối hợp làm việc; tránh mỗi người xây dựng theo một cách khác nhau.
2. **Hạn chế chi phí sửa đổi về sau:** Nếu kiến trúc ban đầu không phù hợp, khi hệ thống phát triển có thể phải sửa lại nhiều chức năng, cơ sở dữ liệu hoặc cách các module giao tiếp.
3. **Đáp ứng nhu cầu mở rộng:** Một kiến trúc phù hợp giúp hệ thống phát triển khi lượng người dùng, dữ liệu và tính năng tăng lên.
4. **Đảm bảo các yêu cầu chất lượng:** Kiến trúc ảnh hưởng trực tiếp đến hiệu năng, bảo mật, khả năng bảo trì, tính sẵn sàng và khả năng kiểm thử.
5. **Giúp quản lý rủi ro:** Nhóm có thể nhận diện sớm những điểm khó như tích hợp bên thứ ba, quá tải hệ thống hoặc yêu cầu dữ liệu nhất quán.

**Ví dụ:** Một startup bán hàng trực tuyến cần ra mắt sản phẩm trong 3 tháng, đội ngũ chỉ có 4 lập trình viên. Nếu chọn Microservices ngay từ đầu, nhóm phải mất thêm công sức cho triển khai nhiều service, giao tiếp qua mạng và giám sát hệ thống. Trong trường hợp này, **Modular Monolith** (ứng dụng đơn khối nhưng chia module rõ ràng) có thể là lựa chọn tốt hơn: phát triển nhanh, dễ quản lý, đồng thời vẫn có khả năng tách thành các service khi thực sự cần.

> **Nhận xét:** Không có một kiến trúc nào tốt nhất cho mọi dự án. Kiến trúc tốt là kiến trúc **phù hợp với quy mô, mục tiêu kinh doanh, nhân lực và yêu cầu chất lượng** của dự án.

### Câu 2. Liệt kê các loại kiến trúc phần mềm phổ biến

| Kiến trúc | Đặc điểm chính | Ví dụ áp dụng |
|---|---|---|
| **Monolithic Architecture (Đơn khối)** | Các chức năng được triển khai chung trong một ứng dụng. | Website quản lý bán hàng nhỏ. |
| **Layered Architecture (Phân lớp)** | Chia thành các lớp như Controller, Service, Repository; mỗi lớp có trách nhiệm riêng. | Ứng dụng Spring Boot quản lý sinh viên. |
| **Microservices Architecture (Vi dịch vụ)** | Hệ thống gồm nhiều dịch vụ độc lập, mỗi dịch vụ phụ trách một nhóm chức năng nghiệp vụ. | Nền tảng thương mại điện tử lớn. |
| **Event-Driven Architecture (Hướng sự kiện)** | Các thành phần trao đổi thông tin thông qua sự kiện, có thể sử dụng Kafka hoặc RabbitMQ. | Gửi thông báo sau khi đơn hàng thanh toán thành công. |
| **Client–Server Architecture (Máy khách – máy chủ)** | Frontend gửi yêu cầu đến backend để xử lý và trả về kết quả. | React gọi REST API của Java Spring Boot. |

**Lưu ý:** Các kiến trúc trên không hoàn toàn loại trừ nhau. Ví dụ, một hệ thống Microservices vẫn có thể dùng kiến trúc phân lớp trong từng service và dùng Event-Driven để giao tiếp giữa các service.

### Câu 3. Các thuộc tính chất lượng (Quality Attributes) của phần mềm

**Quality Attributes** là các tiêu chí dùng để đánh giá **hệ thống hoạt động tốt đến mức nào**, bên cạnh việc hệ thống có đủ chức năng hay không.

| Thuộc tính | Giải thích | Ví dụ |
|---|---|---|
| **Scalability – Khả năng mở rộng** | Hệ thống có thể đáp ứng khi số người dùng hoặc lượng yêu cầu tăng. | Tăng số instance của service xử lý đơn hàng trong đợt giảm giá. |
| **Availability – Tính sẵn sàng** | Hệ thống duy trì khả năng phục vụ người dùng trong khoảng thời gian mong muốn. | Trang mua hàng vẫn truy cập được khi một máy chủ gặp sự cố. |
| **Performance – Hiệu năng** | Khả năng xử lý yêu cầu nhanh, sử dụng tài nguyên hợp lý. | API tìm kiếm sản phẩm trả kết quả trong thời gian ngắn. |
| **Reliability – Độ tin cậy** | Hệ thống hoạt động đúng và ổn định, hạn chế lỗi hoặc mất dữ liệu. | Đơn hàng không bị tạo trùng khi người dùng gửi lại yêu cầu. |
| **Maintainability – Khả năng bảo trì** | Dễ đọc hiểu, sửa lỗi và bổ sung chức năng. | Sửa logic tính giảm giá mà không cần thay đổi module vận chuyển. |
| **Security – Bảo mật** | Bảo vệ dữ liệu và ngăn truy cập trái phép. | Xác thực JWT, phân quyền và mã hóa mật khẩu. |
| **Testability – Khả năng kiểm thử** | Dễ viết và chạy các bài kiểm thử để phát hiện lỗi. | Kiểm thử riêng phần tính tổng tiền đơn hàng. |
| **Observability – Khả năng quan sát** | Có log, metric, trace để theo dõi và xác định nguyên nhân sự cố. | Truy vết yêu cầu đi từ Order Service sang Payment Service. |
| **Usability – Tính dễ sử dụng** | Người dùng dễ học và thao tác với hệ thống. | Quy trình đặt hàng rõ ràng, ít bước. |
| **Interoperability – Khả năng tích hợp** | Có thể trao đổi dữ liệu và làm việc với hệ thống khác. | Tích hợp cổng thanh toán và đơn vị vận chuyển. |

**Mối quan hệ giữa kiến trúc và chất lượng:** Kiến trúc quyết định cách phân chia trách nhiệm, xử lý sự cố, truy cập dữ liệu và mở rộng hệ thống. Vì vậy, kiến trúc phù hợp sẽ giúp đạt được những thuộc tính chất lượng được ưu tiên.

Tuy nhiên, các thuộc tính có thể **đánh đổi** cho nhau. Chẳng hạn, việc chia nhỏ thành Microservices giúp mở rộng từng bộ phận độc lập, nhưng làm giao tiếp qua mạng phức tạp hơn và có thể tăng độ trễ.

**Kết luận bài 1:** Thành công của dự án không chỉ nằm ở việc có đủ chức năng. Một kiến trúc phù hợp còn giúp sản phẩm hoạt động ổn định, dễ phát triển, dễ bảo trì và đáp ứng nhu cầu kinh doanh về lâu dài.

---

## Bài tập 2: Cân nhắc bài toán đánh đổi với Microservices

### 1. So sánh Monolith và Microservices

| Tiêu chí | Monolith | Microservices |
|---|---|---|
| **Cấu trúc** | Các chức năng nằm trong một ứng dụng. | Chia thành nhiều service theo trách nhiệm nghiệp vụ. |
| **Triển khai** | Thường triển khai toàn bộ ứng dụng cùng lúc. | Có thể triển khai từng service độc lập. |
| **Mở rộng** | Thường nhân bản toàn bộ ứng dụng; vẫn có thể mở rộng tốt ở nhiều quy mô. | Có thể mở rộng riêng service đang chịu tải cao. |
| **Phát triển ban đầu** | Thường đơn giản và nhanh hơn với dự án nhỏ. | Tốn thêm công sức thiết lập hạ tầng và giao tiếp giữa các service. |
| **Giao tiếp** | Chủ yếu gọi hàm nội bộ, ít phát sinh độ trễ mạng giữa module. | Giao tiếp qua HTTP, message broker... nên có độ trễ và nguy cơ lỗi mạng. |
| **Dữ liệu** | Thường thuận lợi hơn cho giao dịch trong một cơ sở dữ liệu. | Dữ liệu phân tán, xử lý nhất quán phức tạp hơn. |
| **Vận hành** | Dễ triển khai, theo dõi hơn khi ứng dụng còn nhỏ. | Cần giám sát phân tán, tự động hóa triển khai và xử lý lỗi. |
| **Phù hợp** | Startup, MVP, nhóm nhỏ, nghiệp vụ chưa quá phức tạp. | Hệ thống có nghiệp vụ và quy mô lớn, cần nhiều nhóm triển khai độc lập. |

### 2. Ưu điểm của Microservices

1. **Triển khai độc lập:** Có thể cập nhật Payment Service mà không nhất thiết triển khai lại toàn hệ thống.
2. **Mở rộng linh hoạt:** Khi lượng tìm kiếm sản phẩm tăng mạnh, chỉ cần tăng tài nguyên cho Search Service thay vì mở rộng mọi thành phần.
3. **Phân chia trách nhiệm rõ ràng:** Mỗi service tập trung vào một nhóm nghiệp vụ như đơn hàng, thanh toán hoặc vận chuyển.
4. **Hỗ trợ nhiều nhóm phát triển:** Các nhóm có thể phụ trách những service khác nhau và triển khai theo lịch riêng nếu giao diện tích hợp được thống nhất.
5. **Linh hoạt về công nghệ:** Về mặt kỹ thuật, mỗi service có thể dùng ngôn ngữ hoặc công nghệ phù hợp, dù thực tế vẫn nên giới hạn số công nghệ để dễ quản lý.
6. **Cô lập lỗi tốt hơn khi thiết kế đúng:** Lỗi của service gửi thông báo không nhất thiết khiến toàn bộ hệ thống đặt hàng ngừng hoạt động; vẫn cần timeout, retry và cơ chế dự phòng.

### 3. Nhược điểm và thách thức của Microservices

1. **Vận hành phức tạp:** Phải triển khai, cấu hình và giám sát nhiều ứng dụng; thường cần CI/CD, container và công cụ quan sát hệ thống.
2. **Độ trễ và lỗi mạng:** Gọi API giữa các service chậm hơn gọi hàm trong cùng ứng dụng và có thể gặp timeout, mất kết nối.
3. **Khó đảm bảo nhất quán dữ liệu:** Một đơn hàng có thể liên quan đến Order, Inventory và Payment; rất khó dùng một transaction cơ sở dữ liệu duy nhất cho tất cả service.
4. **Khó kiểm thử và tìm lỗi tổng thể:** Một yêu cầu có thể đi qua nhiều service, nên cần integration test, contract test, logging và distributed tracing.
5. **Chi phí hạ tầng cao hơn:** Nhiều instance, database, message broker và hệ thống giám sát có thể làm tăng chi phí vận hành.
6. **Rủi ro chia service sai:** Nếu tách các chức năng quá nhỏ hoặc phụ thuộc chặt chẽ, các service phải gọi nhau liên tục, làm hệ thống khó thay đổi.
7. **Yêu cầu năng lực đội ngũ:** Nhóm cần hiểu hệ thống phân tán, bảo mật, triển khai và xử lý lỗi liên dịch vụ.

### 4. Ví dụ về sự đánh đổi trong thực tế

Giả sử người dùng đặt hàng:

- Với **Monolith**, phần đặt hàng, trừ kho và cập nhật thanh toán có thể được xử lý trong một ứng dụng và thuận tiện hơn khi dùng chung một database/transaction.
- Với **Microservices**, Order Service phải phối hợp với Inventory Service và Payment Service qua API hoặc sự kiện. Nếu thanh toán thành công nhưng cập nhật đơn hàng thất bại, hệ thống phải có cơ chế thử lại, chống xử lý trùng hoặc quy trình bù trừ để khôi phục trạng thái hợp lý.

Do đó, Microservices có thể giúp triển khai và mở rộng độc lập, nhưng đánh đổi bằng độ phức tạp trong giao tiếp, dữ liệu và vận hành.

### 5. Có nên chuyển toàn bộ hệ thống cũ sang Microservices?

**Không nên chuyển đổi chỉ vì Microservices đang phổ biến.** Doanh nghiệp cần đánh giá:

- Hệ thống hiện tại có thực sự gặp vấn đề về mở rộng, tốc độ triển khai hoặc chia trách nhiệm nhóm không?
- Đội ngũ có đủ năng lực quản lý hệ thống phân tán không?
- Chi phí chuyển đổi và vận hành có tương xứng với lợi ích không?
- Các ranh giới nghiệp vụ đã rõ ràng để tách service chưa?

**Đề xuất:** Nếu hệ thống Monolith hiện tại vẫn đáp ứng tốt, nên ưu tiên cải thiện cấu trúc module, chất lượng code và hệ thống kiểm thử. Khi xuất hiện nhu cầu rõ ràng, có thể tách dần những module độc lập, chịu tải cao hoặc thay đổi thường xuyên thành service; không nhất thiết viết lại toàn bộ cùng lúc.

**Kết luận bài 2:** Microservices **không phải “viên đạn bạc”**. Đây là giải pháp phù hợp với một số bài toán về quy mô và tổ chức, nhưng không tự động làm hệ thống nhanh hơn, rẻ hơn hoặc ổn định hơn.

---

## Bài tập 3: Tư duy thiết kế theo Domain (DDD)

### 1. DDD là gì?

**Domain-Driven Design (DDD)** là phương pháp thiết kế phần mềm lấy **nghiệp vụ (domain)** làm trung tâm. Thay vì chỉ tập trung vào bảng database hoặc công nghệ, nhóm phát triển cần hiểu cách doanh nghiệp hoạt động, các quy tắc nghiệp vụ và cách người dùng trong lĩnh vực đó sử dụng thuật ngữ.

Ví dụ, trong hệ thống thương mại điện tử, những khái niệm quan trọng có thể là **Đơn hàng (Order)**, **Thanh toán (Payment)**, **Tồn kho (Inventory)** và **Vận chuyển (Shipping)**. Mỗi khái niệm có các quy tắc và vòng đời riêng.

### 2. Những khái niệm DDD cần biết

| Khái niệm | Giải thích dễ hiểu | Ví dụ |
|---|---|---|
| **Domain** | Lĩnh vực nghiệp vụ mà phần mềm giải quyết. | Thương mại điện tử. |
| **Ubiquitous Language** (Ngôn ngữ chung) | Các bên cùng sử dụng một bộ thuật ngữ để tránh hiểu sai. | Thống nhất “đơn đã thanh toán” nghĩa là gì. |
| **Bounded Context** (Ngữ cảnh giới hạn) | Ranh giới mà trong đó một mô hình và bộ thuật ngữ có ý nghĩa nhất quán. | Order quản lý đơn hàng; Payment quản lý giao dịch thanh toán. |
| **Entity** (Thực thể) | Đối tượng có danh tính riêng và được theo dõi qua thời gian. | Order có `orderId`. |
| **Value Object** (Đối tượng giá trị) | Đối tượng được nhận diện qua các giá trị thuộc tính, không phải ID riêng. | Địa chỉ giao hàng, số tiền. |
| **Aggregate** | Nhóm đối tượng nghiệp vụ được quản lý và kiểm soát quy tắc nhất quán thông qua một gốc (Aggregate Root). | Order và các OrderItem thuộc cùng một đơn hàng. |
| **Domain Event** (Sự kiện nghiệp vụ) | Sự kiện phản ánh một điều có ý nghĩa đã xảy ra trong nghiệp vụ. | `OrderPlaced`, `PaymentSucceeded`. |

**Lưu ý:** DDD **không đồng nghĩa với Microservices**. Có thể áp dụng DDD trong một Modular Monolith, sau đó cân nhắc tách service khi cần.

### 3. Ưu điểm của DDD

1. **Code bám sát nghiệp vụ:** Cấu trúc hệ thống phản ánh các khái niệm thật của doanh nghiệp, dễ hiểu mục đích từng phần.
2. **Có ngôn ngữ chung:** Lập trình viên, BA, tester và chuyên gia nghiệp vụ trao đổi bằng thuật ngữ thống nhất, giảm hiểu nhầm.
3. **Giảm phụ thuộc giữa các phần:** Khi xác định đúng Bounded Context, những thay đổi ở nghiệp vụ thanh toán ít ảnh hưởng trực tiếp đến nghiệp vụ vận chuyển.
4. **Dễ bảo trì quy tắc phức tạp:** Các quy tắc như tính tiền, áp dụng mã giảm giá, chuyển trạng thái đơn hàng được đặt trong đúng miền nghiệp vụ.
5. **Hỗ trợ phân chia module/service:** Các Bounded Context là cơ sở tham khảo tốt để xây dựng ranh giới module; tuy nhiên không bắt buộc mỗi context phải là một Microservice.

### 4. Nhược điểm và khó khăn của DDD

1. **Khó học ở giai đoạn đầu:** Cần thời gian hiểu Bounded Context, Aggregate, Entity, Value Object và các cách mô hình hóa nghiệp vụ.
2. **Tốn thời gian tìm hiểu nghiệp vụ:** Phải trao đổi với người am hiểu nghiệp vụ để thống nhất thuật ngữ và quy tắc.
3. **Không phù hợp với mọi bài toán:** Một chương trình CRUD nhỏ, nghiệp vụ đơn giản có thể không cần áp dụng đầy đủ DDD.
4. **Ranh giới nghiệp vụ khó xác định:** Nếu tách sai, các module vẫn phụ thuộc chặt chẽ hoặc một quy tắc bị lặp ở nhiều nơi.
5. **Tăng công sức thiết kế ban đầu:** Nhóm cần thảo luận mô hình, kiểm chứng giả định và điều chỉnh khi nghiệp vụ thay đổi.

### 5. Ví dụ áp dụng DDD vào hệ thống bán hàng

Giả sử người dùng đặt một sản phẩm:

- **Order Context:** Quản lý việc tạo đơn, danh sách sản phẩm đã đặt và trạng thái đơn hàng.
- **Payment Context:** Quản lý giao dịch, xác nhận thanh toán và hoàn tiền.
- **Inventory Context:** Quản lý số lượng hàng còn, giữ hàng và xuất kho.

Khi người dùng đặt hàng, Order Context tạo đơn và yêu cầu các context liên quan thực hiện trách nhiệm của mình. Payment Context quyết định giao dịch đã thành công hay chưa; Inventory Context quyết định có đủ hàng để giữ hay không. **Order không trực tiếp sửa bảng thanh toán hoặc số lượng tồn kho** của các context khác.

Cách tổ chức này giúp hạn chế việc nghiệp vụ bị trộn lẫn và giảm tình trạng **Big Ball of Mud** (code phụ thuộc chằng chịt, khó sửa đổi).

**Kết luận bài 3:** Giá trị lớn nhất của DDD là **hiểu đúng và mô hình hóa đúng nghiệp vụ cốt lõi**. DDD không chỉ là sắp xếp folder hay sử dụng một bộ công nghệ cụ thể.

---

## Bài tập 4: Phân rã Shopee theo Business Capability

> **Giả định thiết kế:** Dưới đây là phương án phân rã một nền tảng thương mại điện tử **tương tự Shopee** để làm bài tập. Đây **không phải** sơ đồ kiến trúc nội bộ được xác nhận của Shopee.

### 1. Business Capability là gì?

**Business Capability** là một khả năng mà doanh nghiệp cần có để tạo ra giá trị cho khách hàng, ví dụ: quản lý sản phẩm, bán hàng, thanh toán và giao hàng.

Phân rã theo Business Capability nghĩa là xác định **doanh nghiệp cần làm những việc gì**, rồi xây dựng module/service chịu trách nhiệm rõ ràng cho từng nhóm nghiệp vụ, thay vì chia theo các lớp kỹ thuật như `Controller Service`, `Database Service`.

### 2. Danh sách các module/service chính

| STT | Module / Service | Trách nhiệm chính |
|---|---|---|
| 1 | **Account Service** | Đăng ký, đăng nhập, quản lý tài khoản, hồ sơ và quyền truy cập của người mua/người bán. |
| 2 | **Catalog Service** | Quản lý thông tin sản phẩm, danh mục, mô tả, hình ảnh và thuộc tính sản phẩm. |
| 3 | **Inventory Service** | Quản lý số lượng tồn kho, giữ hàng khi đặt mua và hoàn tồn khi cần. |
| 4 | **Cart Service** | Quản lý giỏ hàng: thêm, sửa số lượng, xóa sản phẩm dự định mua. |
| 5 | **Order Service** | Tạo đơn hàng, lưu các dòng hàng và tổng tiền tại thời điểm đặt, quản lý vòng đời/trạng thái đơn. |
| 6 | **Payment Service** | Tạo và theo dõi giao dịch thanh toán, làm việc với cổng thanh toán, xử lý hoàn tiền. |
| 7 | **Shipping Service** | Tạo yêu cầu vận chuyển, quản lý giao nhận, theo dõi mã vận đơn và trạng thái giao hàng. |
| 8 | **Promotion Service** | Quản lý mã giảm giá, điều kiện áp dụng và số tiền ưu đãi. |
| 9 | **Review Service** | Quản lý đánh giá, số sao và bình luận của khách hàng về sản phẩm/đơn đã mua. |
| 10 | **Notification Service** | Gửi thông báo qua email, SMS hoặc thông báo trong ứng dụng khi có các sự kiện nghiệp vụ. |

### 3. Nguyên tắc tránh chồng chéo trách nhiệm

Để các service độc lập tương đối với nhau, mỗi nghiệp vụ quan trọng nên có **một nơi chịu trách nhiệm chính**:

- **Catalog** quản lý *thông tin mô tả sản phẩm*; **Inventory** quản lý *số lượng còn trong kho*. Hai trách nhiệm này không nên viết lẫn vào nhau.
- **Cart** chỉ lưu *ý định mua hàng*; **Order** lưu *đơn đã được tạo* và dữ liệu mua hàng tại thời điểm đặt.
- **Order** quản lý *vòng đời đơn hàng*; **Payment** quản lý *giao dịch và kết quả thanh toán*.
- **Shipping** quản lý *quá trình giao hàng*; **Notification** chỉ chịu trách nhiệm *gửi thông tin* đến người dùng, không quyết định đơn đã giao thành công hay chưa.
- **Promotion** xác định *điều kiện/giá trị ưu đãi*; **Order** sử dụng kết quả hợp lệ để lưu tổng tiền của đơn khi đặt.

**Nguyên tắc dữ liệu:** Trong phương án Microservices, mỗi service nên sở hữu dữ liệu nghiệp vụ của mình; service khác truy cập thông qua API hoặc sự kiện thay vì sửa trực tiếp bảng database của nhau. Thông tin hiển thị có thể là bản sao dữ liệu cần thiết, nhưng phải xác định nguồn dữ liệu chính (source of truth).

### 4. Ví dụ luồng đặt hàng giữa các service

```text
Người mua
   |
   v
Cart Service (chọn sản phẩm muốn mua)
   |
   v
Order Service (tạo đơn hàng)
   |                  \
   v                   v
Inventory Service     Promotion Service
(kiểm tra/giữ hàng)   (kiểm tra ưu đãi)
   |                   |
   +---------+---------+
             |
             v
      Payment Service
      (xử lý thanh toán)
             |
             v
       Order Service
 (ghi nhận kết quả, cập nhật trạng thái đơn)
             |
             v
      Shipping Service
      (tạo yêu cầu giao hàng)
             |
             v
    Notification Service
     (thông báo người mua)
```

*Sơ đồ là luồng nghiệp vụ minh họa, không phải thứ tự gọi API bắt buộc. Trong hệ thống thực tế, các bước có thể bất đồng bộ; cần thiết kế xử lý thất bại, chống trùng lặp và bù trừ nếu một bước không thành công.*

**Ví dụ cụ thể:**

1. Khách hàng cho tai nghe vào giỏ, Cart Service lưu sản phẩm và số lượng dự định mua.
2. Khi khách xác nhận mua, Order Service phối hợp kiểm tra ưu đãi tại Promotion Service và giữ số lượng hàng tại Inventory Service.
3. Payment Service xử lý giao dịch và công bố kết quả thanh toán.
4. Order Service cập nhật trạng thái đơn theo kết quả nhận được; nếu thanh toán thất bại, hệ thống xử lý hủy đơn hoặc giải phóng hàng đã giữ theo quy tắc nghiệp vụ.
5. Nếu đơn đủ điều kiện giao, Shipping Service tạo yêu cầu vận chuyển.
6. Notification Service gửi thông báo cho người mua dựa trên sự kiện đơn hàng hoặc vận chuyển.

### 5. Vì sao cách chia này phù hợp?

- **Theo nghiệp vụ:** Mỗi service tương ứng với một khả năng kinh doanh rõ ràng.
- **Dễ quản lý trách nhiệm:** Nhóm phát triển biết nên đặt tính năng mới vào đâu.
- **Giảm trùng lặp:** Ví dụ chỉ Payment quản lý trạng thái giao dịch, chỉ Inventory quản lý tồn kho.
- **Hỗ trợ mở rộng chọn lọc:** Khi có nhiều đơn đặt hàng, có thể tăng tài nguyên cho các service xử lý đơn hàng, thanh toán hoặc tồn kho tùy điểm nghẽn thực tế.
- **Dễ phát triển theo giai đoạn:** Ban đầu có thể xây dựng các module này trong một Modular Monolith; khi quy mô và nhu cầu tăng, cân nhắc tách service thích hợp.

**Kết luận bài 4:** Phân rã hệ thống theo Business Capability giúp các module có trách nhiệm rõ ràng, hạn chế chồng chéo và tạo nền tảng để bảo trì, mở rộng hệ thống thương mại điện tử.

---

## Kết luận

Qua bốn bài tập, có thể rút ra những bài học chính:

1. **Kiến trúc phần mềm phải phù hợp với mục tiêu dự án:** Startup cần cân bằng giữa tốc độ ra mắt và khả năng mở rộng, thay vì chạy theo xu hướng công nghệ.
2. **Thuộc tính chất lượng cần được ưu tiên và đánh đổi hợp lý:** Hiệu năng, tính sẵn sàng, bảo mật, khả năng mở rộng và khả năng bảo trì đều ảnh hưởng đến sự thành công của sản phẩm.
3. **Microservices có cả lợi ích và chi phí:** Chỉ nên sử dụng khi các lợi ích về mở rộng, triển khai độc lập và tổ chức nhóm đủ lớn để bù đắp độ phức tạp.
4. **DDD giúp phần mềm phản ánh đúng nghiệp vụ:** Ngôn ngữ chung và ranh giới nghiệp vụ rõ ràng giúp tránh code phụ thuộc chằng chịt.
5. **Phân rã theo Business Capability tạo cấu trúc có ý nghĩa:** Mỗi module/service nên có trách nhiệm chính riêng, phối hợp với phần khác thông qua giao diện rõ ràng.

**Đề xuất cho startup trong đề bài:** Bắt đầu bằng **Modular Monolith được chia theo nghiệp vụ rõ ràng**, bảo đảm code dễ kiểm thử, dễ bảo trì và dễ triển khai. Khi doanh nghiệp tăng trưởng, dựa trên những điểm nghẽn có thật để lựa chọn tách dần thành Microservices. Như vậy vừa tối ưu thời gian đưa sản phẩm ra thị trường, vừa hạn chế rủi ro kỹ thuật lâu dài.
