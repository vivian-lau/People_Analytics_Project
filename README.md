# People_Analytics_Project
This project will takes a two-step approach to understand and address attrition by perform Employee Segmentation (Clustering) and Attrition Prediction (Classification). This combination helps HR teams not only identify who is at risk but also why certain groups are more vulnerable, enabling targeted interventions.

# HR Attrition Analysis & Clustering Project

## 📌 Overview
This project explores an HR dataset (provided in the repository) to identify key factors associated with employee attrition.  
The workflow includes:
- Data exploration & cleaning
- Feature selection using **Chi-Square** (categorical features) and **ANOVA** (numerical features)
- Clustering using **KMeans**
- Dimensionality reduction with **PCA**
- Iterative refinement of clustering after noticing department features dominated the clusters

The goal is to uncover patterns of employee attrition and segment employees into meaningful groups for HR insights.

---

## 📊 Dataset
The dataset includes both categorical and numerical employee attributes such as:
- **Categorical**: Gender, Department, JobRole, MaritalStatus, Overtime, etc.
- **Numerical**: Age, MonthlyIncome, JobLevel, DistanceFromHome, JobSatisfaction, YearsAtCompany, etc.
- **Target**: Attrition (Yes/No)

---

## 🔍 Steps Taken

### 1. Data Exploration & Cleaning
- Converted `Attrition` from *Yes/No* → *0/1*
- Encoded categorical variables (`Gender`, `Department`, etc.) using dummy variables
- Handled irrelevant columns (e.g., dropped `EmployeeID`)

### 2. Feature Selection
- **Chi-Square Test**: evaluated dependence between categorical features and attrition  
  - Example: `Department` was significant, `Gender` was not  
  ![Chi-Square Feature Importance](output/chi_square.png)

- **ANOVA (F-test)**: tested differences in numeric features across attrition groups  
  - Key predictors: `JobLevel`, `MonthlyIncome`, `Age`, `JobSatisfaction`, `DistanceFromHome`  
  ![ANOVA F-scores](output/anova.png)

### 3. Initial KMeans Clustering
- Applied KMeans to scaled features
- Found 4 clusters using elbow method (inertia values) and silhouette scores.

### 4. Dimensionality Reduction with PCA
- Used PCA to reduce dimensionality for visualization
- Observed PC2 was almost entirely explained by Department. The Department data strongly influenced the cluster formation and overshadowed more nuanced employee characteristics, so I decided to drop the Department data from KMean model.
- Decision: rerun KMeans without Department to uncover **behavioral/attrition-related** patterns  
  ![PCA Department Overlay](output/department.png)

### 5. Refined KMeans Clustering
- Re-ran clustering without Department
- Used elbow method and silhouette scores to come up with 4 clusters and visualie using PCA
- Interpreted 4 distinct employee clusters:
  - **Cluster 0 – Senior & Stable**: Older, higher-level, higher income, stable satisfaction  
  - **Cluster 1 – Early Career but Satisfied**: Younger, entry-level, lower income, but happier  
  - **Cluster 2 – Commute Risk Group**: Longer commutes, average pay, lower satisfaction  
  - **Cluster 3 – Disengaged & Dissatisfied**: Younger, lower-level, least satisfied (highest attrition risk)  
  ![Cluster Heatmap](output/kmean4_v2.png)

---

## 📈 Visualizations
- **Chi-Square bar plots** for categorical feature importance  
- **ANOVA F-score plots** for numerical features  
- **PCA scatterplots** to visualize department separation and cluster patterns  
- **Cluster heatmaps** of scaled feature centers for interpretation  

---

## 💡 Key Insights
- `Department` is highly separable but not always helpful for clustering employees by attrition risk  
- Strong predictors of attrition include: **JobLevel, MonthlyIncome, Age, DistanceFromHome, JobSatisfaction**  
- The most vulnerable group is **Cluster 3 – Disengaged & Dissatisfied**, which combines low pay, junior roles, and low satisfaction  
- Clustering is more meaningful when department is analyzed **after** clustering, not included in the clustering features

---

## 🚀 Next Steps
- Overlay **attrition rates per cluster** to validate high-risk groups  
- Build predictive models (Logistic Regression, Random Forest, XGBoost) using selected features  
- Provide HR recommendations for targeted retention strategies  

---

## 🛠️ Tech Stack
- **Python** (pandas, numpy, matplotlib, seaborn, scikit-learn, scipy)
- **Feature selection**: `scipy.stats.chi2_contingency`, `f_oneway`
- **Clustering**: KMeans (`sklearn.cluster`)
- **Dimensionality reduction**: PCA (`sklearn.decomposition`)

---

## 📂 Repository Structure
