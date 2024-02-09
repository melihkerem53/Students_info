#define _CRT_SECURE_NO_WARNINGS
#include <stdio.h>
#include <stdlib.h>
#include <time.h>
#include <ctype.h>

typedef struct {
	char* name;
	char* surname;
	int number;
	int grades[4];
	double average;
} Students;

void Students_Data(Students* s) {
	printf("name    : %s \n", s->name);
	printf("surname : %s \n", s->surname);
	printf("number  : %d \n", s->number);
	printf("grades  : ");
	for (int j = 0; j < 4; j++) {
		printf("%d ", s->grades[j]);
	}
	printf("\naverage : %.2lf\n\n\n", s->average);
}

void Students_Data_All(void(*a)(Students*), Students*(*b)(int, char* (*)(), int(*)(), int(*)()), int Size, char*(*c)(), int(*d)(), int(*e)(),int(*f)(const void*,const void*))
{
	srand((unsigned int)time(NULL));

	Students* students = b(Size, c, d, e);

	qsort(students, Size, sizeof(Students), f);

	printf("Sorted Students :\n\n");

	for (int i = 0; i < Size; i++) {
		printf("%d.Student\n", i+1);
		a(&students[i]);
	}

	for (int i = 0; i < Size; i++) {
		free(students[i].name);
		free(students[i].surname);
	}
	free(students);
}

char* Random_Word_Generator() {
	int N = rand() % 5 + 5;

	char* Word = (char*)malloc((N + 1) * sizeof(char));

	if (!Word) {
		fprintf(stderr, "Insufficient Memory");
		exit(EXIT_FAILURE);
	}

	for (int i = 0; i < N; i++) {
		Word[i] = rand() % 26 + 'a';
	}
	Word[0] = toupper(Word[0]);
	Word[N] = '\0';

	return Word;
}

int Random_Number_Generator() {
	return rand() % 500 + 1000;
}

int Random_Grades_Generator() {
	return rand() % 100 + 1;
}

Students* Generating_Students_Function(int Size, char*(*a)(), int(*b)(), int(*c)()) 
{
	Students* students = (Students*)malloc(Size * sizeof(Students));

	if (!students) {
		fprintf(stderr, "Insufficient Memory");
		exit(EXIT_FAILURE);
	}

	for (int i = 0; i < Size; i++) {
		students[i].name = a();
		students[i].surname = a();
		students[i].number = b();

		int top = 0;
		for (int j = 0; j < 4; j++) {
			students[i].grades[j] = c();
			top += students[i].grades[j];
		}
		students[i].average = (double)top / 4;
	}

	return students;
}

int Sorting_Average_Grades(const void* a, const void* b) {
	const Students* s1 = (const Students*)a;
	const Students* s2 = (const Students*)b;

	if (s1->average < s2->average) return 1;
	else if (s1->average > s2->average) return -1;
	else return 0;
}

int main() 
{
	int Size;
	printf("How many students are included: ");
	scanf("%d", &Size);

	Students_Data_All(Students_Data, Generating_Students_Function, Size, Random_Word_Generator, Random_Number_Generator, Random_Grades_Generator,Sorting_Average_Grades);

	return 0;
}

