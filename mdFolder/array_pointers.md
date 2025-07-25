
# C Pointers and Arrays
```
This guide explains pointers, arrays, and how they interact in functions, returns, memory, and with function pointers. Includes simple code and common mistakes.
```
---

## 1. What is a Pointer?
```
//A pointer stores the memory address of another variable.

int x = 10;
int *p = &x;
printf("%d", *p);  // Output: 10
```

**Common Error:**
```c
int *p;
*p = 5;  // ❌ Undefined behavior – p is not initialized
```

---

## 2. Array vs Pointer (Non-Function Context)

### Array

```c
int arr[3] = {1, 2, 3};  // Stored in stack
```

### Pointer

```c
int *p = arr;           // Points to arr[0]
printf("%d", *(p + 1)); // Output: 2
```

---

## 3. Array vs Pointer in Functions

### Arrays decay to pointers when passed to a function

```c
void printArr(int arr[], int size) {
    for (int i = 0; i < size; i++) {
        printf("%d ", arr[i]);
    }
}
```

### Using pointer as parameter (same as above)

```c
void printArr(int *arr, int size) { ... }
```

---

## 4. Returning Arrays – Stack vs Heap

### ❌ Wrong: Return local array (stack memory is destroyed)

```c
int* badFunc() {
    int arr[5] = {1, 2, 3, 4, 5};
    return arr;  // ❌ DO NOT DO THIS
}
```

### ✅ Correct: Return pointer to heap-allocated memory

```c
int* goodFunc() {
    int *arr = malloc(5 * sizeof(int));
    arr[0] = 10;
    return arr;
}
```

**Common Error:**
```c
int* arr = malloc(5 * sizeof(int));
arr[5] = 100;  // ❌ Out-of-bounds access
```

---

## 5. Dynamic Memory Allocation

### Allocate memory using malloc

```c
int *data = malloc(5 * sizeof(int));
if (data == NULL) {
    // handle error
}
free(data);
```

**Common Error:**
- Not checking for `NULL` after `malloc`
- Not calling `free()`

---

## 6. Function Pointers

A function pointer stores the address of a function.

```c
int add(int a, int b) {
    return a + b;
}

int main() {
    int (*fptr)(int, int) = add;
    printf("%d", fptr(3, 4));  // Output: 7
}
```

**Common Errors:**
- Wrong signature:
  ```c
  int (*fptr)(void) = add;  // ❌ Doesn't match
  ```
- Using pointer without initialization:
  ```c
  int (*fptr)(int, int);
  fptr(2, 3);  // ❌ fptr is uninitialized
  ```

---

## 7. Array of Function Pointers

```c
int add(int x, int y) { return x + y; }
int sub(int x, int y) { return x - y; }

int (*ops[2])(int, int) = { add, sub };

int main() {
    printf("%d", ops[0](5, 2));  // 7
    printf("%d", ops[1](5, 2));  // 3
}
```

---

## Summary

- Use `*` to declare pointers and access values.
- Arrays decay to pointers in functions.
- Don't return local arrays.
- Use `malloc` for dynamic memory and `free` to clean up.
- Function pointers give flexibility to call functions dynamically.
