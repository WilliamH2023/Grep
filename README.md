# Grep





Honor Certification:
My work is indeed my own, and not copied or adapted from any outside resource.


Outline:
For grep, I mainly use a hash table to store information of every word. Firstly,
I store file directories and every line of every file in 2 separate vectors.
In this way, every file directory and every line in every file, which can take 
a relatively large space since it is a long string, will only be stored once.
In other words, no lines or file directories will be stored in multiple location
which will take unnecessary space.

Then I build a hash table using the example_hash function for hashing and 
a Word class that stores 4 variables: the word itself as a string, the line 
number in which the word appears, the index of the vector where the line is 
stored, and the index of the vector where the file directory is stored. Since 
there are only 4 variables in 1 Word class object, and 3 of them are int, each
Word class object takes a relatively small space. But with the indices stored,
we can access the actual content of the file directory and the line in which
the word appears from the 2 vectors we set up. My hash table is a vector of 
vectors so that each slot in the vector is a bucket, and we can apply chaining
to deal with collision.

Algorithm:
My grep firstly reads every file under the given directory and store the file 
directories in a vector and mark the indices. Then while reading a file, for 
every line, it stores the lines in a vector and mark indices as well. Then while
reading a line, it creates a Word class object for every word with line numbers
and the 2 indices stored within. By using the example_hash function, we obtain
the index to store each Word class objects. Note that to minimize risk for the 
user to tamper info in Word class objects, only the "reading files" part of
the grep can edit the Word class objects. Afterwards, all Word class objects
in the hash table are set as const.

Then based on the user's input in the interface, the grep performs search,
case_insensitive_search, or quit. To search, we simply apply the example_hash
function to obtain the index of the hash table, search through the bucket and 
find all Word class objects that matches with the query. With every Word class 
object, with its info stored within, we can apply them to the 2 vectors that
store file directories and lines and cout all info needed by the spec.

To cater for case insensitive search, while reading the files, all case 
insensitive variants of a word that appear in any file will be hashed to the 
same index as the lower-cased word. In other words, each bucket in the hash 
table contains not only the word itself, but also its case insensitive variants.
Therefore, when performing case_insensitive_search, we can search through the 
one bucket of the lower-cased query, find all of its case insensitive variants,
and cout all info needed. Admittedly, this is not the most efficient data 
structure, as it tends to contribute to uneven distribution of elements in hash
table. However, this is a relatively easy implementation. A potentially more 
efficient design is to hash all case insensitive variants of a word to their 
own bucket instead of just one bucket, write a function that creates all
potential case insensitive variants, go to their individual buckets and find 
them all. This will create a more evenly distribution in the hash table.

Testing:
For testing, I mostly diff my outputs against demo's outputs. For helper 
functions, I created small tests as I wrote the functions. I began with tiny and 
debug from there. I spent most time testing the big functions: reading 
directory, reading files, search, and case_insensitive_search with tiny. Then I 
moved onto diff against small, medium, and big for optimizing running time and 
space. To further minimize space it takes, for all functions that requires my 
big vectors (line, file directory, word hash table) as inputs, I passed them 
by reference instead of passing by value, which won't create copies of them 
when running grep. As a result, my running time and space are a lot less than 
the demo and the requirement. For example, for medium, it takes about 18s and 
0.53GB; for large, it takes about 1min 5s and 2.2GB.


I approximately 30 hours on the assignment. 
