# Khóa Luận: Dự Đoán Kết Cục Tử Vong ở Bệnh Nhân Suy Tim

## 📋 Tổng Quan

Dự án này nghiên cứu việc sử dụng các mô hình Machine Learning để dự đoán kết cục tử vong ở bệnh nhân suy tim, so sánh với thang điểm GWTG-HF (Get With The Guidelines - Heart Failure) truyền thống.

## 🎯 Mục Tiêu

- Xây dựng và đánh giá các mô hình Machine Learning để dự đoán kết cục tử vong
- So sánh hiệu suất của các mô hình ML với thang điểm GWTG-HF
- Xử lý vấn đề mất cân bằng dữ liệu bằng ClassWeight và ADASYN
- Phân tích độ quan trọng của các đặc trưng và diễn giải mô hình

## 📊 Dữ Liệu

- **Tổng số mẫu**: 329 bệnh nhân
- **Tập Development**: 246 mẫu (75%)
- **Tập Test**: 83 mẫu (25%)
- **Biến mục tiêu**: `ketcuc_mahoa` (0: Không tử vong, 1: Tử vong)
- **Số đặc trưng ban đầu**: 38 biến
- **Số đặc trưng sau xử lý**: 34 biến (sau khi loại bỏ các biến có >10% dữ liệu thiếu)

### 15 Đặc Trưng Quan Trọng Nhất (được chọn bởi Mutual Information)

1. `choangtim_mahoa` (0.0695)
2. `matbu_mahoa` (0.0628)
3. `suyhohap_mahoa` (0.0527)
4. `gioi_tinh_ma_hoa` (0.0473)
5. `ntprobnp_value` (0.0428)
6. `hcvc_mahoa` (0.0308)
7. `Troponin_I_hs` (0.0202)
8. `HATT` (0.0200)
9. `AST` (0.0200)
10. `dtd_mahoa` (0.0181)
11. `bmi` (0.0135)
12. `HATTr` (0.0117)
13. `Creatinine` (0.0091)
14. `adhere` (0.0064)
15. `K` (0.0054)

## 🔬 Phương Pháp

### Các Mô Hình Được Đánh Giá

1. **Random Forest (RF)**
2. **Logistic Regression (LogReg)**
3. **Support Vector Machine (SVM)**
4. **XGBoost**

### Kỹ Thuật Xử Lý Mất Cân Bằng Dữ Liệu

- **ClassWeight**: Điều chỉnh trọng số lớp (scale_pos_weight = 11.30)
- **ADASYN**: Adaptive Synthetic Sampling

### Quy Trình Xử Lý Dữ Liệu

1. **Tiền xử lý**:
   - Loại bỏ các biến có >10% dữ liệu thiếu
   - Điền dữ liệu thiếu bằng Iterative Imputer với Random Forest Regressor
   - Chuẩn hóa dữ liệu bằng StandardScaler

2. **Lựa chọn đặc trưng**:
   - Sử dụng Mutual Information để chọn 15 đặc trưng quan trọng nhất

3. **Đánh giá mô hình**:
   - 5-Fold Stratified Cross-Validation trên tập Development
   - Đánh giá trên tập Test độc lập

## 📈 Kết Quả

### Kết Quả Đánh Giá Chéo 5-Fold (Tập Development)

| Mô hình | AUC (Mean ± Std) |
|---------|------------------|
| RF (ADASYN) | 0.8102 ± 0.1282 |
| XGBoost (ClassWeight) | 0.7928 ± 0.1121 |
| SVM (ClassWeight) | 0.7655 ± 0.1522 |
| RF (ClassWeight) | 0.7640 ± 0.1634 |
| XGBoost (ADASYN) | 0.7701 ± 0.1593 |
| SVM (ADASYN) | 0.7587 ± 0.1329 |
| LogReg (ClassWeight) | 0.7459 ± 0.0902 |
| LogReg (ADASYN) | 0.7414 ± 0.0960 |

### Kết Quả Trên Tập Test

**Mô hình tốt nhất**: **RF (ClassWeight)** với AUC = **0.8712**

### So Sánh với GWTG-HF Score

| Mô hình | AUC Test | Hiệu AUC (ML - GWTG) | 95% CI | P-value |
|---------|----------|---------------------|--------|---------|
| RF (ClassWeight) | 0.8712 | 0.2330 | [0.0376, 0.4372] | 0.0220 |
| RF (ADASYN) | - | 0.2539 | [0.0431, 0.4854] | 0.0210 |
| XGBoost (ADASYN) | - | 0.2167 | [0.0110, 0.4535] | 0.0411 |
| XGBoost (ClassWeight) | - | 0.2119 | [-0.0205, 0.4589] | 0.0751 |
| LogReg (ClassWeight) | - | 0.1696 | [-0.0808, 0.4383] | 0.1823 |
| LogReg (ADASYN) | - | 0.1548 | [-0.0950, 0.4231] | 0.2283 |
| SVM (ClassWeight) | - | 0.1393 | [-0.0796, 0.4125] | 0.2704 |
| SVM (ADASYN) | - | 0.1164 | [-0.0887, 0.3746] | 0.3345 |
| GWTG-HF | 0.6382 | - | - | - |

### Phân Tích Hiệu Chuẩn (Calibration)

**GWTG-HF (Isotonic)**:
- Brier Score: 0.0825
- Intercept: -1.516
- Slope: 0.308

### Top 10 Đặc Trưng Quan Trọng Nhất (RF ClassWeight)

1. `Troponin_I_hs` (0.1281)
2. `matbu_mahoa` (0.1120)
3. `AST` (0.0977)
4. `choangtim_mahoa` (0.0969)
5. `ntprobnp_value` (0.0833)
6. `Creatinine` (0.0766)
7. `WBC` (0.0754)
8. `HATT` (0.0740)
9. `bmi` (0.0724)
10. `mach` (0.0572)

### Phân Tích Logistic Regression

Các yếu tố có ý nghĩa thống kê (p < 0.05):

| Đặc trưng | OR | OR 95% CI | P-value |
|-----------|-----|-----------|---------|
| `ntprobnp_value` | 5.41 | [2.33, 12.55] | <0.001 |
| `choangtim_mahoa` | 2.39 | [1.35, 4.21] | 0.003 |
| `WBC` | 1.64 | [1.10, 2.43] | 0.014 |
| `Creatinine` | 0.57 | [0.35, 0.94] | 0.027 |

## 🛠️ Cài Đặt

### Yêu Cầu

```bash
pip install xgboost shap imbalanced-learn lightgbm
```

### Các Thư Viện Chính

- `pandas`, `numpy`
- `scikit-learn`
- `xgboost`
- `lightgbm`
- `shap`
- `imbalanced-learn`
- `matplotlib`, `seaborn`
- `statsmodels` (cho phân tích thống kê)

## 🚀 Sử Dụng

1. Mở file `KhoaLuan.ipynb` trong Jupyter Notebook hoặc JupyterLab
2. Chạy các cell theo thứ tự từ trên xuống dưới
3. Kết quả sẽ được hiển thị sau mỗi phần

### Các Phần Chính trong Notebook

1. **Phần 0**: Cài đặt thư viện
2. **Phần 1**: Thiết lập và phân chia dữ liệu
3. **Phần 2**: Tiền xử lý sơ bộ
4. **Phần 3**: Xây dựng các pipeline đã cải tiến
5. **Phần 4**: Huấn luyện và đánh giá tất cả mô hình
6. **Phần 5**: Vẽ biểu đồ so sánh cuối cùng trên tập Test
7. **Phần 6**: Phân tích sâu các mô hình
8. **Phần 7**: Phân tích thống kê và diễn giải mô hình nâng cao
9. **Phần 8**: Phân tích hệ số của mô hình Logistic Regression
10. **Phần 10**: Ma trận so sánh độ quan trọng của đặc trưng

## 📊 Biểu Đồ và Hình Ảnh

Notebook bao gồm các biểu đồ sau:

1. **Boxplot so sánh AUC qua 5-Fold CV** (ClassWeight và ADASYN)
2. **ROC Curves** so sánh các mô hình trên tập Test
3. **Calibration Plots** đánh giá hiệu chuẩn của các mô hình
4. **Feature Importance** cho từng mô hình
5. **Confusion Matrices** với ngưỡng tối ưu
6. **Correlation Heatmap** giữa các đặc trưng
7. **Distribution Plots** của các đặc trưng quan trọng

## 🔑 Tham Số Quan Trọng

- **Random Seed**: 38 (để đảm bảo tính tái lập)
- **Test Size**: 25% (83/329 mẫu)
- **Cross-Validation**: 5-Fold Stratified
- **Số đặc trưng được chọn**: 15 (bởi Mutual Information)
- **Ngưỡng tối ưu**: Được tối ưu trên OOF với ràng buộc Recall ≥ 0.70

## 📝 Kết Luận

1. **Mô hình RF (ClassWeight)** đạt hiệu suất tốt nhất với AUC = 0.8712 trên tập test
2. Tất cả các mô hình ML đều vượt trội so với GWTG-HF score (AUC = 0.6382)
3. RF (ClassWeight) và RF (ADASYN) có sự khác biệt có ý nghĩa thống kê so với GWTG-HF (p < 0.05)
4. Các đặc trưng quan trọng nhất bao gồm: Troponin I, tình trạng mất bù, AST, choáng tim, và NT-proBNP

## 👤 Tác Giả

**Đỗ Nhật Huy** - Học viên Bác sĩ Nội trú, Trường Y, Đại học Y Dược TP.HCM
**Trần Văn Anh Thư** - Sinh viên Khoa Công Nghệ Thông Tin, Đại học Khoa học Tự nhiên TP.HCM

## 📄 Giấy Phép

Dự án này được tạo cho mục đích nghiên cứu và học tập.

## 🙏 Lời Cảm Ơn

Cảm ơn các bác sĩ và nhân viên y tế đã cung cấp dữ liệu và hỗ trợ trong quá trình nghiên cứu.

---

**Lưu ý**: Dự án này chỉ phục vụ mục đích nghiên cứu. Không sử dụng cho mục đích chẩn đoán lâm sàng mà không có sự giám sát của chuyên gia y tế.

# ml-heart-failure-prediction
