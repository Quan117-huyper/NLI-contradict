# ⚖️ Document Contradiction Detection (DCD) System

Hệ thống phát hiện mâu thuẫn trong văn bản dài và biên bản thẩm vấn sử dụng kiến trúc ModernBERT và kỹ thuật Multi-hop Reasoning.

## 🌟 Tổng Quan Dự Án
Dự án này giải quyết bài toán phát hiện các điểm sai lệch, mâu thuẫn trong các tài liệu pháp lý hoặc lời khai. Thay vì chỉ so sánh các câu đơn lẻ, hệ thống có khả năng truy xuất thông tin liên quan từ hàng ngàn trang tài liệu và thực hiện suy luận kết nối (Reasoning) để tìm ra những mâu thuẫn ẩn.

## 🏗️ Kiến Trúc Hệ Thống

### 1. Engine Chính (Core Model)
*   **Model NLI**: `model5` (Dựa trên kiến trúc **ModernBERT-Large**). Đây là mô hình đã được fine-tune chuyên biệt cho các tình huống hình sự/pháp lý.
*   **Embedding**: `nomic-ai/modernbert-embed-base` dùng để vector hóa văn bản.

### 2. Các Thành Phần Kỹ Thuật
*   **Data Generation**: Quy trình tự động sinh dữ liệu NLI chất lượng cao bằng Llama-4 Scout và Gemini 1.5 Flash.
*   **Retrieval (IR)**: Sử dụng **FAISS Indexing** để tìm kiếm nhanh các đoạn văn (spans) có liên quan nhất tới giả thuyết cần đối soát.
*   **Multi-hop Reasoning**:
    *   *Concat Reasoning*: Gộp nhiều đoạn văn bản liên quan để tạo ngữ cảnh rộng.
    *   *Chain Reasoning*: Xét chuỗi các sự kiện để phát hiện mâu thuẫn về mặt thời gian (Sequence Conflict).
*   **LLM Integration**: Sử dụng Falcon-3B để tóm tắt các khối câu hỏi/trả lời thành giả thuyết ngắn gọn.

## 📁 Cấu Trúc Tài liệu (Notebooks)
*   **`test_model.ipynb`**: Hệ thống đánh giá tổng hợp, chạy từ Baseline đến các kỹ thuật Multi-hop nâng cao.
*   **`test_model2.ipynb`**: Tập trung vào quy trình Indexing dữ liệu lớn và tối ưu hóa tìm kiếm bằng FAISS.
*   **`fork-of-model (1).ipynb`**: "Nhà máy" sản xuất dữ liệu và quy trình huấn luyện (Fine-tuning) mô hình ModernBERT.
*   **`README.md`**: Tài liệu hướng dẫn hệ thống.

## 🚀 Quy Trình Vận Hành (Pipeline)
1.  **Huấn luyện**: Sử dụng `fork-of-model (1).ipynb` để tạo dữ liệu và luyện mô hình `model5`.
2.  **Chuẩn bị**: Nạp mô hình vào môi trường Kaggle, cấu hình `model_type: modernbert`.
3.  **Indexing**: Chạy `test_model2` để tạo chỉ mục FAISS cho tài liệu cần kiểm tra.
4.  **Inference**: Nhập giả thuyết (Hypothesis) -> Hệ thống truy xuất đoạn văn liên quan -> Chấm điểm mâu thuẫn bằng `model5` -> Xuất báo cáo.

## 🛠️ Yêu Cầu Hệ Thống
*   **GPU**: Khuyến khích NVIDIA T4 (Kaggle) trở lên để chạy ModernBERT và FAISS.
*   **Thư viện**: `transformers`, `faiss-cpu`, `sentence-transformers`, `llama-cpp-python`.

---
**Dự án được thiết kế để xử lý các tài liệu có độ phức tạp cao, đặc biệt hiệu quả trong lĩnh vực điều tra tội phạm và rà soát hợp đồng pháp lý.**
