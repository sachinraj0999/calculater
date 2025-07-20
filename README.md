# calculater
# project 1
import tkinter as tk

def on_click(value):
    current = display.get()
    display.delete(0, tk.END)
    display.insert(0, current + value)

def clear():
    display.delete(0, tk.END)

def backspace():
    current = display.get()
    display.delete(0, tk.END)
    display.insert(0, current[:-1])

def calculate():
    try:
        result = eval(display.get())
        display.delete(0, tk.END)
        display.insert(0, str(result))
    except:
        display.delete(0, tk.END)
        display.insert(0, "Error")

def exit_fullscreen(event=None):
    window.attributes("-fullscreen", False)

# Create main window
window = tk.Tk()
window.title("Full-Screen Calculator")
window.attributes("-fullscreen", True)
window.configure(bg="#1e1e1e")

# Bind ESC key to exit fullscreen
window.bind("<Escape>", exit_fullscreen)

# Entry display
display = tk.Entry(window, font=("Courier", 36), bd=10, bg="#292929", fg="white", relief=tk.FLAT, justify="right")
display.grid(row=0, column=0, columnspan=4, padx=20, pady=30)

# Button styling
button_style = {
    "font": ("Courier", 24),
    "width": 5,
    "height": 2,
    "bd": 0,
    "bg": "#3c3f41",
    "fg": "white",
    "activebackground": "#5c5f61"
}

# Define button layout
buttons = [
    ('7', 1, 0), ('8', 1, 1), ('9', 1, 2), ('/', 1, 3),
    ('4', 2, 0), ('5', 2, 1), ('6', 2, 2), ('*', 2, 3),
    ('1', 3, 0), ('2', 3, 1), ('3', 3, 2), ('-', 3, 3),
    ('0', 4, 0), ('.', 4, 1), ('=', 4, 2), ('+', 4, 3),
    ('C', 5, 0), ('⌫', 5, 1), ('Exit', 5, 2)
]

for (text, row, col) in buttons:
    def make_cmd(t=text):  # closure to fix lambda issue
        if t == '=':
            return calculate
        elif t == 'C':
            return clear
        elif t == '⌫':
            return backspace
        elif t == 'Exit':
            return window.quit
        else:
            return lambda: on_click(t)

    btn = tk.Button(window, text=text, command=make_cmd(), **button_style)
    btn.grid(row=row, column=col, padx=10, pady=10)

# Fill extra column to balance layout
tk.Label(window, bg="#1e1e1e").grid(row=5, column=3)

# Start the app
window.mainloop()
