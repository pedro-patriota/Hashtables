#include <iostream>
#include <string>

using namespace std;

// Cemetery struct
struct cemetery
{
  int security_num = 1000000;
  int is_del;
};

// Main struct
struct whole_cemetery{
  cemetery* my_cemetery;
  int* qnt_per_floor_whole;
  int size;
};


int // Hash Function
hashfunction (int social_num, int num_floor)
{
  int index = social_num % num_floor;
  return index;
}

int
fake_binary_search (int social_num, int index, int caves_per_floor,
		    cemetery arr[], int qnt_per_floor[])
{
  int qnt = qnt_per_floor[index -1]; // Receives the last occupied positiion
  int l = ((index - 1) * caves_per_floor);
  int r = l + (qnt -1);

  while (l <= r) // Binary search logic
    {
      int m = (int) ((l + r) / 2);
      if (social_num <= arr[m].security_num)
	{
	  r = m - 1;
	}
      else
	{
	  l = m + 1;
	}
    }
  return l; // return the position where the value should be
}

void add_function (int social_num, int caves_per_floor, cemetery arr[], int num_floor, bool should_print, int qnt_per_floor[])
{
    int index = hashfunction(social_num, num_floor); // index
    for (int i = index*caves_per_floor; i <= num_floor*caves_per_floor; i += caves_per_floor){ // Goes through each floor
        if (i == num_floor*caves_per_floor){ // Makes the "for" a cycle
          i = 0;
        }

        int my_floor = i/caves_per_floor;  
        if (qnt_per_floor[my_floor] == caves_per_floor){ // If the floor is full
          continue;
        }
        
        int pos = fake_binary_search(social_num, my_floor + 1, caves_per_floor, arr, qnt_per_floor); // Where the value should be
        
        for (int k = (i + qnt_per_floor[my_floor]); k > pos; k--) { // Modify the floor to receive the new value
            arr[k] = arr[k - 1];
        }

        arr[pos].security_num = social_num; // Inserts the new value
        arr[pos].is_del = 1; 
        qnt_per_floor[my_floor] = qnt_per_floor[my_floor] + 1;
        
    
        if (should_print){ 
          cout << my_floor << "." << pos - i << "\n";
        }
        return;



    }
}

bool find (int social_num, int num_floor, int caves_per_floor, cemetery arr[],int answer[], int qnt_per_floor[]){
 

  int temp_index = 0; 

  for (int i = 0; i < num_floor; i ++){ // Goes through each floor
      temp_index = hashfunction (social_num + i, num_floor); // Gets the index where the value should be
      temp_index++;
      int pos = fake_binary_search (social_num,temp_index, caves_per_floor, arr, qnt_per_floor); // Gets the pos where the value should be
      
      if (arr[pos].security_num == social_num) // Look for the value
        {
          answer[0] = temp_index;
          answer[1] = pos;
          return false;
        }      
  }
  return true;

}




whole_cemetery rehashing (int num_floor, int caves_per_floor, cemetery arr[], int qnt_per_floor[])
{   
    int size = 0;
    whole_cemetery my_entire_cemetery;  // Creates the new main struct
    int new_num_floor = num_floor * 2 + 1; // Increases the number of floor
    cemetery *auxiliary = new cemetery[new_num_floor * caves_per_floor]; // Creates the auxiliary structures
    int *qnt_per_floor_auxiliary = new int[new_num_floor];
    
  
    for (int i = 0; i < num_floor * caves_per_floor; i++)
      {
        if (arr[i].is_del != -1){ // Insert the past values into the new structures
            size++;
            int social_num = arr[i].security_num;
            add_function(social_num, caves_per_floor, auxiliary, new_num_floor, false, qnt_per_floor_auxiliary);
        }
        
      }

    
    my_entire_cemetery.my_cemetery = auxiliary; // Inserts the values of the main struct
    my_entire_cemetery.qnt_per_floor_whole = qnt_per_floor_auxiliary;
    my_entire_cemetery.size = size;
    
    return my_entire_cemetery; // Return the new hash

}




int
main ()
{
  string my_input, num_floor2, caves_per_floor2, command, social_num2;
  int empty_space;

  getline (cin, my_input); // Gets input
  empty_space = my_input.find (" ");

  num_floor2 = my_input.substr (0, empty_space);
  caves_per_floor2 = my_input.substr (empty_space + 1, my_input.length ()); // Separates each defined input

  int num_floor = stoi (num_floor2);
  int caves_per_floor = stoi (caves_per_floor2); // Transform them into int values
  
  cemetery *hash = new cemetery[num_floor * caves_per_floor]; // Creates the structures

  int *qnt_per_floor = new int[num_floor];

  whole_cemetery my_entire_cemetery;

  int capacity = 0; // Counter
  

  while (getline (cin, my_input) && my_input != "END")
    {

      empty_space = my_input.find (" ");
      command = my_input.substr (0, empty_space);
      social_num2 = my_input.substr (empty_space + 1, my_input.length ());  // Gets input
      int social_num = stoi (social_num2);

      if (command == "ADD")
      {
          
        if (capacity == caves_per_floor*num_floor) // If it is full;
          {     
                my_entire_cemetery = rehashing (num_floor, caves_per_floor, hash, qnt_per_floor);  // Rehash
                hash = my_entire_cemetery.my_cemetery;
                qnt_per_floor = my_entire_cemetery.qnt_per_floor_whole; // Gets the new values
                capacity = my_entire_cemetery.size;
                num_floor = num_floor * 2 + 1;

          }
        
      add_function (social_num, caves_per_floor, hash, num_floor, true, qnt_per_floor); // Add the social number
      capacity ++; // Increases capacity
      

      }
      else if (command == "QRY")
	{
	  int answer[2];
	  bool a = false;
	  a = find (social_num, num_floor, caves_per_floor, hash, answer, qnt_per_floor); // Finds the value
	  if (a) // The number is not found
	    {
	      cout << "?.?\n";
	    }
	  else
	    {
	      if (hash[answer[1]].is_del == -1) // Number has been deleted
		{
		  cout << "?.?\n";
		}
	      else
		{
		    
		  cout << answer[0] -1 << "." << (answer[1]) % caves_per_floor << "\n"; // Finds the number

		}

	    }
	}
      else if (command == "REM")
	{
	  int answer[2];
	  bool a = false;
	  a = find(social_num, num_floor, caves_per_floor, hash, answer, qnt_per_floor); // Fins the value
	  
	  if (a) // Does not exist;
	    {
	      cout << "?.?\n";
	    }
    else
      {
        hash[answer[1]].is_del = -1;  // Delete it 
        cout << answer[0] -1 << "." << (answer[1]) % caves_per_floor << "\n"; // Output
      }
	    
	}



    }

  delete[](qnt_per_floor); // Deleting the pointers 
  delete[](hash);
  return 0;
}
