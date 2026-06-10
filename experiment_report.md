# Experiment Report: Data Quality Impact on AI Agent

**Student Email:** 26ai.anhhkt@vinuni.edu.vn
**Name:** Hoàng Kim Tuấn Anh
**Date:** 2026-06-10

---

## 1. Kết quả thí nghiệm

Chạy `agent_simulation.py` với 2 bộ dữ liệu và ghi lại kết quả:

| Scenario | Agent Response | Accuracy (1-10) | Notes |
|----------|----------------|-----------------|-------|
| Clean Data (`processed_data.csv`) | Agent: Based on my data, the best choice is Laptop at $1200. | 9 | Trả lời đúng sản phẩm electronics đắt nhất sau khi ETL lọc dữ liệu |
| Garbage Data (`garbage_data.csv`) | Agent: Based on my data, the best choice is Nuclear Reactor at $999999. | 2 | Trả lời sai do outlier và dữ liệu không được validate |

---

## 2. Phan tich & nhận xét

### Tai sao Agent trả lời sai khi dùng Garbage Data?

Khi dùng Clean Data từ ETL pipeline, Agent hoạt động ổn định vì dữ liệu đã được validate (price > 0, category đầy đủ) và chuẩn hóa (Title Case). Ngược lại, Garbage Data chứa nhiều vấn đề chất lượng: Duplicate IDs (hai record cùng id=1), sai kiểu dữ liệu (price là chuỗi "ten dollars"), giá trị null (id, category trống), và outlier cực đoan (Nuclear Reactor giá 999999). Agent chọn sản phẩm có giá cao nhất trong nhóm electronics bằng `idxmax()`, nên bị đánh lừa bởi outlier thay vì Laptop. Dù prompt hỏi về sản phẩm tốt nhất, logic Agent phụ thuộc trực tiếp vào chất lượng CSV đầu vào. Nếu không có bước ETL lọc và chuẩn hóa trước, AI sẽ đưa ra kết quả sai lệch hoặc có thể crash khi gặp kiểu dữ liệu hỗn hợp.

---

## 3. Kết luận

**Quality Data > Quality Prompt?** Đồng ý. Prompt chỉ định hướng câu hỏi, nhưng Agent vẫn đọc trực tiếp từ file CSV. Nếu dữ liệu rác (outlier, null, sai type), kết quả sẽ sai dù prompt tốt đến đâu. ETL pipeline là lớp bảo vệ bắt buộc trước khi đưa dữ liệu vào hệ thống AI.
