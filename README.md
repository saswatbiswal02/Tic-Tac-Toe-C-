//# Tic-Tac-Toe-C-
//Tic-Tac-Toe  code in modern C++ compiler
#include <iostream>
#include <limits>

class TicTacToe {
private:
    char square[10];
    int player, choice, ctr, winCountPlayer1, winCountPlayer2, drawnMatchCount;
    char mark, ch;

public:
    TicTacToe();
    void displayStats();
    void displayBoard();
    void clearBoard();
    int checkWin();
    void play();
};

TicTacToe::TicTacToe() : player(1), ch('y'), ctr(1), winCountPlayer1(0), winCountPlayer2(0), drawnMatchCount(0) {
    for (int i = 0; i < 10; ++i) {
        square[i] = i + '0';
    }
}

void TicTacToe::displayStats() {
    std::cout << "Total turns played: " << ctr << std::endl;
    std::cout << "Player 1 wins: " << winCountPlayer1 << ((winCountPlayer1 <= 1) ? " time" : " times") << std::endl;
    std::cout << "Player 2 wins: " << winCountPlayer2 << ((winCountPlayer2 <= 1) ? " time" : " times") << std::endl;
    std::cout << "Game drawn: " << drawnMatchCount << ((drawnMatchCount <= 1) ? " time" : " times") << std::endl;
    std::cin.ignore(std::numeric_limits<std::streamsize>::max(), '\n');
    std::cin.get();
}

void TicTacToe::displayBoard() {
    // Clear screen - Platform-dependent, adjust accordingly
    // You might need to use system("cls") for Windows
    // and system("clear") for Unix-like systems
    system("clear");  // Adjust this for your platform
    std::cout << "\n\n\t Tic Tac Toe \n\n";
    std::cout << "Player 1 is X - Player 2 is O" << std::endl << std::endl;
    std::cout << "   |   | " << std::endl;
    std::cout << " " << square[1] << " | " << square[2] << " | " << square[3] << " " << std::endl;
    std::cout << "   |   | " << std::endl;
    std::cout << "   |   | " << std::endl;
    std::cout << " " << square[4] << " | " << square[5] << " | " << square[6] << " " << std::endl;
    std::cout << "   |   | " << std::endl;
    std::cout << "   |   | " << std::endl;
    std::cout << " " << square[7] << " | " << square[8] << " | " << square[9] << " " << std::endl;
    std::cout << "   |   | " << std::endl;
    std::cout << "   |   | " << std::endl;
}

void TicTacToe::clearBoard() {
    for (int i = 0; i < 10; ++i) {
        square[i] = i + '0';
    }
}

int TicTacToe::checkWin() {
    // Check rows
    for (int i = 1; i <= 7; i += 3) {
        if (square[i] == square[i + 1] && square[i + 1] == square[i + 2]) {
            return 1;
        }
    }

    // Check columns
    for (int i = 1; i <= 3; ++i) {
        if (square[i] == square[i + 3] && square[i + 3] == square[i + 6]) {
            return 1;
        }
    }

    // Check diagonals
    if (square[1] == square[5] && square[5] == square[9]) {
        return 1;
    }
    if (square[3] == square[5] && square[5] == square[7]) {
        return 1;
    }

    // Check if the board is full (draw)
    for (int i = 1; i <= 9; ++i) {
        if (square[i] != 'X' && square[i] != 'O') {
            return 0; // Board is not full yet
        }
    }

    return -1; // Draw
}

void TicTacToe::play() {
    do {
        do {
            displayBoard();
            player = (player % 2) ? 1 : 2;
            std::cout << "Player " << player << " enter a number from 1-9: ";
            std::cin >> choice;

            while (std::cin.fail() || choice < 1 || choice > 9 || square[choice] == 'X' || square[choice] == 'O') {
                std::cout << "Invalid input. Enter a number from 1-9: ";
                std::cin.clear();
                std::cin.ignore(std::numeric_limits<std::streamsize>::max(), '\n');
                std::cin >> choice;
            }

            mark = (player == 1) ? 'X' : 'O';

            if (choice >= 1 && choice <= 9 && square[choice] == (choice + '0')) {
                square[choice] = mark;
            } else {
                std::cout << "Invalid move. Try again.";
                player--;
                std::cin.ignore(std::numeric_limits<std::streamsize>::max(), '\n');
                std::cin.get();
            }

            ctr++;
            player++;

        } while (checkWin() == -1);

        displayBoard();

        if (checkWin() == 1) {
            if (player % 2 == 1) {
                clearBoard();
                std::cout << "==> Player " << 2 << " wins. Press Enter to continue.\n";
                winCountPlayer2++;
            } else if (player % 2 == 0) {
                clearBoard();
                std::cout << "==> Player " << 1 << " wins. Press Enter to continue.\n";
                winCountPlayer1++;
            }
        } else {
            clearBoard();
            std::cout << "Game Drawn.\n";
            drawnMatchCount++;
        }

        std::cout << "Do you want to play again? Press 'y' or 'n': ";
        std::cin >> ch;

        if ((ch == 'y') || (ch == 'Y')) {
            clearBoard();
        } else {
            displayStats();
        }

        std::cin.ignore(std::numeric_limits<std::streamsize>::max(), '\n');

    } while (ch == 'y' || ch == 'Y');
}

int main() {
    TicTacToe game;
    game.play();
    return 0;
}
