import os
from colorama import Fore, Style 


def read_sequences_from_file(filename):
    sequences = []

    try:
        with open(
            filename, "r", encoding="utf-8-sig"
        ) as file:  # Use utf-8-sig to handle BOM
            for line in file:
                sequences.append(line.strip())
    except FileNotFoundError:
        print(f"File not found: {filename}")

    return sequences


# Function to calculate the number of matches between two DNA sequences
def calculate_matches(sequence1, sequence2):
    if len(sequence1) != len(sequence2):
        print("Error: Sequences must be of the same length.")
        return None

    matching_positions = [
        i for i in range(len(sequence1)) if sequence1[i] == sequence2[i]
    ]

    match_count = len(matching_positions)

    # Visualize the matches
    for i in range(len(sequence1)):
        if i in matching_positions:
            print(
                f"         {Fore.GREEN + Style.BRIGHT}{sequence1[i]}  {sequence2[i]}{Style.RESET_ALL}"
            )
        else:
            print(f"         {sequence1[i]}  {sequence2[i]}", end="  (mismatch)\n")

    return match_count


# Function to calculate the number of matches with shifts for a given shift value
def calculate_matches_for_shift(sequence1, sequence2, shift):
    shifted_sequence1 = sequence1[-shift:] + sequence1[:-shift]

    matching_positions = [
        i for i in range(len(shifted_sequence1)) if shifted_sequence1[i] == sequence2[i]
    ]
    match_count = len(matching_positions)

    # Visualize the matches
    for i in range(len(shifted_sequence1)):
        if i in matching_positions:
            print(
                f"         {Fore.GREEN + Style.BRIGHT}{shifted_sequence1[i]}  {sequence2[i]}{Style.RESET_ALL}"
            )
        else:
            print(
                f"         {shifted_sequence1[i]}  {sequence2[i]}", end="  (mismatch)\n"
            )

    return match_count


# Function to calculate the best number of matches with shifts by iterating from 1 to max shift
def calculate_best_matches_with_shifts(sequence1, sequence2, max_shift):
    if len(sequence1) != len(sequence2):
        print("Error: Sequences must be of the same length.")
        return None, 0

    best_match_count = 0
    best_shift = 0

    for shift in range(1, max_shift + 1):
        match_count = calculate_matches_for_shift(sequence1, sequence2, shift)

        if match_count > best_match_count:
            best_match_count = match_count
            best_shift = shift

    print(f"Best Number of Matches with Shifts ({best_shift}): {best_match_count}")
    print("Visualizing Matches:")
    for i in range(len(sequence1)):
        if i in range(len(sequence1)):
            if sequence1[i] == sequence2[i]:
                print(
                    f"         {Fore.GREEN + Style.BRIGHT}{sequence1[i]}  {sequence2[i]}{Style.RESET_ALL}"
                )
            else:
                print(f"         {sequence1[i]}  {sequence2[i]}", end="  (mismatch)\n")

    return best_match_count, best_shift


# Function to calculate the maximum contiguous chain for a given shift value
def calculate_max_contiguous_chain_for_shift(sequence1, sequence2, shift):
    shifted_sequence1 = sequence1[-shift:] + sequence1[:-shift]
    max_chain = 0
    current_chain = 0
    max_chain_alphabets = []
    current_chain_alphabets = []

    for char1, char2 in zip(shifted_sequence1, sequence2):
        if char1 == char2:
            current_chain += 1
            current_chain_alphabets.append(char1)
        else:
            if current_chain > max_chain:
                max_chain = current_chain
                max_chain_alphabets = current_chain_alphabets
            current_chain = 0
            current_chain_alphabets = []

    if current_chain > max_chain:
        max_chain = current_chain
        max_chain_alphabets = current_chain_alphabets

    return max_chain, max_chain_alphabets


# Function to calculate the maximum contiguous chain by iterating from 1 to max shift
def calculate_maximum_contiguous_chain(sequence1, sequence2, max_shift):
    if len(sequence1) != len(sequence2):
        print("Error: Sequences must be of the same length.")
        return None, 0, []

    max_chain = 0
    best_shift = 0
    max_chain_alphabets = []

    for shift in range(1, max_shift + 1):
        chain_length, chain_alphabets = calculate_max_contiguous_chain_for_shift(
            sequence1, sequence2, shift
        )

        if chain_length > max_chain:
            max_chain = chain_length
            best_shift = shift
            max_chain_alphabets = chain_alphabets

    if max_chain > 0:
        max_chain_str = "".join(max_chain_alphabets)
    else:
        max_chain_str = "No match found."

    print(f"Maximum Contiguous Chain ({best_shift}): {max_chain_str}")
    print("Visualizing Maximum Contiguous Chain:")
    for i in range(len(sequence1)):
        if i in range(len(max_chain_alphabets)):
            if sequence1[i] == max_chain_alphabets[i]:
                print(
                    f"         {Fore.RED + Style.BRIGHT}{sequence1[i]}  {max_chain_alphabets[i]}{Style.RESET_ALL}"
                )
            else:
                print(f"         {sequence1[i]}  {max_chain_alphabets[i]}")
        else:
            print(f"         {sequence1[i]}  -  (no match)")

    return max_chain, best_shift


# Function to set the maximum shift based on user input
def set_max_shift():
    max_shift = int(input("Enter the maximum shift: "))
    return max_shift


# Main menu for user interaction
def main_menu():
    while True:
        print("\nMain Menu:")
        print("1. Calculate Number of Matches without Shifts (Console Input)")
        print("2. Calculate Best Number of Matches with Shifts (Console Input)")
        print("3. Calculate Maximum Contiguous Chain (Console Input)")
        print("4. Calculate Number of Matches without Shifts (File Input)")
        print("5. Calculate Best Number of Matches with Shifts (File Input)")
        print("6. Calculate Maximum Contiguous Chain (File Input)")
        print("7. Exit")
        choice = input("Enter your choice: ")

        if choice == "1":
            sequence1 = input("Enter the first DNA sequence: ").upper()
            sequence2 = input("Enter the second DNA sequence: ").upper()
            if len(sequence1) != len(sequence2):
                print("Error: Sequences must be of the same length.")
            else:
                match_count = calculate_matches(sequence1, sequence2)
                print(f"Number of Matches without Shifts: {match_count}")
        elif choice == "2":
            sequence1 = input("Enter the first DNA sequence: ").upper()
            sequence2 = input("Enter the second DNA sequence: ").upper()
            if len(sequence1) != len(sequence2):
                print("Error: Sequences must be of the same length.")
            else:
                max_shift = set_max_shift()
                best_match_count, best_shift = calculate_best_matches_with_shifts(
                    sequence1, sequence2, max_shift
                )
                print(
                    f"Best Number of Matches with Shifts ({best_shift}): {best_match_count}"
                )
        elif choice == "3":
            sequence1 = input("Enter the first DNA sequence: ").upper()
            sequence2 = input("Enter the second DNA sequence: ").upper()
            if len(sequence1) != len(sequence2):
                print("Error: Sequences must be of the same length.")
            else:
                max_shift = set_max_shift()
                (
                    max_chain,
                    best_shift,
                    max_chain_alphabets,
                ) = calculate_maximum_contiguous_chain(sequence1, sequence2, max_shift)
        elif choice == "4":
            filename = input("Enter the filename: ")
            sequences = read_sequences_from_file(filename)
            if len(sequences) >= 2:
                if len(sequences[0]) != len(sequences[1]):
                    print("Error: Sequences in the file must be of the same length.")
                else:
                    match_count = calculate_matches(sequences[0], sequences[1])
                    print(f"Number of Matches without Shifts: {match_count}")
        elif choice == "5":
            filename = input("Enter the filename: ")
            sequences = read_sequences_from_file(filename)
            if len(sequences) >= 2:
                if len(sequences[0]) != len(sequences[1]):
                    print("Error: Sequences in the file must be of the same length.")
                else:
                    max_shift = set_max_shift()
                    (
                        best_match_count,
                        best_shift,
                        best_match_alphabets,
                    ) = calculate_best_matches_with_shifts(
                        sequences[0], sequences[1], max_shift
                    )
        elif choice == "6":
            filename = input("Enter the filename: ")
            sequences = read_sequences_from_file(filename)
            if len(sequences) >= 2:
                if len(sequences[0]) != len(sequences[1]):
                    print("Error: Sequences in the file must be of the same length.")
                else:
                    max_shift = set_max_shift()
                    (
                        max_chain,
                        best_shift,
                        max_chain_alphabets,
                    ) = calculate_maximum_contiguous_chain(
                        sequences[0], sequences[1], max_shift
                    )
        elif choice == "7":
            print("Exiting program.")
            break
        else:
            print("Invalid choice. Please select a valid option.")


if __name__ == "__main__":
    main_menu()
