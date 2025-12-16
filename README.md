***Automated Article Classification System***

**University of Namur - Mini-Project 3** 

**Language:** Python 3

**Algorithm:** Naive Bayes (Implemented from scratch)

**Project Overview**:

*This project was made in the frame of Python Basics seen in the course of 'Introduction à la programmation' at UNamur University*

This tool automates the organization of textual documents
It uses **Machine Learning** and **Natural Language Processing (NLP)** techniques to "read" a set of unlabelled articles and sort them into thematic folders (Politics, Sports, Tech) based on their content using the basic tools of python

The system learns from a pre-classified dataset (Training) and applies that knowledge to classify new, random files (Testing) using a probabilistic Naive Bayes model

---

*Key Features* 

* **Text Preprocessing:** Automatically cleans data by removing English stop-words (high frequency words, commonly used in english), punctuation, and short words (< 3 characters)
* **Supervised Learning:** Builds a frequency model based on a provided directory of sorted examples
* **Log-Probability Scoring:** Uses logarithmic scoring to prevent arithmetic underflow when calculating probabilities for long documents
* **Automated Sorting:** Generates a new directory structure (`sorted_2`) and physically organizes the files into their predicted categories
* **Accuracy Validation:** Includes a verification module to compare predictions against a ground-truth `labels.txt` file

---

Project Structure and Prerequisites

For the script to work, your directory must be structured exactly as follows:

```text
/project_root
│
├── main.py              # The source code
├── labels.txt           # Ground truth file for checking accuracy (Format: filename theme)
│
├── /sorted              # TRAINING DATA
│   ├── /theme_1         # /Sports
│   │   ├── article1.txt
│   │   └── ...
│   └── /theme_2         # /Politics
│       ├── article2.txt
│       └── ...
│
└── /unsorted            # TESTING DATA (Files to be classified)
    ├── random_file_1.txt
    ├── random_file_2.txt
    └── ...

```

---

How It Works : 

The classifier implements the **Naive Bayes** assumption. It calculates the probability that a specific document belongs to a specific theme based on the words it contains

1. Training Phase : The system scans the `/sorted` folder and builds a "Bag of Words" dictionnary model. It counts the frequency of every word in every theme 

2. Prediction Phase : For a new document, the system calculates a score for every possible theme. To avoid multiplying many small probabilities (which causes errors in computers), the code uses **Logarithms**

The score for a specific **Theme** is calculated as:


$$Score_{theme} = \sum_{word \in document} \log(P(word | theme))$$


Where:

* P(word | theme) is the probability of seeing that word in that theme.
* If a word has never been seen before, a smoothing value is applied:
$\frac{1}{\text{Total Words in Theme}}$.

The document is assigned to the theme with the **highest maximum score**.

---

Usage1. **Configure the Path:**
Open the python script and scroll to the bottom. Change `./your_path` to the actual path of your project directory:
```python
smart_sort_files('./actual/path/to/data')
check_accuracy('./actual/path/to/data')

```


2. **Run the Script:**
```bash
python smart_files_gr04.py

```


3. **Check Results:**
* A new directory `sorted_2` will be created containing your organized files
* The console will print the **Accuracy Percentage** of the model (`85.5` for exemple)



---

*Code Modules* : 

* `unwanted_character(line)`: Filters noise from the text (punctuation, stopwords)
* `apprentissage(path)`: Scans the training data and returns a dictionary of word frequencies per theme
* `calculate_score(path, base)`: Applies the Naive Bayes formula to the unsorted files
* `smart_sort_files(path)`: The main driver function that manages file creation and copying
* `check_accuracy(path)`: Verifies the sorted output against `labels.txt`
