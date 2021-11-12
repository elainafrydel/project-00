# project-00
232 Project 1
#include <iostream>
#include <sstream>
#include <string>
#include <iomanip>

using std::cin; // console input stream
using std::cout; // console output stream
using std::endl; // end of line marker
using std::fixed; // decimala accuracy
using std::setprecision; // decimal accuracy
using std::string; //string
using std::to_string; // convert to string

int get_line_sum(string line_ca)
//Returns an integer that is the sum of the numbers in the string
//Adds number until i is greater than the length of the string
{
    int sum_ca;
    sum_ca = 0;
    for (int i = 0; i < int(line_ca.length()); i++) {
        int num;
        num = line_ca[i] - 48;
        sum_ca += num;
    }
    return sum_ca;
}

char get_next_state(string neighborhood, string multiline)
//Returns char '0' if the string neighborhood is not found within the string multiline
//Else returns the index, as a char, 
//where the neighborhood is found within the multiline
{
    if (multiline.find(neighborhood) == string::npos) {
        return '0';
    }
    else {
        return multiline[7 + multiline.find(neighborhood)];
    }
}

string number(char first, char second, char third) 
//Returns the neighborhood string that was created
//Appends the first char, the second char, and then the third char
{
    string neighborhood = "";
    neighborhood.append(1, first);
    neighborhood.append(1, second);
    neighborhood.append(1, third);
    return neighborhood;
}
void update_line(string& row_of_cells, string rules)
//Returns nothing
//Changes the refrence string based on the string of rules
//calls number function to create the neighborhood
//calls get_next_state to deciede if the neighborhood is located
//within the row_of_cells
{
    char new_num;
    string new_str = "";
    for (int i = 0; i < int(row_of_cells.length()); i++) {
        if (i == 0) {
            char first = row_of_cells[int(row_of_cells.length()) - 1];
            char second = row_of_cells[0];
            char third = row_of_cells[1];
            string neighborhood = number(first, second, third);
            new_num = get_next_state(neighborhood, rules);
        }
        else if (i == (int(row_of_cells.length())) - 1) {
            char first = row_of_cells[(i - 1)];
            char second = row_of_cells[i];
            char third = row_of_cells[0];
            string neighborhood = number(first, second, third);
            new_num = get_next_state(neighborhood, rules);
        }
        else {
            new_num = get_next_state((row_of_cells.substr((i - 1), 3)), rules);
        }
        new_str.append(1, new_num);
    }
    row_of_cells = new_str;
}

string run_cellular_automata(string rules, int num_iterations, string& starting_state)
//Returns a string of the updated lines. Returns the same amount of lines as num_iterations
//Calls get_line_sum to get the sum of all chars within startign_state
//Calls update_line if i > 0 and updates the line according to the string of rules
{
    string line = "";
    for (int i = 0; i < num_iterations; i++) {
        if (i == 0) {
            line.append(starting_state);
            line.append(" sum = ");
            line.append(to_string(get_line_sum(starting_state)));
            line.append(1, '\n');
        }
        else {
            update_line(starting_state, rules);
            line.append(starting_state);
            line.append(" sum = ");
            line.append(to_string(get_line_sum(starting_state)));
            line.append(1, '\n');
        }
    }
    return line;
}

int main()
{
    string rule;
    int num_lines_output;
    string starting_line;
    string lst_rules = "";
    while (cin >> rule) { //Repeat input until a "." is inputted
        if (rule != ".") {
            lst_rules.append(rule);
            lst_rules.append(1, '\n');
        }
        if (rule == ".") {
            cin >> num_lines_output;
            cin >> starting_line;
            string result = run_cellular_automata(lst_rules, num_lines_output, starting_line);
            cout << result << endl;
            break;
        }
        else {
            continue;
        }
    }
    return 0;
}
