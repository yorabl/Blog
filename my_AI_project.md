# Choosing my AI project
## Introduction
The world of LLM AI tools is continuously expending with different tools promising different types of experiences for their users. One of my favorite things in the world is to learn new subjects in depths, but with my ADHD I find myself neglecting the new subject I have just poured an hour or two, days or even several weeks on. Hence I wanted to find a tool that fits my model of learning alongside with keeping me back where I left it off. The goal of this experiment was to create a lab in C programming based on the "C programming language" by K&C, with the end goal of a small sized project. 
## Why NotebookLM
Most of the LLM tools that I tried to work with are limited on two different accounts. The first is the lack of structural form of the information, as most of people I need structure, chapters or units for me to consume and then implement the new information. NotebookLM provides us the ability to upload specific resource and to base the response of the LLM on top of it. 
The second is the format of the interface. I don't find the chat based LLMs comfortable for my long time use; I find myself scrolling up and down for previous responses. NotbookLM it is much easier for my use in this aspect as well. 
# Getting started with NotebookLM 
## Providing resources 
### what I did wrong?
On the face of it, using NotebookLM seems very intuitive. One uploads the resource they want to use and basically roll with it. Well, it does, depends on the size of the resource. Initially, I uploaded the PDF file of the whole book as a single unit. I asked the prompt to summarize whole chapters - that worked, perfectly, in the chat. 
When I started to use the other features, Studio, for the AI generated podcast and the Notes section that provides it didn't work well. The features were to generalized, did touch the specific chapters but the whole books. It all come down to how the user provides the resources. 
### Fixing my process - use multiple small resource files
NotebookLM resources tab is build the right way, I really liked it, once I understood how to use it. I uploaded a whole PDF file as the atomic unit of resources, I needed to breakdown the book to chapters. Once I did it, uploading each chapter as the atomic resource unit, I started to check in and check out chapters from the resources that the notebook uses when I use prompts or better yet, the Studio and Notes sections - they worked as expected. 
## Learning with NotebookLM
### Making it interesting for me, quiz me! 
The reason I choose this whole experiment was to get back writing C for the actual fun of it. Once I went through a chapter and revisited I wanted to engage with the material in a meaningful way so I can. 
### Doing it wrong 
My initial approach was wrong, when trying to go with the naive approach and just ask for tasks or exercises, the chat will provide the questions from the book (if I wanted the exercises of the book I don't need this tool). I tried to ask for different exercises and it provided very limited and heavily inspired by the sources. 
### Changing my approach - ask for real world scenarios
At this point I wasn't having a lot of fun and I noticed that my engagement with the whole task lessened. Following on a irrelevant video on Youtube about an Arduino project I thought about asking the chat prompt for a real time adjacent task after the chapter about control flow. 
The result was a very simple task that asked for a listener to input from user and it provides an output. Of course in this case, I provided 2 answers, with if-else and the other with switch, the AI preferred the switch. 

```c
#include <stdio.h>
#define NORMAL 'n'
#define WARNING 'w'
#define ALERT 'a'

int main(void) {
	int status = NORMAL;
	int previous_status = 0;
	
	while (status != EOF) {
		status = getchar();
		switch (status) {
			case WARNING:
			if (previous_status != status) {
				printf("WARNING\n");
				previous_status = status;
				}
			break;
			case ALERT:
			if (previous_status != status) {
				printf("ALERT\n");
				previous_status = status;
				}
			break;
			case NORMAL:
			if (previous_status != status) {
				printf("NORMAL\n");
				previous_status = status;
				}
			break;
			default:
			break;
			}
		}
	return 0;
}
```
## Choosing a project 
As with any course, I wanted to complete it with a small project that will require to take some file as an input from command line and create an output file. I chose something pretty simple and fun: translate alphabet and numbers to morse and back. 
Throughout the writing process, NotebookLM helped me with my questions. When need to read single lines from file, providing guidance on buffer sizes. For each question, it broke down the answer to easily digestible points with references to the place in the resource:
```
how to read one line at the time from a file?
```
Response: 
*To read one line at a time from a file in C, you typically use functions that read characters until a newline character or an End-Of-File (EOF) indicator is encountered, or a specified maximum number of characters has been read. The standard C library provides fgets, and various examples in the sources illustrate custom getline functions.*
*Here's how you can achieve this, drawing on the provided sources:*
1. *Using the Standard Library Function fgets The fgets function is part of the standard C library, declared in <stdio.h>1. It is designed for reading lines from a specified file stream:*
```c
char *fgets(char *line, int maxline, FILE *fp);
```

*• line: This is a character array (string buffer) where the read line will be stored1.*
*• maxline: This integer specifies the maximum number of characters to read, including the newline character, but excluding the null terminator. fgets will read at most maxline-1 characters into the array1....*
*• fp: This is a FILE * pointer that identifies the file from which to read1.*
*• Return Value: fgets returns the line pointer on success. If an end of file is encountered or an error occurs before any characters are read, it returns NULL1....*
*• Newline and Null Termination: fgets includes the newline character (\n) in the line array if it is encountered within maxline-1 characters. The resulting string is always terminated with a null character (\0)1....*
*Example Usage of fgets (adapted from sources): If you want to read from standard input (keyboard or redirected input), you would use stdin as the FILE * argument4.*
```c
#include <stdio.h>
#include <string.h> // For strlen

#define MAXLINE 1000

int main() {
    char line[MAXLINE];
    long lineno = 0;

    // Read lines from standard input until EOF or error
    while (fgets(line, MAXLINE, stdin) != NULL) {
        lineno++;
        printf("%ld: %s", lineno, line); // Print with line number
    }
    return 0;
```
*A common pattern, as seen in the myfind exercise, is to use a custom getline function that internally calls fgets (or getchar). Such a wrapper function can simplify logic by returning the line length, with 0 indicating EOF.*
2. *Custom getline Functions The provided sources frequently use and define custom getline functions to read input line by line. These functions typically return the length of the line read, and use EOF as a signal for end of file or error.*
*Characteristics of Custom getline (as seen in examples):*
*• Purpose: To read a line of input characters into a character array (s or line) up to a specified limit (lim or maxline)*
*• Return Value: They usually return the length of the line, including the newline character if present. A return value of 0 (or EOF in some cases) signifies end of file or that no characters were read*
*• Newline Handling: These functions read until a newline character is found or the limit is reached. The newline character, if present, is usually stored in the array*
*• Null Termination: The character array is always null-terminated (\0) to form a proper C string.*
*• Input Source: Typically, they read from stdin using getchar()*
*Example Implementation of Custom getline (from the exercise and sources):*
```c
#include <stdio.h> // For getchar, EOF
#define MAXLINE 1000

/*
 * getline: reads a line from standard input into char array s.
 *          Reads at most lim-1 characters, stops on newline or EOF.
 *          Null-terminates the string.
 *          Returns the length of the line (excluding '\0', including '\n' if present).
 *          Returns 0 on EOF or error, if no characters were read.
 * (Adapted from Chapter 1, Section 1.9, page 31) [6, 9]
 */
int getline(char s[], int lim)
{
    int c, i;
    for (i = 0; i < lim - 1 && (c = getchar()) != EOF && c != '\n'; ++i) {
        s[i] = c;
    }
    if (c == '\n') {
        s[i] = c;
        ++i;
    }
    s[i] = '\0'; // Null-terminate the string [3]
    return i; // Return length including newline if present, excluding null terminator
}
```
*Both fgets and custom getline implementations serve the purpose of reading input line by line, but fgets offers a more robust and standardized approach for general file handling. The custom getline functions often simplify the return value and can be tailored for specific program needs.*

---
This was very helpful, with quick and easy for me to adjust this output to my task. 
The writing of the program wasn't the most interesting part, the interesting part is the feedback in the end, I asked Gemini with 2.5 pro:
```
How would you improve this code?
```
```c
morse.c
#include "morse.h"
#include <ctype.h>
#include <malloc/_malloc_type.h>
#include <stdio.h>
#include <stdlib.h>
#include <string.h>

#define MAX_LINE_LENGTH 2024

typedef struct {
	char action;
	char *output_file;
	char *input_file;
} input_args;

int letter_length(char *input, int start) {
	int counter = 1;
	while (input[start + counter] != ' ') {
		counter++;
	}
	return counter;
}

char *get_letter(const char *input, int start, int length) {
	int input_length = strlen(input);
	if (start >= input_length) {
		return NULL;
	}
	char *letter = (char *)malloc((length + 2) * sizeof(char));
	if (letter == NULL) {
		return NULL;
	}
	strncpy(letter, input + start, length);
	letter[length] = ' ';
	letter[length + 1] = '\0';
	return letter;
}

int from_morse(char *input, char *output) {
	int start = 0;
	int length = 0;
	int i = 0;
	char *letter;
	while (input[start] != '\0') {
		if (input[start] == '|') {
			output[i] = ' ';
			start++;
			i++;
		} else {
			length = letter_length(input, start);
			letter = get_letter(input, start, length);
			if (letter == NULL) {
				return i;
			} else if (strcmp(letter, A) == 0) {
				output[i] = 'a';
			} else if (strcmp(letter, B) == 0) {
				output[i] = 'b';
			} else if (strcmp(letter, C) == 0) {
				output[i] = 'c';
			} else if (strcmp(letter, D) == 0) {
				output[i] = 'd';
			} else if (strcmp(letter, E) == 0) {
				output[i] = 'e';
			} else if (strcmp(letter, F) == 0) {
				output[i] = 'f';
			} else if (strcmp(letter, G) == 0) {
				output[i] = 'g';
			} else if (strcmp(letter, H) == 0) {
				output[i] = 'h';
			} else if (strcmp(letter, I) == 0) {
				output[i] = 'i';
			} else if (strcmp(letter, J) == 0) {
				output[i] = 'j';
			} else if (strcmp(letter, K) == 0) {
				output[i] = 'k';
			} else if (strcmp(letter, L) == 0) {
				output[i] = 'l';
			} else if (strcmp(letter, M) == 0) {
				output[i] = 'm';
			} else if (strcmp(letter, N) == 0) {
				output[i] = 'n';
			} else if (strcmp(letter, O) == 0) {
				output[i] = 'o';
			} else if (strcmp(letter, P) == 0) {
				output[i] = 'p';
			} else if (strcmp(letter, Q) == 0) {
				output[i] = 'q';
			} else if (strcmp(letter, R) == 0) {
				output[i] = 'r';
			} else if (strcmp(letter, S) == 0) {
				output[i] = 's';
			} else if (strcmp(letter, T) == 0) {
				output[i] = 't';
			} else if (strcmp(letter, U) == 0) {
				output[i] = 'u';
			} else if (strcmp(letter, V) == 0) {
				output[i] = 'v';
			} else if (strcmp(letter, W) == 0) {
				output[i] = 'w';
			} else if (strcmp(letter, X) == 0) {
				output[i] = 'x';
			} else if (strcmp(letter, Y) == 0) {
				output[i] = 'y';
			} else if (strcmp(letter, Z) == 0) {
				output[i] = 'z';
			} else if (strcmp(letter, ZERO) == 0) {
				output[i] = '0';
			} else if (strcmp(letter, ONE) == 0) {
				output[i] = '1';
			} else if (strcmp(letter, TWO) == 0) {
				output[i] = '2';
			} else if (strcmp(letter, THREE) == 0) {
				output[i] = '3';
			} else if (strcmp(letter, FOUR) == 0) {
				output[i] = '4';
			} else if (strcmp(letter, FIVE) == 0) {
				output[i] = '5';
			} else if (strcmp(letter, SIX) == 0) {
				output[i] = '6';
			} else if (strcmp(letter, SEVEN) == 0) {
				output[i] = '7';
			} else if (strcmp(letter, EIGHT) == 0) {
				output[i] = '8';
			} else if (strcmp(letter, NINE) == 0) {
				output[i] = '9';
			}
			i++;
			start = start + length + 1;
			}
		}
	return i;
}

void insert_morse(char *output, const char *letter, int *current) {
	strcpy(output + *current, letter);
	*current += strlen(letter);
}

int to_morse(char *input, char *output) {
	int j = 0;
	for (int i = 0; input[i] != '\0'; i++) {
		char lower = tolower(input[i]);
		switch (lower) {
			case ('a'):
				insert_morse(output, A, &j);
				break;
			case ('b'):
				insert_morse(output, B, &j);
				break;
			case ('c'):
				insert_morse(output, C, &j);
				break;
			case ('d'):
				insert_morse(output, D, &j);
				break;
			case ('e'):
				insert_morse(output, E, &j);
				break;
			case ('f'):
				insert_morse(output, F, &j);
				break;
			case ('g'):
				insert_morse(output, G, &j);
				break;
			case ('h'):
				insert_morse(output, H, &j);
				break;
			case ('i'):
				insert_morse(output, I, &j);
				break;
			case ('j'):
				insert_morse(output, J, &j);
				break;
			case ('k'):
				insert_morse(output, K, &j);
				break;
			case ('l'):
				insert_morse(output, L, &j);
				break;
			case ('m'):
				insert_morse(output, M, &j);
				break;
			case ('n'):
				insert_morse(output, N, &j);
				break;
			case ('o'):
				insert_morse(output, O, &j);
				break;
			case ('p'):
				insert_morse(output, P, &j);
				break;
			case ('q'):
				insert_morse(output, Q, &j);
				break;
			case ('r'):
				insert_morse(output, R, &j);
				break;
			case ('s'):
				insert_morse(output, S, &j);
				break;
			case ('t'):
				insert_morse(output, T, &j);
				break;
			case ('u'):
				insert_morse(output, U, &j);
				break;
			case ('v'):
				insert_morse(output, V, &j);
				break;
			case ('w'):
				insert_morse(output, W, &j);
				break;
			case ('x'):
				insert_morse(output, X, &j);
				break;
			case ('y'):
				insert_morse(output, Y, &j);
				break;
			case ('z'):
				insert_morse(output, Z, &j);
				break;
			case ('0'):
				insert_morse(output, ZERO, &j);
				break;
			case ('1'):
				insert_morse(output, ONE, &j);
				break;
			case ('2'):
				insert_morse(output, TWO, &j);
				break;
			case ('3'):
				insert_morse(output, THREE, &j);
				break;
			case ('4'):
				insert_morse(output, FOUR, &j);
				break;
			case ('5'):
				insert_morse(output, FIVE, &j);
				break;
			case ('6'):
				insert_morse(output, SIX, &j);
				break;
			case ('7'):
				insert_morse(output, SEVEN, &j);
				break;
			case ('8'):
				insert_morse(output, EIGHT, &j);
				break;
			case ('9'):
				insert_morse(output, NINE, &j);
				break;
			case (' '):
				insert_morse(output, SPACE, &j);
				break;
			case ('.'):
				output[j] = '\n';
				j++;
				break;
			case (','):
				break;
				default:
				j++;
				break;
			}
		}
	return j;
}

void clean_buffer(char *line, int length) {
	for (int i = 0; i < length; i++) {
		line[i] = '\0';
	}
}

int main(int argc, char *argsv[]) {
	input_args tmp_input;
	for (int i = 1; i < argc; i++) {
		if (strcmp(argsv[i], "-o") == 0 || strcmp(argsv[i], "--output-file") == 0) {
			tmp_input.output_file = argsv[i + 1];
			i++;
		} else if (strcmp(argsv[i], "-i") == 0 ||
			strcmp(argsv[i], "--input-file") == 0) {
			tmp_input.input_file = argsv[i + 1];
			i++;
		} else if (strcmp(argsv[i], "encrypt") == 0) {
			tmp_input.action = 1;
		} else if (strcmp(argsv[i], "decrypt") == 0) {
			tmp_input.action = 0;
		}
	}
	if (argc == 1) {
		printf("Please provide input file\n");
	} else {
		FILE *input_file;
		FILE *output_file;
		char line[MAX_LINE_LENGTH];
		int line_length;
		
		input_file = fopen(tmp_input.input_file, "r");
		output_file = fopen(tmp_input.output_file, "a");
		
		if (input_file == NULL) {
			fprintf(stderr, "File doesn't exist\n");
			return 1;
		}
		char *output = malloc(MAX_LINE_LENGTH);
		
		while (fgets(line, MAX_LINE_LENGTH, input_file) != NULL) {
			char *output = malloc(MAX_LINE_LENGTH);
			if (tmp_input.action) {
				line_length = to_morse(line, output);
			} else {
				line_length = from_morse(line, output);
			}
			fprintf(output_file, "%s\n", output);
			clean_buffer(output, line_length);
		}
		free(output);
		fclose(input_file);
	}
	return 0;
}
```
```c
morse.h:
#define A ".- "
#define B "-... "
#define C "-.-. "
#define D "-.. "
#define E ". "
#define F "..-. "
#define G "--. "
#define H ".... "
#define I ".. "
#define J ".--- "
#define K "-.- "
#define L ".-.. "
#define M "-- "
#define N "-. "
#define O "--- "
#define P ".--. "
#define Q "--.- "
#define R ".-. "
#define S "... "
#define T "- "
#define U "..- "
#define V "...- "
#define W ".-- "
#define X "-..- "
#define Y "-.-- "
#define Z "--.. "
#define ONE ".---- "
#define TWO "..--- "
#define THREE "...-- "
#define FOUR "....- "
#define FIVE "..... "
#define SIX "-.... "
#define SEVEN "--... "
#define EIGHT "---.. "
#define NINE "----. "
#define ZERO "----- "
#define SPACE "|"
```
The response of Gemini 2.5 Pro was great:
- *Header inclusion:* 
	- *Replace the non-standard `<malloc/_malloc_type.h>` with `<stdlib.h>`, which provides `malloc`, `free`, and `exit`.* 
		- *I made this error I made due to lack of attention with the autofill of VScode*
- *Constant* 
	- *Consider using an `enum` for actions (`ENCRYPT`, `DECRYPT`) instead of `char` `0` or `1` in `input_args` for better readability.*
- *Error handling:*
	- *Add more robust error checking, especially for file operations and memory allocation.*
	- *Improve command-line argument parsing to handle missing arguments or invalid options.*
- *Memory management:*
	- *Address memory leaks and ensure buffers are appropriately sized.*
- *Readability and Maintainability:*
	- *Replace long `if-else if` chains with more efficient structures like lookup tables.*
	- *Use `const` where appropriate for function parameters that are not modified.*
	- *Add comments to explain complex logic.*
It continued with specific improvements to functions and data structures, fixing each issue and adding memory map entry that made the run time faster and increased  code readability significantly. 
At the end the result was this program that translates Alphabet to morse and back:
```C
morse.c:
#include "morse.h"
#include <ctype.h>
#include <stdio.h>
#include <stdlib.h>
#include <string.h>

#define MAX_INPUT_LINE_LENGTH 2024
#define MAX_OUTPUT_BUFFER_MULTIPLIER 6
#define MAX_OUTPUT_BUFFER_LENGTH \
(MAX_INPUT_LINE_LENGTH * MAX_OUTPUT_BUFFER_MULTIPLIER)

typedef enum { DECRYPT, ENCRYPT } action_type;
typedef struct {
	const char *morse_code;
	char character;
} morse_map_entry;

const morse_map_entry morse_to_char_map[] = {
{A, 'a'}, {B, 'b'}, {C, 'c'}, {D, 'd'}, {E, 'e'},
{F, 'f'}, {G, 'g'}, {H, 'h'}, {I, 'i'}, {J, 'j'},
{K, 'k'}, {L, 'l'}, {M, 'm'}, {N, 'n'}, {O, 'o'},
{P, 'p'}, {Q, 'q'}, {R, 'r'}, {S, 's'}, {T, 't'},
{U, 'u'}, {V, 'v'}, {W, 'w'}, {X, 'x'}, {Y, 'y'},
{Z, 'z'}, {ZERO, '0'}, {ONE, '1'}, {TWO, '2'}, {THREE, '3'},
{FOUR, '4'}, {FIVE, '5'}, {SIX, '6'}, {SEVEN, '7'}, {EIGHT, '8'},
{NINE, '9'}};

typedef struct {
	action_type action;
	char *output_file;
	char *input_file;
} input_args;

int letter_length(const char *input, int start) {
	int counter = 1;
	while (input[start + counter] != ' ' && input[start + counter] != '\0') {
		counter++;
	}
	return counter;
}

char *get_letter(const char *input, int start, int length) {
	if (start >= strlen(input)) {
		return NULL;
	}
	char *letter = (char *)malloc((length + 2) * sizeof(char));
	if (letter == NULL) {
		perror("Failed to allocate memory to letter\n");
		return NULL;
	}
	strncpy(letter, input + start, length);
	letter[length] = ' ';
	letter[length + 1] = '\0';
	return letter;
}

int from_morse(char *input, char *output) {
	const int morse_to_char_map_size = sizeof(morse_to_char_map);
	int start = 0;
	int morse_letter_length = 0;
	int i = 0; // output index
	char *letter_from_morse;
	char decoded_char = 0;
	while (input[start] != '\0' && input[start] != '\n') {
		while (input[start] == ' ') {
		start++;
		}
		if (input[start] == '\0' || input[start] == '\n') {
		break;
		}
		if (input[start] == '|') {
		output[i] = ' ';
		start++;
		i++;
		continue;
		}
		morse_letter_length = letter_length(input, start);
		if (morse_letter_length == 0) {
		if (input[start] != '\0' && input[start] != '\n') {
		start++;
		}
		continue;
		}
		letter_from_morse = get_letter(input, start, morse_letter_length);
		if (letter_from_morse == NULL) {
		output[i] = '\0';
		return i;
		}
		for (int k = 0; k < morse_to_char_map_size; k++) {
		if (strcmp(letter_from_morse, morse_to_char_map[k].morse_code) == 0) {
		decoded_char = morse_to_char_map[k].character;
		break;
		}
		}
		free(letter_from_morse);
		if (decoded_char != 0) {
		output[i++] = decoded_char;
		} else if (letter_from_morse[0] != '\0' && letter_from_morse[0] != ' ') {
		fprintf(stderr, " Warning: Unknown Morse sequence encountered %s\n",
		letter_from_morse);
		}
		start = start + morse_letter_length;
		}
	output[i] = '\0';
	return i;
}
```

I am aware that some of these improvements seems trivial, but do remember I haven't done these type of tasks for a long while and I have lost some of the habits and best practices that I should have known to begin with.
# Conclusion 
If I need to answer the question, would you use NotebookLM from now on? Yes, I would for some things. To be honest, I enjoy the process of learning more than the completion of it. I am driven by my enthusiasm (for better and worse). The biggest strength of this tool for me was the ability to get back on the horse after I lost my initial momentum. For me, this is a real life difficulty that I need to live with and if this tool helps me to get back on a project, with all of the context I had in less than an hour, it is worth it. 
On the technical side, I think this tool provides good feedback when prompt accordingly and the learning slope is not steep. Anyone can takes this and understand how to use rather quickly. 