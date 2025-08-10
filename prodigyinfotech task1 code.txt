import pandas as pd
import matplotlib.pyplot as plt
import seaborn as sns
pd.set_option('display.max_columns', None)

hos_ad = pd.read_csv("C:\\Users\\SURIYA\\Downloads\\HDHI Admission data.csv")
hos_ad

hos_ad.info()
hos_ad.describe()
hos_ad.isna().sum()

num_cols = ['HB', 'TLC', 'PLATELETS', 'GLUCOSE', 'UREA', 'CREATININE', 'EF']
# Convert numeric columns to proper numeric type (float)
for col in num_cols:
    hos_ad[col] = pd.to_numeric(hos_ad[col], errors='coerce')

# Now fill missing values with median
for col in num_cols:
    hos_ad[col].fillna(hos_ad[col].median(), inplace=True)

# Fill BNP with placeholder (-1)
hos_ad['BNP'] = pd.to_numeric(hos_ad['BNP'], errors='coerce')  # ensure BNP is numeric
hos_ad['BNP'].fillna(-1, inplace=True)

# Verify no missing values remain
print(hos_ad.isna().sum())

#-------------------------------
#1.Histogram for Age distribution of patients
#-------------------------------
plt.figure(figsize=(8,5))
sns.histplot(hos_ad['AGE'], bins=20, kde=True, color='skyblue')
plt.title('Age Distribution of Patients')
plt.xlabel('Age')
plt.ylabel('Count')
plt.grid(axis='y', linestyle='--', alpha=0.7)
plt.show()

#-------------------------------
#2.bar plot for gender distribution of patients
#-------------------------------
plt.figure(figsize=(6,4))
sns.countplot(data=hos_ad, x='GENDER', palette='pastel')
plt.title('Gender Distribution of Patients')
plt.xlabel('Gender')
plt.ylabel('Count')
plt.grid(axis='y', linestyle='--', alpha=0.7)
plt.show()

#-------------------------------
#3.Bar chart for Rural vs Urban
#-------------------------------
plt.figure(figsize=(6, 4))
sns.countplot(data=hos_ad, x='RURAL', palette='muted')
plt.title('Rural vs Urban Patients')
plt.xlabel('Rural/Urban')
plt.ylabel('Count')
plt.show()

#-------------------------------
# 4. Bar chart for Outcome (e.g., Discharged vs Expired)
# ------------------------------
plt.figure(figsize=(6, 4))
sns.countplot(data=hos_ad, x='OUTCOME', palette='coolwarm')
plt.title('Patient Outcomes')
plt.xlabel('Outcome')
plt.ylabel('Count')
plt.show()

# ------------------------------
# 5. Bar chart for Type of Admission (Emergency vs OPD)
# ------------------------------
plt.figure(figsize=(6, 4))
sns.countplot(data=hos_ad, x='TYPE OF ADMISSION-EMERGENCY/OPD', palette='Set2')
plt.title('Type of Admission')
plt.xlabel('Admission Type')
plt.ylabel('Count')
plt.show()

# ------------------------------
# 6. Monthly Trends of Admissions
# ------------------------------
plt.figure(figsize=(10, 5))
sns.countplot(data=hos_ad, x='month year', palette='viridis')
plt.title('Monthly Admissions Trend')
plt.xlabel('Month-Year')
plt.ylabel('Count')
plt.xticks(rotation=45)
plt.show()

# ------------------------------
# 7. Histograms for Key Lab Values
# ------------------------------
lab_cols = ['HB', 'TLC', 'PLATELETS', 'GLUCOSE', 'UREA', 'CREATININE', 'EF']
for col in lab_cols:
    plt.figure(figsize=(8, 5))
    sns.histplot(hos_ad[col], bins=20, kde=True, color='orange')
    plt.title(f'Distribution of {col}')
    plt.xlabel(col)
    plt.ylabel('Count')
    plt.show()

# ------------------------------
# 8. Comparative Bar Plot: Outcome vs Gender
# ------------------------------
plt.figure(figsize=(8, 5))
sns.countplot(data=hos_ad, x='OUTCOME', hue='GENDER', palette='Set1')
plt.title('Outcome by Gender')
plt.xlabel('Outcome')
plt.ylabel('Count')
plt.legend(title='Gender')
plt.show()

# ------------------------------
# 9. Comparative Bar Plot: Outcome vs Rural/Urban
# ------------------------------
plt.figure(figsize=(8, 5))
sns.countplot(data=hos_ad, x='OUTCOME', hue='RURAL', palette='Set2')
plt.title('Outcome by Rural/Urban')
plt.xlabel('Outcome')
plt.ylabel('Count')
plt.legend(title='Rural/Urban')
plt.show()

# ------------------------------
# 10. Average Duration of Stay by Gender (Bar Plot)
# ------------------------------
plt.figure(figsize=(6, 4))
sns.barplot(data=hos_ad, x='GENDER', y='DURATION OF STAY', estimator='mean', palette='pastel')
plt.title('Average Duration of Stay by Gender')
plt.xlabel('Gender')
plt.ylabel('Average Duration of Stay')
plt.show()
