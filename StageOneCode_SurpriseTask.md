\#\#Hackbio Team Histidine StageOne Task

\#Surprise Task

\#Author: Saumi Shah

 \#GitHub: https://github.com/saumis22

\# LinkedIn: https://www.linkedin.com/in/saumi-shah-1010991a7

# **PART A \- Gene Expression Analysis**

# **1A–1F: Reproducing all plots in a 3x3 grid**

# **\-----------------------------**

# **Step 0: Imports**

# **\-----------------------------**

import pandas as pd import seaborn as sns import matplotlib.pyplot as plt import numpy as np

# **\-----------------------------**

# **Step 1: Load Datasets**

# **\-----------------------------**

# **1A: Normalized counts for heatmap**

norm\_counts\_url \= "[https://raw.githubusercontent.com/HackBio-Internship/2025\_project\_collection/refs/heads/main/Python/Dataset/hbr\_uhr\_top\_deg\_normalized\_counts.csv](https://raw.githubusercontent.com/HackBio-Internship/2025_project_collection/refs/heads/main/Python/Dataset/hbr_uhr_top_deg_normalized_counts.csv)" df\_norm \= pd.read\_csv(norm\_counts\_url, index\_col=0) df\_norm \= df\_norm.select\_dtypes(include=np.number) \# Keep numeric only

# **1B: DEG results for volcano plot**

deg\_url \= "[https://raw.githubusercontent.com/HackBio-Internship/2025\_project\_collection/refs/heads/main/Python/Dataset/hbr\_uhr\_deg\_chr22\_with\_significance.csv](https://raw.githubusercontent.com/HackBio-Internship/2025_project_collection/refs/heads/main/Python/Dataset/hbr_uhr_deg_chr22_with_significance.csv)" df\_deg \= pd.read\_csv(deg\_url) df\_deg.columns \= df\_deg.columns.str.strip() \# Clean column names color\_map \= {'up':'green', 'down':'blue', 'ns':'orange'}

# **PART B: Breast Cancer Data**

bc\_url \= "[https://raw.githubusercontent.com/HackBio-Internship/2025\_project\_collection/refs/heads/main/Python/Dataset/data-3.csv](https://raw.githubusercontent.com/HackBio-Internship/2025_project_collection/refs/heads/main/Python/Dataset/data-3.csv)" df\_bc \= pd.read\_csv(bc\_url) features \= \['radius\_mean', 'texture\_mean', 'perimeter\_mean', 'area\_mean', 'smoothness\_mean', 'compactness\_mean'\] df\_bc \= df\_bc.dropna(subset=features) \# Remove missing values

# **\-----------------------------**

# **Step 2: Create 3x3 subplot grid**

# **\-----------------------------**

fig, axes \= plt.subplots(3,3, figsize=(20,15)) axes \= axes.flatten() \# Flatten to easily index

# **\-----------------------------**

# **1A: Heatmap of Top DEGs**

# **\-----------------------------**

sns.heatmap(df\_norm, cmap="Blues", ax=axes\[0\]) axes\[0\].set\_title("1A: Heatmap of Top DEGs")

# **\-----------------------------**

# **1B: Volcano Plot**

# **\-----------------------------**

sns.scatterplot( x="log2FoldChange", y="-log10PAdj", hue="significance", data=df\_deg, palette=color\_map, ax=axes\[1\] ) axes\[1\].axvline(x=1, linestyle='--', color='grey') axes\[1\].axvline(x=-1, linestyle='--', color='grey') axes\[1\].set\_xlabel("log2(Fold Change)") axes\[1\].set\_ylabel("-log10(PAdj)") axes\[1\].set\_title("1B: Volcano Plot") axes\[1\].legend(title="Significance")

# **\-----------------------------**

# **1C: Scatter Plot (radius\_mean vs texture\_mean)**

# **\-----------------------------**

sns.scatterplot( x="radius\_mean", y="texture\_mean", hue="diagnosis", data=df\_bc, palette={'M':'blue','B':'orange'}, ax=axes\[2\] ) axes\[2\].set\_title("1C: Radius vs Texture")

# **\-----------------------------**

# **1D: Correlation Heatmap**

# **\-----------------------------**

sns.heatmap(df\_bc\[features\].corr(), annot=True, cmap="Blues", ax=axes\[3\]) axes\[3\].set\_title("1D: Correlation Heatmap")

# **\-----------------------------**

# **1E: Scatter Plot (smoothness\_mean vs compactness\_mean)**

# **\-----------------------------**

sns.scatterplot( x="smoothness\_mean", y="compactness\_mean", hue="diagnosis", data=df\_bc, palette={'M':'blue','B':'orange'}, ax=axes\[4\] ) axes\[4\].set\_title("1E: Smoothness vs Compactness") axes\[4\].grid(True)

# **\-----------------------------**

# **1F: Density Plot (area\_mean by diagnosis)**

# **\-----------------------------**

sns.kdeplot(df\_bc\[df\_bc\['diagnosis'\]=='M'\]\['area\_mean'\], fill=True, color='blue', bw\_adjust=0.5, ax=axes\[5\], label='M') sns.kdeplot(df\_bc\[df\_bc\['diagnosis'\]=='B'\]\['area\_mean'\], fill=True, color='orange', bw\_adjust=0.5, ax=axes\[5\], label='B') axes\[5\].set\_title("1F: Density of Area Mean by Diagnosis") axes\[5\].set\_xlabel("area\_mean") axes\[5\].set\_ylabel("Density") axes\[5\].legend()

# **\-----------------------------**

# **Hide empty subplots (if any)**

# **\-----------------------------**

for i in range(6,9): axes\[i\].axis('off')

plt.tight\_layout() plt.show()  
