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





