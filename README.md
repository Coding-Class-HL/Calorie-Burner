#include <iostream>
#include <fstream>
#include <string>

using namespace std;

const double BIKEMET = 6.0;
const double RUNMET = 9.0;

void CreateUser();
void ViewExistUser();
void EditExistUser();
void StartCalc();
double WorkFormula(double, double);

int main()
{
    char CHOICE[10];
    string line;
    while (CHOICE[0] != 'q') {
        ifstream DATAFILE;
        DATAFILE.open ("Menu.txt", ios::in);
        cout << "\n\n";
        while (getline(DATAFILE, line))
            cout << line << "\n";
        cin >> CHOICE;

        switch (CHOICE[0]) {
            case 'c' : CreateUser(); break;
            case 'v' : cout << "viewing existing user\n"; ViewExistUser(); break;
            case 'e' : cout << "editing existing user\n"; EditExistUser(); break;
            case 's' : cout << "starting calculations\n"; StartCalc(); break;
            case 'q' : cout << "\n\t<Exiting program, have a nice day!>"; break;
            default : "\n\tInvalid Input!";
        }
    }

    return 0;
}

void CreateUser() {
    string userName, userNameFile;
    double weight, calories;

    cout << "\n\tPlease enter your first name: ";
    cin.ignore();
    cin >> userName;

    cout << "\n\tEnter your current weight(lbs): ";
    cin >> weight;

    cout << "\n\tEnter calories to burn: ";
    cin >> calories;

    cout << "\n\t***New user " << userName << " created.\n";
    userNameFile = userName + ".txt";
    ofstream ofile;
    ofile.open(userNameFile);

    ofile << userName << endl;
    ofile << weight << endl;
    ofile << calories << endl;

    ofile.close();

}

void ViewExistUser() {
    string userName, readLine;

    cout << "\n\tEnter user name: ";
    cin.ignore();
    cin >> userName;

    ifstream ifile;
    ifile.open(userName);
    while (ifile >> readLine) {
        if (readLine == userName) {
            cout << "TRUE" << endl;
        }
        else {
            cout << "FALSE" << endl;
        }
    }

    ifile.close();
}

void EditExistUser() {

}

void StartCalc() {
    double weightKG, numCalories, sum;
    char option;
    string userName, userNameFile, readFile, weight, calories;

    cout << "\n\tUse existing user data (Y/N)? ";
    cin >> option;
    option = toupper(option);

    while (option != 'N') {
            cout << "\n\tEnter user name: ";
            cin >> userName;
            userNameFile = userName + ".txt";

            ifstream ifile;
            ifile.open(userNameFile);

            ifile >> readFile;
            userName = readFile;

            ifile >> readFile;
            weight = readFile;

            ifile >> readFile;
            calories = readFile;

            ifile.close();

            cout << "USER: " << userName << endl;
            cout << "WEIGHT: " << weight << endl;
            cout << "CALORIES: " << calories << endl;

            cout << "CORRECT? ";
            cin >> option;
            option = toupper(option);
            if (option == 'Y')
                break;
        }

    //formula
    weightKG = stod(weight) * .4535;
    numCalories = stod(calories);

    sum = WorkFormula(weightKG, numCalories);

    cout << "\n\t\t---EXERCISES to BURN " << numCalories << " CALORIES---" << endl;
    cout << "Bicycle\t" << sum * BIKEMET << endl;
    cout << "Run\t" << sum * RUNMET << endl;
}

double WorkFormula (double weight, double cal){
    double result;
    result = weight / cal;
    return result;

}
