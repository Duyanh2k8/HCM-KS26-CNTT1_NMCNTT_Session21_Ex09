# Bài 9: Xây Dựng Prompt 3 Thành Phần Giải Mã Thông Báo Lỗi Trạm Sạc Cùng Trợ Lý AI
## Phần 1: Thiết kế Prompt 3 Thành phần Chuẩn hóa
### 1. Vì sao câu hỏi ngắn khiến AI đưa ra câu trả lời sai lệch?
Câu hỏi "AI ơi phần mềm trạm sạc của tôi bị hỏng rồi làm sao đây?" khiến AI đưa ra câu trả lời sai lệch vì thiếu thông tin cần thiết để xác định chính xác nguyên nhân sự cố.

Cụ thể, câu hỏi mắc 3 vấn đề:
- **Thiếu ngữ cảnh (Context):** Không nói rõ thiết bị đang gặp lỗi là trụ sạc VoltCharge số 04, hệ điều hành Windows 11 hay tác vụ mà phần mềm đang thực hiện.
- **Không cung cấp lỗi nguyên văn (Raw Error):** Không đưa nhật ký lỗi tiếng Anh nên AI không biết chương trình đang gặp lỗi gì.
- **Yêu cầu quá chung chung (Instruction):** Chỉ nói phần mềm bị hỏng mà không yêu cầu AI phân tích nguyên nhân hoặc đưa ra phương án khắc phục cụ thể.
### 2. Soạn thảo Prompt 3 thành phần hoàn chỉnh
Dưới đây là Prompt chuẩn hóa có thể sử dụng trực tiếp trên ChatGPT hoặc Gemini:

**[1. CONTEXT – NGỮ CẢNH]**

Tôi là nhân viên kỹ thuật đang trực vận hành hệ thống Kiosk nạp năng lượng thông minh VoltCharge tại Bến xe Miền Đông. Trụ sạc nhanh số 04 đột ngột dừng hoạt động. Thiết bị sử dụng hệ điều hành Windows 11 và phần mềm quản lý VoltCharge. Tôi đang kiểm tra nhật ký lỗi trên Terminal để tìm nguyên nhân sự cố.

**[2. RAW ERROR – LỖI NGUYÊN VĂN]**
```text
[TERMINAL ERROR LOG - VOLTCHARGE CHARGER #04]
2026-09-21 21:15:30 [FATAL] ControllerDaemon failed to start.
System.UnauthorizedAccessException:
Access to the path
'C:\Program Files\VoltCharge\data\session.log'
is denied.
at System.IO.FileStream.ValidateFileHandle(SafeFileHandle handle)
at VoltCharge.Core.SessionManager.Initialize()
InnerException: None.
Process terminated with exit code 5 (0x00000005).
Suggestion: Run the application with elevated
administrator privileges or verify NTFS file permissions.
```
**[3. INSTRUCTION – YÊU CẦU]**

Hãy thực hiện các yêu cầu sau:
1. Dịch thông báo lỗi tiếng Anh sang tiếng Việt đơn giản, dễ hiểu cho nhân viên kỹ thuật mới.
2. Phân tích nguyên nhân gốc rễ khiến phần mềm không thể khởi động.
3. Đưa ra 3 bước kiểm tra và khắc phục lỗi quyền truy cập trên Windows 11.
4. Ưu tiên giải pháp an toàn, không yêu cầu tháo vỏ trạm sạc hoặc can thiệp vào hệ thống điện.
5. Không tự suy đoán những thông tin không có trong nhật ký lỗi. Nếu thiếu dữ liệu, hãy nói rõ cần kiểm tra thêm thông tin gì.
6. Cảnh báo trước khi đề xuất các lệnh có thể ảnh hưởng đến hệ thống hoặc dữ liệu.
## Phần 2: Giải mã Lỗi & Kịch bản Khắc phục
### 1. Dịch và giải thích nguyên nhân gốc rễ
#### 1.1. Bản dịch thông báo lỗi
|Thông báo	|Ý nghĩa tiếng Việt|
|-|-|
|FATAL	|Lỗi nghiêm trọng|
|ControllerDaemon failed to start|	Dịch vụ điều khiển không thể khởi động|
|UnauthorizedAccessException|	Ngoại lệ do không có quyền truy cập|
|Access to the path is denied|	Hệ thống từ chối quyền truy cập vào đường dẫn|
|InnerException: None	|Không có lỗi nguyên nhân phụ được ghi nhận|
|Process terminated	|Tiến trình đã bị kết thúc|
|Exit code 5	|Mã lỗi 5 – Access is denied (Truy cập bị từ chối)|
|Run with elevated administrator privileges	|Chạy với quyền Quản trị viên|
#### 1.2. Giải thích nguyên nhân bằng ngôn ngữ đời thường
Hãy tưởng tượng thư mục `Program Files` giống như một căn phòng được khóa bảo vệ. Chỉ những người được cấp quyền mới có thể ghi hoặc chỉnh sửa dữ liệu bên trong.

Khi phần mềm VoltCharge khởi động, nó cần tạo hoặc ghi thông tin phiên hoạt động vào file:
```text
C:\Program Files\VoltCharge\data\session.log
```
Tuy nhiên, Windows phát hiện chương trình không có đủ quyền ghi dữ liệu vào vị trí này nên đã từ chối yêu cầu.

Điều đó khiến:
- ControllerDaemon không thể khởi động.
- SessionManager không thể khởi tạo phiên làm việc.
- Phần mềm bị dừng đột ngột với mã lỗi 5.
### Nguyên nhân gốc rễ
```mermaid
flowchart TD
A["📁 Thư mục data bị giới hạn quyền truy cập"]
B["📄 Phần mềm không thể ghi session.log"]
C["🚫 ControllerDaemon không thể khởi động"]

A --> B
B --> C

style A fill:#f5f5f5,stroke:#888,color:#222
style B fill:#fff0f0,stroke:#ff4444,color:#222
style C fill:#fff0f0,stroke:#ff4444,color:#222
```
### 2. Ba bước xử lý cụ thể trên Windows 11
`Lưu ý an toàn`

Vì đây là hệ thống trạm sạc thực tế, cần đưa trụ số 04 vào trạng thái ngừng phục vụ theo quy trình vận hành và thông báo người phụ trách kỹ thuật. Không tự ý tháo vỏ, can thiệp hệ thống điện hoặc khởi động lại thiết bị sạc khi chưa được cho phép.

**Bước 1. Kiểm tra và chạy phần mềm bằng quyền Administrator**
1. Tìm biểu tượng phần mềm VoltCharge trên Desktop hoặc Start Menu.
2. Nhấn chuột phải vào biểu tượng.
3. Chọn Run as administrator.
4. Nếu Windows hiện thông báo User Account Control (UAC), chỉ xác nhận khi bạn được cấp quyền và đã kiểm tra đúng ứng dụng.

Mục đích: Cho phép chương trình chạy với quyền quản trị viên trong lần khởi chạy được phê duyệt.

**Bước 2. Kiểm tra quyền ghi của thư mục data**
1. Mở File Explorer.
2. Truy cập đường dẫn:
``` text
C:\Program Files\VoltCharge\
```
3. Nhấn chuột phải vào thư mục `data`, chọn Properties.
4. Chọn tab Security.
5. Kiểm tra tài khoản chạy dịch vụ VoltCharge có quyền ghi (Write) hoặc sửa đổi (Modify) hay không.
Không tự ý cấp quyền Full Control cho Everyone hoặc Users. Nếu thiếu quyền, yêu cầu quản trị viên hệ thống cấp quyền Modify phù hợp cho đúng tài khoản dịch vụ theo hướng dẫn của nhà cung cấp.

**Bước 3. Kiểm tra và xác nhận hoạt động**
1. Nhờ quản trị viên kiểm tra lại quyền NTFS và cấu hình tài khoản chạy ControllerDaemon.
2. Sau khi được phê duyệt, khởi động lại dịch vụ theo quy trình của nhà cung cấp VoltCharge.
3. Kiểm tra log để xác nhận lỗi `UnauthorizedAccessException` không còn xuất hiện.
4. Chỉ đưa trụ sạc trở lại hoạt động sau khi hoàn thành quy trình kiểm tra an toàn và xác nhận của người phụ trách.
### 3. Bẫy an ninh – Xử lý API Key trước khi gửi cho AI
Giả sử nhật ký lỗi chứa đoạn:
```text
API_KEY: volt_secret_token_889922
```
Kỹ thuật viên tuyệt đối không được sao chép nguyên văn đoạn chứa API Key lên ChatGPT hoặc Gemini.

**Cách xử lý đúng:**

Thay thế chuỗi bí mật bằng ký hiệu che giấu (Redaction):

**Trước khi che giấu**                `Không an toàn`
```
API_KEY: volt_secret_token_889922
```


**Sau khi che giấu**                  `An toàn hơn`
```
API_KEY: [REDACTED]
```
**Tại sao phải làm như vậy?**
1. **Bảo vệ tài khoản và hệ thống:** API Key có thể được sử dụng để truy cập dịch vụ hoặc tài nguyên được cấp quyền.
2. **Ngăn chặn rò rỉ dữ liệu:** Nếu khóa bí mật bị lộ, người khác có thể lợi dụng nó để truy cập trái phép hoặc sử dụng tài nguyên của doanh nghiệp.
3. **Bảo vệ thông tin doanh nghiệp:** Những dữ liệu như mật khẩu, API Key, mã OTP và thông tin đăng nhập phải được giữ bí mật.

**Nếu đã vô tình gửi API Key lên AI:**
- Thông báo ngay cho người phụ trách bảo mật.
- Thu hồi hoặc vô hiệu hóa API Key cũ.
- Tạo khóa mới và cập nhật vào hệ thống theo quy trình được phê duyệt.
- Kiểm tra nhật ký sử dụng để phát hiện hoạt động bất thường.
