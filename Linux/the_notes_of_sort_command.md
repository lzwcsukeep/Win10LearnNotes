SORT command is used to sort a file, arranging the records in a particular order. By default, the sort command sorts file assuming the contents are ASCII.

description about sort command :

- SORT command sorts the contents of a text file, line by line.
- sort is a standard command-line program that prints the lines of its input or concatenation of all files listed in its argument list in sorted order.
- The sort command is a command-line utility for sorting lines of text files. It supports sorting alphabetically, in reverse order, by number, by month, and can also remove duplicates.
- The sort command can also sort by items not at the beginning of the line, ignore case sensitivity, and return whether a file is sorted or not. Sorting is done based on one or more sort keys extracted from each line of input.
- By default, the entire input is taken as the sort key. Blank space is the default field separator.

**The sort command follows these features as stated below:**

1. Lines starting with a number will appear before lines starting with a letter.
2. Lines starting with a letter that appears earlier in the alphabet will appear before lines starting with a letter that appears later in the alphabet.
3. Lines starting with a uppercase letter will appear before lines starting with the same letter in lowercase.

（sort 遵循的排序原则：数字在前，然后是大写字母，然后是字母表顺序）

what can sort command do :

1. To sort an oridinary text file line by line
   
   ```shell
   $ cat > file.txt
   abhishek
   chitransh
   satish
   rajan
   naveen
   divyam
   harsh
   ```
   
   output of sort :
   
   ```shell
   $ sort file.txt
   abhishek
   chitransh
   divyam
   harsh
   naveen 
   rajan
   satish
   ```

2. To sort a file **numerically** used –n option. -n option is also predefined in Unix as the above options are. 
   
   This option is used to sort the file with numeric data present inside.
   
   **Example :** 
   
   Let us consider a file with numbers:

```shell
$ cat > file1.txt
50
39
15
89
200
```

output of sort command with -n option:

```shell
$ sort -n file1.txt
Output :
15
39
50
89
200
```

3. sorting a table on the **basis of any column number by using -k option**

Use the -k option to sort on a certain column. For example, use “-k 2” to sort on the second column.

**Example :** 

Let us create a table with 2 columns:

```shell
$ cat > employee.txt
manager  5000
clerk    4000
employee  6000
peon     4500
director 9000
guard     3000
```

output of sort command with -k option:

```shell
$ sort -k 2n employee.txt        // the n follwing 2 is the type of key
guard    3000
clerk    4000
peon     4500
manager  5000
employee 6000
director 9000
```

4. **-c option:** This option is used to check if the **file given is already sorted or not** & checks if a file is already sorted pass the -c option to sort. This will write to standard output if there are lines that are out of order. The sort tool can be used to understand if this file is sorted and which lines are out of order

**Example :** 

Suppose a file exists with a list of cars called cars.txt.

```shell
Audi
Cadillac
BMW
Dodge
```

use -c option in sort command:

```shell
$ sort -c cars.txt
sort: cars.txt:3: disorder: BMW

 Note : If there is no output then the file is considered to 
 be already sorted 
```

5. **-u option:** To **sort and remove duplicates** pass the -u option to sort. This will write a sorted list to standard output and remove duplicates.   
   This option is helpful as the duplicates being removed give us a redundant file. 

**Example:** Suppose a file exists with a list of cars called cars.txt.

```shell
Audi
BMW
Cadillac
BMW
Dodge 
```

output:

```shell
$ sort -u cars.txt
Audi
BMW
Cadillac
Dodge
```

6. **-r Option: Sorting In Reverse Order**: You can perform a reverse-order sort using the -r flag. the -r flag is an option of the sort command which sorts the input file in reverse order i.e. descending order by default. 

**Example:** The input file is the same as mentioned above.

```shell
$ cat > file.txt
abhishek
chitransh
satish
rajan
naveen
divyam
harsh
```

output with -r option:

```shell
$ sort -r file.txt
satish
rajan
naveen 
harsh
divyam
chitransh
abhishek
```

7. **-o Option:** Using the built-in sort option -o allows you to specify an output file.

Using the -o option is functionally the same as redirecting the output to a file.

```shell
$ sort file.txt > output.txt 
$ sort -o output.txt file.txt  

Note: Neither one has an advantage over the other. 
```

**Application and uses of sort command:**

1. It can sort any type of file be it table file text file numeric file and so on.
2. Sorting can be directly implemented from one file to another without the present work being hampered.
3. Sorting of table files on the basis of columns has been made way simpler and easier.
4. So many options are available for sorting in all possible ways.
5. The most beneficial use is that a particular data file can be used many times as no change is made in the input file provided.
6. Original data is always safe and not hampered.
