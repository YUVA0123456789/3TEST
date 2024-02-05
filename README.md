1st code Employee details c

#include<iostream>
using namespace std;

class EMPLOYEE {
private:
    int Employee_Number;
    string Employee_Name;
    float Basic, DA, IT, Net_Salary;

public:
    void readData() {
        cout << "Enter Employee Number: ";
        cin >> Employee_Number;
        cout << "Enter Employee Name: ";
        cin.ignore(); // to consume the newline character left in the buffer
        getline(cin, Employee_Name);
        cout << "Enter Basic Salary: ";
        cin >> Basic;
    }

    void calculateNetSalary() {
        DA = 0.52 * Basic;
        IT = 0.3 * (Basic + DA);
        Net_Salary = Basic + DA - IT;
    }

    void printData() {
        cout << "\nEmployee Number: " << Employee_Number;
        cout << "\nEmployee Name: " << Employee_Name;
        cout << "\nBasic Salary: " << Basic;
        cout << "\nDearness Allowance (DA): " << DA;
        cout << "\nIncome Tax (IT): " << IT;
        cout << "\nNet Salary: " << Net_Salary << endl;
    }
};

int main() {
    int N;
    cout << "Enter the number of employees: ";
    cin >> N;

    EMPLOYEE employees[N];

    for (int i = 0; i < N; ++i) {
        cout << "\nEnter details for Employee " << i + 1 << ":\n";
        employees[i].readData();
        employees[i].calculateNetSalary();
    }

    cout << "\nEmployee Details and Net Salary:\n";
    for (int i = 0; i < N; ++i) {
        employees[i].printData();
    }

    return 0;
}


2ND CODE THE DATE CALCULATOR

#include <iostream>
using namespace std;

class DATE {
private:
    int day, month, year;

public:
    DATE(int d = 0, int m = 0, int y = 0) : day(d), month(m), year(y) {}

    // Public member functions to set date
    void setDay(int d) { day = d; }
    void setMonth(int m) { month = m; }
    void setYear(int y) { year = y; }

    DATE operator+(int no_of_days) {
        DATE result = *this;
        result.day += no_of_days;

        while (result.day > 30) {
            result.day -= 30;
            result.month++;

            if (result.month > 12) {
                result.month = 1;
                result.year++;
            }
        }

        return result;
    }

    int operator-(const DATE &other) {
        int no_of_days = 0;

        // Calculate the difference in days
        no_of_days += (year - other.year) * 360;
        no_of_days += (month - other.month) * 30;
        no_of_days += (day - other.day);

        return no_of_days;
    }

    friend ostream &operator<<(ostream &os, const DATE &date) {
        os << date.day << "/" << date.month << "/" << date.year;
        return os;
    }
};

int main() {
    DATE d1, d2;
    int day, month, year; // Separate variables for day, month, and year

    // Input first date
    cout << "Enter first date (dd/mm/yy): ";
    cin >> day >> month >> year;
    d1.setDay(day);
    d1.setMonth(month);
    d1.setYear(year);

    // Input second date
    cout << "Enter second date (dd/mm/yy): ";
    cin >> day >> month >> year;
    d2.setDay(day);
    d2.setMonth(month);
    d2.setYear(year);

    // Calculate and display no_of_days using operator
    int no_of_days = d1 - d2;
    cout << "Difference between dates: " << no_of_days << " days" << endl;

    // Add no_of_days to d1 using operator +
    d2 = d1 + no_of_days;
    cout << "New date after adding " << no_of_days << " days: " << d2 << endl;

    return 0;
}

3RD CODE CONVERSION OF DECIMALS TO HEXA 

#include <iostream>
#include <iomanip>

class NUMBER {
protected:
    int value;

public:
    void setValue(int val) {
        value = val;
    }
};

class HEXADECIMAL : public NUMBER {
public:
    void display() {
        std::cout << "Hexadecimal: 0x" << std::hex << std::uppercase << value << std::nouppercase << std::dec << std::endl;
    }
};

class DECIMAL : public HEXADECIMAL {
public:
    // Inherit display() from HEXADECIMAL, but you can override it if needed.
};

class OCTAL : public DECIMAL {
public:
    void display() {
        std::cout << "Octal: 0" << std::oct << value << std::dec << std::endl;
    }
};

int main() {
    OCTAL obj;

    // Set the value using the base class member function
    obj.setValue(255);

    // Display values in different representations
    obj.HEXADECIMAL::display(); // Display hexadecimal value
    obj.display();               // Display decimal value (inherited from DECIMAL)
    obj.OCTAL::display();        // Display octal value

    return 0;
}

4th program
Design three classes’ student test and results, where result inherited 
from test and test is inherited from students. Write possible functions 
to initialize the values. Also write a main function for execution by 
creating objects.

#include <iostream>
#include <string>

class Student {
protected:
    std::string studentName;

public:
    Student(const std::string& name) : studentName(name) {}

    void setStudentName(const std::string& name) {
        studentName = name;
    }

    void displayStudent() const {
        std::cout << "Student Name: " << studentName << std::endl;
    }
};

class Test : public Student {
protected:
    int testScore;

public:
    Test(const std::string& name, int score) : Student(name), testScore(score) {}

    void setTestScore(int score) {
        testScore = score;
    }

    void displayTest() const {
        displayStudent();
        std::cout << "Test Score: " << testScore << std::endl;
    }
};

class Result : public Test {
private:
    char grade;

public:
    Result(const std::string& name, int score, char grade) : Test(name, score), grade(grade) {}

    void setGrade(char newGrade) {
        grade = newGrade;
    }

    void displayResult() const {
        displayTest();
        std::cout << "Grade: " << grade << std::endl;
    }
};

int main() {
    Result studentResult("John Doe", 85, 'A');

    // Displaying the result
    studentResult.displayResult();

    // Changing the test score
    studentResult.setTestScore(92);

    // Changing the grade
    studentResult.setGrade('B');

    // Displaying the updated result
    studentResult.displayResult();

    return 0;
}

 
PROGRAM 5  
 Design a class bank account with current and saving bank account as 
inherited classes. Class bank account should have following data 
members: Account number, Name, Balance Amount, and member 
functions. To initialize the value to deposit and withdrawal amount 
after checking the minimum balance. 

#include <iostream>
#include <string>

class BankAccount {
protected:
    std::string accountNumber;
    std::string accountHolderName;
    double balance;

public:
    BankAccount(const std::string& accNum, const std::string& accHolder, double initialBalance)
        : accountNumber(accNum), accountHolderName(accHolder), balance(initialBalance) {}

    void deposit(double amount) {
        balance += amount;
        std::cout << "Amount " << amount << " deposited. Current balance: " << balance << std::endl;
    }

    virtual void withdraw(double amount) {
        if (balance - amount >= 0) {
            balance -= amount;
            std::cout << "Amount " << amount << " withdrawn. Current balance: " << balance << std::endl;
        } else {
            std::cout << "Insufficient funds. Withdrawal not allowed." << std::endl;
        }
    }

    void displayAccountDetails() const {
        std::cout << "Account Number: " << accountNumber << std::endl;
        std::cout << "Account Holder: " << accountHolderName << std::endl;
        std::cout << "Balance: " << balance << std::endl;
    }
};

class CurrentAccount : public BankAccount {
public:
    CurrentAccount(const std::string& accNum, const std::string& accHolder, double initialBalance)
        : BankAccount(accNum, accHolder, initialBalance) {}

    void withdraw(double amount) override {
        // No minimum balance check for Current Account
        balance -= amount;
        std::cout << "Amount " << amount << " withdrawn from Current Account. Current balance: " << balance << std::endl;
    }
};

class SavingsAccount : public BankAccount {
public:
    SavingsAccount(const std::string& accNum, const std::string& accHolder, double initialBalance)
        : BankAccount(accNum, accHolder, initialBalance) {}

    void withdraw(double amount) override {
        // Minimum balance check for Savings Account
        const double minimumBalance = 500.0;

        if (balance - amount >= minimumBalance) {
            balance -= amount;
            std::cout << "Amount " << amount << " withdrawn from Savings Account. Current balance: " << balance << std::endl;
        } else {
            std::cout << "Withdrawal not allowed. Minimum balance not maintained." << std::endl;
        }
    }
};

int main() {
    CurrentAccount currentAcc("C123", "John Doe", 1000.0);
    SavingsAccount savingsAcc("S456", "Jane Smith", 1500.0);

    currentAcc.deposit(500.0);
    currentAcc.withdraw(200.0);
    currentAcc.displayAccountDetails();

    std::cout << "--------------------------" << std::endl;

    savingsAcc.deposit(800.0);
    savingsAcc.withdraw(1000.0);
    savingsAcc.displayAccountDetails();

    return 0;
}

PROGRAM 6  
Write a C++ program for desining a caluculator for addition, 
subtraction, multiplication,  
and division.

#include <iostream>

using namespace std;

class Calculator {
public:
    double add(double a, double b) {
        return a + b;
    }

    double subtract(double a, double b) {
        return a - b;
    }

    double multiply(double a, double b) {
        return a * b;
    }

    double divide(double a, double b) {
        if (b != 0) {
            return a / b;
        } else {
            cout << "Error: Division by zero!" << endl;
            return 0.0;
        }
    }
};

int main() {
    Calculator calculator;

    double num1, num2;
    char operation;

    cout << "Enter first number: ";
    cin >> num1;

    cout << "Enter operation (+, -, *, /): ";
    cin >> operation;

    cout << "Enter second number: ";
    cin >> num2;

    switch (operation) {
        case '+':
            cout << "Result: " << calculator.add(num1, num2) << endl;
            break;
        case '-':
            cout << "Result: " << calculator.subtract(num1, num2) << endl;
            break;
        case '*':
            cout << "Result: " << calculator.multiply(num1, num2) << endl;
            break;
        case '/':
            cout << "Result: " << calculator.divide(num1, num2) << endl;
            break;
        default:
            cout << "Invalid operation!" << endl;
    }

    return 0;
}



