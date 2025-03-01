#include <iostream>
#include <string>
using namespace std;
 
string encryptVigenere(const string& plaintext, const string& key) {
    string ciphertext;
    int keyIndex = 0;
    
    for (char c : plaintext) {
        if (isalpha(c)) {
            char base = isupper(c) ? 'A' : 'a';
            int shift = toupper(key[keyIndex % key.length()]) - 'A';
            ciphertext += (c - base + shift) % 26 + base;
            keyIndex++;
        } else {
            ciphertext += c;
        }
    }
    return ciphertext;
}
 
string decryptVigenere(const string& ciphertext, const string& key) {
    string plaintext;
    int keyIndex = 0;
    
    for (char c : ciphertext) {
        if (isalpha(c)) {
            char base = isupper(c) ? 'A' : 'a';
            int shift = toupper(key[keyIndex % key.length()]) - 'A';
            plaintext += (c - base - shift + 26) % 26 + base;
            keyIndex++;
        } else {
            plaintext += c;
        }
    }
    return plaintext;
}
 
int main() {
    string text = "AttackAtDawn";
    string key = "LEMON";
    
    string encrypted = encryptVigenere(text, key);
    cout << "Encrypted: " << encrypted << endl;
    
    string decrypted = decryptVigenere(encrypted, key);
    cout << "Decrypted: " << decrypted << endl;
    
    return 0;
}
