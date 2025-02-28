#include <iostream>
#include <fstream>
#include <string>
using namespace std;
 
const int MIN_CHAR = 32;
const int CHAR_RANGE = 95;
 
string encrypt(string plaintext, string key) {
    string ciphertext;
    int keyLen = key.length();
    
    for(int i=0; i<plaintext.length(); ++i) {
        int plainCode = plaintext[i] - MIN_CHAR;
        int keyCode = key[i % keyLen] - MIN_CHAR;
        ciphertext += (char)((plainCode + keyCode) % CHAR_RANGE + MIN_CHAR);
    }
    return ciphertext;
}
 
string decrypt(string ciphertext, string key) {
    string plaintext;
    int keyLen = key.length();
    
    for(int i=0; i<ciphertext.length(); ++i) {
        int cipherCode = ciphertext[i] - MIN_CHAR;
        int keyCode = key[i % keyLen] - MIN_CHAR;
        plaintext += (char)((cipherCode - keyCode + CHAR_RANGE) % CHAR_RANGE + MIN_CHAR);
    }
    return plaintext;
}
 
void processFile(string inputFile, string outputFile, string key, bool encryptMode) {
    ifstream fin(inputFile);
    ofstream fout(outputFile);
    string content((istreambuf_iterator<char>(fin)), istreambuf_iterator<char>());
    
    if(key.empty()) {
        cerr << "Error: Empty key detected!";
        return;
    }
    
    fout << (encryptMode ? encrypt(content, key) : decrypt(content, key));
    cout << "File processed successfully. Output: " << outputFile << endl;
}
 
void displayMenu() {
    cout << "\n==== Vigenère Cipher ====" 
         << "\n1. Encrypt text"
         << "\n2. Decrypt text"
         << "\n3. Encrypt file"
         << "\n4. Exit"
         << "\nSelect option: ";
}
 
int main() {
    int choice;
    string text, key, inFile, outFile;
 
    while(true) {
        displayMenu();
        cin >> choice;
        cin.ignore();
 
        switch(choice) {
            case 1: {
                cout << "Enter text: ";
                getline(cin, text);
                cout << "Enter key: ";
                getline(cin, key);
                cout << "Result: " << encrypt(text, key) << endl;
                break;
            }
            case 2: {
                cout << "Enter ciphertext: ";
                getline(cin, text);
                cout << "Enter key: ";
                getline(cin, key);
                cout << "Result: " << decrypt(text, key) << endl;
                break;
            }
            case 3: {
                cout << "Input file: ";
                getline(cin, inFile);
                cout << "Output file: ";
                getline(cin, outFile);
                cout << "Enter key: ";
                getline(cin, key);
                processFile(inFile, outFile, key, true);
                break;
            }
            case 4:
                return 0;
            default:
                cerr << "Invalid selection!";
        }
    }
    return 0;
}
