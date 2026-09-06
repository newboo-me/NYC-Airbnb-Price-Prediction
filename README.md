# NYC Airbnb Price Prediction & Classification 

## Project Overview
โปรเจกต์นี้เป็นการวิเคราะห์และสร้างโมเดล Machine Learning เพื่อทำนายและจัดกลุ่มระดับราคาห้องพัก Airbnb ในนิวยอร์ก (New York City) จากข้อมูลขนาดใหญ่ โดยมุ่งเน้นที่การทำความสะอาดข้อมูลที่ซับซ้อน การระวังปัญหา Data Leakage และการสกัดปัจจัยที่มีผลต่อราคาผ่านกระบวนการทางสถิติและภูมิสารสนเทศ (Spatial Analysis) 

## Tech Stack
* **Data Manipulation:** Python, Pandas, NumPy
* **Visualization:** Matplotlib, Seaborn (Spatial Scatter Maps, Heatmaps, Boxplots)
* **Machine Learning:** Scikit-Learn (Random Forest, GridSearchCV)
* **Evaluation:** Classification Report, Feature Importances

## 1. Data Cleansing & Leakage Prevention
* **Currency Formatting:** ทำความสะอาดคอลัมน์ราคาโดยการถอดเครื่องหมาย `$` และ `,` ออกเพื่อแปลงสตริงเป็นข้อมูลแบบ Float
* **Imputation:** จัดการ Missing Values ด้วยหลักสถิติ (ใช้ค่า Median สำหรับข้อมูลตัวเลข และระบบ Tagging สำหรับข้อมูลกลุ่ม)
* **Preventing Data Leakage (Critical Step):** ตรวจพบความสัมพันธ์สมบูรณ์ (Perfect Correlation) ระหว่างตัวแปรเป้าหมายกับตัวแปร `service fee` จึงทำการตัด (Drop) คอลัมน์นี้ทิ้งก่อนเทรนโมเดล เพื่อป้องกันไม่ให้โมเดลโกงผลลัพธ์
* **Quantile Binning:** แปลงโจทย์ที่มีความแปรปรวนของราคาสูง จาก Regression เป็น Binary Classification (จัดกลุ่มระดับราคา Low vs High)

## 2. Exploratory Data Analysis (EDA)
ทำการสำรวจและสร้าง Visualization เพื่อหา Insight ทางสถิติเบื้องต้น:
* ตรวจสอบการกระจายตัวของราคา (Price Distribution) และการกระจุกตัวของราคาตามประเภทห้องพัก (Room Type)
* ทำ Spatial Analysis พล็อตแผนที่จากพิกัด Latitude และ Longitude เพื่อประเมินผลกระทบของ "ทำเลที่ตั้ง" ต่อระดับราคา

*(แนะนำ: ลากรูปกราฟแผนที่ Map Scatter Plot หรือ Heatmap ของคุณมาวางตรงนี้)*

## 3. Machine Learning & Hyperparameter Tuning
ทำการทดลองโมเดล Tree-Based และ Boosting (Random Forest, XGBoost) โดยเลือกลงลึกกับการปรับจูน **Random Forest Classifier**
* ใช้เทคนิค **GridSearchCV (3-Fold CV)** เพื่อค้นหาพารามิเตอร์ที่เหมาะสมที่สุดและหลีกเลี่ยงภาวะ Overfitting
* **Best Parameters:** `n_estimators=300`, `max_depth=20`, `min_samples_split=2`
* **Performance:** โมเดลที่ผ่านการจูนสามารถทำความแม่นยำ (Accuracy) เพิ่มขึ้นจาก Baseline 64.05% เป็น **71.14%** (F1-score = 0.71) บนชุดข้อมูลทดสอบ

## 4. Business Insights (Feature Importance)
จากการสกัดค่าความสำคัญของตัวแปร (Feature Importances) จากโมเดล สามารถสรุปปัจจัยหลักที่ส่งผลต่ออำนาจการตั้งราคาในนิวยอร์กได้ดังนี้:
1. **พิกัดและทำเลที่ตั้ง (Geographical Coordinates):** มีอิทธิพลต่อราคาสูงสุดถึง **28.2%**
2. **อัตราการเข้าพักและการรีวิว (Booking & Review Rates):** มีอิทธิพลรองลงมาที่ **21.8%** สะท้อนให้เห็นว่าความนิยมของห้องพักมีผลต่อการกำหนดระดับราคาอย่างมาก

*(แนะนำ: ลากรูปกราฟแท่ง Feature Importance 10 อันดับแรกมาวางตรงนี้)*
