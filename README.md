%{
#include <stdio.h>
#include <string.h>

#define MAX_WORD_LENGTH 100

char smallest_word[MAX_WORD_LENGTH];
int smallest_length = MAX_WORD_LENGTH;
%}

%%

// Match words (alphanumeric sequences)
[a-zA-Z0-9]+ {
    int length = yyleng;
    if (length < smallest_length) {
        smallest_length = length;
        strncpy(smallest_word, yytext, length);
        smallest_word[length] = '\0'; // Null-terminate the string
    }
}

// Ignore whitespace and punctuation
[ \t\n]+  { /* Ignore whitespace */ }
.         { /* Ignore other characters */ }

%%

// Main function
int main(int argc, char **argv) {
    yylex(); // Start the lexical analysis
    if (smallest_length == MAX_WORD_LENGTH) {
        printf("No words found in the input.\n");
    } else {
        printf("The smallest word is: '%s'\n", smallest_word);
    }
    return 0;
}
