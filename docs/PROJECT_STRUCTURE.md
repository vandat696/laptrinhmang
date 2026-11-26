# Cấu trúc đơn giản cho project cờ vua

Ưu tiên project nhỏ, code C dễ đọc, dễ giải thích và đủ 13 tiêu chí chấm điểm. Chia theo chức năng thành vài file, hạn chế thư mục con và các lớp trung gian.

Người dùng đã chốt cấu trúc thu gọn này ngày 26/11/2026.

Khi chia việc cho ba người, tách thêm lobby.c từ game.c để phần sảnh/thách đấu và phần quản lý trận có chủ file riêng. Server gồm 7 file .c và một header chung; không thêm tầng kiến trúc.

## Cấu trúc đã chốt

```text
Laptrinhmang/
├── server/
│   ├── main.c        # Khởi động và vòng lặp xử lý client
│   ├── server.h      # Struct, hằng số và khai báo hàm
│   ├── network.c     # Socket, gửi/nhận, xử lý truyền dòng
│   ├── account.c     # Tài khoản, phiên, điểm và xếp hạng
│   ├── lobby.c       # Danh sách sẵn sàng và thách đấu
│   ├── game.c        # Quản lý ván, nước đi và kết thúc
│   ├── chess.c       # Bàn cờ, luật đi và kết quả theo luật
│   └── log.c         # Ghi và gửi file log
├── client/           # Giao diện và kết nối tới server
├── res/              # Quân cờ và theme người dùng lựa chọn
│   ├── pieces/       # Hình quân Trắng và Đen trong white/, black/
│   └── themes/       # Hình nền, bàn cờ và mẫu giao diện
├── data/             # Dữ liệu tài khoản và điểm nếu lưu file
├── logs/             # File log các trận đấu
├── docs/             # Yêu cầu, kiến thức và thiết kế
├── output/           # Bản phác thảo giao diện hiện có
├── README.md
└── PROJECT_NOTES.md
```

Đã tạo các thư mục chính server, client, data và logs; chưa tạo các file mã nguồn. Công nghệ giao diện, nền tảng chạy, mô hình I/O và định dạng giao thức chưa chốt. Các tài liệu hiện có trong docs được giữ lại để tham khảo.

## Trách nhiệm các file server

| File | Nhiệm vụ |
| --- | --- |
| main.c | Khởi tạo dữ liệu, chạy server, chuyển yêu cầu tới hàm phù hợp và dọn tài nguyên khi dừng |
| server.h | Khai báo các kiểu như Account, Player, Game, giới hạn số người chơi/trận và các hàm dùng giữa các file |
| network.c | Tạo và đóng socket, gửi/nhận, nhận diện thông điệp đầy đủ, giữ dữ liệu còn thiếu và xử lý gửi chưa hết |
| account.c | Đăng ký, đăng nhập, quản lý phiên, đọc/ghi tài khoản và điểm, tính thứ hạng |
| lobby.c | Danh sách sẵn sàng, giới hạn thách đấu 10 bậc, chuyển/nhận/từ chối lời mời và gọi tạo trận khi nhận lời hợp lệ |
| game.c | Tạo ván, kiểm tra người chơi/lượt, chuyển nước đi, xin ngừng và điều phối kết thúc trận |
| chess.c | Khởi tạo bàn cờ, kiểm tra và áp dụng nước đi, phát hiện thắng/thua/hòa theo phạm vi luật thống nhất |
| log.c | Ghi bắt đầu, kết thúc, IP hai bên, lịch sử nước đi và gửi log cho hai client |

game.c gọi chess.c để kiểm tra luật, gọi account.c để cập nhật điểm đúng một lần khi kết thúc và gọi log.c để hoàn tất log. Server C quyết định trạng thái và kết quả chính thức.

Phân công chủ file và màn hình client nằm trong [kế hoạch nhóm](TEAM_PLAN.md): Đạt phụ trách mạng server/tài khoản, Minh phụ trách mạng client/sảnh/luật, Duy phụ trách mạng ván đấu/truyền log. Mỗi người có phần giao diện và báo cáo tương ứng.

## Client và dữ liệu

- client ban đầu chỉ cần vài file cho giao diện và kết nối; tên file cụ thể chọn sau khi chốt công nghệ. Phần chờ mạng cần tránh làm đứng giao diện.
- res chứa tài nguyên người dùng tự lựa chọn; xem [hướng dẫn đặt quân cờ và theme](../res/README.md). Giao diện sẽ dùng các tài nguyên này khi tích hợp.
- Đề xuất lưu tài khoản và điểm bằng file trong data cho phù hợp quy mô nhỏ; định dạng chưa chốt. Chỉ thêm cơ sở dữ liệu nếu có nhu cầu cụ thể.
- logs chứa file log do server tạo. Mỗi client nhận log về thư mục cục bộ của mình.
- Dữ liệu chạy thật, cấu hình riêng và file biên dịch sẽ được loại khỏi Git khi dựng mã nguồn.

## Cách viết code

- Ưu tiên struct, mảng có giới hạn rõ ràng và các hàm có nhiệm vụ cụ thể.
- Dùng một mô hình phục vụ nhiều client đã học; đề xuất xem xét select cho quy mô demo. Nếu chọn pthread, vẫn phải xử lý đồng bộ dữ liệu chung.
- Giao thức dễ đọc, có quy tắc xác định ranh giới thông điệp. Vẫn xử lý nhận thiếu/gộp, gửi thiếu và lỗi kết nối.
- Chỉ tách thêm file khi một file đã khó đọc. Kiểm tra các luồng chính bằng client thử và hai hoặc nhiều người chơi; bổ sung kiểm thử tự động khi cần.
- Giữ đủ yêu cầu chấm điểm, gồm kiểm tra luật, kết quả, tính điểm, giới hạn thách đấu và chuyển log. Phạm vi luật cờ vua còn cần thống nhất; việc thu gọn cấu trúc không tự động bỏ các luật bắt buộc.

## Rà soát theo đề bài và môn học

Cấu trúc hiện tại đã phân đủ trách nhiệm cho cả 13 tiêu chí, tổng 20 điểm. Chưa có mã nguồn nên đây là đánh giá thiết kế, chưa phải xác nhận sản phẩm đã đáp ứng yêu cầu.

| Nhóm tiêu chí | Điểm | Nơi phụ trách |
| --- | ---: | --- |
| Truyền dòng và vào/ra socket | 1 + 2 | main.c, network.c |
| Đăng ký, quản lý tài khoản, đăng nhập và phiên | 2 + 2 | account.c |
| Danh sách người chơi sẵn sàng | 2 | lobby.c phối hợp account.c và game.c |
| Chuyển lời thách đấu, nhận và từ chối | 2 + 1 | lobby.c, dùng thứ hạng từ account.c và tạo trận qua game.c |
| Chuyển nước đi, kiểm tra hợp lệ, xác định kết quả | 2 + 1 + 1 | game.c, chess.c, network.c |
| Ghi log, chuyển kết quả và log | 1 + 2 | log.c, game.c, network.c |
| Xin ngừng ván cờ | 1 | game.c phối hợp account.c và log.c |

Chi tiết cần bổ sung trước khi viết phần liên quan:

1. Giao thức: định dạng lệnh/phản hồi, ranh giới thông điệp và giới hạn độ dài. Không giả định một lần send tương ứng một lần recv. Gửi file log phải có cách nhận biết đủ dữ liệu.
2. Mô hình I/O: chọn select hoặc pthread theo kiến thức đã học. Đề xuất TCP và một vòng lặp select cho quy mô demo; xử lý nhận thiếu/gộp và gửi thiếu vẫn cần thực hiện. Tránh để chờ một client làm đứng cả server.
3. Phiên và trạng thái: gắn socket với người đã đăng nhập, phân biệt chưa đăng nhập, sẵn sàng và đang chơi; dọn trạng thái khi ngắt kết nối. Cách xử kết quả khi mất kết nối cần được chốt riêng.
4. Xếp hạng và lời mời: quy định điểm đầu, bằng điểm và thời điểm kiểm tra thứ hạng; giới hạn 10 bậc là chênh lệch vị trí, không phải chênh lệch điểm. Kiểm tra lại khả năng tham gia khi nhận lời để một người không vào hai ván.
5. Luật cờ vua: xác định rõ nước đi hợp lệ, an toàn của vua, điều kiện thắng/hòa và các nước đặc biệt. Đây là phần có khả năng tốn công nhất; không tự bỏ luật để rút ngắn code.
6. Kết thúc trận: xử lý điểm +3/0/−3 đúng một lần, gửi kết quả và log đủ thông tin cho cả hai bên, đưa người chơi còn kết nối về sảnh để có thể chơi ván mới. Người xin ngừng bị xử thua.

Phần có thể để ngoài phạm vi: AI đánh cờ, chat, bạn bè, xem đấu, đồng hồ thi đấu và tự động tìm server. Chưa cần thêm framework backend, cơ sở dữ liệu hoặc mô hình nhiều tầng nếu file và các hàm C đã đáp ứng nhu cầu.

Không cần dùng đồng thời fork, pthread, select và UDP để chứng minh đã học đủ chương. Chọn kiến thức phù hợp và giải thích được lựa chọn. Luồng demo tối thiểu nên có hai client, nhận/từ chối thách đấu, nước đi sai, kết thúc/xin ngừng, điểm số, nhận file log; dùng thêm tài khoản mẫu để kiểm tra giới hạn xếp hạng. Kiểm tra riêng việc nhận thông điệp bị chia nhỏ hoặc gộp và gửi/nhận file log đầy đủ.
