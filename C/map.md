# simplest method
```c
int key[max];
int value[max];
int size=0;
```
----
# clean, minimal version you can copy and use:
```c
#include <stdio.h>
#include <string.h>

#define MAX 100

typedef struct {
    char key[50];
    int value;
} MapEntry;

typedef struct {
    MapEntry entries[MAX];
    int size;
} Map;

// Insert or update key-value
void put(Map *map, const char *key, int value) {
    for (int i = 0; i < map->size; i++) {
        if (strcmp(map->entries[i].key, key) == 0) {
            map->entries[i].value = value; // update existing
            return;
        }
    }
    strcpy(map->entries[map->size].key, key);
    map->entries[map->size].value = value;
    map->size++;
}

// Get value by key
int get(Map *map, const char *key) {
    for (int i = 0; i < map->size; i++) {
        if (strcmp(map->entries[i].key, key) == 0) {
            return map->entries[i].value;
        }
    }
    return -1; // not found
}

int main() {
    Map myMap = {.size = 0};

    put(&myMap, "apple", 10);
    put(&myMap, "banana", 20);
    put(&myMap, "apple", 30); // update

    printf("apple -> %d\n", get(&myMap, "apple"));
    printf("banana -> %d\n", get(&myMap, "banana"));
    printf("mango -> %d\n", get(&myMap, "mango")); // not found
}
```
