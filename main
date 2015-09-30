//Grand Lodge at Brian Head Opera room type forecast info consolidator
//Created by Joseph Dec on 9/22/2015

#include <iostream>
#include <string>
#include <fstream>
#include <sstream>

using namespace std;

//Function definitions
void pause();
void showInstructions();
void mainMenu(bool&, int [180][9]);
void convertFile(int [180][9]);
void writeFile(int [180][9]);
int stringToNumber (string);

int main () {
    //Declaring variables and array
    bool exitFlag = false;
    int inventory[180][9] = {0};
    //Array format for room types
    //Month     Day     Year     King     KingView     Queen     QueenView     JuniorSuite     SignatureSuite

    //Main menu
    cout << "Thank you for using the Room Type Forecast info consolidator Ver 1.0!" << endl;
    while (exitFlag == false) {
        mainMenu(exitFlag, inventory);
    }

return 0;
}

//*****Function Definitions*****
void pause() {//Pauses the program until enter is pressed
    cout << "Press enter to continue..." << endl;
    cin.get();
    cin.ignore(0);
}//End function

void showInstructions() {//Displays the instructions for how to operate the program
    cout << "INSTRUCTIONS:" << endl;
    cout << "Step 1: Save a copy of the Room Type Forecast you wish to consolidate." << endl;
    cout << "Step 2: Place the Room Type Forecast in the folder where the program is located." << endl;
    pause();

    cout << "Step 3: After these instructions are done, enter the name of the file." << endl;
    cout << "Final Step: Print the new text file named 'FORECAST.txt'" << endl << endl;

    cout << "See the README file for more information." << endl;
    cout << "Press enter to start the file conversion." << endl << endl;
}//End function

void mainMenu(bool &flag, int inventory1[180][9]) {//Displays the main menu and gathers the user's choice
    string menu = "";
    int choice = 0;
    cout << "Please select an option: " << endl;
    cout << "     1 - Read Instructions" << endl;
    cout << "     2 - Consolidate Info" << endl;
    cout << "     3 - Exit Program" << endl;
    getline(cin, menu, '\n');
    choice = stringToNumber(menu);

    switch(choice) {//Routes user based off of menu choice
    case 1:
        showInstructions();
    case 2:
        convertFile(inventory1);
        writeFile(inventory1);
        flag = true;
        cout << "Your report has been generated, look for the 'FORECAST.txt' file." << endl;
        cout << "Press enter to exit the program...";
        pause();
        break;
    case 3:
        flag = true;
        break;
    default:
        cout << endl;
        cout << "Invalid entry, please try again..." << endl;
        break;
    }//end switch
}//End function

void convertFile(int stats[180][9]) {
    //Declaring variables, arrays, file objects
    ifstream inFile;
    string name = "";
    string line = "";
    string room = "";
    int y = 0;
    int available = 0;

    cout << "Enter in the name of the file you want to consolidate:" << endl;
    do {
        getline(cin, name);
        if (name.find(".txt", 0) > 1000) {//Detecting if file extension was added or not
            name += ".txt";
        }//end if

        //Opening and detecting if file exist
        inFile.open(name.c_str());
        if (inFile.is_open() == false) {
            cout << "Invalid file name, please make sure the file you want to consolidate is in the " << endl;
            cout << "same folder as the program and that you are spelling the name correctly!" << endl;
        }//end if
    } while(inFile.is_open() == false);
    //Populating array
    getline(inFile, line, '\n');
    while (inFile.eof() == false) {
        //Detecting the start of a new daily entry
        if (line.substr(0, 1) == "0" || line.substr(0, 1) == "1") {
                stats[y][0] = stringToNumber(line.substr(0, 2));
                stats[y][1] = stringToNumber(line.substr(3, 2));
                stats[y][2] = stringToNumber(line.substr(6, 2));

                //Accounting for page breaks in text document
                  if (stats[y][0] == stats[y - 1][0] && stats[y][1] == stats[y - 1][1] && stats[y][2] == stats[y - 1][2]) {
                      stats[y][0] = 0;
                      y--;
                  }

                while (line.substr(0, 1) == " " || line.substr(0, 1) == "0" || line.substr(0, 1) == "1") {//Entering room values into array
                    available = stringToNumber(line.substr(105, 6));
                    room = line.substr(20, 6);

                    if (room == "RKT   " || room == "RKS   " || room == "RKSC  " || room == "RKTH  " || room == "RKSHC ") {//Detecting King rooms
                        stats[y][3] += available;
                    }
                    if (room == "RKSV  ") {//Detecting Kings with a View
                        stats[y][4] += available;
                    }
                    if (room == "RQT   " || room == "RQS   " || room == "RQTC  " || room == "RQSC  ") {//Detecting Queen rooms
                        stats[y][5] += available;
                    }
                    if (room == "RQTV  " || room == "RQSV  " || room == "RQTVC " || room == "RQSVC ") {//Detecting Queens with a View
                        stats[y][6] += available;
                    }
                    if (room == "SKSTS " || room == "SKSTTC") {//Detecting Junior Suites
                        stats[y][7] += available;
                    }
                    if (room == "SKSS  " || room == "SKSSC ") {//Detecting Signature Suites
                        stats[y][8] += available;
                    }//end if
                    getline(inFile, line, '\n');
                }//end second while
                y++;
        } else {
            getline(inFile, line, '\n');
        }//end if

    }//end first while
    inFile.close();
}//end function

void writeFile(int stats[180][9]) {
    //Defining file objects and variables
    ofstream outFile;
    int y = 0;
    int lineCount = 0;

    //opening output file
    outFile.open("FORECAST.txt", ios::out);
    //Writing information
    while(stats[y][0] != 0) {
        if ((lineCount % 23) == 0) {//Header for each new page
            outFile << endl << endl;
            if (lineCount > 0) {//Extra line after the first iteration of the heading to control spacing
                outFile << endl;
            }//end if
            outFile << "Date         Kings     KingView    Queens     QueenView    Junior     Signature" << endl;
        } else {
            outFile << endl;
            //Write date
            if (stats[y][0] < 10) {//Adding a '0' before all single digit dates
                outFile << "0";
            }//end if
            outFile << stats[y][0] << "-";
            if (stats[y][1] < 10) {//Adding a '0' before all single digit dates
                outFile << "0";
            }//end if
            outFile << stats[y][1] << "-";
            outFile << stats[y][2] << "      ";

            //Write Kings
            if (stats[y][3] < 10 && stats[y][3] >= 0) {
                outFile << " " << stats[y][3];
            } else {
                outFile << stats[y][3];
            }//end if
            outFile << "         ";

            //Write Kings with a View
            if (stats[y][4] < 10 && stats[y][4] >= 0) {
                outFile << " " << stats[y][4];
            } else {
                outFile << stats[y][4];
            }//end if
            outFile << "          ";

            //Write Queens
            if (stats[y][5] < 10 && stats[y][5] >= 0) {
                outFile << " " << stats[y][5];
            } else {
                outFile << stats[y][5];
            }//end if
            outFile << "          ";

            //Write Queens with a View
            if (stats[y][6] < 10 && stats[y][6] >= 0) {
                outFile << " " << stats[y][6];
            } else {
                outFile << stats[y][6];
            }//end if
            outFile << "          ";

            //Write Junior Suites
            if (stats[y][7] < 10 && stats[y][7] >= 0) {
                outFile << " " << stats[y][7];
            } else {
                outFile << stats[y][7];
            }//end if
            outFile << "          ";

            //Write Signature Suites
            if (stats[y][8] < 10 && stats[y][8] >= 0) {
                outFile << " " << stats[y][8];
            } else {
                outFile << stats[y][8];
            }//end if

            //Moving to next line
            outFile << endl;
            y++;
        }//end if
        lineCount++;
    }//end while
}//end function

int stringToNumber (string word) {//Converts strings to numbers
    int num = 0;
    stringstream convert(word);
    convert >> num;
    return num;
}//end function
