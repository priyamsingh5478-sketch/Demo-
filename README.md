// Demo
// task 1
#include <stdio.h>

int main() {
    float num1, num2, result;
    int choice;
    printf("Enter first number: ");
    scanf("%f", &num1);
    printf("Enter second number: ");
    scanf("%f", &num2);
    printf("\nSelect an operation:\n");
    printf("1. Addition\n");
    printf("2. Subtraction\n");
    printf("3. Multiplication\n");
    printf("4. Division\n");
    printf("Enter your choice (1-4): ");
    scanf("%d", &choice);
    switch(choice) {
        case 1:
            result = num1 + num2;
            printf("Result = %.2f", result);
            break;
        case 2:
            result = num1 - num2;
            printf("Result = %.2f", result);
            break;
        case 3:
            result = num1 * num2;
            printf("Result = %.2f", result);
            break;
        case 4:
            if(num2 != 0)
                printf("Result = %.2f", num1 / num2);
            else
                printf("Error! Division by zero is not allowed.");
            break;
        default:
            printf("Invalid choice!");
    }
    return 0;
}




//Task 3

#include <stdio.h>
#include <stdlib.h>
#include <string.h>

struct Student {
    int roll;
    char name[50];
    float marks;
};

void addStudent();
void displayStudents();
void searchStudent();
void deleteStudent();
void updateStudent();

int main() {
    int choice;
    while(1) {
        printf("\n===== Student Management System =====\n");
        printf("1. Add Student\n");
        printf("2. Display Students\n");
        printf("3. Search Student\n");
        printf("4. Update Student\n");
        printf("5. Delete Student\n");
        printf("6. Exit\n");
        printf("Enter your choice: ");
        scanf("%d", &choice);
        switch(choice) {
            case 1:
                addStudent();
                break;
            case 2:
                displayStudents();
                break;
            case 3:
                searchStudent();
                break;
            case 4:
                updateStudent();
                break;
            case 5:
                deleteStudent();
                break;
            case 6:
                printf("Exiting Program...\n");
                exit(0);
            default:
                printf("Invalid Choice!\n");
        }
    }

  return 0;
}

// Add Student
void addStudent() {
    FILE *fp;
    struct Student s;

  fp = fopen("students.dat", "ab");
    printf("Enter Roll No: ");
    scanf("%d", &s.roll);
    printf("Enter Name: ");
    scanf("%s", s.name);
    printf("Enter Marks: ");
    scanf("%f", &s.marks);
    fwrite(&s, sizeof(s), 1, fp);
    fclose(fp);
    printf("Student Record Added Successfully!\n");
}

// Display Students
void displayStudents() {
    FILE *fp;
    struct Student s;
    fp = fopen("students.dat", "rb");
    if(fp == NULL) {
        printf("No Records Found!\n");
        return;
    }
    printf("\n--- Student Records ---\n");
    while(fread(&s, sizeof(s), 1, fp)) {
        printf("Roll: %d\n", s.roll);
        printf("Name: %s\n", s.name);
        printf("Marks: %.2f\n", s.marks);
        printf("----------------------\n");
    }

  fclose(fp);
}

// Search Student
void searchStudent() {
    FILE *fp;
    struct Student s;
    int roll, found = 0;
    fp = fopen("students.dat", "rb");
    printf("Enter Roll No to Search: ");
    scanf("%d", &roll);
    while(fread(&s, sizeof(s), 1, fp)) {
        if(s.roll == roll) {
            printf("\nRecord Found!\n");
            printf("Roll: %d\n", s.roll);
            printf("Name: %s\n", s.name);
            printf("Marks: %.2f\n", s.marks);
            found = 1;
            break;
        }
    }

  if(!found)
        printf("Record Not Found!\n");

   fclose(fp);
}

// Update Student
void updateStudent() {
    FILE *fp;
    struct Student s;
    int roll, found = 0;

   fp = fopen("students.dat", "rb+");
    printf("Enter Roll No to Update: ");
    scanf("%d", &roll);
    while(fread(&s, sizeof(s), 1, fp)) {
        if(s.roll == roll) {
            printf("Enter New Name: ");
            scanf("%s", s.name);
            printf("Enter New Marks: ");
            scanf("%f", &s.marks);
            fseek(fp, -sizeof(s), SEEK_CUR);
            fwrite(&s, sizeof(s), 1, fp);
            printf("Record Updated Successfully!\n");
            found = 1;
            break;
        }
    }

   if(!found)
        printf("Record Not Found!\n");

  fclose(fp);
}

// Delete Student
void deleteStudent() {
    FILE *fp, *temp;
    struct Student s;
    int roll, found = 0;
    fp = fopen("students.dat", "rb");
    temp = fopen("temp.dat", "wb");
    printf("Enter Roll No to Delete: ");
    scanf("%d", &roll);
    while(fread(&s, sizeof(s), 1, fp)) {
        if(s.roll == roll) {
            found = 1;
        } else {
            fwrite(&s, sizeof(s), 1, temp);
        }
    }

  fclose(fp);
    fclose(temp);
    remove("students.dat");
    rename("temp.dat", "students.dat");
    if(found)
        printf("Record Deleted Successfully!\n");
    else
        printf("Record Not Found!\n");
}

