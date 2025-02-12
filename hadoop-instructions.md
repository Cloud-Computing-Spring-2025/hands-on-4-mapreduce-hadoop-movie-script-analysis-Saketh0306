# Movie Script Analysis with Hadoop MapReduce

## Project Overview
This project analyzes a movie script dataset using Hadoop MapReduce. The analysis includes:

1. Identifying the most frequently spoken words by each character.
2. Calculating the total dialogue length per character.
3. Extracting unique words spoken by each character.

## Approach and Implementation
The implementation consists of three MapReduce tasks:

### 1. **Most Frequently Spoken Words**
- **Mapper:** Extracts each word from a character’s dialogue and emits `(character, word)` pairs with a count of `1`.
- **Reducer:** Aggregates word counts per character and outputs `(character, word, count)`.

### 2. **Dialogue Length Calculation**
- **Mapper:** Splits the dialogue and counts the number of words spoken by each character.
- **Reducer:** Sums up the total word count per character and outputs `(character, total_words_spoken)`.

### 3. **Unique Words per Character**
- **Mapper:** Extracts unique words spoken by each character.
- **Reducer:** Aggregates and outputs the set of unique words spoken by each character.

### Hadoop Counters Used:
- `Total Lines Processed`
- `Total Words Processed`
- `Total Characters Processed`
- `Total Unique Words Identified`
- `Number of Characters Speaking`

## Execution Steps

### **1. Compile the Java Code**
```sh
hadoop com.sun.tools.javac.Main *.java
jar cf movie_script_analysis.jar *.class
```

### **2. Run the MapReduce Jobs**
```sh
hadoop jar movie_script_analysis.jar com.movie.script.analysis.MovieScriptAnalysis input/movie_script.txt output/
```

### **3. View the Results**
```sh
hdfs dfs -cat output/task1/part-r-00000  # Most Frequent Words
hdfs dfs -cat output/task2/part-r-00000  # Dialogue Length
hdfs dfs -cat output/task3/part-r-00000  # Unique Words
```

## Challenges Faced & Solutions

### **1. Handling Irregular Formatting in Scripts**
- **Issue:** Some scripts had missing character names or mixed formats.
- **Solution:** Used regex parsing and ensured only lines containing `:` were processed.

### **2. Memory Usage for Large Datasets**
- **Issue:** Processing large scripts with a high number of characters.
- **Solution:** Used Hadoop combiners to reduce data transfer between mappers and reducers.

### **3. Filtering Out Punctuation**
- **Issue:** Words included punctuation, affecting word count.
- **Solution:** Used regex `[^a-zA-Z]` to clean words before processing.

## Sample Input and Output

### **Input (movie_script.txt)**
```
JACK: I will find the treasure!
ROSE: Are you sure about that, Jack?
JACK: Yes, Rose! Absolutely.
```

### **Output (Most Frequent Words - task1)**
```
JACK	will	1
JACK	find	1
JACK	the	1
JACK	treasure	1
ROSE	are	1
ROSE	you	1
```

### **Output (Dialogue Length - task2)**
```
JACK	8
ROSE	4
```

### **Output (Unique Words - task3)**
```
JACK	will, find, the, treasure, yes, absolutely
ROSE	are, you, sure, about, that
```

