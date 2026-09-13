# BÁO CÁO NGHIỆM THU BÀI LAB 3

## 1. Thông tin bài làm

| Hạng mục | Nội dung |
| :--- | :--- |
| Họ và tên | Phan Thị Khánh Linh |
| Mã sinh viên | 2A202602360 |
| Chủ đề | Trợ lý Học vụ Sinh viên VinUni |
| Kiến trúc | ReAct Agent kết nối MCP Server |
| Provider của trace hiện tại | MockOfflineProvider |

## 2. Đánh giá Agentic Fit

| Tiêu chí | Điểm | Giải thích |
| :--- | :---: | :--- |
| Multi-step Reasoning | 5/5 | TC04 yêu cầu tra cứu cố vấn trước, sau đó mới đặt lịch. |
| Tool Interaction | 5/5 | Agent gọi `academic_query` và `schedule_appointment` qua MCP. |
| Dynamic Decision | 5/5 | Tên cố vấn dùng ở bước đặt lịch được lấy từ Observation trước đó. |
| Long Horizon Goal | 4/5 | Agent duy trì mục tiêu qua nhiều lượt; bài lab chưa triển khai memory dài hạn. |
| **Tổng điểm** | **19/20** | Phù hợp để triển khai Agentic System. |

## 3. Kết quả kiểm thử

Lệnh kiểm thử:

```powershell
$env:LLM_PROVIDER="mock"
.\.venv\Scripts\python.exe src\app.py --all
```

| Test case | Nội dung | Kết quả |
| :--- | :--- | :--- |
| TC01 | Câu hỏi quy chế học vụ chung | PASS, trả lời trực tiếp không gọi tool |
| TC02 | Tra cứu `SV2026001` | PASS, gọi `academic_query` |
| TC03 | Đặt lịch tư vấn | PASS, gọi `schedule_appointment` |
| TC04 | Tra cứu cố vấn rồi đặt lịch | PASS, gọi 2 tool theo đúng thứ tự |
| TC05 | Tra cứu mã không tồn tại | PASS, trả về `NOT_FOUND`, không bịa dữ liệu |
| **Tổng cộng** | 5 test cases | **5/5 PASS** |

## 4. Waterfall Trace

File bằng chứng: `docs/trace_waterfall.json`

- Tổng số sự kiện: **10**
- Số lượt gọi tool: **5**
- Số câu trả lời cuối: **5**
- Phân bổ lượt gọi tool: TC02 = 1, TC03 = 1, TC04 = 2, TC05 = 1.

### Trace tiêu biểu của TC04

```json
[
  {
    "step": 1,
    "action_type": "TOOL_EXECUTION",
    "tool_name": "academic_query",
    "arguments": {
      "student_id": "SV2026001"
    },
    "observation": {
      "status": "SUCCESS",
      "data": {
        "advisor": "PGS.TS Nguyễn Văn A"
      }
    }
  },
  {
    "step": 2,
    "action_type": "TOOL_EXECUTION",
    "tool_name": "schedule_appointment",
    "arguments": {
      "student_id": "SV2026001",
      "datetime_str": "14:00 15/09/2026",
      "advisor_name": "PGS.TS Nguyễn Văn A"
    },
    "observation": {
      "status": "SUCCESS",
      "booking_id": "BK-SV2026001-99"
    }
  }
]
```

Trace chứng minh chuỗi `Thought -> Action -> Observation`: Agent lấy thông tin cố vấn từ lần tra cứu thứ nhất, dùng thông tin đó để tạo request đặt lịch ở bước thứ hai, rồi sinh câu trả lời cuối.

## 5. Trạng thái nộp bài

- [x] Hoàn thiện Tool Schema và MCP JSON-RPC dispatcher.
- [x] Hoàn thiện ReAct loop và xử lý multi-step.
- [x] Chạy thành công 5/5 test case ở chế độ Mock Offline.
- [x] Sinh file `docs/trace_waterfall.json`.
- [x] Chạy nghiệm thu bằng API thật Gemini/OpenAI.
- [x] Commit và push repository lên GitHub cá nhân.

> .
