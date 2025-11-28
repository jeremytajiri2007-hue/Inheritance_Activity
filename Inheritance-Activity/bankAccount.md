```cpp

#include "bankAccount.h"
#include <iostream>

bankAccount::bankAccount() = default;

bankAccount::bankAccount(int accountNumber, double balance)
    : accountNumber(accountNumber), balance(balance) {}

void bankAccount::setAccountNumber(int accountNumber) {
    this->accountNumber = accountNumber;
}

int bankAccount::getAccountNumber() const {
    return accountNumber;
}

double bankAccount::getAccountBalance() const {
    return balance;
}

void bankAccount::deposit(double amount) {
    balance += amount;
}

void bankAccount::withdraw(double amount) {
    balance -= amount;
}

void bankAccount::print() const {
    std::cout << "Account Number: " << accountNumber << std::endl;
    std::cout << "Balance (USD): " << balance << std::endl;
}
