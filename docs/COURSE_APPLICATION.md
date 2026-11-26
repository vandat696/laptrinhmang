# Vận dụng kiến thức môn Lập trình mạng vào project cờ vua

## Phạm vi và yêu cầu đã thống nhất

Ứng dụng dự kiến là cờ vua trực tuyến, có giao diện và backend chính bằng C. Người dùng yêu cầu vận dụng kiến thức trong 7 bộ slide đã cung cấp khi thiết kế và triển khai project.

Quy mô project nhỏ: ưu tiên vài file C theo chức năng, code dễ đọc và đủ yêu cầu. Người dùng đã chốt [cấu trúc thu gọn](PROJECT_STRUCTURE.md); các kiến thức dưới đây được áp dụng khi phù hợp, không yêu cầu tạo một module hoặc tính năng riêng cho từng chương.

Yêu cầu chức năng và bảng 20 điểm trong [tài liệu ghi nhớ](../PROJECT_NOTES.md) là cơ sở nghiệm thu. Những đề project và bài tập có trong slide dùng để tham khảo kiến thức, không thay thế đề bài người dùng đã gửi. Đặc biệt, các ví dụ project cờ vua ở Chapter 2 ghi mức điểm riêng của slide; không dùng mức đó thay cho bảng 20 điểm hiện tại.

Các lựa chọn kiến trúc bên dưới là đề xuất để thảo luận. Người dùng chưa chốt desktop hay web, hệ điều hành, công nghệ giao diện, mô hình vào/ra socket hoặc định dạng giao thức.

## Tài liệu môn học

Các số slide dưới đây tính theo thứ tự trong file, bắt đầu từ 1.

| Tài liệu | Số slide | Kiến thức liên quan |
| --- | ---: | --- |
| [Chapter 1](<C:/Users/vatme/Desktop/Tài liệu đại học/lập trình mạng/Chapter 1.pptx>) | 30 | Mô hình mạng, TCP/IP, TCP/UDP, client/server, socket |
| [Chapter 2](<C:/Users/vatme/Desktop/Tài liệu đại học/lập trình mạng/Chapter 2.pptx>) | 45 | Lập trình UNIX C/C++, biên dịch, con trỏ, bộ nhớ, chuỗi, file, tiến trình, ví dụ project |
| [Chapter 3](<C:/Users/vatme/Desktop/Tài liệu đại học/lập trình mạng/Chapter 3.pptx>) | 33 | Socket API, địa chỉ IPv4/IPv6, port, chuyển đổi địa chỉ, byte order |
| [Chapter 4](<C:/Users/vatme/Desktop/Tài liệu đại học/lập trình mạng/Chapter 4.pptx>) | 36 | TCP client/server, UDP, broadcast, định dạng thông điệp và truyền file |
| [Chapter 5](<C:/Users/vatme/Desktop/Tài liệu đại học/lập trình mạng/Chapter 5.pptx>) | 29 | Server đa tiến trình, fork, signal, SIGCHLD, wait/waitpid |
| [Chapter 6](<C:/Users/vatme/Desktop/Tài liệu đại học/lập trình mạng/Chapter 6.pptx>) | 21 | pthread, dữ liệu dùng chung, mutex, server đa luồng, truyền file |
| [Chapter 7](<C:/Users/vatme/Desktop/Tài liệu đại học/lập trình mạng/Chapter 7.pptx>) | 20 | Các mô hình I/O, select/poll, fd_set, kết hợp thread và select |

## Cách vận dụng từng chương

### Chapter 1 và mô hình ứng dụng

Nguồn chính: slide 14–18 và 22–29.

- Áp dụng mô hình client/server: người chơi kết nối tới server trung tâm.
- Dùng IP và port để xác định server cần kết nối.
- Đề xuất dùng TCP cho đăng nhập, thách đấu, nước đi, kết quả và file log.
- Thiết kế giao thức ứng dụng cho cờ vua chạy trên TCP. TCP đảm nhiệm truyền byte; các lệnh và ý nghĩa của chúng do project định nghĩa.
- Để server giữ trạng thái bàn cờ và quyết định kết quả, phù hợp yêu cầu server chuyển nước đi và xử lý trận đấu của đề bài.

### Chapter 2 và cấu trúc chương trình C

Nguồn chính: slide 4–6, 8–24 và 42–43.

- Chia chương trình thành vài file C: main, network, account, game, chess và log; dùng server.h cho các khai báo chung khi quy mô còn nhỏ.
- Vận dụng con trỏ, mảng, cấp phát bộ nhớ và thao tác bộ nhớ để quản lý danh sách người chơi, bàn cờ, lịch sử nước đi và bộ đệm truyền nhận.
- Vận dụng xử lý chuỗi để phân tích lệnh nếu chọn payload dạng văn bản.
- Vận dụng file I/O cho log và, nếu chọn lưu file, dữ liệu tài khoản/điểm số.
- Kiểm tra giá trị trả về, errno và báo lỗi khi thao tác mạng hoặc file thất bại.
- Biên dịch phần backend bằng trình biên dịch C; các ví dụ g++ trong slide không làm thay đổi lựa chọn dùng C của project.

Slide 42–43 có ví dụ Chess server/client với hai hướng giao thức văn bản hoặc nhị phân. Ví dụ bao gồm tài khoản, bàn cờ, cập nhật nước đi, xác định kết thúc ván, ghép người rảnh và tự định nghĩa giao thức. Đây là phần liên hệ trực tiếp với project, nhưng project hiện tại còn phải đáp ứng các yêu cầu về xếp hạng, giới hạn thách đấu và file log trong đề bài riêng.

### Chapter 3 và địa chỉ socket

Nguồn chính: slide 6–17 và 24–32.

- Vận dụng socket TCP hai chiều để người chơi gửi yêu cầu và server gửi phản hồi/thông báo trên cùng kết nối.
- Sử dụng cấu trúc địa chỉ và các hàm chuyển đổi địa chỉ phù hợp với nền tảng đã chọn.
- Lấy địa chỉ đầu bên kia từ thông tin kết nối do server nhận được để ghi IP vào log trận đấu.
- Dùng htons/ntohs cho các trường port; dùng htonl/ntohl hoặc cách mã hóa đã quy định nếu giao thức có trường số nguyên nhị phân nhiều byte.
- Nếu hỗ trợ tên miền hoặc IPv6, vận dụng getaddrinfo/getnameinfo theo phạm vi đã thống nhất.

### Chapter 4 và truyền nhận TCP

Nguồn chính: slide 5–20 và 29–35.

- Server sử dụng luồng socket → bind → listen → accept.
- Client sử dụng luồng socket → connect.
- Hai bên gửi/nhận bằng send/recv và đóng socket khi kết thúc phiên kết nối.
- Tự định nghĩa cách đóng khung thông điệp để đáp ứng tiêu chí xử lý truyền dòng. Ví dụ có thể dùng tiền tố độ dài, tương tự định dạng độ dài dữ liệu trong các bài tập; số byte và cấu trúc cụ thể chưa chốt.
- Có bộ đệm nhận riêng cho từng kết nối, giữ phần thông điệp còn thiếu và tách được nhiều thông điệp có trong cùng một lần recv.
- Xử lý gửi chưa hết dữ liệu, lỗi và trường hợp recv trả về 0 khi đầu bên kia đóng kết nối có trật tự.
- Truyền log có thông tin độ dài để client biết khi nào nhận đủ file. Việc nhận log không mặc nhiên yêu cầu đóng kết nối chơi cờ.

UDP và broadcast có thể dùng cho tính năng tìm server trong LAN nếu sau này cần. Tính năng này chưa thuộc phạm vi đã thống nhất; không cần đưa UDP vào đường truyền nước đi chỉ để sử dụng mọi API đã học.

### Chapter 5 và server đa tiến trình

Nguồn chính: slide 5–9 và 14–27.

Vận dụng để đánh giá phương án tạo tiến trình con bằng fork cho kết nối. Nếu chọn mô hình này, phải đóng đúng descriptor ở cha/con và thu hồi tiến trình con bằng wait/waitpid, xử lý SIGCHLD phù hợp.

Với cờ vua, danh sách online, lời thách đấu, bàn cờ và điểm số cần được phối hợp giữa nhiều người chơi. Mô hình fork cho từng kết nối cần thêm cách trao đổi hoặc chia sẻ trạng thái giữa các tiến trình. Vì vậy, đây là phương án để so sánh, chưa phải lựa chọn ưu tiên. Kiến thức signal cũng liên quan đến việc dừng server và thu hồi tài nguyên đúng cách.

### Chapter 6 và server đa luồng

Nguồn chính: slide 5–15 và 18–20.

- Nếu chọn mô hình đa luồng, vận dụng pthread_create và quản lý vòng đời thread bằng join/detach phù hợp thiết kế.
- Dùng mutex bảo vệ dữ liệu được nhiều thread truy cập: tài khoản, người chơi online, lời mời, bàn cờ, điểm số và file dùng chung.
- Gắn thao tác kiểm tra và cập nhật với nhau để tránh hai lời mời cùng đưa một người vào hai trận hoặc một trận bị cộng điểm hai lần.
- Nếu nhiều thread có thể gửi tới cùng một socket, cần tuần tự hóa việc gửi trọn thông điệp để các byte của những thông điệp không xen lẫn nhau.
- Không giữ mutex bảo vệ trạng thái chung trong lúc chờ truyền mạng kéo dài; xác định rõ thứ tự khóa và phạm vi khóa khi thiết kế.

### Chapter 7 và I/O multiplexing

Nguồn chính: slide 4–7 và 11–19.

- Có thể dùng select với FD_ZERO, FD_SET, FD_CLR, FD_ISSET để theo dõi socket lắng nghe và các kết nối người chơi.
- Phân biệt socket sẵn sàng đọc với việc đã nhận đủ một thông điệp. Bộ đệm và bộ phân tích giao thức vẫn phải xử lý dữ liệu thiếu/gộp.
- Nếu chọn vòng lặp I/O không chặn, duy trì bộ đệm gửi và theo dõi khả năng ghi để client chậm không giữ toàn bộ server.
- Có thể dùng timeout để kiểm tra lời mời hết hạn hoặc công việc định kỳ nếu thống nhất có các chức năng đó.
- Mô hình kết hợp thread và select được giới thiệu trong slide 18–19 là phương án mở rộng, chưa phải yêu cầu của project.

Các giá trị giới hạn descriptor và chi tiết API cần được kiểm tra theo hệ điều hành khi triển khai; không lấy một con số minh họa trong slide làm giới hạn chung cho mọi nền tảng.

## Đối chiếu với tiêu chí chấm điểm

Các kiến thức trong slide là nền tảng lập trình. Logic tài khoản, xếp hạng, lời mời và luật cờ vẫn cần được thiết kế riêng cho project.

| Tiêu chí | Điểm | Kiến thức vận dụng | Nội dung cần thể hiện trong project |
| --- | ---: | --- | --- |
| Xử lý truyền dòng | 1 | Chapter 3–4: byte stream, send/recv, định dạng dữ liệu | Đóng khung, nhận thiếu/gộp, gửi thiếu, bộ đệm từng kết nối |
| Cơ chế vào/ra socket trên server | 2 | Chapter 4–7: TCP server, fork, pthread, select | Chọn và giải thích mô hình phục vụ nhiều client; xử lý lỗi và đóng kết nối |
| Đăng ký và quản lý tài khoản | 2 | Chapter 2: chuỗi, bộ nhớ, file; Chapter 4: giao thức | Lệnh đăng ký, kiểm tra dữ liệu, lưu tài khoản; đồng bộ nếu có truy cập song song |
| Đăng nhập và quản lý phiên | 2 | Chapter 2–4: dữ liệu, kết nối, giao thức | Gắn kết nối với người chơi đã xác thực, theo dõi trạng thái phiên |
| Danh sách người chơi sẵn sàng | 2 | Chapter 1: client/server; Chapter 2: danh sách; Chapter 6 hoặc 7: phối hợp truy cập | Lọc online và chưa trong trận, gửi danh sách cho client |
| Chuyển lời thách đấu | 2 | Chapter 2: xử lý dữ liệu; Chapter 4: gửi/nhận; Chapter 6 hoặc 7: quản lý trạng thái | Kiểm tra chênh lệch tối đa 10 bậc và chuyển tới đúng người |
| Từ chối hoặc nhận lời | 1 | Chapter 4: giao thức; Chapter 6 hoặc 7: quản lý trạng thái | Theo dõi lời mời, xác nhận khả năng tham gia và tạo trận hợp lệ |
| Chuyển thông tin nước đi | 2 | Chapter 3–4: TCP hai chiều, send/recv | Xác định trận và người gửi, chuyển nước đi được chấp nhận tới hai bên |
| Kiểm tra nước đi hợp lệ | 1 | Chapter 2: lập trình và dữ liệu | Module luật cờ vua bằng C, chạy trên server |
| Xác định kết quả ván cờ | 1 | Chapter 2: xử lý trạng thái; Chapter 6 hoặc 7: phối hợp truy cập | Xác định kết thúc, lưu kết quả và cập nhật điểm đúng một lần |
| Ghi log | 1 | Chapter 2: file I/O; Chapter 3: địa chỉ IP | Ghi bắt đầu, kết thúc, IP hai bên và danh sách nước đi |
| Chuyển kết quả và log | 2 | Chapter 2: đọc file; Chapter 4 và 6: truyền file | Gửi kết quả và đủ byte của file log cho cả hai client |
| Xin ngừng ván | 1 | Chapter 4: lệnh giao thức; Chapter 6 hoặc 7: quản lý trạng thái | Xử thua người xin ngừng, kết thúc trận, cập nhật điểm và trạng thái sẵn sàng |

## Đề xuất kiến trúc để thảo luận

Đề xuất ban đầu là ứng dụng desktop kết nối TCP trực tiếp đến server C. Server dùng API POSIX trên Linux để bám sát các header, fork, pthread và select trong slide. Nếu phát triển trên máy Windows, có thể cân nhắc môi trường Linux qua WSL hoặc máy ảo; môi trường cụ thể chưa chốt.

Hai phương án vào/ra socket phù hợp để lựa chọn:

| Phương án | Kiến thức chính | Lợi ích cho project | Công việc cần chú ý |
| --- | --- | --- | --- |
| Một vòng lặp select | Chapter 4 và 7 | Trạng thái người chơi và bàn cờ tập trung, xử lý các thay đổi tuần tự | Bộ đệm từng kết nối, gửi/nhận không chặn, tránh thao tác lâu trong vòng lặp |
| pthread cho từng kết nối | Chapter 4 và 6 | Dễ liên hệ bài server đa luồng, các thread chia sẻ dữ liệu | Mutex, vòng đời thread, tranh chấp trạng thái và gửi cùng socket |

Chọn một mô hình chính và giải thích bằng yêu cầu của project. Không cần ghép fork, pthread và select chỉ để có đủ tên các chương. Nếu giảng viên có yêu cầu cụ thể về mô hình server, yêu cầu đó cần được bổ sung trước khi chốt.

Giao diện có thể dùng công nghệ riêng, nhưng các chức năng backend theo tiêu chí chấm điểm phải được thực hiện trong C. Nếu có yêu cầu phần socket phía client cũng dùng C, cần đưa yêu cầu đó vào thiết kế client.

## Trình tự áp dụng

1. Chốt môi trường chạy và mô hình vào/ra server dựa trên slide.
2. Định nghĩa giao thức cờ vua, định dạng thông điệp và mã phản hồi.
3. Làm server C và client dòng lệnh để kiểm chứng socket và truyền dòng.
4. Hoàn thiện tài khoản, phiên, danh sách sẵn sàng và thách đấu.
5. Tích hợp luật cờ, nước đi, kết quả, điểm số và xin ngừng ván.
6. Hoàn thiện ghi/truyền log và tích hợp giao diện.
7. Demo nhiều client, thông điệp thiếu/gộp, client đóng kết nối và các tình huống tranh chấp liên quan đến mô hình đã chọn.
