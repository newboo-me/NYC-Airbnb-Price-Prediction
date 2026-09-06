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

<img width="1480" height="502" alt="Screenshot 2026-09-06 134811" src="https://github.com/user-attachments/assets/2e1c294a-e45d-4945-aec3-95438ef3c3e2" />
<img width="1427" height="584" alt="Screenshot 2026-09-06 134837" src="https://github.com/user-attachments/assets/f1addc6f-5027-430a-b7a5-feac8348cfee" />
<img width="942" height="556" alt="Screenshot 2026-09-06 134921" src="https://github.com/user-attachments/assets/d0de1833-f228-4aa4-89d2-09c4d6f9d1ef" />
<img width="391" height="355" alt="Screenshot 2026-09-06 134941" src="https://github.com/user-attachments/assets/ccff3452-1356-43de-9df3-f2e13b494acb" />
<img width="1050" height="643" alt="Screenshot 2026-09-06 135018" src="https://github.com/user-attachments/assets/f3922bca-a62b-45c9-adbc-eb54c61b3e0f" />
<img width="533" height="522" alt="Screenshot 2026-09-06 135037" src="https://github.com/user-attachments/assets/ae835043-8aef-4c60-9b03-5db8034d57c6" />
<img width="521" height="497" alt="Screenshot 2026-09-06 135059" src="https://github.com/user-attachments/assets/537072ca-ce94-4357-afcd-a21c5fcc524a" />
<img width="399" height="206" alt="Screenshot 2026-09-06 135117" src="https://github.com/user-attachments/assets/98a2e390-3292-4a7e-a09b-dc9845e3466f" />
<img width="443" height="233" alt="Screenshot 2026-09-06 135133" src="https://github.com/user-attachments/assets/40bbb5b4-96b8-420d-b566-32489b93e9d5" />
<img width="550" height="252" alt="Screenshot 2026-09-06 135217" src="https://github.com/user-attachments/assets/29a1e454-6dec-461c-9adf-ccbb91e64fa7" />


