```cpp
#include <iostream>
#include <iomanip>

// Name: Jeremy Tajiri
// Course: CISC 192 - C++ Programming
// Date: 11/27/2025
// Assignment: Inheritance_Activity

// ========================
// bankAccount class
// ========================
class bankAccount {
private:
    int accountNumber = 0;
    double balance = 0;

public:
    bankAccount() = default;
    bankAccount(int accNum, double bal) : accountNumber(accNum), balance(bal) {}

    void setAccountNumber(int accNum) { accountNumber = accNum; }
    int getAccountNumber() const { return accountNumber; }

    double getAccountBalance() const { return balance; }

    void deposit(double amt) { balance += amt; }
    void withdraw(double amt) { balance -= amt; }

    void print() const {
        std::cout << "Account Number: " << accountNumber << std::endl;
        std::cout << "Balance (USD): " << balance << std::endl;
    }
};

// ========================
// checkingAccount class
// ========================
class checkingAccount : public bankAccount {
private:
    double interestRate = 0.0;
    double minBalance = 0.0;
    double serviceCharges = 0.0;

public:
    checkingAccount() = default;
    checkingAccount(int accNum, double bal, double rate, double minBal, double charges)
        : bankAccount(accNum, bal), interestRate(rate), minBalance(minBal), serviceCharges(charges) {}

    void setInterestRate(double rate) { interestRate = rate; }
    double getInterestRate() const { return interestRate; }

    void setMinBalance(double minBal) { minBalance = minBal; }
    double getMinBalance() const { return minBalance; }

    void setServiceCharges(double charges) { serviceCharges = charges; }
    double getServiceCharges() const { return serviceCharges; }

    void postInterest() const {
        double daily = getAccountBalance() * (interestRate / 100.0 / 365.0);
        std::cout << "Your daily interest is: " << daily << std::endl;
    }

    bool verifyBalance() const {
        double bal = getAccountBalance();
        std::cout << "Your balance is: " << bal << std::endl;
        return bal >= minBalance;
    }

    void deposit(double amt) { bankAccount::deposit(amt); }

    void withdraw(double amt) {
        double bal = getAccountBalance();
        if (amt > bal) {
            std::cout << "Not enough balance to withdraw " << amt << std::endl;
            return;
        }
        double fee = 0;
        if (amt > 500) {
            fee = amt * (serviceCharges / 100.0);
            amt += fee;
            std::cout << "Service fee applied: " << fee << std::endl;
        }
        bankAccount::withdraw(amt);
        std::cout << "New balance: " << getAccountBalance() << std::endl;
    }

    void print() const {
        bankAccount::print();
        std::cout << "Interest Rate %: " << interestRate << std::endl;
        std::cout << "Min Balance $: " << minBalance << std::endl;
        std::cout << "Service Charges %: " << serviceCharges << std::endl;
    }
};

// ========================
// savingsAccount class
// ========================
class savingsAccount : public bankAccount {
private:
    double interestRate = 0.0;

public:
    savingsAccount() = default;
    savingsAccount(int accNum, double bal, double rate) {
        bankAccount::setAccountNumber(accNum);
        bankAccount::deposit(bal);
        interestRate = rate;
    }

    void setInterestRate(double rate) { interestRate = rate; }
    double getInterestRate() const { return interestRate; }

    void postInterest() const {
        double daily = getAccountBalance() * (interestRate / 100.0 / 365.0);
        std::cout << "Your daily interest is: " << daily << std::endl;
    }

    void withdraw(double amt) {
        if (amt > getAccountBalance()) {
            std::cout << "Cannot withdraw " << amt << ", not enough funds!\n";
        } else {
            bankAccount::withdraw(amt);
            std::cout << "You withdrew " << amt << ". New balance: " << getAccountBalance() << std::endl;
        }
    }

    void print() const {
        bankAccount::print();
        std::cout << "Interest Rate %: " << interestRate << std::endl;
    }
};

// ========================
// main function
// ========================
int main() {
    int accountType, accountNumber, input;
    double amount, interestRate, minBalance, serviceCharges;

    std::cout << "Select account type (1 = Checking, 2 = Savings): ";
    std::cin >> accountType;

    std::cout << "Enter account number: ";
    std::cin >> accountNumber;
    std::cout << "Enter balance (USD): ";
    std::cin >> amount;
    std::cout << "Enter interest rate (%): ";
    std::cin >> interestRate;

    checkingAccount* cAccount = nullptr;
    savingsAccount* sAccount = nullptr;

    if (accountType == 1) {
        std::cout << "Enter min balance (USD): ";
        std::cin >> minBalance;
        std::cout << "Enter service charges (%): ";
        std::cin >> serviceCharges;
        cAccount = new checkingAccount(accountNumber, amount, interestRate, minBalance, serviceCharges);
    } else if (accountType == 2) {
        sAccount = new savingsAccount(accountNumber, amount, interestRate);
    } else {
        std::cout << "Invalid account type. Exiting...\n";
        return 0;
    }

    do {
        std::cout << "\nMenu:\n1- Deposit\n2- Withdraw\n3- Check balance\n4- Print details\n5- Daily interest\n0- Exit\n";
        std::cin >> input;

        if (cAccount) {
            switch(input) {
                case 0: std::cout << "Goodbye!\n"; break;
                case 1: std::cout << "Deposit amount: "; std::cin >> amount; cAccount->deposit(amount); break;
                case 2:
                    std::cout << "If over $500, service fee applies.\n";
                    std::cout << "Withdraw amount: "; std::cin >> amount;
                    cAccount->withdraw(amount);
                    break;
                case 3: 
                    if (cAccount->verifyBalance()) std::cout << "Balance above minimum.\n";
                    else std::cout << "Balance below minimum.\n";
                    break;
                case 4: cAccount->print(); break;
                case 5: cAccount->postInterest(); break;
                default: std::cout << "Invalid input\n";
            }
        } else if (sAccount) {
            switch(input) {
                case 0: std::cout << "Goodbye!\n"; break;
                case 1: std::cout << "Deposit amount: "; std::cin >> amount; sAccount->deposit(amount); break;
                case 2: std::cout << "Withdraw amount: "; std::cin >> amount; sAccount->withdraw(amount); break;
                case 3:
                    if (sAccount->getAccountBalance() >= 0) std::cout << "Balance positive.\n";
                    else std::cout << "Balance negative.\n";
                    break;
                case 4: sAccount->print(); break;
                case 5: sAccount->postInterest(); break;
                default: std::cout << "Invalid input\n";
            }
        }
    } while (input != 0);

    delete cAccount;
    delete sAccount;

    return 0;
}
