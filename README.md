import random
import string
import pyperclip

def generate_password(length, strength, require_minimum_characters, avoid_ambiguous):
    # Define basic character sets
    lowercase = string.ascii_lowercase
    uppercase = string.ascii_uppercase
    digits = string.digits
    special_characters = string.punctuation
    
    # Handle ambiguous characters if user wants to avoid them
    if avoid_ambiguous:
        ambiguous_characters = "l1O0"
        lowercase = lowercase.translate(str.maketrans('', '', ambiguous_characters))
        uppercase = uppercase.translate(str.maketrans('', '', ambiguous_characters))
        digits = digits.translate(str.maketrans('', '', ambiguous_characters))
    
    # Define character sets based on strength
    if strength == "weak":
        characters = lowercase
    elif strength == "medium":
        characters = lowercase + uppercase
    elif strength == "strong":
        characters = lowercase + uppercase + digits + special_characters
    else:
        raise ValueError("Invalid strength choice. Please choose 'weak', 'medium', or 'strong'.")
    
    # Ensure minimum character requirements if selected
    password = []
    if require_minimum_characters:
        password.append(random.choice(lowercase))
        password.append(random.choice(uppercase))
        password.append(random.choice(digits))
        password.append(random.choice(special_characters))
        length -= 4  # Reduce length as 4 characters are pre-selected
    
    # Generate remaining password characters
    password.extend(random.choice(characters) for _ in range(length))
    
    # Shuffle the password list to ensure randomness
    random.shuffle(password)
    
    return "".join(password)

if __name__ == "__main__":
    # Get input for password length
    length = int(input("Enter the desired length of the password: "))
    
    # Get input for password strength
    strength = input("Enter the desired strength (weak, medium, strong): ").lower()
    
    # Ask if minimum character requirements are needed
    require_minimum_characters = input("Do you want to include at least one lowercase, uppercase, digit, and special character? (yes/no): ").lower() == 'yes'
    
    # Ask if ambiguous characters should be avoided
    avoid_ambiguous = input("Do you want to avoid ambiguous characters (l, 1, O, 0)? (yes/no): ").lower() == 'yes'
    
    # Generate the password
    try:
        password = generate_password(length, strength, require_minimum_characters, avoid_ambiguous)
        print("Generated Password:", password)
        
        # Option to copy to clipboard
        copy_to_clipboard = input("Do you want to copy the password to the clipboard? (yes/no): ").lower()
        if copy_to_clipboard == 'yes':
            pyperclip.copy(password)
            print("Password copied to clipboard.")
    except ValueError as e:
        print(e)
