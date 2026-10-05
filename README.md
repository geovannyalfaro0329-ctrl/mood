# mood
#include <iostream>
#include <string>
#include <iomanip>

using namespace std;


class MoodEntry {
private:
    string ratingDate; // YYYY-MM-DD
    int ratingScore;   // 1=Unhappy, 2=Neutral, 3=Happy, 4=Very Good
    string ratingWord; // Short single-line description

public:
    // Default Constructor
    MoodEntry() {
        ratingDate = "Unrecorded";
        ratingScore = 3;
        ratingWord = "Normal";
    }

    
    MoodEntry(string date, int rating, string word) {
        ratingDate = date;
        setRating(rating); // Uses setter validation
        ratingWord = word;
    }

    // Setters
    void setDate(string date) {
        ratingDate = date;
    }

    void setRating(int rating) {
        if (rating >= 1 && rating <= 4) {
            ratingScore = rating;
        } else {
            ratingScore = 3; // Default to 3 if out of bounds
        }
    }

    void setWord(string word) {
        ratingWord = word;
    }

    // Getters
    string getDate() const { return ratingDate; }
    int getRating() const { return ratingScore; }
    string getWord() const { return ratingWord; }

    // Print Method
    void print() const {
        string textRating = "";
        switch (ratingScore) {
            case 1: textRating = "Unhappy"; break;
            case 2: textRating = "Neutral"; break;
            case 3: textRating = "Happy"; break;
            case 4: textRating = "Very Good"; break;
            default: textRating = "Happy"; break;
        }
        cout << "Date: " << ratingDate 
             << " | Rating: " << textRating 
             << " | Note: " << ratingWord << endl;
    }
};


int main() {
    const int MAX_ENTRIES = 20;
    MoodEntry moodLog[MAX_ENTRIES]; // Array of 20 mood objects
    int currentCount = 0;           // Tracks how many entries are added
    int choice = 0;

    do {
      
        cout << "\n===== MoodMinder Menu =====" << endl;
        cout << "1. Add entry" << endl;
        cout << "2. View entries" << endl;
        cout << "3. View average rating" << endl;
        cout << "4. Exit" << endl;
        cout << "Enter your choice (1-4): ";
        cin >> choice;

        // Input validation for menu selection
        if (cin.fail()) {
            cin.clear();
            cin.ignore(10000, '\n');
            cout << "Invalid choice. Please try again." << endl;
            continue;
        }

        if (choice == 1) {
            // Add Entry
            if (currentCount >= MAX_ENTRIES) {
                cout << "Error: Mood log is full (Maximum " << MAX_ENTRIES << " entries)." << endl;
            } else {
                string date, word;
                int rating;

                cout << "Enter date (YYYY-MM-DD): ";
                cin >> date;
                
                cout << "Enter rating (1=Unhappy, 2=Neutral, 3=Happy, 4=Very Good): ";
                cin >> rating;
                
                cin.ignore(); // Clear buffer before reading a line of text
                cout << "Enter a short description: ";
                getline(cin, word);

                // Use the parameterized constructor to instantiate and store the entry
                moodLog[currentCount] = MoodEntry(date, rating, word);
                currentCount++;
                cout << "Entry added successfully!" << endl;
            }
        } 
        else if (choice == 2) {
            // View Entries
            if (currentCount == 0) {
                cout << "No entries recorded yet." << endl;
            } else {
                cout << "\n--- Your Mood Log Entries ---" << endl;
                for (int i = 0; i < currentCount; i++) {
                    moodLog[i].print();
                }
            }
        } 
        else if (choice == 3) {
            // View Average Rating
            if (currentCount == 0) {
                cout << "No entries recorded yet to calculate an average." << endl;
            } else {
                double totalScore = 0;
                for (int i = 0; i < currentCount; i++) {
                    totalScore += moodLog[i].getRating();
                }
                double average = totalScore / currentCount;
                cout << fixed << setprecision(2);
                cout << "Your average mood rating is: " << average << " / 4.00" << endl;
            }
        } 
        else if (choice == 4) {
            cout << "Exiting MoodMinder. Goodbye!" << endl;
        } 
        else {
            cout << "Invalid selection. Please choose 1, 2, 3, or 4." << endl;
        }

    } while (choice != 4);

    return 0;
}
