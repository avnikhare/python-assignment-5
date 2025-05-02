import pandas as pd
import numpy as np
import matplotlib.pyplot as plt
import seaborn as sns

df = pd.read_csv("school_scores.csv")
print("First 5 Rows:\n", df.head()) print("\nLast 5 Rows:\n", df.tail())

print("\nSummary Statistics:\n", df.describe())
df_filled = df.fillna(df.mean(numeric_only=True))

subject_cols = [
    'Total.Math', 'Total.Verbal',
    'Academic Subjects.Arts/Music.Average GPA',
    'Academic Subjects.English.Average GPA',
    'Academic Subjects.Foreign Languages.Average GPA',
    'Academic Subjects.Mathematics.Average GPA',
    'Academic Subjects.Natural Sciences.Average GPA',
    'Academic Subjects.Social Sciences/History.Average GPA'
]

subject_means = df_filled[subject_cols].mean() 
print("\nAverage Score in Each Subject:\n", subject_means)

subject_stats = df_filled[subject_cols].agg(['mean', 'median', 'min', 'max'])
print("\nSubject Statistics (mean, median, min, max):\n", subject_stats)

plt.hist(df_filled['Total.Math'], bins=20, color='skyblue', edgecolor='black')
plt.title('Distribution of Total Math Scores')
plt.xlabel('Math Score')
plt.ylabel('Number of Students')
plt.grid(True)
plt.show()

plt.scatter(df_filled['Total.Math'], df_filled['Total.Verbal'], alpha=0.6, color='green') plt.title('Math vs Verbal Scores') 
plt.xlabel('Total Math Score') 
plt.ylabel('Total Verbal Score') 
plt.grid(True)
plt.show()

avg_gpas = subject_means[2:] avg_gpas.plot(kind='bar', color='orange') 
plt.title('Average GPA by Subject') 
plt.ylabel('GPA') plt.xticks(rotation=45, ha='right') 
plt.tight_layout() 
plt.show()

labels = ['A', 'B', 'C', 'D or lower']
sizes = [
    df_filled['GPA.A minus.Test-takers'].sum(),
    df_filled['GPA.B.Test-takers'].sum(),
    df_filled['GPA.C.Test-takers'].sum(),
    df_filled['GPA.D or lower.Test-takers'].sum()]

plt.pie(sizes, labels=labels, autopct='%1.1f%%', colors=['lightgreen', 'gold', 'lightskyblue', 'lightcoral'])
plt.title('Test-Taker Grade Distribution')
plt.axis('equal')
plt.show()

sns.boxplot(data=df_filled[['Total.Math', 'Total.Verbal']])
plt.title('Boxplot of Math and Verbal Scores')
plt.ylabel('Score')
