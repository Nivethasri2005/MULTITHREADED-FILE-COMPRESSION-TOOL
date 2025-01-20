#include <iostream>
#include <sstream>
#include <stdexcept>
#include <cctype>

using namespace std;

class Parser {
private:
    string expression;
    size_t position;

    char peek() {
        while (position < expression.size() && isspace(expression[position])) {
            position++;
        }
        return position < expression.size() ? expression[position] : '\0';
    }

    char get() {
        char current = peek();
        position++;
        return current;
    }

    double parseNumber() {
        size_t start = position;
        while (isdigit(peek()) || peek() == '.') {
            get();
        }
        string numberStr = expression.substr(start, position - start);
        try {
            return stod(numberStr);
        } catch (invalid_argument&) {
            throw runtime_error("Invalid number: " + numberStr);
        }
    }

    double parseFactor() {
        char current = peek();
        if (isdigit(current)) {
            return parseNumber();
        } else if (current == '(') {
            get(); // Consume '('
            double result = parseExpression();
            if (get() != ')') {
                throw runtime_error("Missing closing parenthesis");
            }
            return result;
        } else {
            throw runtime_error(string("Unexpected character: ") + current);
        }
    }

    double parseTerm() {
        double result = parseFactor();
        while (true) {
            char current = peek();
            if (current == '*') {
                get();
                result *= parseFactor();
            } else if (current == '/') {
                get();
                double divisor = parseFactor();
                if (divisor == 0) {
                    throw runtime_error("Division by zero");
                }
                result /= divisor;
            } else {
                break;
            }
        }
        return result;
    }

    double parseExpression() {
        double result = parseTerm();
        while (true) {
            char current = peek();
            if (current == '+') {
                get();
                result += parseTerm();
            } else if (current == '-') {
                get();
                result -= parseTerm();
            } else {
                break;
            }
        }
        return result;
    }

public:
    double parse(const string& input) {
        expression = input;
        position = 0;
        double result = parseExpression();
        if (peek() != '\0') {
            throw runtime_error("Unexpected input after valid expression");
        }
        return result;
    }
};

int main() {
    cout << "Simple Arithmetic Expression Evaluator" << endl;
    cout << "Enter expressions to evaluate (type 'exit' to quit):" << endl;

    Parser parser;
    string input;

    while (true) {
        cout << ">> ";
        getline(cin, input);

        if (input == "exit") {
            break;
        }

        try {
            double result = parser.parse(input);
            cout << "Result: " << result << endl;
        } catch (const runtime_error& e) {
            cerr << "Error: " << e.what() << endl;
        }
    }

    return 0;
}
