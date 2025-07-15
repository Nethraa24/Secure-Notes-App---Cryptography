# Secure Notes App - Cryptography

The Secure Notes App is a Python-based desktop application that allows users to securely save and retrieve personal notes using encryption. It provides a simple and user-friendly graphical interface built with Tkinter.

## Features

- Save and read personal notes securely
- Notes are encrypted using AES encryption with the `cryptography` library
- Automatically generates and stores an encryption key
- Lightweight graphical user interface using Tkinter
- Optionally packaged as a standalone `.exe` for Windows

## Technologies Used

 Language     -> Python 3            
 GUI          -> Tkinter             
 Encryption   -> cryptography (Fernet) 
 Packaging    -> PyInstaller         

## Setup Instructions

### 1. Install Dependencies

```bash
pip install cryptography
```

### 2. Run the Application

```bash
python secure_notes.py
```

This will launch the GUI where you can save and retrieve secure notes.

---

## Build the Application as `.exe`

To convert the application to a standalone Windows executable:

1. Install PyInstaller:

```bash
pip install pyinstaller
```

2. Build the executable:

```bash
pyinstaller --onefile --windowed --icon=note_icon.ico secure_notes.py
```

3. The generated `.exe` will be located in the `dist/` folder.

---

## secure\_notes.py

```python
import tkinter as tk
from tkinter import messagebox
from cryptography.fernet import Fernet
import os

KEY_FILE = "secret.key"
NOTE_FILE = "secure_note.txt"

def generate_key():
    key = Fernet.generate_key()
    with open(KEY_FILE, "wb") as key_file:
        key_file.write(key)
    return key

def load_key():
    if os.path.exists(KEY_FILE):
        with open(KEY_FILE, "rb") as key_file:
            return key_file.read()
    else:
        return generate_key()

def save_note():
    note = text_input.get("1.0", tk.END).strip()
    if not note:
        messagebox.showwarning("Warning", "Note is empty.")
        return

    key = load_key()
    fernet = Fernet(key)
    encrypted = fernet.encrypt(note.encode())

    with open(NOTE_FILE, "wb") as note_file:
        note_file.write(encrypted)

    messagebox.showinfo("Success", "Note saved securely.")
    text_input.delete("1.0", tk.END)

def read_note():
    if not os.path.exists(NOTE_FILE):
        messagebox.showerror("Error", "No saved note found.")
        return

    key = load_key()
    fernet = Fernet(key)

    try:
        with open(NOTE_FILE, "rb") as note_file:
            encrypted = note_file.read()
            decrypted = fernet.decrypt(encrypted).decode()
            text_input.delete("1.0", tk.END)
            text_input.insert(tk.END, decrypted)
    except Exception as e:
        messagebox.showerror("Error", f"Failed to decrypt note.\n{e}")

# GUI Setup
root = tk.Tk()
root.title("Secure Notes App")
root.geometry("400x300")
root.configure(bg="#fcefee")

text_input = tk.Text(root, height=10, width=40, bg="#fffaf0", fg="#333", font=("Segoe UI", 11))
text_input.pack(pady=20)

button_frame = tk.Frame(root, bg="#fcefee")
button_frame.pack()

save_btn = tk.Button(button_frame, text="Save Note", command=save_note, bg="#ffc4c4", fg="#000", font=("Segoe UI", 10, "bold"))
save_btn.grid(row=0, column=0, padx=10)

read_btn = tk.Button(button_frame, text="Read Note", command=read_note, bg="#a0d8ef", fg="#000", font=("Segoe UI", 10, "bold"))
read_btn.grid(row=0, column=1, padx=10)

root.mainloop()
```

## Output:
<img width="1066" height="601" alt="image" src="https://github.com/user-attachments/assets/60bef6d2-a6af-4e2b-a2b6-9dbee7963e92" />

<img width="1067" height="805" alt="image" src="https://github.com/user-attachments/assets/f1ad89cb-57f5-4e5a-ae42-5a8723efefd4" />

<img width="680" height="514" alt="image" src="https://github.com/user-attachments/assets/3a94f265-96ae-487c-8b19-7390d5076a20" />

<img width="497" height="498" alt="image" src="https://github.com/user-attachments/assets/3f71bbc0-a1ec-485d-8e1d-3e9ef62358b2" />

