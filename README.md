# CIS-22B-LAB-3.10
//
//  main.cpp
//  Lab 3.10
//
//  Created by Tedla Boke on 5/1/22.
//

/*~*~*~*~*~*~
 Array of structures: Binary Search, Insertion Sort
 Name: Tedla Boke
 IDE: Online C++ Compiler
*~*/

#include <iostream>
#include <fstream>
#include <sstream>
#include <string>

using namespace std;

/* Write the definition of the Movie structure with the following fields:
- year, an integer
- awards, an integer
- nominations, an integer
- title, a string
*/
struct Movie {
   int year;
   int awards;
   int nominations;
   string title;
   };

// function prototypes
/* Write your code here
*/
void readMovies(string filename, Movie list[], int limit, int &size);
void printMovies(Movie list[], int size);
void searchTestDriver(Movie list[], int size);
void insertSort(Movie list[], int size);
int binarySearch(Movie list[], int l, int r, string name);

int main(){
    // constants definitions
    const int MOVIES = 20; // maximum size of array
    
    // array definition
    Movie list[MOVIES];
    
    // other variables
    int noMovies;           // actual size of array
    string filename;
    
    // function calls
    cout << "Popular Classic Movies" << endl;
    cout << "==> Enter input file name: ";
    getline(cin, filename);
    readMovies(filename, list, MOVIES, noMovies);
    cout << "==> Movies Sorted by Title:" << endl;
    printMovies(list, noMovies);
    cout << "==> Search Movies:" << endl;
    searchTestDriver(list, noMovies);
    insertSort(list, noMovies);
    cout << "==> Movies Sorted by Year:" << endl;
    printMovies(list, noMovies);
    return 0;
}

/* Write your code here. For each function definition write a short comment too,
as demonstrated in previous assignments */

/*~*~*~*~*~*~
This function reads data
*~*/
void readMovies(string filename, Movie list[], int limit, int &size)
{
    ifstream inputFile;
    
    // open file
    inputFile.open(filename.c_str());

    // validation
    if(inputFile.fail()){
        cout << "Error opening " << filename << " for reading." << endl;
        exit(EXIT_FAILURE);
    }
     
    // read data from file into an array of structures
    string line;
    size = 0;
    while (size < limit && getline(inputFile, line)){
       stringstream temp(line);
       temp >> list[size].year;
       temp >> list[size].awards;
       temp >> list[size].nominations;
       temp.ignore(); // ignore the space in front of the name
       getline(temp, list[size].title);
       size++;
    }

    // check if size reaches maximum size of array and there is more data in the file
    string test;
    if(size == limit && getline(inputFile, test)){
        cout << filename << " is too large." << endl;
        exit(EXIT_FAILURE);
    }

    // close file
    inputFile.close();
}

/*~*~*~*~*~*~
 This function performs the binary search on a Movie array, sorted by title.
 The array has size elements. A value stored in this array will be searched. It will
 return the array subscript if found. Otherwise, -1 will be returned.
*~*/
int binarySearch(Movie list[], int l, int r, string name){
   while(l <= r){
      int m = l + (r - l)/2;
      if(list[m].title.compare(name) == 0){
         return m;
         }
         if(list[m].title.compare(name) < 0){
            l = m + 1;
         }
         else{
            r = m - 1;
         }
      }
      return -1;
   }

/*~*~*~*~*~*~
 This function prints the movies year and title
*~*/
/* Define the printMovies  function here */
void printMovies(Movie list[], int size){
   for(int i = 0;i < size; i++){
      cout << list[i].year << " "<< list[i].title << endl;
      }
   }
/*~*~*~*~*~*~
 This function calls binary search in a loop
*~*/
void searchTestDriver(Movie list[], int size){
   string target;
   string again = " ";
   int x = -1;
   // prompt users to enter movie title
   do {
      cout << "What movie are you looking for? ";
      getline(cin, target);
      /* Call binary search then display results */
      x = binarySearch(list,0,size - 1,target);
      if(x == -1){
         cout << target << " not found!" << endl;
         }
      else {
         cout << "Found: " << list[x].title << ", " << list[x].year << ", " << list[x].awards << ", " << list[x].nominations << endl;
      }
      // prompt user for another input
      cout << "Would you like to look up another movie(Y/N)? ";
      getline(cin, again);
   } while(again == "Y" || again == "y");
}

/*~*~*~*~*~*~
 This function sort the array of Movie structures in descending order by year
*~*/
/* Define the insertSort function here */
void insertSort(Movie list[], int size){
   int i,key,j;
   for(i = 1;i < size; i++){
      key = list[i].year;
      Movie k;
      k.year = list[i].year;
      k.awards = list[i].awards;
      k.nominations = list[i].nominations;
      k.title = list[i].title;
      j = i - 1;
      while( j >= 0 && list[j].year < key){
         list[j+1].year = list[j].year;
         list[j+1].awards = list[j].awards;
         list[j+1].nominations = list[j].nominations;
         list[j+1].title = list[j].title;
         j = j-1;
         }
         list[j+1].year = k.year;
         list[j+1].awards = k.awards;
         list[j+1].nominations = k.nominations;
         list[j+1].title = k.title;
      }
   }
