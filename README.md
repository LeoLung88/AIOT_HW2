# 學生表現預測專案

## 專案簡介

Kaggle : https://www.kaggle.com/datasets/jayaantanaath/student-habits-vs-academic-performance/data
本專案使用線性回歸模型預測學生的考試成績，採用 CRISP-DM (Cross-Industry Standard Process for Data Mining) 流程進行資料科學專案開發。專案目標是識別影響學生表現的關鍵因素，並建立準確的預測模型。

## 資料說明

- **資料檔案**: `student_habits_performance.csv`
- **資料結構**:
  - 總共 15 個欄位
  - 第一欄：`student_id` (學生ID，已移除)
  - 最後一欄：`exam_score` (考試成績，目標變數)
  - 中間 14 欄：特徵變數
    - 數值特徵：`age`, `study_hours_per_day`, `social_media_hours`, `netflix_hours`, `attendance_percentage`, `sleep_hours`, `exercise_frequency`, `mental_health_rating`
    - 類別特徵：`gender`, `part_time_job`, `diet_quality`, `parental_education_level`, `internet_quality`, `extracurricular_participation`

## CRISP-DM 流程說明

### 1. 商業理解 (Business Understanding)

**目標**：
- 預測學生的考試成績
- 識別影響學生表現的關鍵因素
- 提供教育機構改善學生表現的建議

**成功標準**：
- 建立準確的預測模型（R² > 0.7，RMSE 盡可能低）
- 識別影響學生表現的重要特徵
- 提供可解釋的模型結果

### 2. 資料理解 (Data Understanding)

**執行內容**：
- 載入資料並檢視基本資訊
- 檢查缺失值
- 探索目標變數分佈
- 分析類別變數分佈
- 計算數值變數與目標變數的相關係數
- 繪製相關係數熱圖

**主要發現**：
- 資料完整性檢查
- 目標變數分佈分析
- 特徵與目標變數的相關性分析

### 3. 資料準備 (Data Preparation)

#### 3.1 資料清理
- 移除學生ID欄位（不具預測價值）
- 分離特徵變數和目標變數

#### 3.2 特徵編碼
- 對類別變數進行編碼處理
- 二元類別變數（`gender`, `part_time_job`, `extracurricular_participation`）使用標籤編碼
- 多類別變數（`diet_quality`, `parental_education_level`, `internet_quality`）使用標籤編碼

#### 3.3 特徵選擇
採用三種方法進行特徵選擇，並綜合結果：

1. **相關係數方法**：計算特徵與目標變數的相關係數，選擇相關係數較高的特徵
2. **SelectKBest (F-test)**：使用 F 檢定統計量評估特徵重要性
3. **遞迴特徵消除 (RFE)**：使用線性回歸模型進行遞迴特徵消除

最終選擇至少被兩種方法選中的特徵，或選擇票數最高的前 10 個特徵。

#### 3.4 資料標準化與分割
- 將資料分割為訓練集（70%）和測試集（30%）
- 使用 StandardScaler 對特徵進行標準化（對線性回歸模型很重要）

### 4. 建模 (Modeling)

#### 4.1 建立線性回歸模型
- 使用 scikit-learn 的 `LinearRegression` 建立模型
- 訓練模型並顯示模型係數
- 視覺化特徵重要性

#### 4.2 模型預測
- 對訓練集和測試集進行預測

### 5. 評估 (Evaluation)

#### 5.1 計算評估指標
使用多種指標評估模型表現：
- **R² 分數 (R-squared)**：反映模型解釋變異的比例，範圍 0-1，越高越好
- **RMSE (Root Mean Squared Error)**：預測誤差的標準差，單位與目標變數相同，越低越好
- **MAE (Mean Absolute Error)**：平均絕對誤差，更直觀的誤差指標，越低越好

#### 5.2 視覺化評估結果
- 實際值 vs 預測值散點圖
- 殘差分析圖
- 殘差分佈圖

#### 5.3 模型解釋性分析
- 分析模型係數的意義
- 解釋特徵對目標變數的影響方向（正向/負向）
- 識別最重要的特徵

### 6. 總結與結論

**模型表現總結**：
- 評估指標的意義和解釋
- 模型準確性和穩定性分析

**主要發現**：
- 重要特徵識別
- 模型預測能力評估
- 過度擬合檢查

**後續建議**：
- 嘗試其他模型進行比較
- 進行更詳細的特徵工程
- 使用交叉驗證進行更穩健的評估

## 程式設計流程

### 檔案結構
```
AIOT_HW2/
├── student_performance_prediction.ipynb  # 主要程式檔案（Jupyter Notebook）
├── student_habits_performance.csv        # 資料檔案
└── README.md                            # 專案說明文件
```

### 執行步驟

1. **環境準備**
   ```bash
   # 確保已安裝必要的 Python 套件
   pip install pandas numpy matplotlib seaborn scikit-learn jupyter
   ```

2. **開啟 Jupyter Notebook**
   ```bash
   jupyter notebook student_performance_prediction.ipynb
   ```

3. **執行所有 Cell**
   - 按照順序執行所有程式碼單元格
   - 每個程式碼單元格之間都有 Markdown 說明

### 主要套件

- `pandas`: 資料處理和分析
- `numpy`: 數值計算
- `matplotlib`: 資料視覺化
- `seaborn`: 統計圖表繪製
- `scikit-learn`: 機器學習模型和工具
  - `LinearRegression`: 線性回歸模型
  - `StandardScaler`: 資料標準化
  - `LabelEncoder`: 類別變數編碼
  - `SelectKBest`, `RFE`: 特徵選擇
  - `train_test_split`: 資料分割
  - `mean_squared_error`, `r2_score`, `mean_absolute_error`: 評估指標

## 修改紀錄

### 版本 1.0 (初始版本)
- **日期**: 2024
- **修改內容**:
  - 建立專案架構
  - 實作完整的 CRISP-DM 流程
  - 實作資料理解階段（資料探索、EDA）
  - 實作資料準備階段（資料清理、特徵編碼、特徵選擇）
  - 實作線性回歸模型
  - 實作模型評估（多種評估指標和視覺化）
  - 建立 README.md 文件

### 主要功能

1. **資料探索**：
   - 自動檢查資料基本資訊
   - 缺失值分析
   - 目標變數分佈視覺化
   - 相關係數分析

2. **特徵選擇**：
   - 三種特徵選擇方法（相關係數、SelectKBest、RFE）
   - 綜合投票機制選擇最佳特徵

3. **模型建立**：
   - 線性回歸模型訓練
   - 特徵重要性分析

4. **模型評估**：
   - 多種評估指標（R²、RMSE、MAE）
   - 視覺化評估結果
   - 殘差分析

## 注意事項

1. **資料檔案位置**：確保 `student_habits_performance.csv` 與 notebook 在同一目錄
2. **隨機種子**：使用 `random_state=42` 確保結果可重現
3. **特徵選擇**：最終選擇的特徵數量可能因資料而異，預設選擇前 10 個重要特徵
4. **標準化**：線性回歸模型使用標準化特徵，確保不同尺度的特徵可以公平比較

## 未來改進方向

1. **模型優化**：
   - 嘗試其他回歸模型（如 Ridge、Lasso、Elastic Net）
   - 使用交叉驗證進行超參數調整
   - 嘗試非線性模型（如多項式回歸、隨機森林）

2. **特徵工程**：
   - 建立特徵交互項
   - 進行特徵變換（如對數轉換）
   - 處理異常值

3. **模型解釋**：
   - 使用 SHAP 值進行更詳細的特徵重要性分析
   - 提供更直觀的模型解釋

4. **部署**：
   - 將模型打包為可部署的格式
   - 建立預測 API
   - 建立互動式儀表板

---
## NotebookLM 
相關研究 : https://www.nature.com/articles/s41597-025-04913-0
### 報告摘要

#### 1. 研究目標與問題定義

該研究旨在**解決物理教育研究 (PER) 缺乏專用、多維度資料集**的問題。儘管 PER 學者已開始運用機器學習 (ML) 技術預測學生表現，但目前公開的資料集（如 OULAD）缺乏物理教育所需的認知、科學能力和學習態度等特定特徵。提供 SPHERE 資料集至關重要，因為它能促進 PER 領域使用 ML 和資料探勘 (EDM) 技術的發展，並幫助教師透過回應式和適應性的回饋，**有效監控學生學習進度**，進而提高物理教育的有效性。

#### 2. 主要方法與創新點

核心方法是透過收集物理教育學者建立的**研究型評估工具 (RBAs)** 數據，建立一個包含學生概念理解、科學能力和學習態度等**多領域表現**的表格化資料集。

創新點在於：**SPHERE 資料集本身是第一個被建立、驗證並公開發佈的、專門為 PER 領域預測建模目的設計的多維度學生表現資料集**。

#### 3. 實驗與結果描述

研究使用 **497 筆紀錄**和 **331 個特徵**的 SPHERE 資料集，採用 **隨機森林 (Random Forest, RF)** 演算法進行二元分類，以預測學生期末表現（FINTEST2：高成就者 vs. 低成就者）。實驗採用 **$k$ 摺交叉驗證**方法，並重複十次訓練與測試來報告平均性能指標。

研究訓練了四個 RF 模型，其中 **RF4 模型**結合了概念評估、科學能力、學習態度以及人口統計變數中的**十個最重要特徵**。

**基準線 (Baseline)** 是物理教師根據其主觀判斷和先前經驗對學生表現所做的預測 (TEACHPRED)。

**提升幅度：** 表現最佳的 **RF4 模型**在 AUROC (曲線下面積) 和特異性 (Specificity) 方面優於其他 RF 模型。更關鍵的是，所有基於 RF 的預測模型都**優於**教師的主觀判斷（TEACHPRED）。

#### 4. 關鍵洞見

此研究的本質貢獻在於**工程與資源層面的提供**：它透過釋出一個經過嚴格驗證和可靠 RBAs 構建的 SPHERE 資料集，**彌補了 PER 領域的數據稀缺性**。這項工作為 PER 學者提供了新的基礎，使得未來能夠應用更穩健的統計技術（如 ML），以**更有效的方式**（相對於人類主觀判斷）提取隱藏的學習模式，並提供早期預警和支援差異化教學。
