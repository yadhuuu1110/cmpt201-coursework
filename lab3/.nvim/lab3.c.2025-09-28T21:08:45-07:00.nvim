#define _POSIX_C_SOURCE 200809L
#include <stdio.h>
#include <stdlib.h>
#include <string.h>

#define MAX_LINES 5

typedef struct {
  char *array[MAX_LINES];
  int index;
  int count;
} dynamicArray;

void initArray(dynamicArray *rb);
void free_buffer(dynamicArray *rb);
void store_line(dynamicArray *rb, const char *s);
void print_buffer(const dynamicArray *rb);

int main(void) {
  dynamicArray rb;
  initArray(&rb);

  char *in = NULL;
  size_t len = 0;

  while (1) {
    printf("Enter input: ");
    ssize_t nread = getline(&in, &len, stdin);
    if (nread == -1)
      break;

    if (nread > 0 && in[nread - 1] == '\n')
      in[nread - 1] = '\0';

    if (strcmp(in, "print") == 0) {
      print_buffer(&rb);
    } else {
      store_line(&rb, in);
    }
  }

  free(in);
  free_buffer(&rb);
  return 0;
}

void initArray(dynamicArray *rb) {
  for (int i = 0; i < MAX_LINES; i++)
    rb->array[i] = NULL;
  rb->index = 0;
  rb->count = 0;
}

void free_buffer(dynamicArray *rb) {
  for (int i = 0; i < MAX_LINES; i++) {
    free(rb->array[i]);
    rb->array[i] = NULL;
  }
}

void store_line(dynamicArray *rb, const char *s) {
  free(rb->array[rb->index]); // safe if NULL
  rb->array[rb->index] = strdup(s);
  if (!rb->array[rb->index]) {
    perror("strdup");
    exit(EXIT_FAILURE);
  }
  rb->index = (rb->index + 1) % MAX_LINES;
  rb->count++;
}

void print_buffer(const dynamicArray *rb) {
  int total = (rb->count < MAX_LINES) ? rb->count : MAX_LINES;
  int start = (rb->count < MAX_LINES) ? 0 : rb->index;
  for (int i = 0; i < total; i++) {
    int pos = (start + i) % MAX_LINES;
    printf("%s\n", rb->array[pos]);
  }
}
