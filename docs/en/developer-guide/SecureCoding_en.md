# openEuler Secure Coding Guide

This document provides some secure coding suggestions based on the C language to guide development practices.



# Data Types

## Ensuring That Signed Integer Operations Do Not Overflow

**Description**
Signed integer overflow is an undefined behavior. For security purposes, ensure that no overflow occurs when signed integers in external data are used in the following scenarios:

-   Integer operand of the pointer operation (pointer offset value)

-   Array index

-   Length of the variable-length array (plus with the length operation expression)

-   Length of the memory copy

-   Parameters of the memory allocation function

-   Loop judgment condition

When performing an operation on an integer type of which the precision is lower than **int**, take integer promotion into consideration. Programmers also need to master integer conversion rules (including implicit conversion rules) to design safe arithmetic operations.

(1) Addition

**Incorrect example** (addition):

In the following example, the integer involved in the addition operation is from external data and is not verified before being used which may result in integer overflow.

```
int num_a =... // From external data
int num_b =... // From external data
int sum = num_a + num_b;
...
```

**Correct example** (addition):

```
int num_a =... // From external data
int num_b =... // From external data
int sum = 0;
if (((num_a > 0) && (num_b > (INT_MAX - num_a))) ||
	((num_a < 0) && (num_b < (INT_MIN - num_a)))) {
 	... // Error handling
}
sum = num_a + num_b;
...
```

(2) Subtraction

**Incorrect example** (subtraction):

In the following example, the integer involved in the subtraction operation is from external data and is not verified before being used. As a result, integer overflow may occur, which can cause memory copy. Further, buffer overflow occurs.

```
unsigned char *content =... // Pointer pointing to the packet header.
size_t content_size =... // Total length of the buffer.
int total_len =... // Total length of the packet.
int skip_len =... // Length of the data to be ignored parsed from the message.
// When calculating the remaining data length by using totalLen - skipLen, integer overflow may occur.
(void)memmove(content, content + skip_len, total_len - skip_len);
...
```

**Correct example** (subtraction):

The following code uses a variable of the **size_t** type to indicate the data length and checks whether the external data length is within the valid range.

```
unsigned char *content = ... // Pointer pointing to the packet header.
size_t content_size =... // Total length of the buffer.
size_t total_len =... // Total length of the packet.
size_t skip_len = ... // Length of the data to be ignored parsed from the message.
if (skip_len >= total_len || total_len > content_size) {
	... // Error handling
}

(void)memmove(content, content + skip_len, total_len - skip_len);
...
```



(3) Multiplication

**Incorrect example** (multiplication):

In the following example, the kernel code verifies the value range from the user mode. However, **ULONG_MAX** is incorrectly used in the verification while opt is of the **int** type. As a result, integer overflow occurs.

```
int opt =... // From the user mode
if ((opt < 0) || (opt > (ULONG_MAX / (60 * HZ)))) { // ULONG_MAX is incorrectly used in the upper limit 
	return -EINVAL;
}

... = opt * 60 * HZ; // Integer overflow may occur.
...
```

**Correct example** (multiplication):

A solution is to change the opt type to the **unsigned long** type. This solution is applicable to the scenario where the variable type is modified to better comply with the service logic.

```
unsigned long opt =... // Reconstruct the type to unsigned long.
if (opt > (ULONG_MAX / (60 * HZ))) {
	return -EINVAL;
}
... = opt * 60 * HZ;
...
```

Another solution is to change the upper limit of the value to **INT_MAX**.

```
int opt =... // From the user mode
if ((opt < 0) || (opt > (INT_MAX / (60 * HZ)))) { // INT_MAX is used as the upper limit.
	return -EINVAL;
}
... = opt * 60 * HZ;
```

(4) Division

**Incorrect example** (subtraction):

In the following example, only whether the divisor is 0 is checked before a division operation is performed. The value range is not verified, which may cause integer overflow.

```
int num_a =  ... // From external data
int num_b =  ... // From external data
int result = 0;

if (num_b == 0) {
	... // Handle the error that the divisor is 0.
}

result = num_a / num_b; // Integer overflow may occur.
...
```

**Correct example** (division):

In the following example, the integer is verified based on the maximum allowed value to prevent integer overflow. In addition, stricter value range verification can be performed based on specific scenarios during programming.

```
int num_a =... // From external data

int num_b =... // From external data

int result = 0;

// Check whether the divisor is 0 and whether a division overflow error occurs.

if ((num_b == 0) || ((num_a == INT_MIN) && (num_b == -1))) {

 ... // Error handling

}

result = num_a / num_b;
...
```

(5) Remainder

**Incorrect example** (remainder):

```
int num_a =... // From external data
int num_b =... // From external data
int result = 0;
if (num_b == 0) {
	... // Handle the error that the divisor is 0.
}

result = num_a % num_b; // Integer overflow may occur.
...
}
```

**Correct example** (remainder):

In the following example, the integer is verified based on the maximum allowed value to prevent integer overflow. In addition, stricter value range verification can be performed based on specific scenarios during programming.

```
int num_a =  ... // From external data
int num_b =  ... // From external data
int result = 0;

// Check whether the divisor is 0 and whether a division overflow error occurs.
if ((num_b == 0)  || ((num_a == INT_MIN) && (num_b == -1))) {
	... // Error handling
}

result = num_a % num_b;
...
}
```

(6) Unary minus

When the operand is equal to the minimum value of the signed integer type, overflow occurs during the unary minus of binary complementary code.

**Incorrect example** (unary minus):

In the following example, the value range is not verified before operation, which may cause integer overflow.

```
int num_a =... // From external data
int result = -num_a; // Integer overflow may occur.
...
```

**Correct example** (unary minus):

In the following example, the integer is verified based on the maximum allowed value to prevent integer overflow. In addition, stricter value range verification can be performed based on specific scenarios during programming.

```
int num_a =  ... // From external data
int result = 0;

if (num_a == LNT_MIN) {
	... // Error handling
}

result = -num_a;
...
```

## Ensuring That Unsigned Integers Do Not Wraparound

**[Description]**

Calculations involving unsigned operands never overflow, because a calculation result that goes beyond the range of unsigned integers will be the actual result modulo (the maximum value of the result type + 1).

This is often informally called unsigned integer wraparound.

When performing an operation on an integer type of which the precision is lower than **int**, take integral promotion into consideration. Programmers also need to master integer conversion rules (including implicit conversion rules) to design safe arithmetic operations.

For security purposes, ensure that no wraparound occurs when unsigned integers in external data are used in the following scenarios:

-   Integer operand of the pointer operation (pointer offset value)

-   Array index

-   Length of the variable-length array (plus with the length operation expression)

-   Length of the memory copy

-   Parameters of the memory allocation function

-   Loop judgment condition

(1) Addition

**Incorrect example** (addition):

The following code checks whether the sum of the length of the next sub-packet and the length of the processed packet exceeds the maximum length of the whole packet. The addition operation in the verification may cause integer wrap, which results in the verification bypass.

```
size_t total_len =... // Total length of the packet.
size_t read_len = 0 // Record the length of the processed packet.
...
size_t pkt_len = parse_pkt_len(); // Length of the next sub-packet parsed from the network packet.
if (read_len + pkt_len > total_len) { // Integer wraparound may occur.
	... // Error handling
}

...
read_len += pkt_len;
...
```

**Correct example** (addition):

The **read_len** variable records the length of the processed packet, which is definitely less than **total_len**. Therefore, the addition operation in the code is modified to the subtraction operation to avoid condition bypass.

```
size_t total_len = ... // Total length of the packet.
size_t read_len = 0; // Record the length of the processed packet.
...

size_t pkt_len = parse_pkt_len(); // From the network packet.
if (pkt_len > total_len - read_len) {
	... // Error handling
}

...
read_len += pkt_len;
...
```

(2) Subtraction

**Incorrect example** (subtraction):

In the following example, integer wrap may occur in the operation of verifying the valid range of len. As a result, the condition is bypassed.

```
size_t len =... // From user-mode input
if (SCTP_SIZE_MAX - len < sizeof(SctpAuthBytes)) { // Integer wraparound may occur when performing a subtraction.
	... // Error handling
}

... = kmalloc(sizeof(SctpAuthBytes) + len, gfp); // Integer wraparound may occur.
...
```



**Correct example** (subtraction):

In the following example, the position of the subtraction operation is adjusted (ensure that the value of the subtraction expression is not reversed during compilation) to avoid integer wrap.

```
size_t len =... // From user-mode input
if (len > SCTP_SIZE_MAX - sizeof(SctpAuthBytes)) { // Ensure that the value of a subtraction expression is not reversed during compilation.
	... // Error handling
}

... = kmalloc(sizeof(SctpAuthBytes) + len, gfp);
...
```



(3) Multiplication

**Incorrect example** (multiplication):

In the following example, no verification is performed when external data is used to calculate the length of the memory to be applied for. As a result, integer wraparound may occur.

```
size_t width =... // From external data
size_t hight =... // From external data
unsigned char  *buf = (unsigned char  *)malloc(width  * hight);
```

Unsigned integer wraparound may result in cause insufficient allocated memory.

**Correct example** (multiplication):

The following code is a solution to verify the integer value range involved in the multiplication operation to ensure that no integer wraparound occurs.

```
size_t width =... // From external data
size_t hight =... // From external data
if (width == 0 || hight == 0) {
	... // Error handling
}

if (width  > SIZE_MAX / hight) {
	... // Error handling
}

unsigned char  *buf = (unsigned char  *)malloc(width  * hight);
```

**Exceptions:**
When necessary, wraparound of unsigned integers can be allowed for proper program execution. It is recommended that the variable declaration be explicitly commented as supporting modulus behavior, and each operation on the integer be explicitly commented as supporting modulus behavior.

**[Software CWE ID]** CWE-190

## Ensuring That Division and Remainder Operations Do Not Cause Divide-by-Zero Errors

**[Description]**

If the second operation values of integer division and remainder operations are 0, the program will generate undefined behaviors. Therefore, ensure that the integer division and remainder operations do not cause divide-by-zero errors.

(1) Division

**Incorrect example** (division):

If the division operation of the signed integer type is not properly restricted, overflow occurs.

The following example prevents overflow for division operations of signed integers, but cannot prevent the divide-by-zero error when the signed operands **num_a** is divided by **num_b**.

```
int num_a =  ... // From external data
int num_b =  ... // From external data
int result = 0;

if ((num_a == INT_MIN) && (num_b == -1)) {
	... // Error handling
}

result = num_a / num_b; // A divide-by-zero error may occur.
...
```

**Correct example** (division):

In the following example, the verification of whether the value of **num_b** is 0 is added to prevent the divide-by-zero error.

```
int num_a =  ... // From external data
int num_b =  ... // From external data
int result = 0;

if ((num_b == 0)  | | ((num_a == INT_MIN) && (num_b == -1))) {
	... // Error handling
}

result = num_a / num_b;
...
```

(2) Remainder

**Incorrect example** (remainder):

Similar to the incorrect division example, divide-by-zero errors may occur in the following code, because many platforms use the same instruction to implement remainder and division operations.

```
int num_a =  ... // From external data
int num_b =  ... // From external data
int result = 0;

if ((num_a == INT_MIN) && (num_b == -1)) {
	... // Error handling
}

result = num_a % num_b; // A divide-by-zero error may occur.
...
```

**Correct example** (remainder):

In the following example, the verification of whether the value of **num_b** is 0 is added to prevent the divide-by-zero error.

```
int num_a =  ... // From external data
int num_b =  ... // From external data
int result = 0;

if ((num_b == 0)  | | ((num_a == INT_MIN) && (num_b == -1))) {
	... // Error handling
}

result = num_a % num_b;
...
```

# Variables

## Do Not Use Uninitialized Variables

**[Description]**
The variables here refer to local dynamic variables, including allocated memory blocks on the heap.
It is prohibited to read their values without valid initialization because their initial values are unpredictable.

```
void foo( ...)
{
	int data;
	bar(data); // Incorrect: It is used without initialization.
	...
}
```

If there are different branches, ensure that the variable is initialized in each branch.

```
#define CUSTOMIZED_SIZE 100
void foo( ...)
{
	int data;
	if (condition > 0) {
		data = CUSTOMIZED_SIZE;
	}

	bar(data); // Incorrect: The value is not initialized in some branches.
	...
}
```

## Assigning a New Value to the Variable Pointing to a Resource Handle or Descriptor Immediately After the Resource Is Freed

**[Description]**
Variables pointing to resource handles or descriptors include pointers, file descriptors, socket descriptors, and other variables pointing to resources.
Take a pointer as an example. After the pointer successfully applies for a memory segment and the memory segment is released, if the pointer is not set to NULL immediately and no new object is allocated, the pointer is a dangling pointer.
If the dangling pointer is operated again, the memory may be repeatedly released or the released memory may be accessed, causing security vulnerabilities.
An effective way to mitigate this vulnerability is to immediately set a new value, for example, NULL, to the freed pointer. For a global resource handle or descriptor, set a new value immediately after the resource is released to prevent it from using the released invalid value. For a resource handle or descriptor that is used only in a single function, ensure that the invalid value is not used again after the resource is released.

**Incorrect example**:
In the following example, the message is processed based on the message type. After the processing is complete, the memory to which **body** or **head** points is released. However, the pointer is not set to NULL after the memory is released. If other functions process the message structure again, the memory may be released repeatedly or the released memory may be accessed.

```
int foo(void)
{
	SomeStruct *msg = NULL;
	... // Initialize msg->type and allocate memory space for msg->body.
	if (msg->type == MESSAGE_A) {
		...
		free(msg->body);
	}

	...
EXIT:
	...
	free(msg->body);
	return ret;
}
```

**Correct example**:
In the following example, the released pointer is set to NULL immediately to avoid repeated pointer release.

```
int foo(void)
{
	SomeStruct  *msg = NULL;
	... // Initialize msg->type and allocate memory space for msg->body.

	if (msg->type == MESSAGE_A) {
		...
		free(msg->body);
		msg->body = NULL;
	}

	...
EXIT:
	...
	free(msg->body);
	return ret;
}
```

When the input parameter of the free() function is NULL, the function does not perform any operation.

**Incorrect example**:
In the following example, no new value is assigned to the file descriptor after it is closed.

```
SOCKET s = INVALID_SOCKET;
int fd = -1;
...
closesocket(s);
...
close(fd);
...
```

**Correct example**:
In the following example, after the resource is released, a new value is assigned to the corresponding variable immediately.

```
SOCKET s = INVALID_SOCKET;
int fd = -1;
...
closesocket(s);
s = INVALID_SOCKET;
...
close(fd);
fd = -1;
...
```

# Pointers and Arrays

## Ensuring That the Index Is Within the Array Bound When External Data Is Used as an Array Index

**[Description]**
When external data is used as an array index to access the memory, the data size must be strictly verified to ensure that the array index is within the valid range. Otherwise, a serious error occurs.
When a pointer points to an array element, it may point to a location following the last element of the array, but cannot access the memory at that location.

**Incorrect example**:
In the following example, an off-by-one error exists in the **set_dev_id()** function. When the index is equal to **DEV_NUM**, an element is written out of bounds.
The **get_dev()** function also has an off-by-one error. Although the function is executed properly, the behavior is undefined when the pointer returned by the function is dereferenced.

```
#define DEV_NUM 10
#define MAX_NAME_LEN 128
typedef struct {
	int id;
	char name[MAX_NAME_LEN];
} Dev;

static Dev devs[DEV_NUM];
int set_dev_id(size_t index, int id)
{
	if (index > DEV_NUM) {// Error: Off-by-one error.
 		... // Error handling
	}

	devs[index].id = id;
	return 0;
}

static Dev *get_dev(size_t index)
{
	if (index > DEV_NUM) {// Error: Off-by-one error.
 		... // Error handling
	}

	return devs + index;
}
```



**Correct example**:
In the following example, the index verification condition is modified to prevent the off-by-one error.

```
#define DEV_NUM 10
#define MAX_NAME_LEN 128
typedef struct {
	int id;
	char name[MAX_NAME_LEN];
} Dev;

static Dev devs[DEV_NUM];

int set_dev_Id (size_t index, int id)
{
	if (index >= DEV_NUM) {
		... // Error handling
	}

	devs[index].id = id;
	return 0;
}

static Dev *get_dev(size_t index)
{
	if (index >= DEV_NUM) {
 		... // Error handling
	}

	return devs + index;
}
```

**[Software CWE IDs]**: CWE-119, CWE-123, CWE-125

## Do Not Perform the **sizeof** Operation on a Pointer Variable to Obtain the Array Size

**[Description]**
When the pointer is used as an array to perform the **sizeof** operation, the actual result is not as expected. For example, in the variable definition **char *p = array** where **array** is defined as **char array[LEN]**, the result of the expression **sizeof(p)** is the same as that of **sizeof(char *)**, but not the length of the array.

**Incorrect example**:
In the following example, **buffer** and **path** indicate the pointer and array, respectively. The programmer wants to clear the two memories. However, the programmer mistakenly set the memory size as **sizeof(buffer)**, which is not as expected.

```
char path[MAX_PATH];
char *buffer = (char *)malloc(SIZE);
...
(void)memset(path, 0, sizeof(path));
// sizeof is not as expected. The result is the size of the pointer rather than the size of the buffer.
(void)memset(buffer, 0, sizeof(buffer));
```

**Correct example**:
In the following example, change **sizeof(buffer)** to the size of the requested buffer.

```
char path[MAX_PATH];
char *buffer = (char *)malloc(SIZE);
...
(void)memset(path, 0, sizeof(path));
(void)memset(buffer, 0, SIZE); // Use the size of the requested buffer.
```

# Character Strings

## Ensuring That the Memory Buffers for Strings Have Sufficient Space to Store Character Data and NULL Terminators

**[Description]**
Copying data to a buffer that is insufficient to hold the data causes a buffer overflow. Buffer overflows often occur in string operations. To avoid this error, truncating the copied data to limit the byte length of the string is a countermeasure, but the best practice is to ensure that the target buffer is large enough to hold the copied data and NULL terminators. When a string is stored in heap space, ensure that sufficient memory space is allocated.

Some string processing functions may be used inappropriately due to insufficient security design or some hidden target buffer length requirements. As a result, buffer overflow occurs. Typical functions of this type include the **itoa()** and **realpath()** functions that are not in the C standard library.

**Incorrect example** :**itoa()**
Some functions, such as **itoa()** and **realpath()**, need to write data to the location of the input buffer pointer, but the functions do not specify the buffer length. Therefore, sufficient buffers must be provided before these functions are called.
In the following example, a number is converted to a character string, but the reserved length of the target storage space is insufficient.

```
int num = ...
char str[8];
itoa(num, str, 10); // The maximum storage length for a decimal integer is 12 bytes.
```

**Correct example**:
In the following example, the size of **name** is considered when external data is parsed and saved to **name**:

```
int num = ...
char str[13];
itoa(num, str, 10); // The maximum storage length for a decimal integer is 12 bytes.
```

**Incorrect example**: **realpath()**

In the following example, the function attempts to standardize the path, but the length of the target storage space is insufficient:

```
#define MAX_PATH_LEN 100
char resolved_path[MAX_PATH_LEN];
/ *
- The storage buffer length of the **realpath** function is defined by the **PATH_MAX** constant,
- or configured by the **_PC_PATH_MAX** system variable. Generally, the value is greater than 100 bytes.
*/

char  *res = realpath(path, resolved_path);
...
```

**Correct example**:

You can set the second parameter of **realpath** to NULL so that the system can automatically allocate proper memory.

```
char *resolved_path = NULL;
resolved_path = realpath(path, NULL);
if (resolved_path == NULL) {
	... // Error handling
}

...
if (resolved_path != NULL) {
	free(resolved_path);
	resolved_path = NULL;
}
...
```

## Ensuring That Strings Have Null Terminators When Storing Strings

**[Description]**
When processing strings, some functions truncate the strings that exceed the specific length. For example, the **strncpy()** function copies a maximum of *n* characters to the target buffer. If the length of the source string is greater than n, the NULL terminator is not written to the target buffer. The target buffer contains only the copied *n* characters. When such functions are used, data may be lost due to unintentional truncation, and software vulnerabilities may occur in some cases.
Therefore, when storing a string, ensure that the string has a NULL terminator. Otherwise, out-of-bounds memory access may occur in subsequent operations such as **strlen()**.

**Incorrect example**:
In the following example, truncation may occur when the **strncpy** function is used to copy a string and strlen(name) > sizeof(file_name) - 1. When truncation occurs, the content of **file_name** is incomplete and lacks the '\0' terminator. Subsequent operations on **file_name** may cause software vulnerabilities.

```
#define FILE_NAME_LEN 128
char file_name [FILE_NAME_LEN ];
(void)strncpy(file_name, name, sizeof(file_name) - 1);
...
```

**Correct example**:

```
#define FILE_NAME_LEN 128
char file_name[FILE_NAME_LEN ];

if (strlen(name)  > FILE_NAME_LEN - 1) {
	... // Error handling.
}

(void)strcpy(file_name, name);
...
```

**Exceptions**:

The programmer wants to truncate the string.

**[Software CWE IDs]** CWE-170, CWE-464

# Assertions

## Do Not Use Assertions to Detect Errors That May Occur During Program Running

**[Description]**
Assertion is mainly used during debugging. It is disabled when the release version is compiled. Therefore, assertions should be used to prevent incorrect assumptions and should not be used to check for errors that occur during program running in release versions.

Assertions must not be used to detect runtime (as opposed to logic) errors, such as:

-   Invalid user input (including command line parameters and environment variables)

-   File errors (for example, an error occurs when opening, reading, or writing a file)

-   Network errors (including network protocol errors)

-   Insufficient memory (for example, an error caused by **malloc()**)

-   Exhaustion of system resources (for example, file descriptors, processes, and threads)

-   System call errors (for example, an error occurs when a file is executed or a mutex is locked or unlocked)

-   Invalid permissions (for example, file, memory, or user permissions)

For example, code that prevents buffer overflow cannot be implemented using assertions because the code must be compiled into a release executable.
If a malicious user triggers an assertion failure when the server program is running online, a denial of service (DoS) attack may occur. In this case, the soft fault mode, such as writing to a log file or rejecting requests, are more appropriate.

**Incorrect example**:
The usage of ASSERT in the following example is incorrect. For example, the ASSERT macro is incorrectly used to verify whether the memory allocation is successful because the memory availability depends on the overall state of the system and may be exhausted at any time when the program runs. The memory allocation must be properly handled in a resilient manner and the program must be recovered from the memory exhaustion. Therefore, it is inappropriate to use the ASSERT macro to verify whether the memory allocation is successful, because doing so may cause the process to terminate suddenly, increasing the possibility of denial of service attacks.

```
FILE  *fp = fopen(path,  "r");
ASSERT(fp != NULL); // Incorrect usage: The file may fail to be opened.
char  *str = (char *)malloc(MAX_LINE);
ASSERT(fp != NULL); // Incorrect usage: Memory allocation may fail.
ReadLine(fp, str);
char  *p = strstr(str, "age=");
ASSERT(p != NULL); //Incorrect usage: The character string may not exist in the file.
char  *end = NULL;
long age = strtol(p + 4, &end, 10);
ASSERT(age > 0); //Incorrect usage: The file content may not be as expected.
```

**Correct example**:
The following code shows how to reconstruct the preceding incorrect code:

```
FILE  *fp = fopen(path,  "r");
if (fp == NULL) {
	... // Error handling
}

char  *str = (char *)malloc(MAX_LINE);
if (str == NULL) {
	... // Error handling
}

read_line(fp, str);
char  *p = strstr(str,  "age=");
if (p == NULL) {
	... // Error handling
}

char *end = NULL;

long age = strtol(p + 4, &end, 10);
if (age <= 0) {
	... // Error handling
}
```

## Do Not Change the Running Environment in Assertions

**[Description]**
Assertions are not compiled when the program is released. To ensure the function consistency between the debugging version and the release version, do not perform operations such as value assignment, variable modification, resource operation, and memory application in assertions.

For example, the following assertions are incorrect:

```
ASSERT(p1 = p2); // p1 is modified.
ASSERT(i++  > 1000); // i is modified.
ASSERT(close(fd) == 0); // fd is closed.
```

# Function Design

## When an Array Is Used as a Function Parameter, Its Length Must Also Be Used as a Function Parameter

**[Description]**
When an array is transferred to a function as a parameter, the length of the array, rather than the size of array in bytes, must also be transferred to the function. Similarly, when a memory block is transferred to a function for read and write operations, the size of memory block must also be transferred. Otherwise, the function cannot determine the valid offset range when accessing the memory offset, causing an out-of-bounds error. The "array" mentioned in this rule is not limited to an array type variable, but also includes a string or a pointer pointing to a consecutive memory block.

**Incorrect example**:
In the following example, the **pars_msg** function cannot obtain the rangeof **msg**, which may cause an out-of-bounds memory access vulnerability.

```
int parse_msg(unsigned char *msg)
{
	...
}

void foo(void)
{
	size_t len = get_msg_len();
	...
	unsigned char *msg = (unsigned char  *)malloc(len);
	...
	parse_msg(msg);
	...
}
```

**Correct example**:
The correct solution is to transfer the size of **msg** to the **parse_msg** function as a parameter. The code is as follows:

```
int parse_msg(unsigned char *msg, size_t msg_len)
{
	ASSERT(msg != NULL);
	ASSERT(msg_len != 0);
	...
}

void foo(void)
{
	size_t len = get_msg_len();
 	...
	unsigned char *msg = (unsigned char *)malloc(len);

 	...
	parse_msg(msg, len);
 	...
}
```

## Pointer in a Function That Does Not Point to an Object to Be Modified Should Be Declared as a Pointer Pointing a Constant

**[Description]**
Declaring a pointer parameter as a constant will prevent the function from modifying the pointed object using the pointer. This improves the security and reliability of the function.

Example: In the **strncmp** function, the pointer parameters that point to unchanged objects are declared as constants.

```
// Correct: Pointer parameters pointing to unchanged objects are declared as constants.
int strncmp(const char *s1, const char *s2, size_t n);
```

Note:

Whether to declare a pointer parameter as a constant depends on the function design. It does not matter whether the object is modified in the function body.

## Format Parameters of Formatted Input/Output Functions Must Not Be Controlled by External Data

**[Description]**
If the format parameter of a formatted input/output function is provided by or concatenated from external data, a string formatting vulnerability occurs.

If an attacker can completely or partially control the format string content, the attacker can crash the attacked process, view the stack content, view the memory content, or write data to any memory location. As a result, the attacker can execute arbitrary code with the permissions of the attacked process.

Formatted output functions are particularly dangerous because many programmers do not realize that they can be exploited. For example, a formatted output function can use the %n conversion character to write an integer value to a specified address.

Formatted input/output functions include:
Formatted output function: xxxprintf;
Formatted input function: xxxscanf;
Formatted error message functions: err(), verr(), errx(), verrx(), warn(), vwarn(), warnx(), vwarnx(), error(), error_at_line();
Formatted log functions: syslog(), vsyslog().

**Incorrect example**:
In the following example, the **incorrect_password()** function is used to display an error message when the authentication fails (the specified user is not found or the password is incorrect).
This function accepts a string **user** from the user. The string is not verified and is externally controllable.
The function produces an error message that contains **user** and prints the message to standard error output using the C standard library function **fprintf**.

```
// The invoker must ensure that the length of the input parameter user is limited to 256 bytes or less.
void incorrect_password(const char *user)
{
	int ret = -1;
	static const char msg_format[] = "%s cannot be authenticated.\n";
	size_t len = strlen(user) + 1 + sizeof(msg_format);

	char *msg = (char *)malloc(len);
	if (msg == NULL) {
 		... // Error handling
	}

	ret = snprintf(msg, msg_format, user);
	if (ret == -1) {
 		... // Error handling
	} else {
		fprintf(stderr, msg); // msg contains unverified external data, causing a formatting vulnerability.
	}

	free(msg);
}
```

In the sample code, the message length is calculated, the memory is allocated, and then the **snprintf()** function is used to concatenate the message content. Therefore, the message content contains the content of **msg_format** and the content of **user**.
If the input parameter **user** contains formatting characters (such as %s, %p, and %n) entered by a user, **fprintf()** parses **msg** as a format string instead of directly outputting the message content.
In this case, the content in **msg** is not directly printed to standard error output. Instead, some unknown data is printed, causing undefined behavior of the program. This is a very serious formatting vulnerability.

**Correct example**:
A recommended method is as follows. In the code, **fputs()** is used instead of **fprintf()**. **fputs()** directly outputs the msg content to standard error output without parsing it.

```
// The length of the input parameter user is limited to 256 bytes or less.
void incorrect_password(const char *user)
{
	int ret = -1;
	static const char msg_format[] = "%s cannot be authenticated.\n";

	// The addition operation does not cause integer overflow because the length of user is limited.
	size_t len = strlen(user) + 1 + sizeof(msg_format);
	char *msg = (char *)malloc(len);
	if (msg == NULL) {
 		... // Error handling
	}

	ret = snprintf(msg, msg_format, user);
	if (ret == -1) {
 		... // Error handling
	} else {
		fputs(stderr, msg); // fprintf is replaced with fputs.
	}

	free(msg);
}
```

**Correct example**:
Another recommended method is as follows. In the code, the untrusted user input user is used as an optional parameter of fprintf(). **user** is specified as a string by **%s** instead of a part of a format string and is output to standard error output. This eliminates the possibility of format string vulnerabilities.

```
void incorrect_password(const char  *user)
{
	static const char msg_format[] = "%s cannot be authenticated.\n";
	fprintf(stderr, msg_format, user);
}
```

**Incorrect example**:
In the following example, the POSIX function **syslog()** is used, which may cause a format string vulnerability.

```
void foo(void)
{
	char  *msg = get_msg();
	...

	syslog(LOG_INFO, msg); // A formatting vulnerability exists.
}
```

**Correct example**:
A recommended method is as follows. In the code, the untrusted user input **msg** is used as an optional parameter of **syslog**()**. **msg** is specified as a string by **%s** instead of a part of a format string and is output to system logs. This eliminates the possibility of format string vulnerabilities.

```
void foo(void)
{
	static const char msg_format[] =  "%s cannot be authenticated.\n";
	char  *msg = get_msg();
	...
	syslog(LOG_INFO, msg_format, msg); // Formatting vulnerability does not exist here.
}
```



# Function Usage

## Using Valid Format Strings When Calling Formatted Input/Output Functions

**Description**

Formatted input/output functions (such as **fscanf()**, **fprintf()**, and other related functions) convert, format, and print arguments under the control of format strings.

Common mistakes during format string creation are as follows:

-   The number of parameters in the format string is not the same as the number of arguments.

-   Invalid conversion indicators.

-   Flag characters that are incompatible with the conversion indicators.

-   Length modifiers that are incompatible with the conversion indicators.

-   The conversion indicator in the format string does not match the argument type.

-   Using an argument of a type other than **int** to specify the width or precision.

Do not provide unknown or invalid conversion specifications for formatted input/output functions, or invalid combinations of flag characters, precision, length modifiers, and conversion indicators. Also, do not provide arguments that do not match the conversion indicator types in format strings. This may lead to undefined behavior.

**Incorrect example**:

In the following example, the type of the **infoLevel** argument of **printf()** does not match the corresponding conversion indicator **%s**. The correct conversion indicator is **%d**. Similarly, the type of the **infoMsg** argument does not match the corresponding conversion indicator **%d**. The correct conversion indicator is **%s**.
These usages cause undefined behavior. For example, **printf()** interprets the **infoLevel** argument as a pointer and attempts to read a string from the address contained in **infoLevel**, resulting in unauthorized access.

```
void foo(void)
{
	const char *info_msg = "Information seed to user.";
	int info_level = 3;
	...
	printf("infoLevel: %s, infoMsg: %d\n", info_level, info_msg);
	...
}
```

**Correct example**:

The correct approach is to ensure that the arguments of the **printf()** function match the conversion indicators of the format string.

```
void foo(void)
{
	const char *info_msg = "Information seed to user.";
	int info_level = 3;
	...

	printf("infoLevel: %d, infoMsg: %s\n", info_level, info_msg);
	...
}
```

**Impact**

An incorrect format string may cause memory damage or abnormal program termination.

## Do Not Use the alloca() Function to Apply for Stack Memory

**[Description]**
the behavior of **alloca()** is not defined in POSIX or C99. Some platforms do not support this function. Using **alloca()** will reduce the compatibility and portability of the program. This function applies for memory in stack frames, and the size of the applied memory may exceed the stack boundary, affecting subsequent code execution.

Use **malloc()** to dynamically allocate heap memory.

**[Impact]**

The size of the program stack is limited. If allocation causes stack overflow, the program generates undefined lines.

## Do Not Use the realloc() Function

**[Description]**
The **realloc()** function is a special function. Its prototype is as follows:

void *realloc(void *ptr, size_t size);

The behavior varies according to the parameter.

-   When **ptr** is not NULL and **size** is not 0, this function adjusts the memory size and returns a new memory pointer, ensuring that the content of the minimum **size** remains unchanged.

-   If **ptr** is NULL but **size** is not 0, the behavior of **ptr** is the same as that of **malloc(size)**.

-   If **size** is 0, the behavior is the same as that of **free(ptr)**.

This simple C function is programmed with three behaviors. It is not a well-designed function. Although it provides some convenience during programming, various bugs may occur due to improper usage.

**Incorrect example**:
In the following example, memory leakage occurs due to improper use of **realloc()**.
The code attempts to expand the space of **ptr**. When **realloc()** fails to allocate space, NULL is returned. However, the memory of **ptr** is not released. If the return value of **realloc()** is directly assigned to **ptr**, the memory that **ptr** originally points to is lost, causing memory leakage.

```
// When realloc() fails to allocate memory, NULL is returned, causing memory leakage.
char *ptr = (char  *)realloc(ptr, NEW_SIZE);
if (ptr == NULL) {
	... // Error handling
}
```

**Correct example**:
**malloc()** is used instead of **realloc()**.

```
// malloc() is used instead of realloc().
char *new_ptr = (char *)malloc(NEW_SIZE);
if (new_ptr == NULL) {
	... // Error handling
}

(void)memcpy(new_ptr, old_ptr, old_size);
... // Release old_Ptr before returning.
```

**Impact**

Inappropriate use of functions may cause memory leakage and double free errors. Incorrect memory alignment may cause abnormal object access.

## Do Not Use External Controllable Data as Parameters of Process Startup Functions

**[Description]**
In this section, process startup functions include **system**, **popen**, **execl**, **execlp**, **execle**, **execv** and **execvp**.

Functions such as **system()** and **popen()** create new processes. If external controllable data is used as parameters of these functions, injection vulnerabilities may occur.

When functions such as **execl()** and **execlp()** start new processes, command injection risks also exist if shell is used to start new processes.

Therefore, the C standard functions are always preferred to implement required functions. If process startup functions are necessary, use the whitelist mechanism to ensure that the parameters of these functions are not affected by any external data.

**Incorrect example**:
In the following example, the **system()** function is used to execute the **cmd** command string from external data, allowing attackers to execute any command.

```
char  *cmd = get_cmd_from_remote();
if (cmd == NULL) {
	... // Error handling.
}

system(cmd);
```

In the following example, a part of the **cmd** command string executed by the **system()** function is from external data. An attacker may enter the **'some dir;useradd xxx'** string to create a user named **xxx**.

```
char cmd[MAX_LEN ];
int ret;

char  *name = get_dir_name_from_remote();
if (name == NULL) {
	... // Error handling.
}

ret = sprintf(cmd, "ls %s", name);
...
system(cmd);
```

Avoid command injection when using **exec** functions. Note that the **path** and **file** parameters cannot be command interpreters (such as **/bin/sh**).

```
int execl(const char *path, const char *arg,  ...);
int execlp(const char *file, const char *arg,  ...);
int execle(const char *path, const char *arg,  ..., char * const envp[]);
int execv(const char *path, char *const argv[]);
int execvp(const char *file, char *const argv[]);
```

For example, do not use the following method:

```
char *cmd = get_dir_name_from_remote();
execl("/bin/sh", "sh",  "-c", cmd, NULL);
```

**Correct example** (using library functions):

To print the file names in the current directory in Linux, you can use functions such as **opendir()**, **readdir()**, and **stat()** to implement the **ls-l** command function without using the **system()** function. The following is a simplified example version of **ls -l**, listing information about a file specified in the program. This function does not consider the reentrant scenario.

```
static int OutputFileInfo(const char *file_name)
{
	const char priv[] = {'x', 'w', 'r'};
	ASSERT(file_name != NULL);

	struct stat st;
	int ret = stat(file_name, &st);
	if (ret == -1) {
		return -1;
	}

	const struct passwd *pw = getpwuid(st.st_uid);
	if (pw == NULL) {
		return -1;
	}

	const struct group *gp = getgrgid(st.st_gid);
	if (gp == NULL) {
		return -1;
	}

	if (S_ISREG(st.st_mode)) {
		printf("-");
	} else if (S_ISDIR(st.st_mode)) {
		printf("d");
	}

	for (int i = 8; i >= 0; i --) {
		if ((st.st_mode & (1 < < i)) != 0) {
			printf("%c", priv[i % 3]);
		} else {
			printf("-");
		}
	}

	printf("%s %s %ld %s\n",
	pw->pw_name,
	gp->gr_name,
	st.st_size,
	file_name);
	return 0;
}
```

**Correct example** (using **exec** functions):

For functions that can be simply implemented by using library functions (such as the preceding example), do not use command interpreters to execute external commands. If a command needs to be invoked, whitelist it and transfer it to an **exec** function as a parameter.

```
pid_t pid;
char * const envp[] = { NULL };
...
char *file_name = get_dir_name_from_remote();
if (file_name == NULL) {
	... // Error handling.
}
...
if ((pid = fork()) < 0) {
	...
} else if (pid == 0) {
	// Using some_tool to process a specified file.
	execle( "/bin/some_tool", "some_tool", file_name, NULL, envp);
	_Exit(-1);
}

...
int status;
waitpid(pid, &status, 0);
FILE *fp = fopen(file_name, "r");
...
```

In this case, the externally input **file_name** is used only as a parameter of the **some_tool** command. There is no command injection risk.

**Correct example** (using a whitelist)

A proper whitelist is used to check the entered file name, preventing command injection.

```
char *cmd = get_cmd_from_remote();
if (cmd == NULL) {
	... // Error handling.
}

// Using a whitelist to check whether the command is valid. Only a "some_tool_a" or "some_tool_b" command is allowed, which is not externally controlled.
if (!is_valid_cmd(cmd)) {
	... // Error handling.
}

system(cmd);
...
```

**[Software CWE IDs]** CWE-676, CWE-88

## Do Not Call Non-Async-Signal-Safe Functions Within Signal Handlers

**[Description]**
Call only async-signal-safe functions within signal handlers.

In addition to some C standard library functions, some system functions are also async-signal-safe. Before using these functions in a signal handler, ensure that the called functions are async-signal-safe in all possible execution environments.

**Incorrect example**:
In the following example, the signal handler invokes **printf()**, which is not async-signal-safe:

```
void handler(int num)
{
	printf("receive signal = %d \n", SIGINT);
}

int main(int argc, char **argv)
{
	if (signal(SIGINT, handler) == SIG_ERR) {
 		... // Error handling
	}

	while (true) {
 		... // Main loop code of the program
	}

	return 0;
}
```

**Correct example**:
In the following example, the signal handler does not invoke other functions. Only variables of the **volatile sig_atomic_t** type are changed by the signal handler.

```
static volatile sig_atomic_t g_flag = 0;
void handler(int num)
{
	g_flag = 1;
}

int main(int argc, char **argv)
{
	if (signal(SIGINT, handler) == SIG_ERR) {
	... // Error handling
	}

	while (true) {
		if (g_flag != 0) {
			printf("receive signal = %d\n", SIGINT);
		}

		... // Main loop code of the program
	}

	...
	return 0;
}
```

**[Software CWE ID]** CWE-479

# Memory

## Check Whether Memory Allocation Is Successful

**[Description]**
If memory allocation fails, undefined behavior may exist in subsequent operations. For example, a null pointer is returned when **malloc** fails to apply for memory. Dereference of a null pointer is an undefined behavior.

**Incorrect example**:
In the following example, after **malloc** is invoked to allocate memory, the pointer is referenced without checking whether the memory allocation is successful. If **malloc** fails, it returns a null pointer and passes it to the pointer. When the following code dereferences the null pointer during memory copy, undefined behavior occurs.

```
struct tm *make_tm(int year, int mon, int day, int hour, int min, int sec)
{
	struct tm *tmb = (struct tm*)malloc(sizeof(*tmb));
	tmb->year = year;
	...

	return tmb;
}
```

**Correct example**:
In the following example, after memory is allocated by the **malloc** function, the program immediately checks whether the memory is successfully allocated, eliminating the preceding risk.

```
struct tm  *make_tm(int year, int mon, int day, int hour, int min, int sec)
{
	struct tm  *tmb = (struct tm *)malloc(sizeof(*tmb));
	if (tmb == NULL) {
		... // Error handling
	}

	tmb->year = year;
	...
	return tmb;
}
```

## The External Input Must Be Verified When Used as the Length of Memory to Be Copied

**[Description]**
Copying data to memory that is not large enough to hold it can cause buffer overflow. To prevent such errors, you must either limit the size of the data to be copied based on the capacity of target memory, or ensure that the target memory is large enough to hold the data to be copied.

**Incorrect example**:
External input data may not be directly used as the memory copy length, but may indirectly participate in the memory copy operations.
In the following example, **input_table->count** is from an external packet. Although it is not directly used as the memory copy length, it is used in the test expression of the for loop and indirectly participates in the memory copy operation. The memory copy length is not verified, which may cause buffer overflow.

```
typedef struct {
	size_t count;
	int val[MAX_num_bERS];
} ValueTable;

ValueTable *value_table_dup(const ValueTable *input_table)
{
	ValueTable *output_table = ... // Memory allocation
	...
	for (size_t i = 0; i  < input_table->count; i++) {
		output_table->val[i] = input_table->val[i];
	}
 	...
}
```

**Correct example**:
In the following example, **input_table->count** is verified.

```
typedef struct {
size_t count;
int val[MAX_num_bERS];
}ValueTable;

ValueTable *value_table_dup(const ValueTable *input_table)
{
	ValueTable *output_table = ... // Memory allocation
	...

	/ *
	- Based on the application scenario, the loop length input_table->count, which is from an external packet,
	- is compared with the length of the output_table->val array to avoid buffer overflow.
	*/
	if (input_table->count  > sizeof(output_table->val) / sizeof(output_table->val[0]){
		return NULL;
	}

	for (size_t i = 0; i  < input_table->count; i++) {
		output_table->val[i] = input_table->val[i];
	}

	...
}
```

## Zeroing Out Sensitive Information in the Memory Immediately After Use

**[Description]**
Sensitive information (such as passwords and keys) in the memory should be zerod out immediately after being used to prevent information interception or from being leaked to low-privileged users. The memory includes but is not limited to the following:

-   Dynamically allocated memory

-   Statically allocated memory

-   Automatically allocated (stack and heap) memory

-   Memory cache

-   Disk cache

**Incorrect example**:
Generally, memory data is cleared before the memory is released to reduce running overhead. Therefore, after the memory is released, the data is still stored in the memory. Sensitive data in the memory may be leaked unexpectedly. To prevent sensitive information leakage, you must zero out the sensitive information in the memory before releasing the memory.
In the following example, the sensitive information **secret** stored in the referenced dynamic memory is copied to the newly dynamically allocated buffer **newSecret** and is released using **free()**. Because the memory data is not cleared before the memory is released, the memory may be reallocated to another part of the program. As a result, sensitive information stored in **newSecret** may be leaked accidentally.

```
char *secret = NULL;

/ *
- Assume that secret points to sensitive information containing a byte string whose length is less than SIZE_MAX characters
- and ends with a null byte.
*/
size_t size = strlen(secret);
char *new_secret = NULL;
new_secret = (char *)malloc(size + 1);
if (new_secret == NULL) {
	... // Error handling
} else {
	strcpy(new_secret, secret);
	... // Processing new_secret ...
	free(new_secret);
	new_secret = NULL;
}

...
```

**Correct example**:
In the following example, to prevent information leakage, the dynamic memory that contains sensitive information is zeroed out (filled with '\0' characters) before release.

```
char *secret = NULL;
/ *
- Assume that secret points to sensitive information containing a byte string whose length is less than SIZE_MAX characters
- and ends with a null byte.
*/
size_t size = strlen(secret);
char *new_secret = NULL;
new_secret = (char *)malloc(size + 1);
if (new_secret == NULL) {
	... // Error handling
} else {
	strcpy(new_secret, secret);
	... // Processing new_secret ...
	(void)memset(new_secret, 0, size + 1);
	free(new_secret);
	new_secret = NULL;
}
...
```

**Correct example**:
The following example is another scenario involving sensitive information clearance. After the program obtains the password, the password is saved to **password** for verification. After **password** is used, the **memset** function is used to zero out the password.

```
int foo(void)
{
	char password [MAX_PWD_LEN ] = {0};
	if (!get_password(password, sizeof(password))) {
		... // Error handling.
	}

	if (!verify_password(password)) {
		   // Rectify the fault.
	}

	...
	(void)memset(password, 0, sizeof(password));
	...
}
```



# Files

## Creating Files with Appropriate Access Permissions Explicitly Specified

**[Description]**
If proper access permissions are not explicitly specified when a file is created, unauthorized users may access the file. As a result, information leakage, file data tampering, and malicious code injection may occur.

Although file access permissions also depend on the file system, many file creation functions (such as the POSIX **open** function) can set (or affect) file access permissions. Therefore, when using these functions to create files, you must explicitly specify proper file access permissions to prevent unwanted access.

**Incorrect example**:
The POSIX **open()** function is used to create a file but the access permission of the file is not specified. The file may have excessive access permissions, which may cause vulnerabilities.

```
void foo(void)
{
	int fd = -1;
	char *file_name = NULL;
	... // Initializing file_name.
	fd = open(file_name, O_CREAT | O_WRONLY); // The access permission is not explicitly specified.
	if (fd == -1) {
		... // Error handling
	}
	...
}
```

**Correct example**:
The access permission for the newly created file is explicitly specified in the third parameter of **open**. You can set access permissions based on the actual application of the file.

```
void foo(void)
{
	int fd = -1;
	char *file_name = NULL;
	... // Initializing file_name and specifying its access permission.
	// Explicitly specify the access permission for the file based on the actual requirements.
	int fd = open(file_name, O_CREAT | O_WRONLY, S_IRUSR | S_IWUSR);
	if (fd == -1) {
		... // Error handling
	}
	...
}
```

## Using Normalized File Paths

**[Description]**
When the file path is from external data, the validity of the file path must be verified. Otherwise, system files may be accessed arbitrarily. However, do not verify a file path directly. You need to normalize the file path first, because:
A file can be described and referenced by multiple paths, for example, an absolute path or a relative path. In addition, the path name, directory name, and file name may contain characters that make verification difficult and inaccurate, for example, "**.**" and "**..**". In addition, the file path can be a symbolic link, which further obscures the actual location or identifier of the file, and increases difficulty and inaccuracy of verification. Therefore, you must normalize the file path using methods like the **realpath** function.

A simple case can be as follows:

If a file path comes from external data, the file path must be normalized. An attacker can use a file path that is not standardized to access files without authorization.

For example, an attacker can use a file path like **../../../etc/passwd** to access any file.

**Incorrect example**:
In this incorrect example, **argv[1]** contains a file name from a contaminated source and the file is open for writing. The file name should have been verified before it is used to ensure that it references a valid file.
Unfortunately, the file name referenced by **argv[1]** may contain special characters, such as directory characters, which make validation difficult, if not impossible. In addition, **argv[1]** may contain a symbolic link that points to any file path. Even if the file name passes the verification, the file name is invalid.
In this scenario, the file name cannot be thoroughly verified. Invoking fopen() may access an unexpected file.

```
...
if (!verify_file(input_file_name) { // Verifying **input_file_name** without normalization.
	... // Error handling
}

if (fopen(input_file_name, "w") == NULL) {
	... // Error handling
}
...
```

**Correct example**:
Normalizing file names can be difficult because it requires knowledge about the underlying file system.
The POSIX **realpath()** function can help convert a path name to a canonical form.

For the preceding incorrect example, the following solution can be used:

```
char *real_path_res = NULL;
...
// Normalize **input_file_name** before verification.
real_path_res = realpath(input_file_name, NULL);
if (real_path_res == NULL) {
	... // Normalization error handling
}

// Path verification after normalization
if (!verify_file(real_path_res) {
	... // Verification error handling
}

// Usage
if (fopen(real_path_res, "w") == NULL) {
	... // Actual operation error handling
}

...
free(real_path_res);
real_path_res = NULL;
...
```

**Correct example**:
Based on the actual scenario, we can also use another solution as follows:

If **PATH_MAX** is defined as a constant, it is safe to call **realpath()** with a non-null **resolved_path**.
In this case, the **realpath()** function expects **resolved_path** to reference a character array that is large enough to hold a normalized path.
If **PATH_MAX** is defined, a buffer whose size is **PATH_MAX** is allocated to hold the result of **realpath()**. The example is as follows:

```
char *real_path_res = NULL;
char *canonical_file_name = NULL;
size_t path_size = 0;
...
path_size = (size_t)PATH_MAX;
if (verify_path_size(path_size) == TRUE) {
	canonical_file_name = (char *)malloc(path_size);
	if (canonical_file_name == NULL) {
		... // Error handling
	}

	real_path_res = realpath(inputFilename, canonical_file_name);
}

if (real_path_res == NULL) {
	... // Error handling
}

if (verify_file(real_path_res) == FALSE) {
	... // Error handling
}

if (fopen(real_path_res, "w") == NULL ) {
	... // Error handling
}

...
free(canonical_file_name);
canonical_file_name = NULL;
...
```

**Incorrect example**:
The following code obtains a file name from external system, generates a file path based on the file name, and then directly read the file content. As a result, the attacker can read the content of any file.

```
char *file_name = get_msg_from_remote();
...
sprintf(untrust_path, "/tmp/%s", file_name);
char *text = read_file_content(untrust_path);
```

**Correct example**:
The correct method is to normalize the path and then check whether the path is valid.

```
char *file_name = get_msg_from_remote();
...
sprintf(untrust_path, "/tmp/%s", file_name);
char path[PATH_MAX] = {0};
if (realpath(untrust_path, path) == NULL) {
	... // Error handling.
}

if (!is_valid_path(path)) { // Checking whether the file path is valid.
	... // Error handling.
}

char *text = read_file_content(path);
```

**Exceptions**:

For a command-line program running on the console, manually entering the file path into the console is an exception to this rule.

```
int main(int argc, char **argv)
{
	int fd = -1;
	if (argc == 2) {
		fd = open(argv[1], O_RDONLY);
		...
	}

	...
}
```

## Do Not Create Temporary Files in Shared Directories

**[Description]**
The temporary files of a program must be exclusive to the program. If the temporary files of a program are stored in a shared directory, non-privileged users may obtain additional information of the program, causing information leakage. Therefore, do not create temporary files that are used only by the program itself in any shared directory.

Programmers usually create temporary files in shared directories (for example, **/tmp** and **/var/tmp**), and may periodically clean up these temporary files (for example, every night or during restart), but may not clean up them carefully.

Temporary files are used to store temporary data or data that cannot reside in the memory. They can also be used for inter-process communication (data transmission through the file system). For example, when a process creates a temporary file in a shared directory, if the file name is a well-known name or a temporarily created name, the information can be shared between processes through the file. This method of sharing files between processes by creating temporary files in the shared directory is risky because the files in the shared directory are easily hijacked or manipulated by attackers. The following are several risk mitigation strategies:

-   1. Use other low-level IPC (inter-process communication) mechanisms, such as sockets or shared memory.

-   2. Use higher-level IPC mechanisms, such as remote procedure call (RPC).

-   3. Use a secure directory that can be accessed only by the program itself. (Avoid race conditions in multi-thread/process scenarios.)

In addition, the following lists the methods for creating and using temporary files. A product can use one or more of the following methods based on the actual requirements and even customize proper methods.

-   1. The file must have proper permissions. Only users with required permissions can access the file.

-   2. The name of the file to be created is unique or unpredictable.

-   3. Creates and opens a file only when the file does not exist (atom creation and opening).

-   4. Exclusive access can be used to avoid race conditions.

-   5. Remove the file before exiting the program.

In addition, when a shared directory is granted the read/write permission to multiple users or a group of users, the potential security risk of the shared directory is far greater than that of accessing temporary files in the directory.

It is not easy to safely create temporary files in a shared directory without being threatened. For example, code for a locally mounted file system can be attacked when it is shared with a remotely mounted file system. And the safe version of the function above is also limited by the version of the C runtime library, operating system, and file system. The only secure solution is not to create temporary files in a shared directory.

**Incorrect example**:
In the following example, the program creates a temporary file in the shared directory **/tmp** of the Linux system to store temporary data. In addition, the file name is hardcoded.
Since the file name is hardcoded, it is predictable. Attackers only need to replace the file with a symbolic link, and then the referenced target file is opened and is written with new content.

```
void proc_data(const char  *file_name)
{
	FILE  *fp = fopen(file_name, "wb+");
	if (fp == NULL) {
		... // Error handling
	}

	... // Write the file.
	fclose(fp);
}

int main(void)
{
	// Not compliant: 1. A temporary file is created in the shared directory of the system. 2. The temporary file name is hardcoded.
	char  *real_file = "/tmp/data";
 	...
	proc_data(real_file);
 	...
	return 0;
}
```

**Correct example**:

The **/tmp** directory in the Linux system is a shared directory that can be accessed by all users. Do not create temporary files that are used only by your program in this directory.

**[CVE ID]** CVE-2004-2502

# Others

## Do Not Access Shared Objects in Signal Handlers

**[Description]**
Accessing and modifying shared objects in a signal handler can cause race conditions that can leave data in an inconsistent state.
This rule is not applicable to the following two scenarios (section 5.1.2.3, paragraph 5 of the C11 standard):

1) Reading and writing lock-free atomic objects.

2) Reading and writing an object of the **volatile sig_atomic_t** type. Such an object can be accessed as an atomic entity even in the presence of asynchronous interrupts, which is async-signal-safe.

In addition, in a signal handler, if you need to call a function, call only async-signal-safe functions.

**Incorrect example**:
The following signal handler uses **p_msg** as a shared object and updates the content of the object upon receiving SIGINT signals. However, **p_msg** is not of the **volatile sig_atomic_t** type and is therefore not async-signal-safe. 

```
#define MAX_MSG_SIZE 32
static char g_msg_buf[MAX_MSG_SIZE] = {0};
static char *g_msg = g_msg_buf;
void signal_handler(int signum)
{
	// The following operation on g_msg is not non-compliant because it is not async-signal-safe.
	(void)memset(g_msg, 0, MAX_MSG_SIZE);
	strcpy(g_msg,  "signal SIGINT received.");
	... //
}

int main(void)
{
	strcpy(g_msg,  "No msg yet."); // Initializing the message content.
	signal(SIGINT, signal_handler); // Setting signal handler for the SIGINT signals.
	... // Main loop code of the program
	return 0;
}
```

**Correct example**:
In the following example, only an entity of the **volatile sig_atomic_t** type is used as a shared object in the signal handler.

```
#define MAX_MSG_SIZE 32
volatile sig_atomic_t g_sig_flag = 0;
void signal_handler(int signum)
{
	g_sig_flag = 1; // Compliant
}

int main(void)
{
	signal(SIGINT, signal_handler);
	char msg_buf[MAX_MSG_SIZE];
	strcpy(msg_buf, "No msg yet."); // Initializing the message content.
	... // Main loop code of the program
	if (g_sig_flag == 1) { // Updating the message content based on the status of the sigFlag after the main loop exits.
		strcpy(msgBuf, "signal SIGINT received.");
	}

	return 0;
}
```

**[Software CWE IDs]** CWE-662 and CWE-828

## Do Not Use **rand()** to Generate Pseudorandom Numbers for Security Purposes

**[Description]**
The **rand()** function in the C standard library generates pseudo-random numbers. The quality of the generated random number sequences cannot be ensured. Therefore, do not use the random numbers generated by the **rand()** function for security purposes. Use secure random number generation methods, such as the **/dev/random** file.

Typical security usage scenarios include but are not limited to the following:

-   Generation of a session ID;

-   Random number generation in the Challenge-Handshake Authentication Protocol;

-   Random number generation for verification codes;

-   Random number generation for cryptographic algorithm purposes (for example, generation of initialization vectors, salts, and keys).

**Incorrect example**:
The programmer wants to generate a unique HTTP session ID that cannot be guessed. However, the ID is a random number generated by invoking the **rand()** function. The ID can be guessed and the quality of randomness is low.

**Correct example** (POSIX):
Use the **/dev/random** file to generate random numbers.

**Impact**

Random numbers generated using the **rand()** function can be guessed.

# Kernel Operations

## Ensure that the Validity of the Mapping Start Address and Size Is Verified in the Implementation of the Kernel mmap API

**Description**

**Note**: In the **mmap** interface of the Linux kernel, the **remap_pfn_range()** function is often used to map the physical memory of the device to the user process space. If parameters such as the mapping start address are controlled by a user-mode process and no validity check is performed, the user-mode process can read and write any kernel address through mapping. If an attacker constructs input parameters carefully, the attacker can even run any code in the kernel.

**Incorrect example**:

In the following example, **remap_pfn_range()** is used for memory mapping, but the validity of the user-controlled mapping start address and space size is not verified. As a result, the kernel may crash and any code may be executed.

```
static int incorrect_mmap(struct file *file, struct vm_area_struct *vma)
{
	unsigned long size;
	size = vma->vm_end - vma->vm_start;
	vma->vm_page_prot = pgprot_noncached(vma->vm_page_prot);
	// Error: The validity of the mapping start address and space size is not verified.
	if (remap_pfn_range(vma, vma->vm_start, vma->vm_pgoff, size, vma->vm_page_prot)) { 
		err_log("%s, remap_pfn_range fail", __func__);
		return EFAULT;
	} else {
		vma->vm_flags &=  ~VM_IO;
	}

	return EOK;
}
```

**Correct example**:

The validity of parameters such as the mapping start address is verified.

```
static int correct_mmap(struct file *file, struct vm_area_struct *vma)
{
	unsigned long size;
	size = vma->vm_end - vma->vm_start;
	// Modification: Add a verification function to check whether the mapping start address and space size are valid.
	if (!valid_mmap_phys_addr_range(vma->vm_pgoff, size)) { 
		return EINVAL;
	}

	vma->vm_page_prot = pgprot_noncached(vma->vm_page_prot);
	if (remap_pfn_range(vma, vma->vm_start, vma->vm_pgoff, size, vma->vm_page_prot)) {
		err_log( "%s, remap_pfn_range fail ", __func__);
		return EFAULT;
	} else {
		vma->vm_flags &=  ~VM_IO;
	}

	return EOK;
}
```

## Kernel Programs Must Use Dedicated Kernel Functions to Read and Write User-Mode Buffers

**Description**

During data exchange between user-mode and kernel-mode processes, if the user-mode input pointer is directly referenced without any verification (for example, verification of address range and null pointers) in the kernel, the kernel may crash or any address may be accessed when an invalid pointer is imported from the user-mode process. Therefore, insecure functions, such as **memcpy()** and **sprintf()**, should not be used. Instead, dedicated functions provided by the kernel that incorporate input parameter verification capabilities, such as **copy_from_user()**, **copy_to_user()**, **put_user()**, and **get_user()**, are used to access the user-mode buffer.

The prohibited functions are **memcpy()**, **bcopy()**, **memmove()**, **strcpy()**, **strncpy()**, **strcat()**, **strncat()**, **sprintf()**, **vsprintf()**, **snprintf()**, **vsnprintf()**, **sscanf()** and **vsscanf()**.

**Incorrect example**:

The kernel-mode process directly uses the **buf** pointer transferred by the use-mode process as the parameter of **snprintf()**. When **buf** is **NULL**, the kernel may crash.

```
ssize_t incorrect_show(struct file *file, char__user *buf, size_t size, loff_t *data)
{
	//Error: The user-mode pointer is directly referenced. If the buffer is NULL, a null pointer exception occurs, causing the kernel to crash.
	return snprintf(buf, size, "%ld\n", debug_level); 
}
```

**Correct example**:

**copy_to_user()** is used instead of **snprintf()**.

```
ssize_t correct_show(struct file *file, char __user *buf, size_t size, loff_t *data)
{
	int ret = 0;
	char level_str[MAX_STR_LEN] = {0};
	snprintf(level_str, MAX_STR_LEN, "%ld \n", debug_level);
	if(strlen(level_str) >= size) {
		return EFAULT;
	}
	
	// Modification: Use the dedicated function copy_to_user() to write data to the user-mode buffer and prevent buffer overflow.
	ret = copy_to_user(buf, level_str, strlen(level_str)+1); 
	return ret;
}
```

**Incorrect example**:

The kernel-mode process directly uses the **user_buf** pointer transferred by the user-mode process as the data source to perform the **memcpy()** operation. When **user_buf** is **NULL**, the kernel may crash.

```
size_t incorrect_write(struct file  *file, const char __user  *user_buf, size_t count, loff_t  *ppos)
{
	...
	char buf [128] = {0};
	int buf_size = 0;
	buf_size = min(count, (sizeof(buf)-1));
	//Error: The user-mode pointer is directly referenced. If user_buf is NULL, the kernel may crash.
	(void)memcpy(buf, user_buf, buf_size); 
	...
}
```

**Correct example**:

**copy_from_user()** is used instead of **memcpy()**.

```
ssize_t correct_write(struct file *file, const char __user *user_buf, size_t count, loff_t *ppos)
{
	...
	char buf[128] = {0};
	int buf_size = 0;

	buf_size = min(count, (sizeof(buf)-1));
	// Modification: Use the dedicated function copy_from_user() to write data to the kernel-mode buffer and prevent buffer overflow.
	if (copy_from_user(buf, user_buf, buf_size)) { 
		return EFAULT;
	}

	...
}
```

## Verifying the Copy Length of **copy_from_user()** to Prevent Buffer Overflow

Note: The **copy_from_user()** function is usually used when a kernel-mode process copies data from a user-mode process. If the copy length is not verified or the verification is improper, the kernel buffer overflows, causing kernel panic or privilege escalation.

**Incorrect example**:

The copy length is not verified.

```
static long gser_ioctl(struct file  *fp, unsigned cmd, unsigned long arg)
{
	char smd_write_buf[GSERIAL_BUF_LEN];
	switch (cmd)
	{
		case GSERIAL_SMD_WRITE:
			if (copy_from_user(&smd_write_arg, argp, sizeof(smd_write_arg))) {...}
			// Error: The copy length parameter smd_write_arg.size is input by the user-mode process and is not verified.
			copy_from_user(smd_write_buf, smd_write_arg.buf, smd_write_arg.size); 
			...
	}
}
```

**Correct example**:

Length verification is added.

```
static long gser_ioctl(struct file *fp, unsigned cmd, unsigned long arg)
{
	char smd_write_buf[GSERIAL_BUF_LEN];
	switch (cmd)
	{
		case GSERIAL_SMD_WRITE:
			if (copy_from_user(&smd_write_arg, argp, sizeof(smd_write_arg))){...}
			// Modification: Verification is added.
			if (smd_write_arg.size  >= GSERIAL_BUF_LEN) {......} 
			copy_from_user(smd_write_buf, smd_write_arg.buf, smd_write_arg.size);
 			...
	}
}
```

## Initializing Data Copied by copy_to_user() to Prevent Information Leakage

**Description**

**Description**: When **copy_to_user()** is used to copy data from a kernel-mode process to a user-mode process, if the data is not completely initialized (for example, no value is assigned to a structure member or a memory hole occurs due to byte alignment), sensitive information such as stack pointers will be leaked. This allows attackers to bypass security mechanisms such as kernel address space layout randomization (KASLR).

**Incorrect example**:

The data structure members are not completely initialized.

```
static long rmnet_ctrl_ioctl(struct file *fp, unsigned cmd, unsigned long arg)
{
	struct ep_info info;
	switch (cmd) {
		case FRMNET_CTRL_EP_LOOKUP:
			info.ph_ep_info.ep_type = DATA_EP_TYPE_HSUSB;
			info.ipa_ep_pair.cons_pipe_num = port->ipa_cons_idx;
			info.ipa_ep_pair.prod_pipe_num = port->ipa_prod_idx;
			// Error: The info structure has four members, but not all of them are assigned with values.
			ret = copy_to_user((void __user *)arg, &info, sizeof(info)); 
			...
	}
}
```

**Correct example**:

All structure members are initialized.

```
static long rmnet_ctrl_ioctl(struct file *fp, unsigned cmd, unsigned long arg)
{
	struct ep_info info;
	// Modification: memset is used to initialize the buffer to ensure that no memory hole exists due to byte alignment or missing values.
	(void)memset(&info, '0', sizeof(ep_info)); 
	switch (cmd) {
		case FRMNET_CTRL_EP_LOOKUP:
			info.ph_ep_info.ep_type = DATA_EP_TYPE_HSUSB;
			info.ipa_ep_pair.cons_pipe_num = port->ipa_cons_idx;
			info.ipa_ep_pair.prod_pipe_num = port->ipa_prod_idx;
			ret = copy_to_user((void __user *)arg, &info, sizeof(info));
			...
	}
}
```

## Do Not Use the BUG_ON Macro in Exception Handlers to Avoid Kernel Panic

**Description**

The **BUG_ON** macro calls the **panic()** function of the kernel to prints error information and proactively crashes the system. In normal logic processing (for example, the command parameter of the input/output control interface cannot be identified), the system should not crash. Do not use the Bug_ON macro in such exception handling scenarios. The WARN_ON macro is recommended.

**Incorrect example**:

The **BUG_ON** macro is used in the normal process.

```
/* Check whether the timer is busy. 1: busy; 0: not busy */
static unsigned int is_modem_set_timer_busy(special_timer *smem_ptr)
{
	int i = 0;
	if (smem_ptr == NULL) {
		printk(KERN_EMERG"%s:smem_ptr NULL!\n", __FUNCTION__);
		// Error: The BUG_ON system macro calls panic() after printing the call stack. As a result, the kernel rejects services. This macro should not be used in the normal process.
		BUG_ON(1); 
		return 1;
	}

	...
}
```

**Correct example**:

The bug_ON macro is removed.

```
/* Check whether the timer is busy. 1: busy; 0: not busy */
static unsigned int is_modem_set_timer_busy(special_timer *smem_ptr)
{
	int i = 0;
	if (smem_ptr == NULL) {
		printk(KERN_EMERG"%s:smem_ptr NULL!\n",  __FUNCTION__);
		// Modification: Remove BUG_ON or use WARN_ON.
		return 1;
	}

	...
}
```

## Do Not Use Functions That Cause Process to Sleep in the Code of Interrupt Handlers or Processes That Hold Spinlocks

**Description**

Linux schedules processes. A Linux interrupt can be interrupted only by interrupts that have a higher priority. The system cannot schedule processes when processing interrupts. If the interrupt handler is sleeping, the kernel cannot be woken up. As a result, the kernel is suspended.

Kernel preemption is disabled when a spinlock is in use. If a process that holds the spinlock goes to sleep, other processes will stop running because they cannot acquire the CPU core. As a result, the system does not respond and is suspended.

Therefore, do not use functions that may cause the process to sleep (such as **vmalloc()** and **msleep()**), block (such as **copy_from_user()**, **copy_to_user()**), or consume a lot of time (such as **printk()**) in the code of an interrupt handler or a process that will acquire the spinlock.

## Preventing Kernel Stack Overflow

**Description**

In Linux, kernel stack resources are precious because the size of kernel stack is fixed (8 KB for a 32-bit system and 16 KB for a 64-bit system). Improper use of the kernel stack may cause stack overflow and system suspension. Therefore, the following requirements must be met:

-   The memory space applied for in the stack cannot exceed the size of the kernel stack.

-   Pay attention to the depth of nested functions.

-   Do not define too many variables.

**Incorrect example**:

In the following example, the variable is too large, causing stack overflow.

```
...
struct result
{
	char name[4];
	unsigned int a;
	unsigned int b;
	unsigned int c;
	unsigned int d;
}; // The size of the result structure is 20 bytes.

int foo()
{
	struct result temp[512];
	// Error: The temp array contains 512 elements, and the total size is 10 KB, which is far greater than the size of the kernel stack.
	(void)memset(temp, 0, sizeof(result) * 512); 
	... // use temp do something
	return 0;
}

...
```

In the code, the array temp has 512 elements, and the total size is 10 KB, which is much greater than the kernel size 8 KB. As a result, stack overflow occurs.

**Correct example**:

**kmalloc()** is used instead.

```
...
struct result
{
	char name[4];
	unsigned int a;
	unsigned int b;
	unsigned int c;
	unsigned int d;
}; // The size of the result structure is 20 bytes.

int foo()
{
	struct result  *temp = NULL;
	temp = (result *)kmalloc(sizeof(result) * 512, GFP_KERNEL); // Modification: kmalloc() is used to apply for memory.
	... // check temp is not NULL
	(void)memset(temp, 0, sizeof(result)  * 512);
	... // use temp do something
	... // free temp
	return 0;
}
...
```

## Restoring the Temporarily Disabled Address Verification Mechanism After Use

**Description**

The Supervisor Mode Execution Protection (SMEP) mechanism prevents the kernel from executing user-space code. In the ARM environment, a similar mechanism called Privileged Execute Never (PXN) is implemented. System calls (such as **open()** and **write()**) are originally intended for user-space programs. By default, these functions verify the input parameter address. If the input parameter address is not a user space address, an error is reported. Therefore, to use these system calls in the kernel program, the parameter address verification function must be disabled. **set_fs()** and **get_fs()** can be used to achieve this purpose. For details, see the following example:

```
...
mmegment_t old_fs;
printk("Hello, I'm the module that intends to write message to file.\n");
if (file == NULL) {
	file = filp_open(MY_FILE, O_RDWR | O_APPEND | O_CREAT, 0664);
}

if (IS_ERR(file)) {
	printk("Error occured while opening file %s, exiting ...\n", MY_FILE);
	return 0;
}

sprintf(buf, "%s", "The Message.");
old_fs = get_fs(); // get_fs() is used to obtain the upper limit of the user space address. 
                   // #define get_fs() (current->addr_limit
set_fs(KERNEL_DS); // set_fs is used to increase the upper limit of the space address to KERNEL_DS so that the kernel code can call system functions.
file->f_op->write(file, (char *)buf, sizeof(buf), &file->f_pos); // The kernel-mode code can call the write() function.
set_fs(old_fs); // Restore the original user space address limit after use.
...
```

According to the preceding code, the key is to restore the address verification function immediately after the operation is complete. Otherwise, the absence of SMEP/PXN security mechanism allows attackers to exploit many vulnerabilities.

**Incorrect example**:

In the error handling section of the program, the address verification function is not restored using **set_fs()**.

```
...
oldfs = get_fs();
set_fs(KERNEL_DS);
/* Create the done file in the timestamp directory. */
fd = sys_open(path, O_CREAT | O_WRONLY, FILE_LIMIT);
if (fd < 0) {
	BB_PRINT_ERR("sys_mkdir[%s] error, fd is[%d]\n", path, fd);
	return; // Error: The address verification mechanism is not restored in the error handling section.
}

sys_close(fd);
set_fs(oldfs);
...
```

**Correct example**:

The address verification function is restored in the error handling section.

```
...
oldfs = get_fs();
set_fs(KERNEL_DS);

/* Create the done file in the timestamp directory. */
fd = sys_open(path, O_CREAT | O_WRONLY, FILE_LIMIT);
if (fd < 0) {
	BB_PRINT_ERR("sys_mkdir[%s] error, fd is[%d] \n", path, fd);
	set_fs(oldfs); // Modification: The address verification function is restored in the error handling section.
	return;
}

sys_close(fd);
set_fs(oldfs);
...
```
