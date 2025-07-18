#  AI Chatbot – Data Extraction & Gemini Answering 
## Giới thiệu: dữ liệu sẽ được lấy từ trang web [https://www.presight.io/privacy-policy.html](tại đây) từ dữ liệu trang web ta sẽ dựa vào đó cho Gemini Answering 
##  Tổng quan
Dự án xây dựng một **Chatbot kết hợp 2 mô hình AI**:
- **SentenceTransformer (all-mpnet-base-v2)**: trích xuất embedding và tìm kiếm ngữ nghĩa.
- **Gemini 1.5 Flash** (Google Generative AI): tạo câu trả lời tự nhiên, chi tiết và chính xác.

Chatbot hoạt động bằng cách:
1. **Thu thập dữ liệu** từ một trang web.
2. **Tiền xử lý** và chuyển dữ liệu thành vector embedding.
3. **Tìm kiếm câu trả lời phù hợp** dựa trên độ tương đồng cosine.
4. **Sinh câu trả lời tự động** sử dụng mô hình Gemini Flash.

##  Các thành phần chính

### 1. Thu thập dữ liệu (Data Crawling)
- Sử dụng **BeautifulSoup** để crawl nội dung từ website.
- Các thẻ HTML được xử lý: `h2`, `i`, `p`, `li`.
- Phân chia nội dung thành từng **segment** nhỏ.

### 2. Làm sạch dữ liệu (Preprocessing)
- Xóa stopwords tiếng Anh.
- Loại bỏ ký tự đặc biệt.
- Ẩn thông tin cá nhân (email...).

### 3. Sinh embedding (SentenceTransformer)
- Sử dụng mô hình pretrained `all-mpnet-base-v2`.
- Biến từng đoạn nội dung thành vector embedding.

### 4. Tìm kiếm ngữ nghĩa (Semantic Search)
- Tính **cosine similarity** giữa câu hỏi và các đoạn văn bản.
- Trả về **Top K đoạn nội dung** phù hợp nhất.

### 5. Sinh câu trả lời (Gemini 1.5 Flash)
- Kết hợp câu hỏi và context để tạo prompt.
- Sử dụng Gemini Flash để sinh câu trả lời.

##  Cấu trúc class chính

| Hàm / Phương thức             | Chức năng chính                                           |
|-------------------------------|-----------------------------------------------------------|
| `__init__()`                  | Khởi tạo, tải dữ liệu, sinh embedding, cấu hình Gemini.   |
| `crawl_data()`                | Thu thập và phân chia dữ liệu từ URL đầu vào.             |
| `clean()`                     | Làm sạch văn bản, loại bỏ stopwords.                      |
| `search_query()`              | Tìm kiếm context phù hợp bằng cosine similarity.          |
| `generate_answer_with_gemini()` | Sinh câu trả lời bằng Gemini Flash.                     |
| `make_request()`              | Giao diện chính xử lý câu hỏi và trả về kết quả.          |

##  Hướng dẫn sử dụng

```python
url = "https://www.presight.io/privacy-policy.html"
api_key = 'your_gemini_api_key'

chatbot = Chatbot(api_key, url)
chatbot.make_request("Câu hỏi của bạn?")
