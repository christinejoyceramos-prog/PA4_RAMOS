PA4 | ECE2112 | EXPERIMENT 4 | RAMOS
---
DATA WRANGLING ABD DATA VISUALIZATION
--- 
Submitted by: Ramos, Christine Joyce S. | 2ECE-A | 09.19.2026
---
OBJECTIVES
---
At the end of this laboratory activity, the student should be able to:

1. filter tabular data using several categorical and numerical conditions;

2. construct focused DataFrames by selecting relevant features;

3. summarize the relationship between categorical features and a numerical variable; and

4. communicate a data comparison using clear and correctly labeled plots.
   
INSTRUCTIONS
---
Use the same ECE Board Exam 2 dataset supplied for Experiment 4. Work in a Jupyter Notebook
using Pandas and a Python plotting library used in class. Use the dataset’s existing column labels,
including Name, Gender, Track, Hometown, Math, GEAS, Electronics, and Average.

• Derive all tables and plot values from the dataset. Do not manually type rows, category means, or
plotted values.

• When applying more than one condition, make every condition explicit in the filtering expression.
• Keep the original DataFrame unchanged.

• Every graph must have a title, axis labels, readable category labels, and a consistent scale appropriate to the data.

PROGRAMMING PROBLEMS
---
PROBLEM A. VISAYAS COMMUNICATION DATAFRAME
---
Create a DataFrame named VisComm containing students whose Hometown is Visayas and whose Track
is Communication. Retain only these columns, in the stated order:
Name, Gender, Math, Electronics, Average
Display the resulting DataFrame and its number of rows. Both filtering conditions must be applied to
the source dataset before the columns are selected.

CODE
---
```
df = pd.read_excel("board2.xlsx") // this reads and stores board2 file to df

if "Average" not in df.columns:
    df["Average"] = df[["Math", "Electronics", "GEAS", "Communication"]].mean(
        axis=1)

VisComm = df[(df["Hometown"] == "Visayas") & (df["Track"] == "Communication")][
    ["Name", "Gender", "Math", "Electronics", "Average"]
]// this filters out the other columns to 'Name', 'Gender', 'Math', 'Electronics', 'Average'

print("--- Problem A: VisComm DataFrame ---")
print(VisComm)
print(f"\nNumber of rows in VisComm: {len(VisComm)}") // this displays the 'Name', 'Gender', 'Math', 'Electronics', 'Average'
```
PROBLEM B. VISAYAS FEMALE DATAFRAME
---
Create a second DataFrame named VisFemale containing students whose Hometown is Visayas and
whose Gender is Female. Retain only:
Name, Track, GEAS, Electronics, Average
Display VisFemale. Then display only the rows of VisFemale whose Average is at least 60. Do not
overwrite VisFemale when performing this second filter.

CODE
---
```
VisFemale = df[(df["Hometown"] == "Visayas") & (df["Gender"] == "Female")][
    ["Name", "Track", "GEAS", "Electronics", "Average"] // this sets a new DataFrame(VisFemale) filtered to Visayas and Female
]

print("\n--- Problem B: VisFemale DataFrame ---")
print(VisFemale) // this displays the average less than or equal to 60 of the female students in Visayas
```
PROBLEM C. CATEGORY-AVERAGE VISUALIZATION
---
Examine how the recorded Average differs across the three categorical features Track, Gender, and
Hometown.

a. For each feature, compute the mean of Average for every category using Pandas.

b. Display the three summary tables.

c. Create one figure containing three bar charts: mean Average by Track, by Gender, and by
Hometown.

d. Below the figure, write three concise statements identifying the category with the highest sample
mean for each feature.

Interpretation rule: Describe the observed dataset only. A difference in group means does not, by
itself, establish that a feature causes a higher board-exam score.

CODE
---
```
import matplotlib.pyplot as plt
import seaborn as sns

mean_track = df.groupby("Track")["Average"].mean().reset_index()
mean_gender = df.groupby("Gender")["Average"].mean().reset_index()
mean_hometown = df.groupby("Hometown")["Average"].mean().reset_index()

print("\n--- Summary Table 1: Mean Average by Track ---")
print(mean_track)

print("\n--- Summary Table 2: Mean Average by Gender ---")
print(mean_gender)

print("\n--- Summary Table 3: Mean Average by Hometown ---")
print(mean_hometown)

sns.barplot(
    data=mean_track,
    x="Track",
    y="Average",
    ax=axes[0],
    palette="Blues_d",
    hue="Track",
    legend=False,
)
axes[0].set_title("Mean Average by Track", fontsize=12, fontweight="bold")
axes[0].set_xlabel("Track", fontsize=10)
axes[0].set_ylabel("Mean Average Score", fontsize=10)
axes[0].grid(axis="y", linestyle="--", alpha=0.7)

for p in axes[0].patches:
    axes[0].annotate(
        f"{p.get_height():.2f}",
        (p.get_x() + p.get_width() / 2.0, p.get_height()),
        ha="center",
        va="bottom",
        fontsize=9,
        xytext=(0, 3),
        textcoords="offset points",
    )
sns.barplot(
    data=mean_gender,
    x="Gender",
    y="Average",
    ax=axes[1],
    palette="Greens_d",
    hue="Gender",
    legend=False,
)
axes[1].set_title("Mean Average by Gender", fontsize=12, fontweight="bold")
axes[1].set_xlabel("Gender", fontsize=10)
axes[1].set_ylabel("")
axes[1].grid(axis="y", linestyle="--", alpha=0.7)

for p in axes[1].patches:
    axes[1].annotate(
        f"{p.get_height():.2f}",
        (p.get_x() + p.get_width() / 2.0, p.get_height()),
        ha="center",
        va="bottom",
        fontsize=9,
        xytext=(0, 3),
        textcoords="offset points",
    )
  sns.barplot(
    data=mean_hometown,
    x="Hometown",
    y="Average",
    ax=axes[2],
    palette="Oranges_d",
    hue="Hometown",
    legend=False,
)
axes[2].set_title("Mean Average by Hometown", fontsize=12, fontweight="bold")
axes[2].set_xlabel("Hometown", fontsize=10)
axes[2].set_ylabel("")
axes[2].grid(axis="y", linestyle="--", alpha=0.7)

for p in axes[2].patches:
    axes[2].annotate(
        f"{p.get_height():.2f}",
        (p.get_x() + p.get_width() / 2.0, p.get_height()),
        ha="center",
        va="bottom",
        fontsize=9,
        xytext=(0, 3),
        textcoords="offset points",
    )
    plt.ylim(0, 100)
plt.suptitle(
    "Comparison of Mean Average Scores Across Categories",
    fontsize=14,
    fontweight="bold",
    y=1.03,
)
plt.tight_layout()
plt.show()
top_track = mean_track.loc[mean_track["Average"].idxmax()]
top_gender = mean_gender.loc[mean_gender["Average"].idxmax()]
top_hometown = mean_hometown.loc[mean_hometown["Average"].idxmax()]

print("\n--- Interpretation Statements ---")
print(
    f"1. Among the tracks, students in the {top_track['Track']} track recorded the highest sample mean Average score ({top_track['Average']:.2f})."
)
print(
    f"2. Between genders, {top_gender['Gender']} students achieved the highest sample mean Average score ({top_gender['Average']:.2f})."
)
print(
    f"3. Across hometown regions, students from {top_hometown['Hometown']} obtained the highest sample mean Average score ({top_hometown['Average']:.2f})."
)
```
END OF NOTEBOOK
---
