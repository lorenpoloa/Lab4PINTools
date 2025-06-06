![Logo](https://github.com/lorenpoloa/KangarooPINTools/blob/main/Logo.png)


# KangarooPINTool
Some PIN Tool for Computer Architecture Labs.


This is a tool developed using the [Intel® Pin](https://www.intel.com/content/www/us/en/developer/articles/tool/pin-a-dynamic-binary-instrumentation-tool.html) framework.
## Requirements

- Intel PIN (not included, download from the official website)
- Compatible compiler (gcc/g++, clang, etc.)


## What is Kangaroo PIN Tools
Is a set of PIN Tools to get stats about jumps instructions in a program.

### JumpsStatsSimple
Simple stats about the number of conditional jumps instructions and the ratio of taken jumps.

### JumpsStatsByType
More detailed stats about each type of jumps instructions and the ratio of them that are taken.

### JumpsStatsWithPredictor
Stats about differents jumps predictors simulations

## How to use

### Compiling and Using <br>

Copy all files from this repository to [your pin path]/source/tools/JumpsTools

Copy makefile from any tools directori (ex. /tools/MyPinTool/makefile) to /tools/JumpsTools/makefile

Compile:
```bash

make
```

### Exec with PIN:
```bash

pin -t JumpsStatsSimple.so -- ./your_program

```


## JumpsStatsByType 
## 📘 Manual and Description

### 📝 General Description
JumpsStatsByType is an instrumentation tool developed with Intel® Pin Tool. Its goal is to collect detailed statistics of branch instructions executed by a program, classified by jump type (e.g., je, jne, jmp, call, etc.).

The tool analyzes whether each branch was taken or not taken and provides a summary both by instruction type and overall.

### 🔍 What does it measure?
For each type of branch instruction (such as je, jmp, jne, etc.), the tool reports:

Number of times the branch was taken.

Number of times the branch was not taken.

Total number of times that branch instruction was executed.

Percentage of times the branch was taken.

Additionally, it provides a total summary of all branches encountered during program execution.


### ▶️ Usage
To use the tool, run your target program with Pin and this tool as follows:

```bash

pin -t obj-intel64/JumpsStatsByType.so -o results.txt -- ./my_program
Parameters

-t obj-intel64/JumpsStatsByType.so: path to the compiled tool file.

-o results.txt: (optional) name of the output file. If omitted, the output will be shown in the console.

-- ./my_program: the program you want to instrument and analyze.
```

### 📄 Example Output

```text
======= Branch Statistics =======
Type      Taken     Not-Taken      Total     Percent Taken
JL        16618     11137          27755     59.87
JLE       777522    86634          864156    89.97
JMP       6697168   0              6697168   100.00
JNB       1         0              1         100.00
JNBE      0         1              1         0.00
JNL       1495623   96834          1592457   93.92
JNLE      2997847   2700035        5697882   52.61
JNZ       29622463  599877         30222340  98.02
JP        0         2              2         0.00
JZ        138542    6895682        7034224   1.97
=======================================
Total Taken     = 40964646
Total Not Taken = 10252691
Total Branches  = 51217337

```

### ⚠️ Considerations

The tool only counts branch-type instructions and classifies them by their mnemonic (e.g., jne, jmp).

It does not distinguish between direct and indirect branches, but you can extend it to do so.



## JumpsStatsWithPredictor 
## 📘 Manual and Description


### 📝 General Description
JumpsStatsWithPredictor is a PIN Tool that instruments conditional branch instructions in a running binary. Its goal is to evaluate the accuracy of several commonly used branch prediction algorithms:

Always Taken

Always Not Taken

1-bit Predictor

2-bit Predictor

The tool collects runtime statistics during the program execution and generates a summary report at the end.

### ⚙️ Components and Operation

Branch Prediction Logic
File: Jump_Predictor_Sim.cpp

Implemented Predictors:
Always Taken: Always predicts that the branch will be taken.

Always Not Taken: Always predicts the branch will not be taken.

1-bit Predictor: Stores the last branch result (true/false) for each instruction address and uses it for the next prediction.

2-bit Predictor: Uses a 2-bit saturating counter (values 0-3) per address:

0–1 → predict not taken

2–3 → predict taken

The counter is incremented/decremented based on the current result.


### ▶️ Usage

Execution
```bash

pin -t obj-intel64/JumpsStatsWithPredictor.so -o results.txt -- ./your_program

```

-t: specifies the tool

-o: output file for the results

--: separates PIN arguments from the program to run

### 📄 Sample Output
```text

===== Branch Predictor Results =====
Always Taken
    Correct: 140
    Total: 300
    Accuracy: 46.67%
Always Not Taken
    Correct: 160
    Total: 300
    Accuracy: 53.33%
1-bit
    Correct: 200
    Total: 300
    Accuracy: 66.67%
2-bit
    Correct: 250
    Total: 300
    Accuracy: 83.33%
```

### 🧾 File Structure

```bash

.
├── JumpsStatsWithPredictor.cpp   ← Main PIN tool
├── Jump_Predictor_Sim.cpp        ← Predictor implementations
└── Jump_Predictor_Sim.h          ← Predictor data structures and API
```

### ⚠️ Considerations

The tool only counts predictions statistics from the simulated jump predictor, not about real jump predictor from the CPU that execute the program.

It does not distinguish between direct and indirect branches, but you can extend it to do so.



## License

This project is licensed under the CC0 License.
**Note**: Intel PIN has its own proprietary license. Please refer to Intel's terms before using PIN.
