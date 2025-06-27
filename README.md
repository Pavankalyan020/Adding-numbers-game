# Adding-numbers-game

import random
import time

def display_message_box(title, message):
    """
    Simulates a message box in the console for user notifications.
    This function pauses execution until the user presses Enter.
    """
    print("\n" + "=" * 40)
    print(f"  {title.upper()}")
    print("=" * 40)
    print(f"\n{message}\n")
    print("=" * 40 + "\n")
    input("Press Enter to continue...") # Pause execution until user acknowledges

def get_random_int(min_val, max_val):
    """
    Generates a random integer within a specified range (inclusive).
    Used for creating the numbers in the addition problems.
    """
    return random.randint(min_val, max_val)

def generate_question(level_config):
    """
    Generates an addition question based on the current level's configuration.
    The difficulty is determined by 'maxNumber' in the level_config.
    Returns a tuple: (num1, num2, correct_answer).
    """
    num1 = get_random_int(1, level_config['maxNumber'])
    num2 = get_random_int(1, level_config['maxNumber'])
    correct_answer = num1 + num2
    return num1, num2, correct_answer

def run_game():
    """
    Main function to run the addition challenge game in the console.
    This function manages game flow, levels, scoring, and user interaction.
    """
    # Game configuration variables: Defines the levels,
    # including max number for questions and questions per level.
    LEVELS = [
        {'id': 1, 'maxNumber': 10, 'questionsPerLevel': 5},   # Level 1: Numbers up to 10
        {'id': 2, 'maxNumber': 50, 'questionsPerLevel': 5},   # Level 2: Numbers up to 50
        {'id': 3, 'maxNumber': 100, 'questionsPerLevel': 5}  # Level 3: Numbers up to 100
    ]

    while True: # Outer loop to allow playing the game multiple times
        current_level_index = 0
        score = 0
        
        # Display welcome message
        print("\n" + "#" * 40)
        print("  WELCOME TO THE ADDITION CHALLENGE!")
        print("#" * 40)
        time.sleep(1) # Pause for better user experience

        # Iterate through each level defined in LEVELS
        for level_config in LEVELS:
            print(f"\n--- Starting Level {level_config['id']} ---")
            print(f"Numbers will be up to {level_config['maxNumber']}. Get ready!")
            time.sleep(1.5)

            questions_answered_in_level = 0
            # Loop for questions within the current level
            while questions_answered_in_level < level_config['questionsPerLevel']:
                num1, num2, correct_answer = generate_question(level_config)

                user_answer = None
                # Loop to ensure valid numeric input from the user
                while True: 
                    try:
                        print(f"\nLevel: {level_config['id']} | Score: {score}")
                        user_input = input(f"What is {num1} + {num2}? Your Answer: ")
                        user_answer = float(user_input) # Attempt to convert input to a float
                        break # Exit input loop if conversion is successful
                    except ValueError:
                        print("Invalid input. Please enter a valid number.") # Error message for non-numeric input

                # Check if the user's answer is correct
                if user_answer == correct_answer:
                    print("Correct! +10 points!")
                    score += 10 # Award points for correct answer
                else:
                    print(f"Incorrect! The correct answer was {correct_answer}.")
                
                questions_answered_in_level += 1
                time.sleep(1) # Short pause after each question before the next one

            # After completing all questions in a level
            print(f"\n--- Level {level_config['id']} Complete! ---")
            print(f"Current Score: {score}")
            # Check if there are more levels to go
            if current_level_index < len(LEVELS) - 1:
                display_message_box("Level Complete!", 
                                    f"You finished Level {level_config['id']}!\nMoving to Level {LEVELS[current_level_index + 1]['id']}.")
            
            current_level_index += 1 # Move to the next level index

        # Game Over - all levels completed
        print("\n" + "=" * 50)
        print("                 GAME OVER!")
        print("=" * 50)
        display_message_box("Game Over!", 
                            f"You completed all levels!\nYour final score: {score} points!")

        # Ask if the user wants to play again
        play_again = input("Do you want to play again? (yes/no): ").lower().strip()
        if play_again != 'yes':
            print("Thanks for playing! Goodbye!")
            break # Exit the main game loop if user doesn't want to play again

if __name__ == "__main__":
    # Ensures run_game() is called only when the script is executed directly
    run_game()
