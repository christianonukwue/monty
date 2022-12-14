# ​🧑‍💻​  C - Stacks, Queues - LIFO, FIFO
## Learning Objectives
------------
At the end of this project, you are expected to be able to explain to anyone, without the help of Google:

### General
- What do LIFO and FIFO mean
- What is a stack, and when to use it
- What is a queue, and when to use it
- What are the common implementations of stacks and queues
- What are the most common use cases of stacks and queues
- What is the proper way to use global variables

## Requirements
------------
### General

- Allowed editors: `vi`, `vim`, `emacs`
- All your files will be compiled on Ubuntu 20.04 LTS using gcc, using the options `-Wall -Werror -Wextra -pedantic -std=gnu89`.
- All your files should end with a new line.
- A README.md file, at the root of the folder of the project is mandatory.
- Your code should use the Betty style. It will be checked using betty-style.pl and betty-doc.pl
- You allowed to use a maximum of one global variable.
- No more than 5 functions per file.
- You are allowed to use the C standard library.
- The prototypes of all your functions should be included in your header file called monty.h.
- All your header files should be include guarded

## More Info
------------
### Data Structures

```
    /**
     * struct stack_s - doubly linked list representation of a stack (or queue)
     * @n: integer
     * @prev: points to the previous element of the stack (or queue)
     * @next: points to the next element of the stack (or queue)
     *
     * Description: doubly linked list node structure
     * for stack, queues, LIFO, FIFO
     */
    typedef struct stack_s
    {
            int n;
            struct stack_s *prev;
            struct stack_s *next;
    } stack_t;
```
```
/**
 * struct instruction_s - opcode and its function
 * @opcode: the opcode
 * @f: function to handle the opcode
 *
 * Description: opcode and its function
 * for stack, queues, LIFO, FIFO
 */
typedef struct instruction_s
{
        char *opcode;
        void (*f)(stack_t **stack, unsigned int line_number);
} instruction_t;
```
## Compilation & Output
----
The code will be compiled this way:

``` $ gcc -Wall -Werror -Wextra -pedantic -std=gnu89 *.c -o monty ```

- Any output must be printed on stdout
- Any error message must be printed on stderr

## ​💥​ The Monty language
-----
Monty 0.98 is a scripting language that is first compiled into Monty byte codes (Just like Python). It relies on a unique stack, with specific instructions to manipulate it. The goal of this project is to create an interpreter for Monty ByteCodes files.
### Monty byte code files
---
Files containing Monty byte codes usually have the `.m `extension. Most of the industry uses this standard but it is not required by the specification of the language. There is not more than one instruction per line. There can be any number of spaces before or after the opcode and its argument:
```
group_luis_carlos@ubuntu:~/monty$ cat -e bytecodes/000.m
push 0$
push 1$
push 2$
  push 3$
                   pall    $
push 4$
    push 5    $
      push    6        $
pall$
group_luis_carlos@ubuntu:~/monty$
```
Monty byte code files can contain blank lines (empty or made of spaces only, and any additional text after the opcode or its required argument is not taken into account:
```
group_luis_carlos@ubuntu:~/monty$ cat -e bytecodes/001.m
push 0 Push 0 onto the stack$
push 1 Push 1 onto the stack$
$
push 2$
  push 3$
                   pall    $
$
$
                           $
push 4$
$
    push 5    $
      push    6        $
$
pall This is the end of our program. Monty is awesome!$
group_luis_carlos@ubuntu:~/monty$
```

### The monty program
---
- Usage: `monty file`
    - where `file` is the path to the file containing Monty byte code.
- If the user does not give any file or more than one argument to your program, print the error message `USAGE: monty file`, followed by a new line, and exit with the status `EXIT_FAILURE`.
- If, for any reason, it’s not possible to open the file, print the error message `Error: Can't open file <file>`, followed by a new line, and exit with the status `EXIT_FAILURE`
    - where `<file>` is the name of the file.
- If the file contains an invalid instruction, print the error message `L<line_number>: unknown instruction <opcode>`, followed by a new line, and exit with the status `EXIT_FAILURE`.
    - Where is the line number where the instruction appears.
    - Line numbers always start at 1.
- The monty program runs the bytecodes line by line and stop if either:
    - it executed properly every line of the file
    - it finds an error in the file
    - an error occured
- If you can’t malloc anymore, print the error message `Error: malloc failed`, followed by a new line, and exit with status `EXIT_FAILURE`.
- You have to use `malloc` and `free` and are not allowed to use any other function from `man malloc` (realloc, calloc, …)

## Opcodes
---
| Opcode | Description                    |
| ------------- | ------------------------------ |
| `push`      | The opcode push pushes an element to the stack.       |
| `pall`   | The opcode pall prints all the values on the stack, starting from the top of the stack.     |
| `pint`      | The opcode pint prints the value at the top of the stack, followed by a new line.       |
| `pop`   | The opcode pop removes the top element of the stack.    |
| `swap`      | The opcode swap swaps the top two elements of the stack.       |
| `add`   | The opcode add adds the top two elements of the stack.     |
| `nop`      | The opcode nop doesn’t do anything.       |
| `sub`   | The opcode sub subtracts the top element of the stack from the second top element of the stack.     |
| `div`      | The opcode div divides the second top element of the stack by the top element of the stack.       |
| `mul`   | The opcode mul multiplies the second top element of the stack with the top element of the stack.     |
| `mod`      | The opcode mod computes the rest of the division of the second top element of the stack by the top element of the stack.       |
| `pchar`   | The opcode pchar prints the char at the top of the stack, followed by a new line.     |
| `pstr`      | The opcode pstr prints the string starting at the top of the stack, followed by a new line.       |
| `rotl`   | The opcode rotl rotates the stack to the top.     |
| `rotr`      | The opcode rotr rotates the stack to the bottom.       |
| `stack`   | The opcode stack sets the format of the data to a stack (LIFO). This is the default behavior of the program.     |
| `queue`   | The opcode queue sets the format of the data to a queue (FIFO).  

