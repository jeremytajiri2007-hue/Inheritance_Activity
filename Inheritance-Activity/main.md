```cpp

#include <iostream>
#include "checkingAccount.h"
#include "savingsAccount.h"

int main() {
    int accountType, accountNumber, input;
    double amount, interestRate, minBalance, serviceCharges;

    std::cout << "Select account type (1 = Checking, 2 = Savings): ";
    std::cin >> accountType;

    std::cout << "Enter account number: ";
    std::cin >> accountNumber;
    std::cout << "Enter balance (in USD): ";
    std::cin >> amount;
    std::cout << "Enter interest rate (%): ";
    std::cin >> interestRate;

    checkingAccount* cAccount = nullptr;
    savingsAccount* sAccount = nullptr;

    if (accountType == 1) {
        std::cout << "Enter min balance (in USD): ";
        std::cin >> minBalance;
        std::cout << "Enter service charges (%): ";
        std::cin >> serviceCharges;
        cAccount = new checkingAccount(accountNumber, amount, interestRate, minBalance, serviceCharges);
    } else if (accountType == 2) {
        sAccount = new savingsAccount(accountNumber, amount, interestRate);
    } else {
        std::cout << "Invalid account type selected. Exiting." << std::endl;
        return 0;
    }

    do {
        std::cout << "\nWhich service do you want to use?" << std::endl;
        std::cout << "1 <-- Deposit" << std::endl;
        std::cout << "2 <-- Withdraw" << std::endl;
        std::cout << "3 <-- Verify balance" << std::endl;
        std::cout << "4 <-- Print account details" << std::endl;
        std::cout << "5 <-- Print daily interest" << std::endl;
        std::cout << "0 <-- Exit" << std::endl;

        std::cin >> input;

        if (cAccount) {
            switch (input) {
                case 0: std::cout << "Thank you. Goodbye!" << std::endl; break;
                case 1: std::cout << "Deposit amount: "; std::cin >> amount; cAccount->deposit(amount); break;
                case 2: std::cout << "Service charge of " << cAccount->getServiceCharges() 
                                   << "% applies if withdrawal > $500." << std::endl;
                        std::cout << "Enter withdrawal amount: "; std::cin >> amount; 
                        cAccount->withdraw(amount); break;
                case 3: if (cAccount->verifyBalance()) 
                            std::cout << "Your account balance is above the minimum." << std::endl; 
                        else 
                            std::cout << "Your account balance is below the minimum." << std::endl; break;
                case 4: cAccount->print(); break;
                case 5: cAccount->postInterest(); break;
                default: std::cout << "Invalid input." << std::endl;
            }
        } else if (sAccount) {
            switch (input) {
                case 0: std::cout << "Thank you. Goodbye!" << std::endl; break;
                case 1: std::cout << "Deposit amount: "; std::cin >> amount; sAccount->deposit(amount); break;
                case 2: std::cout << "Enter withdrawal amount: "; std::cin >> amount; sAccount->withdraw(amount); break;
                case 3: if (sAccount->getAccountBalance() >= 0) 
                            std::cout << "Your account balance is positive." << std::endl; 
                        else 
                            std::cout << "Your account balance is negative." << std::endl; break;
                case 4: sAccount->print(); break;
                case 5: sAccount->postInterest(); break;
                default: std::cout << "Invalid input." << std::endl;
            }
        }
    } while (input != 0);

    delete cAccount;
    delete sAccount;

    return 0;
}
