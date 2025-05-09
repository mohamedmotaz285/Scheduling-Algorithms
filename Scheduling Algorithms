#include <iostream>
#include <vector>
#include <algorithm>
#include <cstdlib>
#include <ctime>
#include <string>
#include <sstream>
#include <iomanip>
#include <cctype>

using namespace std;

// Function prototypes
void printMovements(const vector<int>& movements, int total_seek);
int fcfs(vector<int> queue, int head, int cylinders);
int scan(vector<int> queue, int head, int cylinders);
int cscan(vector<int> queue, int head, int cylinders);

// Function to print the head movement sequence and total seek time
void printMovements(const vector<int>& movements, int total_seek) {
    cout << "Head Movement Sequence:\n";
    for (int i = 0; i < movements.size(); i++) {
        cout << movements[i];
        if (i < movements.size() - 1) {
            cout << " -> ";
        }
        // Add line break every 8 elements for readability
        if ((i + 1) % 8 == 0 && i != movements.size() - 1) {
            cout << "\n";
        }
    }
    cout << "\n\n";
    
    cout << "Total Seek Time: " << total_seek << " cylinder movements\n";
}

// First-Come, First-Served (FCFS) algorithm
int fcfs(vector<int> queue, int head, int cylinders) {
    cout << "Executing FCFS algorithm...\n";
    
    int seek_time = 0;
    int current_pos = head;
    vector<int> movements;
    
    // Add initial head position to movements
    movements.push_back(head);
    
    // Process each request in the order they arrive
    for (int i = 0; i < queue.size(); i++) {
        // Calculate seek time for this move
        int distance = abs(queue[i] - current_pos);
        seek_time += distance;
        
        // Update current position
        current_pos = queue[i];
        
        // Record the movement
        movements.push_back(current_pos);
    }
    
    // Print the movement sequence and total seek time
    printMovements(movements, seek_time);
    
    // Additional output format showing direction changes clearly
    cout << "\nDetailed movement description:\n";
    cout << "Started at cylinder " << movements[0] << endl;
    
    for (int i = 1; i < movements.size(); i++) {
        // For C-SCAN, when we jump from max to 0
        if (movements[i-1] == cylinders - 1 && movements[i] == 0) {
            cout << "Went to cylinder " << movements[i-1] << " then jumped to cylinder " << movements[i] << endl;
        } else {
            cout << "Moved to cylinder " << movements[i] << endl;
        }
    }
    
    return seek_time;
}

// SCAN algorithm+
int scan(vector<int> queue, int head, int cylinders) {
    cout << "Executing SCAN algorithm...\n";
    
    // Create a copy of the queue and add the head position
    vector<int> all_positions = queue;
    all_positions.push_back(head);
    
    // Sort all positions
    sort(all_positions.begin(), all_positions.end());
    
    // Find the index of the head position
    int head_index = 0;
    for (int i = 0; i < all_positions.size(); i++) {
        if (all_positions[i] == head) {
            head_index = i;
            break;
        }
    }
    
    // Start from head and move toward smaller values first (direction = 0)
    vector<int> movements;
    movements.push_back(head);
    
    int seek_time = 0;
    int current_pos = head;
    
    // First move toward the beginning (0)
    for (int i = head_index - 1; i >= 0; i--) {
        seek_time += abs(all_positions[i] - current_pos);
        current_pos = all_positions[i];
        movements.push_back(current_pos);
    }
    
    // If we're not at the beginning, go there (cylinder 0)
    if (current_pos > 0) {
        seek_time += current_pos; // Distance from current position to 0
        current_pos = 0;
        movements.push_back(current_pos);
    }
    
    // Then move toward the end (largest cylinder)
    for (int i = head_index + 1; i < all_positions.size(); i++) {
        seek_time += abs(all_positions[i] - current_pos);
        current_pos = all_positions[i];
        movements.push_back(current_pos);
    }
    
    // Go all the way to the end if not already there
    if (current_pos < cylinders - 1) {
        seek_time += abs((cylinders - 1) - current_pos);
        current_pos = cylinders - 1;
        movements.push_back(current_pos);
    }
    
    // Print the movement sequence and total seek time
    printMovements(movements, seek_time);
    
    // Additional output format showing direction changes clearly
    cout << "\nDetailed movement description:\n";
    cout << "Started at cylinder " << movements[0] << endl;
    
    for (int i = 1; i < movements.size(); i++) {
        // For C-SCAN, when we jump from max to 0
        if (movements[i-1] == cylinders - 1 && movements[i] == 0) {
            cout << "Went to cylinder " << movements[i-1] << " then jumped to cylinder " << movements[i] << endl;
        } else {
            cout << "Moved to cylinder " << movements[i] << endl;
        }
    }
    
    return seek_time;
}

// C-SCAN algorithm (Circular SCAN)
int cscan(vector<int> queue, int head, int cylinders) {
    cout << "Executing C-SCAN algorithm...\n";
    
    // Create a copy of the queue and add the head position
    vector<int> all_positions = queue;
    all_positions.push_back(head);
    
    // Sort all positions
    sort(all_positions.begin(), all_positions.end());
    
    // Find the index of the head position
    int head_index = 0;
    for (int i = 0; i < all_positions.size(); i++) {
        if (all_positions[i] == head) {
            head_index = i;
            break;
        }
    }
    
    // Start from head and move toward larger values
    vector<int> movements;
    movements.push_back(head);
    
    int seek_time = 0;
    int current_pos = head;
    
    // First move toward the end (largest cylinder)
    for (int i = head_index + 1; i < all_positions.size(); i++) {
        seek_time += abs(all_positions[i] - current_pos);
        current_pos = all_positions[i];
        movements.push_back(current_pos);
    }
    
    // If we're not at the end, go there (boundary movement)
    if (current_pos < cylinders - 1) {
        seek_time += abs((cylinders - 1) - current_pos);
        current_pos = cylinders - 1;
        movements.push_back(current_pos);
    }
    
    // Jump to the beginning (0) - this doesn't count toward seek time in C-SCAN
    // Only add the value to movements, don't add to seek time
    current_pos = 0;
    movements.push_back(current_pos);
    
    // Then move from beginning toward the head again
    for (int i = 0; i < head_index; i++) {
        seek_time += abs(all_positions[i] - current_pos);
        current_pos = all_positions[i];
        movements.push_back(current_pos);
    }
    
    // Print the movement sequence and total seek time
    printMovements(movements, seek_time);
    
    // Additional output format showing direction changes clearly
    cout << "\nDetailed movement description:\n";
    cout << "Started at cylinder " << movements[0] << endl;
    
    for (int i = 1; i < movements.size(); i++) {
        // For C-SCAN, when we jump from max to 0
        if (movements[i-1] == cylinders - 1 && movements[i] == 0) {
            cout << "Went to cylinder " << movements[i-1] << " then jumped to cylinder " << movements[i] << endl;
        } else {
            cout << "Moved to cylinder " << movements[i] << endl;
        }
    }
    
    return seek_time;
}

int main(int argc, char* argv[]) {
    // Variables
    int queue_size;
    int head_pos;
    int num_cylinders;
    string algorithm;
    vector<int> queue;
    
    // Check if arguments are provided via command line
    if (argc > 1) {
        // Process command line arguments
        if (argc < 5) {
            cout << "Usage: " << argv[0] << " <queue_size> <initial_head> <num_cylinders> <algorithm> [queue values...]" << endl;
            return 1;
        }
        
        queue_size = stoi(argv[1]);
        head_pos = stoi(argv[2]);
        num_cylinders = stoi(argv[3]);
        algorithm = argv[4];
        
        // Get queue values from command line if provided
        if (argc >= 5 + queue_size) {
            for (int i = 0; i < queue_size; i++) {
                queue.push_back(stoi(argv[5 + i]));
            }
        } else {
            // Generate random queue if not enough values provided
            cout << "Generating random queue of size " << queue_size << endl;
            srand(time(NULL));
            for (int i = 0; i < queue_size; i++) {
                queue.push_back(rand() % num_cylinders);
            }
        }
    } else {
        // Interactive mode
        cout << "Enter queue size: ";
        cin >> queue_size;
        
        cout << "Enter initial head position: ";
        cin >> head_pos;
        
        cout << "Enter number of cylinders: ";
        cin >> num_cylinders;
        
        cout << "Enter algorithm (FCFS, SCAN, C-SCAN): ";
        cin >> algorithm;
        
        cout << "Enter " << queue_size << " queue values (separated by space or comma): ";
        string line;
        cin.ignore(); // Clear any leftover input
        getline(cin, line);
        
        // Process the line to extract numbers, handling both spaces and commas
        stringstream ss(line);
        string token;
        int count = 0;
        
        while (getline(ss, token, ',') && count < queue_size) {
            // Remove any spaces
            token.erase(remove_if(token.begin(), token.end(), ::isspace), token.end());
            if (!token.empty()) {
                try {
                    int val = stoi(token);
                    queue.push_back(val);
                    count++;
                } catch (const std::invalid_argument& e) {
                    cout << "Warning: Skipping invalid number: " << token << endl;
                }
            }
        }
        
        // If we didn't get enough numbers from comma separation, try space separation
        if (count < queue_size) {
            ss.clear();
            ss.str(line);
            while (ss >> token && count < queue_size) {
                try {
                    int val = stoi(token);
                    queue.push_back(val);
                    count++;
                } catch (const std::invalid_argument& e) {
                    cout << "Warning: Skipping invalid number: " << token << endl;
                }
            }
        }
        
        // If we still don't have enough numbers, prompt for more
        while (count < queue_size) {
            cout << "Need " << (queue_size - count) << " more values. Enter next value: ";
            int val;
            cin >> val;
            queue.push_back(val);
            count++;
        }
    }
    
    // Validate head position
    if (head_pos < 0 || head_pos >= num_cylinders) {
        cout << "Error: Head position must be between 0 and " << (num_cylinders-1) << endl;
        return 1;
    }
    
    // Display input information
    cout << "\n=== Disk Scheduling Simulation ===\n";
    cout << "Queue size: " << queue_size << endl;
    cout << "Queue: ";
    for (int val : queue) {
        cout << val << " ";
    }
    cout << endl;
    cout << "Initial head position: " << head_pos << endl;
    cout << "Number of cylinders: " << num_cylinders << " (0 to " << (num_cylinders-1) << ")" << endl;
    cout << "Algorithm: " << algorithm << endl;
    cout << "==============================\n\n";
    
    // Execute the chosen algorithm
    int total_seek_time = 0;
    
    transform(algorithm.begin(), algorithm.end(), algorithm.begin(), ::toupper);
    
    if (algorithm == "FCFS") {
        total_seek_time = fcfs(queue, head_pos, num_cylinders);
    } else if (algorithm == "SCAN") {
        total_seek_time = scan(queue, head_pos, num_cylinders);
    } else if (algorithm == "C-SCAN" || algorithm == "CSCAN") {
        total_seek_time = cscan(queue, head_pos, num_cylinders);
    } else {
        cout << "Error: Unknown algorithm. Choose from FCFS, SCAN, or C-SCAN." << endl;
        return 1;
    }
    
    return 0;}
