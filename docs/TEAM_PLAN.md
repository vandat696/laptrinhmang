# Phân công nhóm project cờ vua

Nhóm gồm Văn Thành Đạt, Đặng Hoàng Minh và Trần Văn Duy. Phân công theo yêu cầu chia đều phần lập trình mạng và các việc còn lại, mỗi file có một người viết chính. Chưa có hạn nộp nên dùng mốc hoàn thành thay cho ngày cụ thể.

## Phân công chính

| Thành viên | Phần mạng phụ trách | Phần còn lại | File phụ trách |
| --- | --- | --- | --- |
| Văn Thành Đạt | Socket server, phục vụ nhiều client, truyền dòng, giao thức đăng ký/đăng nhập và phiên | Lưu tài khoản/điểm, xếp hạng; giao diện tài khoản và khung mở/chuyển màn hình | server/main.c, network.c, account.c, server.h; client/app.*, auth.* |
| Đặng Hoàng Minh | Kết nối và nhận/gửi phía client; danh sách sẵn sàng; chuyển, nhận/từ chối thách đấu | Luật cờ vua; giao diện sảnh | server/lobby.c, chess.c; client/network.*, lobby.* |
| Trần Văn Duy | Chuyển nước đi, thông báo kết quả, xin ngừng; truyền và nhận đủ file log | Trạng thái trận, ghi log; giao diện bàn cờ/kết quả và gắn tài nguyên | server/game.c, log.c; client/board.*, result.*; res/ |

Dấu * chỉ phần mở rộng sẽ chọn theo công nghệ client, không yêu cầu tạo nhiều loại file. Tách phần sảnh/thách đấu khỏi game.c thành lobby.c để Minh và Duy có file riêng. Server vẫn gọn với 7 file .c và server.h, không có tầng trung gian mới.

Cả ba đều triển khai một luồng mạng từ yêu cầu, xử lý, phản hồi đến cập nhật client. Không chia một người chỉ làm giao diện hoặc chỉ làm báo cáo.

## Ranh giới để tránh làm trùng

- Đạt sở hữu tài khoản, phiên và hàm cập nhật điểm; tổng hợp server.h sau khi cả nhóm thống nhất các khai báo. Minh và Duy gọi hàm, không tự ghi lại dữ liệu tài khoản.
- Minh sở hữu danh sách/lời mời trong lobby.c. Khi nhận lời, Minh gọi hàm tạo trận của Duy; hàm đó kiểm tra lại hai người còn sẵn sàng và chỉ tạo một trận hợp lệ.
- Minh sở hữu luật trong chess.c: kiểm tra nước đi, cập nhật bàn cờ theo luật và nhận diện kết quả từ thế cờ. File này không gửi socket hoặc cộng điểm.
- Duy sở hữu vòng đời trận trong game.c: tạo trận, kiểm tra người chơi/lượt, gọi luật cờ, chuyển nước đi, xử lý xin ngừng và kết thúc. Chỉ một hàm kết thúc trận của Duy gọi cập nhật điểm của Đạt, hoàn tất log và gửi kết quả, tránh xử lý hai lần.
- Đạt quản lý socket và bộ đệm phía server. Minh quản lý kết nối và bộ đệm phía client, phát thông điệp đã nhận cho hàm xử lý tính năng của mỗi người. Mỗi kết nối chỉ có một nơi đọc socket.
- Duy thực hiện giao thức truyền file log và lưu đủ dữ liệu nhận được; dùng hàm gửi chung, không tạo vòng recv riêng tranh dữ liệu với phần nhận của Minh.
- Hình quân cờ/theme nằm trong res, Duy gắn vào giao diện. Đạt và Minh xây màn hình của mình bằng các màu/tài nguyên đã thống nhất, không tự tạo theme khác.

Các tên như app, auth, network, lobby, board và result là tên file dự kiến trong client. Mỗi người tự kiểm tra màn hình mình phụ trách. Khung app do Đạt nối các màn hình, không chứa lại logic của chúng.

## Giao thức và hàm chung cần chốt trước

Cả ba thống nhất ngắn gọn các nhóm thông điệp tài khoản, sảnh/thách đấu, nước đi/kết quả và file log; ranh giới thông điệp, mã lỗi và giới hạn độ dài. Mỗi người chuẩn bị ví dụ yêu cầu/phản hồi của mình, Duy tổng hợp vào docs/PROTOCOL.md khi thiết kế giao thức. Không sửa đặc tả theo ba hướng khác nhau.

Thống nhất struct người chơi/trận/bàn cờ và các hàm cần gọi giữa các file: lấy phiên/thứ hạng, gửi thông điệp, tạo trận, xử lý nước đi, nhận diện kết quả, kết thúc trận và ghi/gửi log. Đây là các hàm C thông thường. Chốt tên và tham số trước khi làm riêng; Đạt tổng hợp header chung. Nếu GUI dùng ngôn ngữ khác, phần client vẫn phải tuân thủ đặc tả, không mặc nhiên dùng trực tiếp header C.

Môi trường, mô hình I/O, công nghệ GUI, điểm ban đầu, bằng điểm, phạm vi luật cờ và quy tắc mất kết nối còn cần được thống nhất. Không tự coi những lựa chọn này đã chốt qua việc chia người.

## Các mốc đến khi hoàn thành

| Mốc | Đạt | Minh | Duy | Điều kiện hoàn thành |
| --- | --- | --- | --- | --- |
| 1. Thống nhất | Đề xuất I/O server, framing và phiên; tổng hợp header | Đề xuất phía nhận client, dữ liệu sảnh và luật | Đề xuất dữ liệu trận, kết quả và định dạng truyền log | Chốt môi trường, GUI, giao thức/hàm chung và quy tắc còn thiếu |
| 2. Làm nền song song | Server nhận nhiều client; đăng ký/đăng nhập; màn hình tài khoản | Client kết nối server; bàn cờ/luật thử độc lập; sảnh dữ liệu mẫu | Hàm tạo/kết thúc trận thử, ghi/gửi log mẫu, bàn cờ/kết quả dữ liệu mẫu | Có yêu cầu/phản hồi thật; cả ba có phần chạy thử mà không đợi toàn bộ project |
| 3. Ghép tài khoản và thách đấu | Ghép tài khoản/phiên/điểm và chuyển màn hình | Danh sách sẵn sàng, gửi/nhận/từ chối lời mời và giới hạn 10 bậc | Tạo trận từ lời mời hợp lệ, đưa hai client vào bàn cờ | Hai client đăng nhập, thách đấu và vào cùng một trận; client khác vẫn dùng sảnh được |
| 4. Ghép ván đấu hoàn chỉnh | Bảo đảm truyền nhận và lưu điểm, hỗ trợ kiểm tra thế cờ | Hoàn thiện kiểm tra luật và kết quả theo thế cờ | Chuyển nước đi, kết quả/xin ngừng, truyền/nhận log và trở lại sảnh | Chơi trọn ván, điểm đúng một lần, hai client có đủ log và chơi được ván tiếp |
| 5. Kiểm tra và nộp | Demo mạng server/tài khoản, viết phần báo cáo và cách build tương ứng | Demo mạng client/sảnh/thách đấu/luật, viết phần báo cáo tương ứng | Demo nước đi/kết quả/truyền file, viết phần báo cáo tương ứng | Kiểm đủ 13 tiêu chí, chạy trên máy dự kiến demo, hoàn thiện báo cáo và tập bảo vệ |

Ghép code từng mốc. Mỗi người sửa lỗi phần mình, không dồn việc ghép và kiểm thử cho Đạt hoặc Duy. Luật cờ là phần có thể tốn công nhất: Đạt chuẩn bị thêm thế cờ/test, Duy kiểm tra qua GUI; Minh sửa chess.c. Giao diện giữ tối thiểu để không lấn thời gian học phần mạng.

## Đối chiếu tiêu chí chấm

Phân chia dưới đây gần 7–7–6 theo trọng số của đề, dùng để cân đối trách nhiệm, không phải điểm cá nhân được bảo đảm. Các luồng tích hợp cần sự phối hợp của cả ba.

| Tiêu chí | Điểm | Người phụ trách chính |
| --- | ---: | --- |
| Xử lý truyền dòng | 1 | Đạt; Minh thực hiện đúng cùng quy tắc phía client |
| Cơ chế vào/ra socket trên server | 2 | Đạt |
| Đăng ký và quản lý tài khoản | 2 | Đạt |
| Đăng nhập và quản lý phiên | 2 | Đạt |
| Danh sách người chơi sẵn sàng | 2 | Minh |
| Chuyển lời thách đấu và giới hạn 10 bậc | 2 | Minh |
| Nhận hoặc từ chối lời thách đấu | 1 | Minh |
| Kiểm tra tính hợp lệ của nước đi | 1 | Minh |
| Nhận diện kết quả từ thế cờ | 1 | Minh; Duy điều phối kết thúc trận |
| Chuyển thông tin nước đi | 2 | Duy |
| Ghi log | 1 | Duy |
| Chuyển kết quả và log cho hai client | 2 | Duy |
| Xin ngừng ván | 1 | Duy |
| Tổng | 20 | Đạt 7, Minh 7, Duy 6 |

## Phối hợp và bàn giao

- Mỗi người một nhánh Git, ví dụ dat-server, minh-lobby, duy-match. Không tự sửa file người khác đang làm; khi cần đổi giao diện hàm, thống nhất rồi để chủ file cập nhật. Sau khi ghép, cùng chạy lại luồng liên quan.
- Mỗi người viết và thử cả tình huống thành công lẫn lỗi của mình: Đạt thử tài khoản, phiên, dữ liệu thiếu/gộp và nhiều kết nối; Minh thử phía nhận client, thách đấu chênh 10/11 bậc, lời mời đồng thời và luật; Duy thử sai lượt, kết quả trùng, xin ngừng và nhận thiếu file log.
- Cùng thử mất kết nối theo quy tắc đã thống nhất, trạng thái sẵn sàng, điểm không bị cộng hai lần và hai người chơi nhận đủ log.
- Mỗi người viết ngắn phần báo cáo và hướng dẫn chạy của mình, chuẩn bị minh chứng/demo mạng riêng. Cả ba hiểu toàn bộ luồng và có thể giải thích phần giao tiếp giữa các module.
- Chia việc chung: Đạt tổng hợp README và cách build/chạy; Minh tổng hợp danh sách kiểm tra và báo cáo từ phần viết riêng của ba người; Duy tổng hợp đặc tả giao thức. Mỗi tài liệu có một người cập nhật chính, các thành viên gửi nội dung phần mình để ghép.
- Chừa thời gian cuối cho tích hợp, sửa lỗi và bảo vệ. Không thêm tính năng ngoài đề khi chức năng bắt buộc chưa hoàn thiện.

Bàn giao mã nguồn server/client và tài nguyên sử dụng, hướng dẫn build/chạy, báo cáo, dữ liệu giả lập và kịch bản demo đủ tiêu chí. Không cần thêm công cụ quản lý phức tạp cho nhóm ba người.
