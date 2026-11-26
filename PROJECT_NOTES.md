# Ghi nhớ project môn Lập trình mạng

Ngày ghi nhận: 26/11/2026.

Project xây dựng ứng dụng chơi cờ trực tuyến. Sinh viên được tự chọn loại cờ. Tài liệu lưu yêu cầu và thang điểm do người dùng cung cấp, làm cơ sở cho những lần thảo luận và triển khai tiếp theo.

## Trạng thái hiện tại

- Đã tiếp nhận đề bài và các tiêu chí chấm điểm.
- Loại cờ dự kiến: **cờ vua**.
- Backend chính và các phần đáp ứng tiêu chí chấm điểm sẽ được triển khai bằng **C**.
- Ứng dụng sẽ có giao diện; chưa chọn công nghệ frontend.
- Thiết kế và triển khai cần vận dụng kiến thức trong 7 bộ slide Chapter 1–7 do người dùng cung cấp.
- Quy mô nhỏ: ưu tiên code đơn giản, dễ hiểu, ít file/thư mục và đủ yêu cầu đề bài.
- Đã chốt cấu trúc thu gọn ngày 26/11/2026; khi phân công ba người, bổ sung lobby.c để tránh cùng sửa game.c. Server C gồm 7 file .c và server.h, cùng các thư mục client, data, logs và tài liệu.
- Nhóm có 3 thành viên: Văn Thành Đạt, Đặng Hoàng Minh và Trần Văn Duy. Xem [kế hoạch phân công đến khi hoàn thành](docs/TEAM_PLAN.md).
- Yêu cầu phân công: chia đều phần xử lý mạng được chấm điểm và các việc còn lại; mỗi file/màn hình có một người phụ trách để tránh sửa chồng.
- Chưa chốt nền tảng chạy, cơ sở dữ liệu hoặc thiết kế giao thức.
- Người dùng sẽ thảo luận và cung cấp thêm thông tin sau. Các lựa chọn triển khai cần được cập nhật theo những trao đổi đó.

## Định hướng triển khai đã thống nhất

Người dùng học lập trình mạng và các giao thức bằng C. Vì vậy, phần backend chính và việc thực hiện các tiêu chí chấm điểm phải thể hiện bằng mã C.

Phần C sẽ đảm nhiệm xử lý truyền dòng, vào/ra socket trên server, tài khoản, phiên đăng nhập, danh sách người chơi sẵn sàng, thách đấu, xử lý nước đi, kiểm tra nước đi hợp lệ, xác định kết quả, điểm số, ghi và chuyển log, cùng xử lý xin ngừng ván.

Frontend cung cấp giao diện cho người chơi và giao tiếp với backend C. Server C chịu trách nhiệm kiểm tra và quyết định trạng thái trận đấu, kết quả và điểm số. Công nghệ frontend và cách kết nối tới backend sẽ được thảo luận sau.

Người dùng yêu cầu thu gọn thiết kế: chia theo chức năng thành vài file C, ưu tiên struct, mảng và các hàm dễ đọc. Chỉ thêm lớp trung gian, thư mục hoặc công nghệ khi cần để đáp ứng yêu cầu cụ thể.

Ưu tiên xuyên suốt là dễ hiểu và đúng kiến thức môn học: người dùng cần giải thích được luồng xử lý, các lời gọi socket và cách xử lý dữ liệu. Giữ đủ chức năng bắt buộc; không thêm tính năng ngoài đề nếu chưa có yêu cầu.

Phân công cập nhật: Đạt làm mạng server/tài khoản và màn hình tài khoản/khung app; Minh làm mạng client, sảnh/thách đấu, luật cờ và màn hình sảnh; Duy làm truyền nước đi/kết quả/log, quản lý trận và màn hình bàn cờ/kết quả. Nhóm tiêu chí phụ trách cân đối khoảng 7–7–6 điểm. Mỗi người tự kiểm tra và viết báo cáo phần mình, cùng tích hợp theo mốc; không giao toàn bộ GUI hoặc toàn bộ xử lý mạng cho một người.

## Kiến thức môn học cần vận dụng

Đã ghi nhận 7 bộ slide về mô hình mạng, lập trình UNIX C/C++, socket TCP/UDP, server đa tiến trình, server đa luồng và I/O multiplexing. Xem [bản đối chiếu kiến thức với project và thang điểm](docs/COURSE_APPLICATION.md) để biết các chương, số slide và cách áp dụng cụ thể.

- Phần mạng bằng C cần thể hiện socket API, giao thức ứng dụng tự định nghĩa và xử lý truyền dòng.
- Chọn mô hình phục vụ nhiều client dựa trên fork, pthread/mutex hoặc select đã học, tùy thiết kế phù hợp.
- Vận dụng quản lý bộ nhớ, chuỗi và file I/O cho dữ liệu người chơi, thông điệp và log.
- Ví dụ cờ vua ở Chapter 2, slide 42–43, là tài liệu tham khảo; bảng yêu cầu và thang điểm do người dùng gửi vẫn là cơ sở nghiệm thu.
- Desktop kết nối TCP, server C chạy trên Linux và lựa chọn select hoặc pthread hiện là các đề xuất để thảo luận, chưa phải quyết định đã chốt.

## Thiết kế thư mục đã chốt

Xem [cấu trúc thư mục đã thu gọn](docs/PROJECT_STRUCTURE.md).

- server gồm main.c, server.h, network.c, account.c, lobby.c, game.c, chess.c và log.c.
- client chứa giao diện và phần kết nối mạng, chưa chia nhiều thư mục con.
- res chứa quân cờ tại pieces/white và pieces/black, cùng theme tại themes. Người dùng sẽ cung cấp tài nguyên yêu thích; quân cờ và theme trong bản phác thảo hiện tại chưa được chốt.
- data chứa tài khoản/điểm nếu lưu file; logs chứa log trận đấu.
- docs giữ tài liệu; output giữ phác thảo giao diện hiện có.
- Người dùng đã xác nhận cấu trúc thu gọn này. Đã tạo thư mục chính và khung 7 file C cùng server.h, có ghi người phụ trách/nhiệm vụ. main.c mới in thông báo, chưa triển khai socket hoặc chức năng nghiệp vụ. Công nghệ giao diện, mô hình I/O và định dạng dữ liệu còn cần chốt.
- Rà soát theo bảng điểm: cấu trúc đã phân đủ trách nhiệm cho 13 tiêu chí, nhưng chưa thể xác nhận sản phẩm đạt yêu cầu khi chưa có mã nguồn và kiểm chứng. Các chi tiết còn thiếu được ghi trong phần rà soát của tài liệu cấu trúc.

## Yêu cầu chức năng

### Tài khoản và phiên đăng nhập

- Cho phép đăng ký tài khoản mới.
- Cho phép đăng nhập vào tài khoản để chơi.
- Quản lý tài khoản và phiên đăng nhập theo các tiêu chí chấm điểm bên dưới.

### Điểm số và bảng xếp hạng

Người chơi tích lũy điểm qua các trận đấu để phân hạng.

| Kết quả của người chơi | Thay đổi điểm |
| --- | ---: |
| Thắng | +3 |
| Hòa | 0 |
| Thua | -3 |

Mỗi người chơi chỉ được gửi lời thách đấu cho người có vị trí hơn hoặc kém mình tối đa 10 bậc trên bảng xếp hạng. Giới hạn này tính theo **bậc xếp hạng**, không phải chênh lệch điểm số.

### Danh sách người chơi sẵn sàng

Khi một người chơi kết nối tới, server chuyển cho người đó danh sách người chơi sẵn sàng. Một người chơi sẵn sàng phải đồng thời thỏa mãn hai điều kiện:

- Đang online.
- Chưa tham dự ván chơi nào.

### Thách đấu

- Tiếp nhận và chuyển lời thách đấu giữa các người chơi.
- Áp dụng giới hạn chênh lệch tối đa 10 bậc xếp hạng khi gửi lời thách đấu.
- Cho phép người nhận chấp nhận hoặc từ chối lời thách đấu.

### Diễn biến và kết quả ván cờ

- Chuyển nước cờ qua lại giữa hai người chơi.
- Kiểm tra tính hợp lệ của nước đi.
- Server quyết định kết quả của mỗi ván cờ.

### Ghi log và chuyển log

Khi ván cờ kết thúc, chuyển kết quả và file log trận đấu cho cả hai người chơi. Log phải chứa:

- Thời điểm bắt đầu.
- Thời điểm kết thúc.
- Địa chỉ IP của hai đối thủ.
- Danh sách các nước đi.

### Xin ngừng ván cờ

- Cho phép người chơi xin ngừng ván hiện tại và chuyển sang ván mới, với người chơi khác nếu cần.
- Người xin ngừng ván đấu bị xử thua.
- Áp dụng quy tắc điểm số theo kết quả: người xin ngừng bị trừ 3 điểm, đối thủ thắng được cộng 3 điểm.

## Thang điểm của đề bài

| STT | Tiêu chí | Điểm |
| ---: | --- | ---: |
| 1 | Xử lý truyền dòng | 1 |
| 2 | Cài đặt cơ chế vào/ra socket trên server | 2 |
| 3 | Đăng ký và quản lý tài khoản | 2 |
| 4 | Đăng nhập và quản lý phiên | 2 |
| 5 | Cung cấp danh sách người chơi sẵn sàng | 2 |
| 6 | Chuyển lời thách đấu | 2 |
| 7 | Từ chối/Nhận lời thách đấu | 1 |
| 8 | Chuyển thông tin nước đi | 2 |
| 9 | Kiểm tra tính hợp lệ của nước đi | 1 |
| 10 | Xác định kết quả ván cờ | 1 |
| 11 | Ghi log | 1 |
| 12 | Chuyển kết quả và log ván cờ | 2 |
| 13 | Xin ngừng ván cờ | 1 |
| | **Tổng theo bảng được cung cấp** | **20** |

## Các nội dung cần chốt khi thảo luận tiếp

Những mục dưới đây chưa được đề bài quy định cụ thể; cần thống nhất trước khi triển khai phần liên quan.

- Phạm vi luật cờ vua cần hỗ trợ, bao gồm các nước đi đặc biệt và điều kiện thắng, thua, hòa.
- Nền tảng chạy client/server, công cụ biên dịch C và công nghệ frontend.
- Giao thức truyền thông, cách phân tách thông điệp trong dòng dữ liệu và cơ chế vào/ra socket trên server.
- Cách lưu tài khoản, điểm số, phiên và dữ liệu trận đấu.
- Điểm ban đầu, việc cho phép điểm âm, cách xếp hạng khi bằng điểm và thời điểm lấy thứ hạng để kiểm tra lời thách đấu.
- Cách cập nhật danh sách sẵn sàng khi người chơi đăng nhập, đăng xuất, bắt đầu hoặc kết thúc ván.
- Cách xử lý lời thách đấu đồng thời, hết hạn hoặc khi người được thách đấu không còn sẵn sàng.
- Cách xử lý mất kết nối, đăng nhập đồng thời, giới hạn thời gian và khả năng kết nối lại.
- Định dạng file log và cách chuyển file cho hai người chơi.
- Quy mô demo, môi trường chạy, thời hạn và yêu cầu nộp bài bổ sung nếu có.

## Nguyên tắc cập nhật ghi nhớ

- Giữ rõ yêu cầu của đề bài, quyết định đã thống nhất và nội dung còn chờ quyết định.
- Chỉ ghi một lựa chọn kỹ thuật là đã chốt khi có trao đổi xác nhận lựa chọn đó.
- Khi có yêu cầu mới hoặc thay đổi, cập nhật các mục liên quan để tài liệu phản ánh trạng thái hiện tại.
