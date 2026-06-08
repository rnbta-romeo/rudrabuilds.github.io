#### Memory Address
Every variable is stored in the memory and each space has a unique address to it that is in the form of a hexadecimal number. The address can be seen using the `&` operator i.e. `cout << &variable << endl;`

#### Pointers 
Pointers are just special variables that stores the memory address of the variable in it. 
```cpp
int a = 10;
int* ptr_a = &a; //creates a pointer to a

float b;
float *ptr_b = &b; //creates a pointer to b
```

Note that the * operator can either be used with the data type or with the variable name.

Since pointer itself is a variable, it has an address too. We can make a pointer to a pointer to store the address of a pointer. A pointer to the pointer of `a` as defined above would look like:
```cpp
int** ptrToPtr_a = &pointer;
```

#### Dereferencing 
If the `&` operator gives us the address then the `*` operator gives us the value that is stored in that address, we call this the Dereference Operator. To deference a pointer we use `*` once, and to dereference a pointer to a pointer we use double `**`
```cpp
cout << *(ptr_a); //output is 10
cout << **(ptrToPtr_a); //output is 10
```

#### Null Pointer
It is a pointer that does not point to any location.
```cpp
int* ptr = NULL;
```

#### Pass by Reference
There are two ways to pass a variable inside function:
1) **Pass by Pointer:** We store the pointer as the input to a function and then in the main function call the function using the address as an input, this directly makes the changes in the main function without creating a copy of the variable. Lets create a function to change the variable `a` 
```cpp
    void changeA (int* ptr_a) {
        *ptr_a = 20;
    }
    
    int main () {
        int a = 10;
        changeA(&a);
    }
```

2) **Pass by Alias:** Here we use another variable as an input that is just the alias of the variable inside the main function, any changes to the alias changes the main variable because both of them are using the same location in the memory but just with a different name.
```cpp
    void changeA (int &b) {
        b = 20;
    }

    int main () {
        int a = 10;
        changeA(a);
    }
```

#### Array Pointers
When we create an array, it is already a pointer that stores the address of the zeroth index of the array in it. For example:
```cpp
int arr[] = {1,2,3,4,5};
cout << arr; //output is the address of the zeroth index arr[0]
cout << *arr; //output is 1 because we dereferenced the pointer
```

Note that array pointers are constant pointers, that is there location cannot be changed once we define an array.

#### Pointer Arithmetic
1) Increment(++) / Decrement(--)
   When we use the increment or decrement operator, it gets increased or decreased by 1 respectively. But the increment/decrement operator in a pointer shifts the memory address by the size of the data type. For example if its an integer pointer then it shifts by 4 bytes, if its a char pointer then it shifts by 8 bytes and so on.
2) Adding/Subtracting Integer
   Follows the same logic as increment/decrement. If we add 5 to a integer pointer then its address shifts by 20 bytes (since 1 integer = 4 bytes). 
3) Adding/Subtracting two Pointers
   We cannot add two pointers but we can subtract them. The result of subtraction gives by the number of blocks between the pointers. For example if there are two integer pointers say at 100 and 108, then the difference is 8 bytes and the output is 2 because we can store 2 integers between those.
4) Logical operators
   We can compare two pointers logically just like we do with variables using these logical operators: < > <= >= == !=