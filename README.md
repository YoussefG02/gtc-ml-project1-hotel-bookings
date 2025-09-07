# Hotel Booking Cancellations – Data Cleaning & Preprocessing

## Project Overview
This project focuses on **data cleaning and preprocessing** for the **hotel booking dataset**.  
The objective is to prepare the data for building a machine learning model that predicts booking cancellations.  
The work follows the instructions from **GTC ML Project 1**.

---

## Repository Contents
- **`Project_1.ipynb`** → Jupyter Notebook containing the entire workflow (EDA → Cleaning → Feature Engineering → Preprocessing).  
- **`hotel_bookings.csv`** → Raw dataset provided.  
- **`README.md`** → Project documentation (this file).  

---

## Workflow

### Phase 1: Exploratory Data Analysis (EDA)
- Generated summary statistics and dataset info
- Visualized missing values using **Missingno**  
- Detected outliers using **boxplots** and the **IQR method** 
- Documented main data quality issues:  
  - Missing values in `company`, `agent`, `country`, and `children`
  - Found extreme outliers in the following columns: `lead_time`, `adr` `stays_in_week_nights`, `adults`,  `children`, `booking_changes`

### Phase 2: Data Cleaning
1.   Missing Values:
      *   `children` imputed using median
      *   `country` imputed using mode
      *   `agent` replaced missing values with 0
      *   `company` replaced missing values with 0

2.   Duplicates:
      *   Dropped duplicate rows

3.   Outliers:
      *   Applied percentile capping (1st–99th) for: `stays_in_weekend_nights`, `stays_in_week_nights`, `adults`, `children`, `babies`, `booking_changes`
      *   Special handling:
          * For `adr`, capped values above 1000 at 1000
          * For `lead_time`, capped values above 365 at 365
      *   Justification: These rules remove unrealistic values while keeping the overall distribution. Percentile capping reduces noise, and hard caps prevent extreme outliers from skewing results

4.   Data Types:
      *   Converted `reservation_status_date` into datetime format
      *   Created a new `arrival_date` column by combining year, month, and day into a proper datetime

### Phase 3: Feature Engineering & Preprocessing
1. New Features
   - `total_guests` = `adults` + `children` + `babies`
   - `total_nights` = `stays_in_weekend_nights` + `stays_in_week_nights`
   - `is_family` = binary flag (1 if children or babies > 0, else 0)

2. Encoding
   - Applied **One-Hot Encoding** for low-cardinality variables (`hotel`, `meal`, `market_segment`, `distribution_channel`, `reserved_room_type`, `assigned_room_type`, `deposit_type`, `customer_type`)
   - For `country`, grouped rare categories (<1% frequency) into **"Other"**, then applied **frequency encoding**
   - Converted all **boolean columns** to integers (0/1) for ML compatibility

3. Data Leakage Prevention
   - Dropped `reservation_status` and `reservation_status_date`

4. Final Preparation
   - Split dataset into **train (80%)** and **test (20%)** using stratified sampling on `is_canceled`

---

## Technologies Used
- Python  
- Pandas, NumPy  
- Matplotlib, Seaborn, Missingno (for EDA/visualization)  
- Scikit-learn (for train-test split & preprocessing)  

---

## How to Use
1. Clone this repository:  
   ```bash
   git clone https://github.com/YoussefG02/gtc-ml-project1-hotel-bookings.git
