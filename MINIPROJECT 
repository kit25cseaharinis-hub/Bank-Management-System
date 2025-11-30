#include <stdio.h>
#include <string.h>
#include <stdlib.h>
#include <ctype.h>
#include <time.h>

FILE *fp;

struct Bank {
    char name[50];
    long long acc;
    float bal;
    int pin;
} a[100];

int c = 0;

/* -----------------------------------------------------------
   LETTER-ONLY STRING (for Name)
----------------------------------------------------------- */
void safeStringLettersOnly(char *str, int size) {
    while (1) {
        fgets(str, size, stdin);
        str[strcspn(str, "\n")] = '\0';

        int ok = 1;

        if (strlen(str) == 0)
            ok = 0;

        for (int i = 0; str[i]; i++) {
            if (!isalpha(str[i]) && str[i] != ' ') {
                ok = 0;
                break;
            }
        }

        if (ok) return;

        printf("Invalid! Enter LETTERS only: ");
    }
}

/* -----------------------------------------------------------
   DIGIT-ONLY INTEGER
----------------------------------------------------------- */
int safeDigitsOnlyInt() {
    char buf[50];

    while (1) {
        fgets(buf, sizeof(buf), stdin);
        buf[strcspn(buf, "\n")] = 0;

        int ok = 1;

        if (strlen(buf) == 0) ok = 0;

        for (int i = 0; buf[i]; i++) {
            if (!isdigit(buf[i])) {
                ok = 0;
                break;
            }
        }

        if (ok) return atoi(buf);

        printf("Invalid input! Enter INTEGERS ONLY: ");
    }
}

/* -----------------------------------------------------------
   DIGIT-ONLY LONG LONG
----------------------------------------------------------- */
long long safeDigitsOnlyLL() {
    char buf[50];

    while (1) {
        fgets(buf, sizeof(buf), stdin);
        buf[strcspn(buf, "\n")] = 0;

        int ok = 1;

        if (strlen(buf) == 0) ok = 0;

        for (int i = 0; buf[i]; i++) {
            if (!isdigit(buf[i])) {
                ok = 0;
                break;
            }
        }

        if (ok) return atoll(buf);

        printf("Invalid input! Enter DIGITS ONLY: ");
    }
}

/* -----------------------------------------------------------
   STRICT POSITIVE FLOAT
----------------------------------------------------------- */
float safeFloatPositive() {
    char buf[100];

    while (1) {
        fgets(buf, sizeof(buf), stdin);
        buf[strcspn(buf, "\n")] = '\0';

        int len = strlen(buf);
        int ok = 1, dotCount = 0;

        if (len == 0)
            ok = 0;

        for (int i = 0; buf[i]; i++) {
            if (buf[i] == '.') {
                dotCount++;
                if (dotCount > 1) { ok = 0; break; }
            }
            else if (!isdigit(buf[i])) {
                ok = 0;
                break;
            }
        }

        if (!ok) {
            printf("Invalid amount! Enter a POSITIVE number: ");
            continue;
        }

        float val = atof(buf);
        if (val > 0)
            return val;

        printf("Amount must be POSITIVE: ");
    }
}

/* -----------------------------------------------------------
   FILE SAVE / LOAD
----------------------------------------------------------- */
void update() {
    fp = fopen("BMS.txt", "w");

    for (int i = 0; i < c; i++) {
        fprintf(fp, "%s\n%lld\n%.2f\n%d\n",
            a[i].name, a[i].acc, a[i].bal, a[i].pin);
    }

    fclose(fp);
}

void load() {
    fp = fopen("BMS.txt", "r");
    if (!fp) return;

    while (fscanf(fp, "%[^\n]\n%lld\n%f\n%d\n",
                  a[c].name, &a[c].acc, &a[c].bal, &a[c].pin) == 4) {
        c++;
    }

    fclose(fp);
}

/* -----------------------------------------------------------
   FIXED ACCOUNT NUMBER GENERATOR
----------------------------------------------------------- */
long long generateAcc() {
    long long x = ((long long)rand() << 32) ^ rand();
    x = llabs(x) % 9000000000LL + 1000000000LL; // 10-digit number
    return x;
}

/* -----------------------------------------------------------
   CREATE ACCOUNT
----------------------------------------------------------- */
void createAcc() {
    printf("Enter Name: ");
    safeStringLettersOnly(a[c].name, sizeof(a[c].name));

    while (1) {
        printf("Set 4-digit PIN: ");
        a[c].pin = safeDigitsOnlyInt();

        if (a[c].pin >= 1000 && a[c].pin <= 9999)
            break;

        printf("PIN must be exactly 4 digits.\n");
    }

    while (1) {
        printf("Initial Deposit (>= 500): ");
        a[c].bal = safeFloatPositive();

        if (a[c].bal >= 500 && a[c].bal <= 500000) break;

        printf("Deposit must be minimum 500 and maximum 5,00,000.\n");
    }

    a[c].acc = generateAcc();
    update();

    printf("\nAccount Created Successfully!\n");
    printf("Name: %s\nAccount No: %lld\nBalance: %.2f\nPIN: %d\n\n",
           a[c].name, a[c].acc, a[c].bal, a[c].pin);

    c++;
}

/* -----------------------------------------------------------
   DEPOSIT
----------------------------------------------------------- */
void deposit() {
    printf("Enter Account No: ");
    long long acc = safeDigitsOnlyLL();

    printf("Enter PIN: ");
    int pin = safeDigitsOnlyInt();

    for (int i = 0; i < c; i++) {
        if (a[i].acc == acc && a[i].pin == pin) {

            float amt;
            while (1) {
                printf("Enter deposit amount (>=500): ");
                amt = safeFloatPositive();
                if (amt >= 500 && amt <= 500000) break;
                printf("Deposit must be minimum 500 and maximum 5,00,000.\n");
            }

            a[i].bal += amt;
            update();

            printf("Deposit Successful! New Balance: %.2f\n\n", a[i].bal);
            return;
        }
    }

    printf("Invalid Account or PIN!\n");
}

/* -----------------------------------------------------------
   WITHDRAW
----------------------------------------------------------- */
void withdraw() {
    printf("Enter Account No: ");
    long long acc = safeDigitsOnlyLL();

    printf("Enter PIN: ");
    int pin = safeDigitsOnlyInt();

    for (int i = 0; i < c; i++) {
        if (a[i].acc == acc && a[i].pin == pin) {
			float amt;
            printf("Enter withdrawal amount: ");
            amt = safeFloatPositive();
				
           	if (amt > a[i].bal) {
           	    printf("Insufficient balance!\n");
           	    return;
           	}
            a[i].bal -= amt;
            update();

            printf("Withdrawal Successful! New Balance: %.2f\n\n", a[i].bal);
            return;
        }
    }

    printf("Invalid Account or PIN!\n");
}

/* -----------------------------------------------------------
   CHECK BALANCE
----------------------------------------------------------- */
void checkBalance() {
    printf("Enter Account No: ");
    long long acc = safeDigitsOnlyLL();

    printf("Enter PIN: ");
    int pin = safeDigitsOnlyInt();

    for (int i = 0; i < c; i++) {
        if (a[i].acc == acc && a[i].pin == pin) {
            printf("\nName: %s\nBalance: %.2f\n\n", a[i].name, a[i].bal);
            return;
        }
    }

    printf("Invalid Account or PIN!\n");
}

/* -----------------------------------------------------------
   MAIN MENU (FIX: srand() moved here)
----------------------------------------------------------- */
int main() {
    srand(time(NULL));   // FIXED — initialize random only once

    load();
    int choice;

    while (1) {
        printf("------ BANK SYSTEM ------\n");
        printf("1. Create Account\n");
        printf("2. Deposit Money (>500)\n");
        printf("3. Withdraw Money\n");
        printf("4. Check Balance\n");
        printf("5. Exit\n");
        printf("Enter choice: ");

        choice = safeDigitsOnlyInt();

        switch (choice) {
            case 1: createAcc(); break;
            case 2: deposit(); break;
            case 3: withdraw(); break;
            case 4: checkBalance(); break;
            case 5: printf("Exiting...\n"); return 0;
            default: printf("Invalid choice! Try again.\n");
        }
    }
}
