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
Bạn là một AI Agent chuyên xử lý dữ liệu API.

Mục tiêu:
Phân tích dữ liệu JSON đầu vào và trả về kết quả đã chuẩn hóa.

Dữ liệu đầu vào:
{{ $json }}

Luật:
- Không bịa dữ liệu
- Nếu thiếu field, trả về null
- Chỉ trả JSON

Output format:
{
    "name": string,
    "email": string,
    "status": "valid" | "invalid",
}
```
👉 Prompt này đủ **dùng cho production**

### 1.4 Prompt cho AI Agent (Planner Prompt)
```less
Bạn là AI Agent có quyền sử dụng các tool sau:
- search (query)
- api_call (endpoint, params)

Nhiệm vụ:
Giải quyết yêu cầu người dùng từng bước.

Luật:
- Chỉ gọi tool khi cần
- Sau mỗi hành động, phân tích kết quả
- Nếu hoàn thành, trả FINAL ANSWER
```
### 1.5 Prompt patterns BẮT BUỘC BIẾT
| Pattern          | Dùng khi            |
| ---------------- | ------------------- |
| Zero-shot        | Task đơn giản       |
| Few-shot         | Task phức tạp       |
| Chain-of-thought | Cần reasoning       |
| ReAct            | AI + Tool           |
| Self-check       | Tránh hallucination |

![](./static/images/ai%20agent.jpg)

## 2. MCP (MODEL CONTEXT PROTOCOL)
**MCP = cách bạn "đóng gói context" cho AI Agent**

### 2.1 MCP là gì?
MCP giúp:
    - Tách **Prompt logic**
    - Tách **Context**
    - Tách **Tool**
    - Dễ mở rộng Agent

👉 MCP ≠ prompt context
👉 MCP = **chuẩn kiến trúc**

### 2.2 Kiến trúc MCP chuẩn
```pgsql
System Prompt (Role + Rules)
        ↓
Context Providers
        ↓
    User Input
        ↓
      Model
        ↓
    Tool Calls
```

### 2.3 MCP Context Types (rất quan trọng)
| Context        | Ví dụ              |
| -------------- | ------------------ |
| System Context | Luật, vai trò      |
| User Context   | Yêu cầu người dùng |
| Memory Context | Lịch sử            |
| Tool Context   | API, function      |
| RAG Context    | Dữ liệu truy xuất  |

### 2.4 MCP ví dụ (dạng JSON)
```json
{
  "system": "Bạn là AI Agent hỗ trợ automation trong n8n",
  "context": {
    "memory": "User thích câu trả lời ngắn gọn",
    "tools": ["http_request", "db_query"],
    "knowledge": "Không trả dữ liệu ngoài context"
  },
  "input": "Tạo workflow gửi email"
}
```

### 2.5 MCP trong thực tế (n8n)
Bạn sẽ:
- Lưu System Prompt cố định
- Inject Context động
- Inject RAG data
- Gửi 1 request duy nhất cho LLM

## 3. RAG (RETRIEVAL AUGENTED GENERATION)
### 3.1 RAG là gì?
RAG = **Không tin AI nhớ - mà bắt nó tra dữ liệu**
❌ Không RAG: `AI bịa theo trí nhớ`
✅ Có RAG: `AI chỉ trả lời từ dữ liệu bạn cung cấp`

### 3.2 Kiến trúc RAG CHUẨN
```sql
User Query
   ↓
Embedding
   ↓
Vector Search
   ↓
Relevant Chunks
   ↓
Prompt + Context
   ↓
LLM Answer
```
### 3.3 Quy trình RAG từng bước
- **Bước 1: Chuẩn bị dữ liệu**
    - PDF
    - DOC
    - Web
    - Database
- **Bước 2: Chunking (rất quan trọng)**
    ```yaml
    Chunk size: 300-800 tokens
    Overlap: 10-20%
    ```
- **Bước 3: Embedding (Nhúng)**
    - OpenAI
    - Gemini
    - SentenceTranformer
- **Bước 4: Vector DB**
    - Pinecone
    - Subpabase
    - PostgeSQL + pyvector
    - Chroma

### 3.4 Prompt cho RAG (BẮT BUỘC)
```css
Bạn chỉ được trả lời dựa trên CONTEXT bên dưới.
Nếu không có thông tin, trả lời:
"Tôi không tìm thấy dữ liệu phù hợp."

CONTEXT:
{{retrieved_chunks}}

Câu hỏi:
{{user_query}}
```
👉 Câu này **giết hallucination (sự ảo tưởng)**

### 3.5 RAG trong n8n (flow chuẩn)
```sql
Webhook
 ↓
Embedding user query
 ↓
Vector search
 ↓
Build context
 ↓
LLM prompt
 ↓
Response
```

### 3.6 RAG nâng cao (Production)
- Re-ranking
- Metadata filtering
- Citation
- Hybrid search (keyword + vector)
- Cache embeddings

# KẾT HỢP PROMPT + MCP + RAG
**Flow CHUẨN AI Agent**
```sql
User Input
   ↓
MCP (System + Memory + Tools)
   ↓
RAG (Retrieve đúng data)
   ↓
Prompt (Ràng buộc chặt)
   ↓
  LLM
   ↓
Action / Response
```
# Ví dụ thực tế
`Chatbot nội bộ công ty`
- Prompt: Ràng buộc luật trả lời
- MCP: Context + Tool + Memory
- RAG: Tài liệu nội bộ
👉 AI không bịa - không vượt quyền - không leak dữ liệu

# CHECKLIST BẮT BUỘC NHỚ
- Prompt có ROLE + GOAL + FORMAT
- MCP tách logic - không nhét hết vào prompt
- RAG luôn có câu "chỉ trả lời dựa trên context"
- Chunk đúng kích thước
- Log input/output để debug