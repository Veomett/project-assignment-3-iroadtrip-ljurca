# IRoadTrip Class

This is a program written in Java which finds the shortest path to get from one country to another. Dijkastra's algorithm was utilized in order to find this minimum path. The findMinPath() method in this program was inspired by the implementation of Dijkastra's algorithm in the book "Algorithms", by Sanjoy Dasgupta, Christos Papadimitriou, and Umesh Vazirani. 

This program requires 3 files: borders.txt, state_name.tsv, and capdist.csv. 
Border.txt is a text file which lists 253 countries and their bordering countries in a semicolon seperated list. It is in alphabetical order, beginning with Afghanistan and ending in Zimbabwe. 
State_name.tsv is a TSV file which includes the country names of 216 countries, their corresponding state number, and their state ID. (More information is included but this is the relevant information). It is in numerical order by state number. 
Capdist.csv is a 41006 line CSV file. It includes 203 countries, denoted by their country number and country ID (but no country name). Each country included has a "block" which is 202 in size. Except for the block for state number 345, which includes information about Yugoslavia and Serbia, making it double the size of the other blocks. Each block also has the state ID and state number of the other 202 countries and their distance in km. The blocks are in numerical order by state number. The blocks themselves are not ordered. 

This program's length is attributed to the cleanup of these files. 
