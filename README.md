#include <iostream>
#include <string>
#include <vector>
#include <algorithm>
#include <random>

using namespace std;

// Простое шифрование подстановки (Caesar Cipher)
string caesarCipher(const string& text, int shift) {
    string result = text;
    for (size_t i = 0; i < result.size(); ++i) {
        if (isalpha(result[i])) {
            char base = isupper(result[i]) ? 'A' : 'a';
            result[i] = base + (result[i] - base + shift) % 26;
        }
    }
    return result;
}

// Шифрование Виженера
string vigenereCipher(const string& text, const string& key) {
    string result = text;
    int keyIndex = 0;
    for (size_t i = 0; i < result.size(); ++i) {
        if (isalpha(result[i])) {
            char base = isupper(result[i]) ? 'A' : 'a';
            result[i] = base + (result[i] - base + toupper(key[keyIndex]) - 'A') % 26;
            keyIndex = (keyIndex + 1) % key.size();
        }
    }
    return result;
}

// Шифрование XOR
string xorCipher(const string& text, const string& key) {
    string result = text;
    int keyIndex = 0;
    for (size_t i = 0; i < result.size(); ++i) {
        result[i] ^= key[keyIndex];
        keyIndex = (keyIndex + 1) % key.size();
    }
    return result;
}

// Генерация случайного ключа
string generateRandomKey(int length) {
    random_device rd;
    mt19937 generator(rd());
    uniform_int_distribution<> distribution('a', 'z');
    string key;
    for (int i = 0; i < length; ++i) {
        key += distribution(generator);
    }
    return key;
}

int main() {
    string text = "Hello, world!";

    // Шифрование Caesar Cipher
    string encryptedCaesar = caesarCipher(text, 3);
    cout << "Caesar Cipher (Encrypted): " << encryptedCaesar << endl;
    cout << "Caesar Cipher (Decrypted): " << caesarCipher(encryptedCaesar, -3) << endl;

    // Шифрование Виженера
    string key = "secret";
    string encryptedVigenere = vigenereCipher(text, key);
    cout << "Vigenere Cipher (Encrypted): " << encryptedVigenere << endl;
    cout << "Vigenere Cipher (Decrypted): " << vigenereCipher(encryptedVigenere, key) << endl;

    // Шифрование XOR
    string randomKey = generateRandomKey(10);
    string encryptedXOR = xorCipher(text, randomKey);
    cout << "XOR Cipher (Encrypted): " << encryptedXOR << endl;
    cout << "XOR Cipher (Decrypted): " << xorCipher(encryptedXOR, randomKey) << endl;

    return 0;
}
