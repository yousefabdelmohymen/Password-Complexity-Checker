import tkinter as tk
from tkinter import ttk
import re

def password_strength_checker(password):
    # Criteria definitions
    length_criteria = len(password) >= 8
    lowercase_criteria = re.search(r"[a-z]", password) is not None
    uppercase_criteria = re.search(r"[A-Z]", password) is not None
    number_criteria = re.search(r"\d", password) is not None
    special_criteria = re.search(r"[!@#$%^&*(),.?\":{}|<>]", password) is not None

    # Strength assessment
    strength_score = sum([length_criteria, lowercase_criteria, uppercase_criteria, number_criteria, special_criteria])

    # Feedback based on strength score
    if strength_score == 0:
        feedback = "Very Weak"
    elif strength_score == 1:
        feedback = "Weak"
    elif strength_score == 2:
        feedback = "Moderate"
    elif strength_score == 3:
        feedback = "Strong"
    elif strength_score == 4:
        feedback = "Very Strong"
    else:
        feedback = "Extremely Strong"

    return strength_score, feedback

def update_feedback(*args):
    password = entry.get()
    score, feedback = password_strength_checker(password)
    
    # Update progress bar
    progress['value'] = score * 20  # Scale to 100
    
    # Update feedback label
    feedback_label.config(text=f"Strength: {feedback}")
    
    # Update suggestions
    suggestions = []
    if len(password) < 8:
        suggestions.append("Password should be at least 8 characters long.")
    if not re.search(r"[a-z]", password):
        suggestions.append("Password should contain at least one lowercase letter.")
    if not re.search(r"[A-Z]", password):
        suggestions.append("Password should contain at least one uppercase letter.")
    if not re.search(r"\d", password):
        suggestions.append("Password should contain at least one number.")
    if not re.search(r"[!@#$%^&*(),.?\":{}|<>]", password):
        suggestions.append("Password should contain at least one special character.")
    
    suggestion_label.config(text="\n".join(suggestions))

# Set up the GUI
root = tk.Tk()
root.title("Password Complexity Checker")

# Create and place widgets
tk.Label(root, text="Enter your password:").pack(pady=10)
entry = tk.Entry(root, show="*", width=30)
entry.pack(pady=5)
entry.bind("<KeyRelease>", update_feedback)

feedback_label = tk.Label(root, text="Strength: ")
feedback_label.pack(pady=10)

progress = ttk.Progressbar(root, orient="horizontal", length=300, mode="determinate", maximum=100)
progress.pack(pady=10)

suggestion_label = tk.Label(root, text="", wraplength=300, justify="left")
suggestion_label.pack(pady=10)

# Run the application
root.mainloop()
