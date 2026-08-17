### EX3 Implementation of GSP Algorithm In Python
### DATE: 04-08-2026 
### AIM: To implement GSP Algorithm In Python.
### Description:
The Generalized Sequential Pattern (GSP) algorithm is a data mining technique used for discovering frequent patterns within a sequence database. It operates by identifying sequences that frequently occur together. GSP works by employing a depth-first search strategy to explore and extract frequent patterns efficiently.
### Steps:
1. <strong>Database Scanning:</strong> GSP scans the sequence database to determine the support of each item in the dataset.
2. <strong>Candidate Generation:</strong> It generates a set of candidate sequences using frequent items found in the previous step.
3. <strong>Pattern Growth:</strong> It extends the candidate sequences by merging them to form longer patterns, checking their support against a user-defined minimum support threshold.
4. <strong>Repeat:</strong> The process continues until no new sequences meet the minimum support threshold.
<p align="justify">
GSP finds application in various domains such as market basket analysis, web usage mining, bioinformatics, and more. For instance, in retail, GSP can identify common purchasing patterns, helping businesses understand customer behavior for targeted marketing or inventory management.
</p>

### Procedure:
<p align="justify">
1. From collections import defaultdict, from itertools import combinations: Imports necessary libraries/modules. defaultdict is
used to create a dictionary with default values and combinations generates all possible combinations of a sequence.</p>
<p align="justify">
2. generate_candidates(dataset, k): Function to generate candidate k-item sequences from a dataset. It loops through each sequence in the
dataset and finds combinations of length k for each sequence, updating their counts in a dictionary.</p>
<p align="justify">
3. gsp(dataset, min_support): Function that implements the Generalized Sequential Pattern (GSP) algorithm. It iterates through increasing
sequence lengths (k) until no new frequent patterns are found. It calls generate_candidates() to find patterns of varying lengths.</p>
<p align="justify">
4. Example dataset for each category: Defines example sequences for top wear, bottom wear, and party wear categories.</p>
<p align="justify">
5. Minimum support threshold: Sets the minimum support count required for a pattern to be considered frequent.</p>
<p align="justify">
6. Perform GSP algorithm for each category: Applies the GSP algorithm for each category using the defined example datasets and the
minimum support threshold.</p>
<p align="justify">
7. Output the frequent sequential patterns for each category: Prints the frequent sequential patterns 
    along with their support counts
for each wear category.</p>
<p align="justify">
8. Visulaize the sequence patterns using matplotlib.
</p>

### Program:

```python

from collections import defaultdict
import matplotlib.pyplot as plt
 
# ==========================
# PART – 1 Complete this function
# ==========================
def is_subsequence(candidate, sequence):
    candidate_index = 0

    for item in sequence:
        if candidate_index < len(candidate) and item == candidate[candidate_index]:
            candidate_index += 1

        if candidate_index == len(candidate):
            return True

    return False


def generate_L1(database, min_support):
    counts = defaultdict(int)
    for seq in database:
        for item in set(seq):
            counts[(item,)] += 1
    return {k: v for k, v in counts.items() if v >= min_support}
 
 # ==========================
# PART – 2 : Complete this function
# ==========================

def generate_candidates(prev_patterns):
    candidates = set()

    patterns = list(prev_patterns.keys())

    for pattern1 in patterns:
        for pattern2 in patterns:
            if pattern1[1:] == pattern2[:-1]:

                new_candidate = pattern1 + (pattern2[-1],)
                if len(set(new_candidate)) == len(new_candidate):
                    candidates.add(new_candidate)

    return candidates

def count_support(database, candidates, min_support):
    support = defaultdict(int)
    for c in candidates:
        for seq in database:
            if is_subsequence(c, seq):
                support[c] += 1
    return {k: v for k, v in support.items() if v >= min_support}
 
 # ==========================
# PART – 3 : Complete this function
# ==========================
def gsp(database, min_support):

    current_patterns = generate_L1(database, min_support)
    all_patterns = dict(current_patterns)
    while current_patterns:
        candidates = generate_candidates(current_patterns)
        if not candidates:
            break
        current_patterns = count_support(
            database,
            candidates,
            min_support
        )

        all_patterns.update(current_patterns)

    return all_patterns

# Example dataset for each category
top_wear_data = [
    ["blouse", "t-shirt", "tank_top"],
    ["hoodie", "sweater", "top"],
    ["hoodie"],
    ["hoodie", "sweater"]
    # Add more sequences for top wear
]
 

bottom_wear_data = [
    ["jeans", "trousers", "shorts"],
    ["leggings", "skirt", "chinos"],
    # Add more sequences for bottom wear
]
 
party_wear_data = [
    ["cocktail_dress", "evening_gown", "blazer"],
    ["party_dress", "formal_dress", "suit"],
    ["party_dress", "formal_dress", "suit"],
    ["party_dress", "formal_dress", "suit"],
    ["party_dress", "formal_dress", "suit"],
    ["party_dress"],
    ["party_dress"]
    # Add more sequences for party wear
]
 
# Minimum support threshold
min_support = 2
 
# Perform GSP algorithm for each category
top_wear_result = gsp(top_wear_data, min_support)
bottom_wear_result = gsp(bottom_wear_data, min_support)
party_wear_result = gsp(party_wear_data, min_support)
 
# Output the frequent sequential patterns for each category
print("Frequent Sequential Patterns - Top Wear:")
if top_wear_result:
    for pattern, support in sorted(top_wear_result.items()):
        print(f"Pattern: {pattern}, Support: {support}")
else:
    print("No frequent sequential patterns found in Top Wear.")
 
print("\nFrequent Sequential Patterns - Bottom Wear:")
if bottom_wear_result:
    for pattern, support in sorted(bottom_wear_result.items()):
        print(f"Pattern: {pattern}, Support: {support}")
else:
    print("No frequent sequential patterns found in Bottom Wear.")
 
print("\nFrequent Sequential Patterns - Party Wear:")
if party_wear_result:
    for pattern, support in sorted(party_wear_result.items()):
        print(f"Pattern: {pattern}, Support: {support}")
else:
    print("No frequent sequential patterns found in Party Wear.")

```
### Output:

![alt text](screenshots/output.png)


### Visualization:
```python

# Function to visualize frequent sequential patterns using a line plot
def visualize_patterns_line(result, category):
    if not result:
        print(f"No frequent sequential patterns found in {category}.")
        return

    patterns = [str(pattern) for pattern in result.keys()]
    support = list(result.values())

    plt.figure(figsize=(10, 6))
    plt.plot(patterns, support, marker='o', linestyle='-', color='blue')

    # Display support values above each point
    for x, y in zip(patterns, support):
        plt.annotate(
            str(y),
            (x, y),
            textcoords="offset points",
            xytext=(0, 8),
            ha="center"
        )

    plt.xlabel("Patterns")
    plt.ylabel("Support Count")
    plt.title(f"Frequent Sequential Patterns - {category}")
    plt.xticks(rotation=90)
    plt.tight_layout()
    plt.show()


# Visualize frequent sequential patterns for each category
visualize_patterns_line(top_wear_result, "Top Wear")
visualize_patterns_line(bottom_wear_result, "Bottom Wear") #The Bottom Wear visualization is not displayed because its result is empty. This is because the Bottom Wear dataset does not meet the minimum support count.
visualize_patterns_line(party_wear_result, "Party Wear")
```
### Output:

#### Top Wear Visualization

![alt text](screenshots/top-wear-visualization.png)

#### Party wear visualization

![alt text](screenshots/partywear-visualization.png)

### Result:

Thus the Generalized Sequential Pattern (GSP) Algorithm was implemented successfully using Python, and the frequent sequential patterns were discovered and visualized.
