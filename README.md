# IRoadTrip

This is a program written in Java which finds the shortest path to get from one country to another. Dijkastra's algorithm was utilized in order to find this minimum path. The findMinPath() method in this program was inspired by the implementation of Dijkastra's algorithm in the book "Algorithms", by Sanjoy Dasgupta, Christos Papadimitriou, and Umesh Vazirani. 

# This program is split into 3 parts: 
- Processing the files
- Creating the adjacency list
- Finding the shortest path using Dijkastra's algorithm.

# Processing the Files

This program requires 3 files: borders.txt, state_name.tsv, and capdist.csv. 

Borders.txt is a text file which lists 253 countries and their bordering countries in a semicolon seperated list. It is in alphabetical order, beginning with Afghanistan and ending in Zimbabwe. Some of the countries on the right side of the '=' sign, or the bording countries, do not show up on the left of an '=' sign, such as French Guiana. French Guiana appears to be a bordering country to Suriname and Brazil. However, French Guiana is never referenced later on as its own country in the same file. 

State_name.tsv is a TSV file which includes the country names of 216 countries, their corresponding state number, and their state ID. (More information is included but this is the relevant information). It is in numerical order by state number. Some of the country names in this file are not the same as the country names in Borders.txt. 

Capdist.csv is a 41006 line CSV file. It includes 203 countries, denoted by their country number and country ID (but no country name). Each country included has a "block" which is 202 in size. Except for the block for state number 345, which includes information about Yugoslavia and Serbia, making it double the size of the other blocks. Each block also has the state ID and state number of the other 202 countries and their distance in km. The blocks are in numerical order by state number. The blocks themselves are not ordered. 

This program's length is attributed to the cleanup of these files. Another way to store the data could have been through a hashmap, which would hold the country name as the key, and the country number as the value. I chose to use the multidimensional array because I figured the TSV and CSV files look like what I imagine multidimensional arrays look like. 

# IRoadTrip Class / Methods
The constructor in IRoadTrip is responsible for reading in the three files and processing them. 

doFile0(): This method processes borders.txt. There is a giant String array called "newFile", where each line is a line from borders.txt which may have been cleaned up. I use Scanner to read in this file and if the line does not match the corresponding line in the newFile array, it is replaced. I put the countries in this file in an array called "countries".

doFile1(): This method processes state_name.tsv. It follows a similar technique to the previous function. However, stateNums[][] is filled here. The first column has the country name, and the second column has the country number. 

doFile2(): This method processes capdist.csv. Admittedly, we probably should not put the entire corrected 41006 line file in the code. So, a temporary file is created. Some of the blocks are moved around so that the state numbers are in more order. The writef() method does the actual writing onto the temporary file. 

execute(): This method is called in the IRoadTrip class constructor. It is responsible for cleaning up some of the arrays storing country information, as well as creating the Adjacency List Array. This is an array of Adjacency Lists, where the index of the array corresponds to the index of the country in borders.txt. The entries are its neighboring countries, also found in borders.txt.

sort0(): This method uses bubble sort to sort the state names in stateNums[][]. The compareTo() built in method is utilized to swap strings and put them in alphabetical order. 

sort1(): This method also uses bubble sort to sort each block in the distances[][] array. Recall, the blocks in capdist.csv are ordered. But, the blocks themselves are not. Thus, we key on numB, which is the stateID of the second country in the array. The variables a and b are introduced to account for state number 345, which is double in size. 

getDistance(): This is a required method. It only takes in 2 countries as strings and returns their distance in km, if exists. However, it can only be returned if the two countries share a land border. Here the adjacency list array is utilized. If country2 is not in country1's adjacency list, then they do not share a land border and -1 is returned. 

findPath(): This is a required method. It only takes in 2 countries as strings and returns a List of strings. The List will consistent of the shortest path from country1 to country2. A new instance of a "Trip" is created, and the path() method from that class is called. If the path does not exist, an empty List is returned. 

acceptUserInput(): This is a required method. It allows the user to interact with the shortest path algorithm by entering two countries in. The output will be the shortest path if it exists. If not, it will be empty or ask the user to enter a valid country. 

findDistance(): This method uses binary search to find the distance in km from country1 to country2. First, we must find the block where country1 is located. Then, we find the block where country2 is located. (Accounting for the flawed block) 

getname1(): This method returns the country name in file1, or state_name.tsv. It uses the substring method to continuously delete tabs so the actual name of the country is returned. 

index(): This method finds the index of a country in borders.txt, using binary search (if it exists). 

nextTo(): This method returns an array of all the countries adjacent to s, the country passed into the function. It uses subtring to find a countries neighbor in borders.txt. Recall, in borders.txt the neighbors of a country are seperated by semicolons. 

num(): This method returns the state number found in stateNums, using binary search. It returns the state number as an int. 

# Creating the Adjacency List
# gNode Class
A gNode is a single node, or a "Graph Node". Each node has a vertx (vrtx) which is the index the country is in borders.txt. The weight (wght) is the distance in km. It then has a pointer to the next node. 

# AdjList Class / Method
An AdjList is an adjacency list, which is a linked list of gNodes. The driving force of this program is the array of Adjacency List. Here is an example: 

In the 0th index of the array, and in the 0th spot in borders.txt, the country is Afghanistan. The adjacency list in index 0 would include:
- a gNode with China's index in borders.txt & its distance from Afghanistan in km (found in capdist.csv). Then, a pointer to: 
- a gNode with Iran's index in borders.txt & its distance from Afghanistan in km (found in capdist.csv). Then, a pointer to:
- a gNode with Pakistan's index in borders.txt & its distance from Afghanistan in km (found in capdist.csv). Then, a pointer to:
- ... and so on.

add(): This method adds gNodes to an adjacency list, in increasing order by vertex. 

# qNode Class
A qNode (queue node) will be used for Dijkastra's algorithm. A queue node is a node in a doubly linked list. 

# Queue Class / Methods
A queue in this program will be a doubly linked list. 

add(): This method adds qNodes to a doubly linked list, which will be the queue. In the case of this program, the qNode with the minimum distance will be in the front of the queue. 

remove(): This method removes a qNode from a queue. 

decrease(): Called by findMinPath(). This method updates the distance to every vertex adjacent to "u". Because all of the distances in findMinPath() are intialized to infinity, any valid neighbor will "decrease" in distance when their real distance is updated in the queue, as well as its position in the queue. 

deleteMin(): Called by findMinPath(). The node with the minimum distance is always stored at the front of the queue. So, this method removes the first node from the queue, but stores the vertex of the head node.

isNotDone(): Called by findMinPath(). This method checks to see if the algorithm is complete. If the distance in the head node is not the infinite value, the queue still holds more neighbors, and is not done. 

# Using Dijkastra's Algorithm
# Trip Class / Methods()
A new trip instance is created when a new trip begins.

noPath(): This method checks to see if there is a path between two vertices. It uses the explore() method to see if it is possible to reach a vertex from another. 

explore(): This is a recursive function that checks to see if a vertex can be reached by another vertex. 

path(): Calls the findMinPath() function, and returns the minimum path between two countries in an Array List of strings. If the vertex of country1 or country2 is invalid, an empty list is returned (as well as if there is no path). Otherwise, the path() method "unwinds" the array returned by findMinPaths (prev[]). Afterwards, the true minimum path from country1 -> country2 is found and it can be added to the Array List. 

findMinPaths(): This method implements Dijkstra's algorithm to find the shortest path between two vertices. An array is created, where dist[v] will represent the shortest path from vertex s to vertex v. Every dist[v] is intialized to infinity, so every vertex at this point is an infinte distance away from s. However, dist[s] is 0, because a vertex is always 0 _ away from itself. Thus, this will automatically make this node at the front of the queue. 
  An int u will be used to represent the vertex which was previously the min, and we add this node's weight to an int newDist. In the beginning, it will be 0. Next, we search through every vertex v which is adjacent to u and repeat this process. If we find a new distance which is shorter than dist[v], we update dist[v]. At the end, we return an array with the minimum path. 

Distance(): This method searches a block in the distance array (from capdist.csv) to find the distance between two countries. It uses a binary search to first find the block wehre country1 lives, and then to find the line in the block where country2 lives. Then, the third column, which has the distance in km, is returned (if exists). 
