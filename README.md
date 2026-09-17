# Student-performance-Analyzer
A computer based student academic performance Analyzer using basic problem analyzing tactics and techniques
#include <stdio.h>
#include <string.h>

#define MAX_STUDENTS 50
#define SUBJECTS 5

struct Student
{
    int id;
    char name[50];
    float marks[SUBJECTS];
    float total;
    float percentage;
    char grade;
    int pass;
};

void calculate(struct Student *s)
{
    int i;
    s->total = 0;

    for (i = 0; i < SUBJECTS; i++)
    {
        s->total = s->total + s->marks[i];
    }

    s->percentage = s->total / SUBJECTS;

    if (s->percentage >= 90)
        s->grade = 'A';
    else if (s->percentage >= 80)
        s->grade = 'B';
    else if (s->percentage >= 70)
        s->grade = 'C';
    else if (s->percentage >= 60)
        s->grade = 'D';
    else if (s->percentage >= 50)
        s->grade = 'E';
    else
        s->grade = 'F';

    s->pass = 1;

    for (i = 0; i < SUBJECTS; i++)
    {
        if (s->marks[i] < 40)
        {
            s->pass = 0;
        }
    }
}

void addStudent(struct Student students[], int *count)
{
    int i;

    if (*count >= MAX_STUDENTS)
    {
        printf("\nStudent limit reached!\n");
        return;
    }

    printf("\nEnter Student ID: ");
    scanf("%d", &students[*count].id);

    printf("Enter Student Name: ");
    scanf(" %[^\n]", students[*count].name);

    printf("\nEnter marks for 5 subjects:\n");

    for (i = 0; i < SUBJECTS; i++)
    {
        printf("Subject %d: ", i + 1);
        scanf("%f", &students[*count].marks[i]);

        if (students[*count].marks[i] < 0 ||
            students[*count].marks[i] > 100)
        {
            printf("Invalid marks! Enter again.\n");
            i--;
        }
    }

    calculate(&students[*count]);

    (*count)++;

    printf("\nStudent added successfully!\n");
}

void displayStudent(struct Student s)
{
    printf("\n-----------------------------\n");
    printf("Student ID   : %d\n", s.id);
    printf("Name         : %s\n", s.name);
    printf("Total Marks  : %.2f / 500\n", s.total);
    printf("Percentage   : %.2f%%\n", s.percentage);
    printf("Grade        : %c\n", s.grade);

    if (s.pass == 1)
        printf("Result       : PASS\n");
    else
        printf("Result       : FAIL\n");

    printf("-----------------------------\n");
}

void displayAll(struct Student students[], int count)
{
    int i;

    if (count == 0)
    {
        printf("\nNo students available.\n");
        return;
    }

    printf("\n========== ALL STUDENTS ==========\n");

    for (i = 0; i < count; i++)
    {
        displayStudent(students[i]);
    }
}

void searchStudent(struct Student students[], int count)
{
    int id;
    int i;
    int found = 0;

    printf("\nEnter Student ID to search: ");
    scanf("%d", &id);

    for (i = 0; i < count; i++)
    {
        if (students[i].id == id)
        {
            displayStudent(students[i]);
            found = 1;
            break;
        }
    }

    if (found == 0)
    {
        printf("\nStudent not found.\n");
    }
}

void showRanking(struct Student students[], int count)
{
    struct Student temp;
    int i;
    int j;

    if (count == 0)
    {
        printf("\nNo students available.\n");
        return;
    }

    for (i = 0; i < count - 1; i++)
    {
        for (j = 0; j < count - i - 1; j++)
        {
            if (students[j].percentage < students[j + 1].percentage)
            {
                temp = students[j];
                students[j] = students[j + 1];
                students[j + 1] = temp;
            }
        }
    }

    printf("\n========== STUDENT RANKING ==========\n");

    for (i = 0; i < count; i++)
    {
        printf("Rank %d | ID: %d | Name: %s | Percentage: %.2f%%\n",
               i + 1,
               students[i].id,
               students[i].name,
               students[i].percentage);
    }
}

void classAnalytics(struct Student students[], int count)
{
    int i;
    int passCount = 0;
    int failCount = 0;
    float average = 0;
    int highest = 0;
    int lowest = 0;

    if (count == 0)
    {
        printf("\nNo students available.\n");
        return;
    }

    for (i = 0; i < count; i++)
    {
        average = average + students[i].percentage;

        if (students[i].pass == 1)
            passCount++;
        else
            failCount++;

        if (students[i].percentage > students[highest].percentage)
            highest = i;

        if (students[i].percentage < students[lowest].percentage)
            lowest = i;
    }

    average = average / count;

    printf("\n========== CLASS ANALYTICS ==========\n");
    printf("Total Students : %d\n", count);
    printf("Class Average  : %.2f%%\n", average);
    printf("Passed         : %d\n", passCount);
    printf("Failed         : %d\n", failCount);

    printf("\nHighest Performer:\n");
    printf("%s - %.2f%%\n",
           students[highest].name,
           students[highest].percentage);

    printf("\nLowest Performer:\n");
    printf("%s - %.2f%%\n",
           students[lowest].name,
           students[lowest].percentage);
}

void subjectAnalysis(struct Student students[], int count)
{
    int i;
    int j;
    float average;
    float highestAverage;
    float lowestAverage;
    int highestSubject = 0;
    int lowestSubject = 0;

    if (count == 0)
    {
        printf("\nNo students available.\n");
        return;
    }

    printf("\n========== SUBJECT ANALYSIS ==========\n");

    for (j = 0; j < SUBJECTS; j++)
    {
        average = 0;

        for (i = 0; i < count; i++)
        {
            average = average + students[i].marks[j];
        }

        average = average / count;

        printf("Subject %d Average: %.2f\n", j + 1, average);

        if (j == 0)
        {
            highestAverage = average;
            lowestAverage = average;
        }
        else
        {
            if (average > highestAverage)
            {
                highestAverage = average;
                highestSubject = j;
            }

            if (average < lowestAverage)
            {
                lowestAverage = average;
                lowestSubject = j;
            }
        }
    }

    printf("\nStrongest Subject: Subject %d (%.2f)\n",
           highestSubject + 1,
           highestAverage);

    printf("Weakest Subject: Subject %d (%.2f)\n",
           lowestSubject + 1,
           lowestAverage);
}

void deleteStudent(struct Student students[], int *count)
{
    int id;
    int i;
    int j;
    int found = 0;

    printf("\nEnter Student ID to delete: ");
    scanf("%d", &id);

    for (i = 0; i < *count; i++)
    {
        if (students[i].id == id)
        {
            found = 1;

            for (j = i; j < *count - 1; j++)
            {
                students[j] = students[j + 1];
            }

            (*count)--;

            printf("\nStudent deleted successfully.\n");
            break;
        }
    }

    if (found == 0)
    {
        printf("\nStudent not found.\n");
    }
}

int main()
{
    struct Student students[MAX_STUDENTS];
    int count = 0;
    int choice;

    do
    {
        printf("\n\n====================================\n");
        printf("     STUDENT PERFORMANCE ANALYZER\n");
        printf("====================================\n");
        printf("1. Add Student\n");
        printf("2. View All Students\n");
        printf("3. Search Student\n");
        printf("4. Student Ranking\n");
        printf("5. Class Analytics\n");
        printf("6. Subject Analysis\n");
        printf("7. Delete Student\n");
        printf("8. Exit\n");
        printf("====================================\n");

        printf("Enter your choice: ");
        scanf("%d", &choice);

        switch (choice)
        {
            case 1:
                addStudent(students, &count);
                break;

            case 2:
                displayAll(students, count);
                break;

            case 3:
                searchStudent(students, count);
                break;

            case 4:
                showRanking(students, count);
                break;

            case 5:
                classAnalytics(students, count);
                break;

            case 6:
                subjectAnalysis(students, count);
                break;

            case 7:
                deleteStudent(students, &count);
                break;

            case 8:
                printf("\nThank you for using Student Performance Analyzer!\n");
                break;

            default:
                printf("\nInvalid choice! Please try again.\n");
        }

    } while (choice != 8);

    return 0;
}
