#include <iostream>
using namespace std;
#define MAX 10

struct Username
{
    string email;
    string password;
    int balance;
};

struct Collection
{
    Username data[MAX];
    int last;
    Collection() : last(-1) {
        for (int i = 0; i < MAX; i++) {
            data[i].balance = 0;
        }
    };
};

class Datauser
{
private:
    Collection C; // NOTE: call the struct and add name like "U", to use it privately
    int locate(string name, string pin);
    bool duplicate(string name);
    void withdrawMoney(int index);
    void depositMoney(int index);
    int menu(int index);

public:
    void signUp(string name, string pass);
    void logIn(string name, string pass);
};

int Datauser ::menu(int index)
{
    while (true)
    {
        int userChoice;
        cout << "1. Check Balance " << endl;
        cout << "2. Withdraw " << endl;
        cout << "3. Depostit " << endl;
        cout << "4. Log-out " << endl;
        cout << "Please Select from 1-4: " << endl;
        cin >> userChoice;

        switch (userChoice)
        {
        case 1:
            cout << "Your current balance is: " << C.data[index].balance << endl;
            break;
        case 2:
            withdrawMoney(index);
            break;
        case 3:
            depositMoney(index);
            break;
        case 4:
            cout << "You have been logged-out." << endl;
            return 0;
        default:
            cout << "Invalid choice, please choose from 1-4; ";
            break;
        }
    }
}

int mainMenu()
{
    int choice;
    cout << "Welcome to PH Bank" << endl;
    cout << "1. Log-in" << endl;
    cout << "2. Sign-up" << endl;
    cout << "3. Exit" << endl;
    cout << "Please Select from 1-3:" << endl;
    cin >> choice;
    return choice;
}

int Datauser ::locate(string name, string pin)
{
    for (int i = 0; i <= C.last; i++)
    {
        if (C.data[i].email == name && C.data[i].password == pin)
        {
            return i;
        }
    }
    return -1;
}

bool Datauser::duplicate(string name)
{
    for (int i = 0; i <= C.last; i++)
    {
        if (C.data[i].email == name)
            return true;
    }
    return false;
}

void Datauser ::withdrawMoney(int index)
{
    int w;
    cout << "Your balance is: " << C.data[index].balance << endl;
    cout << "How much do you want to withdraw?: ";
    cin >> w;
    if (w > C.data[index].balance)
    {
        cout << "There's not enough balance.";
        system("pause");
    }
    else
    {
        C.data[index].balance -= w;
        cout << "Withdrawal successful! New balance: " << C.data[index].balance << endl;
    }
}

void Datauser ::depositMoney(int index)
{
    int p;
    cout << "Your balance is: " << C.data[index].balance << endl;
    cout << "How much do you want to deposit?: ";
    cin >> p;
    C.data[index].balance += p;
    cout << "Deposit successful! New balance: " << C.data[index].balance << endl;
}

void Datauser::signUp(string name, string pass)
{
    if (duplicate(name))
    {
        cout << "Email already exist.\n";
        return;
    }

    int index = locate(name, pass);
    if (index != -1)
    {
        cout << "Welcome back " << name << "!\n";
        menu(index);
    } 
    else {
        C.last++;
        C.data[C.last].email = name;
        C.data[C.last].password = pass;
        C.data[C.last].balance = 0;
        cout << "Thank you for signing up " << name << "!\n";
        menu(C.last);
    }   
}

void Datauser::logIn(string name, string pass)
{
    int index = locate(name, pass);
    if (index != -1)
    {
        cout << "Welcome back " << name << "!\n";
        menu(index);
    }
    else
    {
        cout << "Invalid login.\n";
    }
}

int main()
{
    Datauser user;
    string nm;
    string pin;
    while (true)
    {
        switch (mainMenu())
        {
        case 1:
            cout << "Enter your account name: ";
            cin.ignore();
            getline(cin, nm);
            cout << "Enter your password: ";
            getline(cin, pin);
            user.logIn(nm, pin);
            break;
        case 2:
            cout << "Enter your account name: ";
            cin.ignore();
            getline(cin, nm);
            cout << "Enter your password: ";
            getline(cin, pin);
            user.signUp(nm, pin);
            break;
        case 3:
            exit(0);
        default:
            cout << "Invalid choice." << endl;
            break;
        }
    }
    return 0;
}
