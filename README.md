#include <stdio.h>
#include <stdlib.h>
#include <time.h>

#define MAX_TICKETS 100

typedef struct {
    int ticketNumber;
    int numbers[6]; // 6 random lottery numbers per ticket
} Ticket;

Ticket tickets[MAX_TICKETS];
int ticketCount = 0;

void generateLotteryNumbers(int numbers[6]) {
    // Generate 6 unique random lottery numbers
    for (int i = 0; i < 6; i++) {
        int num;
        int unique;
        do {
            unique = 1;
            num = rand() % 49 + 1;  // Lottery numbers between 1 and 49
            for (int j = 0; j < i; j++) {
                if (numbers[j] == num) {
                    unique = 0;
                    break;
                }
            }
        } while (!unique);
        numbers[i] = num;
    }
}

void generateTicket(int ticketNumber) {
    tickets[ticketCount].ticketNumber = ticketNumber;
    generateLotteryNumbers(tickets[ticketCount].numbers);
    ticketCount++;
}

void displayTicket(int ticketIndex) {
    printf("Ticket #%d: ", tickets[ticketIndex].ticketNumber);
    for (int i = 0; i < 6; i++) {
        printf("%d ", tickets[ticketIndex].numbers[i]);
    }
    printf("\n");
}

void drawWinningNumbers(int winningNumbers[6]) {
    generateLotteryNumbers(winningNumbers);
    printf("Winning Numbers: ");
    for (int i = 0; i < 6; i++) {
        printf("%d ", winningNumbers[i]);
    }
    printf("\n");
}

void checkWinner(int winningNumbers[6]) {
    for (int i = 0; i < ticketCount; i++) {
        int matchCount = 0;
        for (int j = 0; j < 6; j++) {
            for (int k = 0; k < 6; k++) {
                if (tickets[i].numbers[j] == winningNumbers[k]) {
                    matchCount++;
                    break;
                }
            }
        }
        if (matchCount == 6) {
            printf("Ticket #%d has won! Matching numbers: ", tickets[i].ticketNumber);
            for (int j = 0; j < 6; j++) {
                printf("%d ", tickets[i].numbers[j]);
            }
            printf("\n");
        }
    }
}

int main() {
    srand(time(NULL));  // Initialize random number generator

    int choice, ticketNumber;
    int winningNumbers[6];

    while (1) {
        printf("\nLottery Management System\n");
        printf("1. Buy a Ticket\n");
        printf("2. Draw Winning Numbers\n");
        printf("3. Check Winners\n");
        printf("4. Exit\n");
        printf("Enter your choice: ");
        scanf("%d", &choice);

        switch (choice) {
            case 1:
                printf("Enter your ticket number: ");
                scanf("%d", &ticketNumber);
                generateTicket(ticketNumber);
                printf("Ticket #%d has been purchased!\n", ticketNumber);
                break;
            case 2:
                drawWinningNumbers(winningNumbers);
                break;
            case 3:
                checkWinner(winningNumbers);
                break;
            case 4:
                printf("Exiting Lottery Management System.\n");
                return 0;
            default:
                printf("Invalid choice, try again.\n");
        }
    }

    return 0;
}****
