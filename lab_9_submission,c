#include <stdio.h>
#include <stdlib.h>

// RecordType
struct RecordType
{
    int     id;
    char    name;
    int     order;
};

// Fill out this structure
#define HASH_SIZE 10
struct HashType
{
    struct RecordType *record;
    int occupied;
};

// Compute the hash function
int hash(int x)
{
    return x % HASH_SIZE;
}

// Parses input file to an array of struct RecordType
int parseData(char* inputFileName, struct RecordType** ppData)
{
    FILE* inFile = fopen(inputFileName, "r");
    int dataSz = 0;
    int i, n;
    char c;
    struct RecordType *pRecord;
    *ppData = NULL;

    if (inFile)
    {
        fscanf(inFile, "%d\n", &dataSz);
        *ppData = (struct RecordType*) malloc(sizeof(struct RecordType) * dataSz);
        if (*ppData == NULL)
        {
            printf("Cannot allocate memory\n");
            exit(-1);
        }
        for (i = 0; i < dataSz; ++i)
        {
            pRecord = *ppData + i;
            fscanf(inFile, "%d ", &n);
            pRecord->id = n;
            fscanf(inFile, "%c ", &c);
            pRecord->name = c;
            fscanf(inFile, "%d ", &n);
            pRecord->order = n;
        }

        fclose(inFile);
    }

    return dataSz;
}

// Prints the records
void printRecords(struct RecordType pData[], int dataSz)
{
    int i;
    printf("\nRecords:\n");
    for (i = 0; i < dataSz; ++i)
    {
        printf("\t%d %c %d\n", pData[i].id, pData[i].name, pData[i].order);
    }
    printf("\n\n");
}

// Display records in the hash structure
void displayRecordsInHash(struct HashType *pHashArray, int hashSz)
{
    int i;

    printf("\nHash Table:\n");
    for (i = 0; i < hashSz; ++i)
    {
        if (pHashArray[i].occupied)
        {
            printf("Index %d -> %d %c %d\n", i, pHashArray[i].record->id, pHashArray[i].record->name, pHashArray[i].record->order);
        }
    }
    printf("\n\n");
}

int main(void)
{
    struct RecordType *pRecords;
    int recordSz = 0;

    recordSz = parseData("input.txt", &pRecords);
    printRecords(pRecords, recordSz);

    // Initialize hash table
    struct HashType hashTable[HASH_SIZE];
    int i;
    for (i = 0; i < HASH_SIZE; ++i)
    {
        hashTable[i].record = NULL;
        hashTable[i].occupied = 0;
    }

    // Populate the hash table with records
    for (i = 0; i < recordSz; ++i)
    {
        int h = hash(pRecords[i].id);
        if (!hashTable[h].occupied) {
            hashTable[h].record = &pRecords[i];
            hashTable[h].occupied = 1;
        }
        else {
            // Handle collision - simple linear probing
            int j = (h + 1) % HASH_SIZE;
            while (j != h) {
                if (!hashTable[j].occupied) {
                    hashTable[j].record = &pRecords[i];
                    hashTable[j].occupied = 1;
                    break;
                }
                j = (j + 1) % HASH_SIZE;
            }
            if (j == h) {
                printf("Hash table overflow! Unable to insert record with id %d.\n", pRecords[i].id);
            }
        }
    }

    // Display the records stored in the hash table
    displayRecordsInHash(hashTable, HASH_SIZE);

    // Free allocated memory
    free(pRecords);

    return 0;
}
