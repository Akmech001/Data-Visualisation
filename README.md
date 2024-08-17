# Performance Ratio Evolution Project

This project focuses on preprocessing and visualizing the Performance Ratio (PR) of a photovoltaic system over time, using both PR and Global Horizontal Irradiance (GHI) data. The script processes the data, calculates relevant metrics, and generates a detailed plot illustrating the evolution of PR, along with a target budget line and color-coded GHI values.

## Setup and Requirements

Ensure you have the following libraries installed in your environment:

```bash
pip install pandas matplotlib seaborn numpy
```

## Running the Script

### 1. Preprocessing and Merging Data

The `preprocess_data()` function takes PR and GHI data files stored in separate directories, merges them based on the 'Date' column, and outputs a combined CSV file. Update the paths to the folders containing your PR and GHI data.

```python
import pandas as pd
import os
import glob

# Define the paths
pr_folder = "C:\\Machone learning projects\\INternshala\\job 2\\PR"
ghi_folder = "C:\\Machone learning projects\\INternshala\\job 2\\GHI"
output_file = 'merged_data.csv'

# Call the function to preprocess the data
merged_data = preprocess_data(pr_folder, ghi_folder, output_file)
print("Merged data saved to:", output_file)
```

### 2. Data Visualization

After preprocessing the data, use the following script to visualize the Performance Ratio (PR) over time:

```python
import pandas as pd
import matplotlib.pyplot as plt
import seaborn as sns
from matplotlib.dates import DateFormatter
import numpy as np

# Load the merged data
df = pd.read_csv("merged_data.csv")

# Plotting the PR evolution
plt.figure(figsize=(14, 8))

# Scatter plot for PR values with GHI color mapping
plt.scatter(df['Date'], df['PR'], c=df['Color'], label='PR', edgecolor='black', s=50)

# Plot the 30-day moving average line for PR
plt.plot(df['Date'], df['PR_MA30'], color='red', label='30-d moving average of PR', linewidth=2)

# Plot the budget line
plt.plot(df['Date'], df['Budget'], color='darkgreen', label='Target Budget Yield Performance Ratio', linewidth=2)

# Format the date axis
plt.gca().xaxis.set_major_formatter(DateFormatter('%b/%y'))

# Add labels and title
plt.title('Performance Ratio Evolution\nFrom 2019-07-01 to 2022-03-24', fontsize=16)
plt.xlabel('Date', fontsize=14)
plt.ylabel('Performance Ratio [%]', fontsize=14)

# Add legend
plt.legend(loc='best', fontsize=12)

# Display Average PR for the last periods
average_pr_7d = df['PR'].tail(7).mean()
average_pr_30d = df['PR'].tail(30).mean()
average_pr_60d = df['PR'].tail(60).mean()
average_pr_90d = df['PR'].tail(90).mean()
average_pr_365d = df['PR'].tail(365).mean()
average_pr_lifetime = df['PR'].mean()

# Add text box
plt.text(pd.to_datetime('2021-10-01'), 20, 
         f'Average PR last 7-d: {average_pr_7d:.1f} %\n'
         f'Average PR last 30-d: {average_pr_30d:.1f} %\n'
         f'Average PR last 60-d: {average_pr_60d:.1f} %\n'
         f'Average PR last 90-d: {average_pr_90d:.1f} %\n'
         f'Average PR last 365-d: {average_pr_365d:.1f} %\n'
         f'Average PR Lifetime: {average_pr_lifetime:.1f} %',
         fontsize=12, bbox=dict(facecolor='white', alpha=0.5))

# Show the plot
plt.show()

# Save the plot as an image
plt.savefig('performance_ratio_evolution.png', dpi=300, bbox_inches='tight')
```

### 3. Bonus: Interactive Visualization via Command Line

You can also interactively visualize the data over a specific date range using the following command-line script:

```python
import argparse
import sys

def visualize_data(start_date=None, end_date=None):
    # Load the merged data
    df = pd.read_csv("merged_data.csv")

    # Filter based on date range if provided
    if start_date and end_date:
        mask = (df['Date'] >= start_date) & (df['Date'] <= end_date)
        filtered_data = df.loc[mask]
    else:
        filtered_data = df

    # Visualization code as above

if __name__ == "__main__":
    if not (hasattr(sys, 'ps1') or sys.flags.interactive):
        parser = argparse.ArgumentParser(description='Visualize PR data.')
        parser.add_argument('--start_date', type=str, help='Start date in YYYY-MM-DD format')
        parser.add_argument('--end_date', type=str, help='End date in YYYY-MM-DD format')

        args = parser.parse_args()
        
        visualize_data(args.start_date, args.end_date)
    else:
        # Provide default dates or call the function directly
        visualize_data()
```

### 4. Output

The output of the visualization script is a plot that displays:
- **Performance Ratio (PR)** over time.
- A **30-day moving average** of PR.
- A **target budget line** for the expected PR performance.
- Color-coded data points representing different ranges of Global Horizontal Irradiance (GHI).

