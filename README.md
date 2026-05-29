# LaoVietBank Customer Churn Classification Demo

Demo nay trinh bay quy trinh xay dung mo hinh du doan kha nang khach hang roi bo ngan hang (customer churn) cho LaoVietBank. Notebook di tu buoc doc du lieu, tien xu ly, phan tich, huan luyen mo hinh, danh gia, giai thich ket qua bang SHAP va tao tap du doan co do tin cay bang MAPIE.

## Noi dung repository

| File | Mo ta |
| --- | --- |
| `Demo.ipynb` | Notebook chua source code cho toan bo pipeline machine learning. |
| `Laovietbank.pdf` | Ban in/export cua notebook, bao gom code va output de xem nhanh ket qua demo. |

## Muc tieu demo

- Du doan nhom khach hang co nguy co churn de ngan hang co the chu dong giu chan.
- So sanh nhieu mo hinh classification cho bai toan du lieu mat can bang.
- Giai thich ly do mo hinh dua ra du doan bang SHAP de tang tinh minh bach.
- Bo sung MAPIE/conformal prediction de tao prediction set va muc do tin cay cho du doan.

## Pipeline chinh

1. **Load du lieu**
   - Doc du lieu tu file `BankingDataset.xlsx`.
   - Kiem tra shape, kieu du lieu va missing values.

2. **Tien xu ly va feature engineering**
   - Mapping bien phan loai `Gender`.
   - Tao cac bien moi:
     - `BalancePerProduct`
     - `ActivityPerTenure`
     - `SalaryPerAge`
     - `TransferLoginRatio`
   - Xu ly outlier bang IQR clipping cho cac cot so quan trong.

3. **EDA**
   - Thong ke mo ta du lieu.
   - Ve phan phoi cac bien so.
   - Phan tich tuong quan va Mutual Information de ho tro chon feature.

4. **Training va evaluation**
   - Chia du lieu thanh train/calibration/test theo ty le 70%/10%/20%.
   - Huan luyen va so sanh:
     - Random Forest
     - XGBoost
     - LightGBM
   - Danh gia bang classification report, confusion matrix, ROC-AUC va PR-AUC.
   - Chon mo hinh tot nhat dua tren ROC-AUC.

5. **Model explainability voi SHAP**
   - SHAP summary/bar plot de xem feature importance tong the.
   - SHAP waterfall plot de giai thich du doan tren tung khach hang.

6. **Uncertainty estimation voi MAPIE**
   - Calibrate mo hinh tren calibration set.
   - Tao prediction set theo muc tin cay cau hinh.
   - Kiem tra coverage va do rong prediction set theo cac nguong alpha.

## Thu vien su dung

Notebook su dung cac thu vien Python chinh:

- `pandas`, `numpy`
- `matplotlib`, `seaborn`
- `scikit-learn`
- `xgboost`
- `lightgbm`
- `shap`
- `mapie`
- `joblib`

Cell dau notebook co lenh cai dat nhanh:

```python
!pip install shap mapie xgboost lightgbm -q
```

## Cach chay

1. Mo `Demo.ipynb` bang Jupyter Notebook, JupyterLab hoac Google Colab.
2. Dam bao file du lieu `BankingDataset.xlsx` ton tai dung duong dan notebook dang doc.
3. Chay lan luot cac cell tu tren xuong duoi.
4. Neu chi can xem ket qua da export, mo file `Laovietbank.pdf`.

> Luu y: dataset goc khong nam trong repository nay. Neu chay lai notebook, can cap nhat duong dan doc file du lieu cho phu hop voi moi truong local/Colab.

## Ket qua dau ra

Notebook tao va su dung cac artifact tam thoi trong thu muc `models/` khi duoc chay lai, bao gom model da train, danh sach feature va ten mo hinh tot nhat. Thu muc nay khong bat buoc phai co san trong repository vi co the sinh lai tu notebook.
