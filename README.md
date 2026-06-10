[![Open in Visual Studio Code](https://classroom.github.com/assets/open-in-vscode-2e0aaae1b6195c2367325f4f02e2d04e9abb55f0b24a779b69b11b9e10269abc.svg)](https://classroom.github.com/online_ide?assignment_repo_id=24112914&assignment_repo_type=AssignmentRepo)
# Day 10 Lab: Data Pipeline & Data Observability

**Mã số học viên:** 2A202600574
**Họ tên:** Hoàng Kim Tuấn Anh
**Email:** 26ai.anhhkt@vinuni.edu.vn

---

## Mô tả

Bài lab xây dựng một ETL pipeline tự động gồm 4 bước: **Extract** (đọc JSON), **Validate** (lọc dữ liệu không hợp lệ), **Transform** (giảm giá 10%, chuẩn hóa category, thêm timestamp), và **Load** (xuất CSV). Sau đó thực hiện stress test bằng cách chạy Agent simulation với dữ liệu sạch (`processed_data.csv`) và dữ liệu rác (`garbage_data.csv`) để quan sát ảnh hưởng của chất lượng dữ liệu đến kết quả AI.

---

## Cách chạy (How to Run)

### Yêu cầu cài đặt
```bash
pip install pandas pytest
```

### Chạy ETL Pipeline
```bash
python solution.py
```

### Chạy Agent Simulation (Stress Test)
```bash
python generate_garbage.py
python agent_simulation.py
```

### Chạy autograder tại máy
```bash
python -m pytest tests/test_autograder.py -v
```

---

## Cấu trúc thư mục

```
├── solution.py              # Script ETL Pipeline
├── processed_data.csv       # Kết quả đầu ra của pipeline
├── garbage_data.csv         # Dữ liệu rác cho stress test
├── agent_simulation.py      # Mô phỏng Agent RAG
├── experiment_report.md     # Báo cáo thí nghiệm
└── README.md                # File này
```

---

## Kết quả

**ETL Pipeline:**
- Extract: 5 records từ `raw_data.json`
- Validate: 3 records hợp lệ, 2 records bị loại (id=3: giá ≤ 0; id=4: category rỗng)
- Transform & Load: 3 records lưu vào `processed_data.csv` với cột `discounted_price` và `processed_at`

**Stress Test (Agent Simulation):**
| Scenario | Kết quả |
|----------|---------|
| Clean Data | Agent chọn đúng: **Laptop** ($1200) |
| Garbage Data | Agent chọn sai: **Nuclear Reactor** ($999999) do outlier |

**Kết luận:** Dữ liệu chất lượng cao (sau ETL) giúp Agent trả lời chính xác; dữ liệu rác khiến Agent đưa ra kết quả sai lệch dù prompt không đổi.
