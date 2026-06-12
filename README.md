#include <stdio.h>
#include <stdlib.h>

struct BankAccount
{
    int accNo;
    char name[50];
    float balance;
};

void deposit(struct BankAccount *acc)
{
    float amount;
    printf("\nEnter amount to deposit: ");
    scanf("%f", &amount);

    acc->balance += amount;
    printf("Deposit Successful!\n");
    printf("Updated Balance = %.2f\n", acc->balance);
}

void withdraw(struct BankAccount *acc)
{
    float amount;
    printf("\nEnter amount to withdraw: ");
    scanf("%f", &amount);

    if (amount > acc->balance)
    {
        printf("Insufficient Balance!\n");
    }
    else
    {
        acc->balance -= amount;
        printf("Withdrawal Successful!\n");
        printf("Remaining Balance = %.2f\n", acc->balance);
    }
}

void balanceEnquiry(struct BankAccount acc)
{
    printf("\n----- Account Details -----\n");
    printf("Account Number : %d\n", acc.accNo);
    printf("Account Holder : %s\n", acc.name);
    printf("Current Balance: %.2f\n", acc.balance);
}

void saveToFile(struct BankAccount acc)
{
    FILE *fp;

    fp = fopen("bankdata.txt", "w");

    if (fp == NULL)
    {
        printf("File Error!\n");
        return;
    }

    fprintf(fp, "%d\n%s\n%.2f",
            acc.accNo,
            acc.name,
            acc.balance);

    fclose(fp);
}

int main()
{
    struct BankAccount acc;

    int choice;

    printf("===== BANK ACCOUNT MANAGEMENT SYSTEM =====\n");

    printf("Enter Account Number: ");
    scanf("%d", &acc.accNo);

    printf("Enter Account Holder Name: ");
    scanf(" %[^\n]", acc.name);

    printf("Enter Initial Balance: ");
    scanf("%f", &acc.balance);

    do
    {
        printf("\n");
        printf("\n===== MENU =====\n");
        printf("1. Deposit\n");
        printf("2. Withdraw\n");
        printf("3. Balance Enquiry\n");
        printf("4. Exit\n");

        printf("Enter your choice: ");
        scanf("%d", &choice);

        switch (choice)
        {
        case 1:
            deposit(&acc);
            saveToFile(acc);
            break;

        case 2:
            withdraw(&acc);
            saveToFile(acc);
            break;

        case 3:
            balanceEnquiry(acc);
            break;

        case 4:
            saveToFile(acc);
            printf("\nThank You for Using Banking System!\n");
            break;

        default:
            printf("Invalid Choice!\n");
        }

    } while (choice != 4);

    return 0;
}
