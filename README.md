#include <stdio.h>
#include <stdlib.h>
#include <string.h>
#include <locale.h>
#include <windows.h>
#define MAX_SIZE 100

typedef struct socialnet_t {
    char name[20];
    int age;
    int yeas;
    int numbers;
    char city[20];
};

void searchByName(struct socialnet_t programs[], int size, char Name[]);
void searchByAge(struct socialnet_t programs[], int size, int maxAge);
void searchByCity(struct socialnet_t programs[], int size, char searchCity[]);
void sortByYeas(struct socialnet_t programs[], int size);

int main() {
    struct socialnet_t programs[100];
    int size = 0;
    int choice;
    int kill;
    system("chcp 1251");

    do {
        printf("Меню:\n");
        printf("1) Добавить запись\n");
        printf("2) Поиск по имени пользователя\n");
        printf("3) Поиск по возрасту\n");
        printf("4) Сортировка по дате регистрации\n");
        printf("5) Поиск по названию города\n");
        printf("6) Выход из программы\n");
        printf("Выберите пункт меню: ");
        scanf("%d", &choice);

        switch (choice) {
        case 1:
            if (size < MAX_SIZE) {
                printf("Введите данные для новой программы:\n");
                printf("Имя пользователя: ");
                scanf(" %[^\n]s", programs[size].name);
                printf("Возраст: ");
                scanf("%d", &programs[size].age);
                printf("Выбирите город проживания:\n");
                printf("1-Воронеж\n2-Москва\n3-Санкт-Петербург\n4-Ростов\n:");
                scanf(" %d", &kill);
                switch (kill)
                {
                case 1:
                    strcpy(programs[size].city, "Воронеж");
                    break;
                case 2:
                    strcpy(programs[size].city, "Москва");
                    break;
                case 3:
                    strcpy(programs[size].city, "Санкт-Петербург");
                    break;
                case 4:
                    strcpy(programs[size].city, "Ростов");
                default:
                    printf("Некорректный ввод, введите один из предложенных вариантов");
                }
                printf("Год регистрации: ");
                scanf("%d", &programs[size].yeas);
                printf("Количество друзей: ");
                scanf("%d", &programs[size].numbers);
                size++;
            }
            else {
                printf("База данных полна. Невозможно добавить новую запись.\n");
            }
            break;
        case 2:
            if (size > 0) {
                char Name[10];
                printf("Введите имя пользователя для поиска: ");
                scanf(" %[^\n]s", Name);
                searchByName(programs, size, Name);
            }
            else {
                printf("База данных пуста.\n");
            }
            break;
        case 3:
            if (size > 0) {
                int maxAge;
                printf("Введите возраст для поиска: ");
                scanf("%d", &maxAge);
                searchByAge(programs, size, maxAge);
            }
            else {
                printf("База данных пуста.\n");
            }
            break;
        case 4:
            if (size > 0) {
                sortByYeas(programs, size);
                printf("\nОтсортированные данные по дате регистрации (В порядке убывания):\n");
                for (int i = 0; i < size; ++i) {
                    printf("Имя пользователя: %s, Дата регистрации: %d\n", programs[i].name, programs[i].yeas);
                }
            }
            else {
                printf("База данных пуста.\n");
            }
            break;
        case 5:
            if (size > 0) {
                char searchCity[20];
                printf("Введите название города для поиска: ");
                scanf(" %[^\n]s", &searchCity);
                searchByCity(programs, size, searchCity);
            }
            else {
                printf("База данных пуста. Нечего записывать в файл.\n");
            }
            break;
        case 6:
            printf("Программа завершена.\n");
            break;
        default:
            printf("Некорректный ввод. Пожалуйста, выберите существующий пункт меню.\n");
        }

    } while (choice != 6);
}

void searchByName(struct socialnet_t programs[], int size, char Name[]) {
    printf("\nРезультаты поиска по имени пользователя \"%s\":\n", Name);
    printf("------------------------------------------------\n");
    for (int i = 0; i < size; ++i) {
        if (strstr(programs[i].name, Name) != NULL) {
            printf("Имя пользователя: %s\n", programs[i].name);
            printf("Возраст: %d\n", programs[i].age);
            printf("Город проживания: %s\n", programs[i].city);
            printf("Дата регистрации: %d\n", programs[i].yeas);
            printf("Количество друзей: %d\n", programs[i].numbers);
            printf("------------------------------------------------\n");
        }
    }
}

void searchByAge(struct socialnet_t programs[], int size, int maxAge) {
    printf("Результаты поиска по возрасту до %d:\n", maxAge);
    printf("------------------------------------------------\n");
    for (int i = 0; i < size; ++i) {
        if (programs[i].age <= maxAge) {
            printf("Имя пользователя: %s\n", programs[i].name);
            printf("Возраст: %d\n", programs[i].age);
            printf("Город проживания: %s\n", programs[i].city);
            printf("Дата регистрации: %d\n", programs[i].yeas);
            printf("Количество друзей: %d\n", programs[i].numbers);
            printf("------------------------------------------------\n");
        }
    }
}

void searchByCity(struct socialnet_t programs[], int size, char searchCity[])
{
    printf("\nРезультаты поиска по городу \"%s\":\n", searchCity);
    printf("------------------------------------------------\n");
    for (int i = 0; i < size; ++i) {
        if (strstr(programs[i].city, searchCity) != NULL) {
            printf("Имя пользователя: %s\n", programs[i].name);
            printf("Возраст: %d\n", programs[i].age);
            printf("Город проживания: %s\n", programs[i].city);
            printf("Год регистрации: %d\n", programs[i].yeas);
            printf("Количество друзей: %d\n", programs[i].numbers);
            printf("------------------------------------------------\n");
        }
    }
}

void sortByYeas(struct socialnet_t programs[], int size) {
    for (int i = 0; i < size - 1; ++i) {
        for (int j = 0; j < size - i - 1; ++j) {
            if (programs[j].yeas < programs[j + 1].yeas) {
                struct socialnet_t temp = programs[j];
                programs[j] = programs[j + 1];
                programs[j + 1] = temp;
            }
        }
    }
}
