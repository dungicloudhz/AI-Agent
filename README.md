Đây là **hướng dẫn toàn diện về AI Agent** và một **roadmap chi tiêt** để bạn có thể tự học, phát triển và ứng dụng AI Agent trong thực tế, kể cả khi bạn mới bắt đầu.

# 1. AI Agent là gì?
**AI Agent** là một chương trình (software) tự động thực hiện nhiệm vụ thông minh **dựa trên môi trường, input và mục tiêu**.
AI Agent có thể:
- Tự động hóa nhiệm vụ
- Tương tác với người hoặc hệ thống khác
- Tự học hoặc tối ưu hành vi
- Tích hợp các mô hình (LLM, Vision, Reinforcement learning...)

**Tóm lại**: Một AI Agent là "robot phần mềm có khả năng tư duy/ra quyết định" để hoàn thành mục tiêu đã định

# 2. Phân loại AI Agent (từ đơn giản đến phức tạp)

| Loại agent              | Mức độ        | Đặc điểm                                  |
| ----------------------- | ------------- | ----------------------------------------- |
| **Reactive Agent**      | 🟢 cơ bản     | Chỉ phản ứng theo rule, không nhớ lịch sử |
| **Goal-Oriented Agent** | 🟡 trung bình | Có mục tiêu, biết chọn hành vi phù hợp    |
| **Learning Agent**      | 🔵 nâng cao   | Tự học từ dữ liệu + tối ưu hành vi        |
| **Multi-Agent System**  | 🟣 phức tạp   | Nhiều agent phối hợp nhau                 |

# 3. Trụ cột (PROMPT + MCP + RAG)
**Prompt + MCP + RAG** là 3 "trụ cột" khi làm **AI Agent / n8n / hệ thống AI thực tế.**

## 1. VIẾT PROMPT ĐÚNG CÁCH (PROMPT ENGINEERING)
### 1.1 Prompt không phải là câu hỏi
Prompt = **bản thiết kế hành vi của AI**
❌ Sai: `Viết cho tôi bài quảng cáo`
✅ Đúng: `Bạn là chuyên gia marketing, nhiệm vụ là viết bài quảng cáo ngắn cho sản phẩm X, giọng điệu Y, mục tiêu Z`

### 1.2 Cấu trúc Prompt CHUẨN (BĂC BUỘC THUỘC)
**PROMPT TEMPLATE CHUẨN**
```css
[ROLE]
Bạn là ai?

[GOAL]
Mục tiêu là gì?

[CONTEXT]
Thông tin nền, dữ liệu đầu vào

[CONSTRAINTS]
Giới hạn, luật chơi

[OUTPUT FORMAT]
Định dạng đầu ra

[EXAMPLES] (nếu có)
Ví dụ mẫu
```
### 1.3 Ví dụ Prompt THỰC TẾ
- **Ví dụ: AI xử lý webhook trong n8n**
```word
Bạn là 
```