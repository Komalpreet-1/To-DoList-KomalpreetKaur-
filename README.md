# To-DoList-KomalpreetKaur-
The main aim of the To-Do List application project is to create an intuitive and
user-friendly interface that allows users to manage their tasks effectively. The application should enable
users to add, remove, and mark tasks as complete, providing an organized way to keep track of daily
activities.
Hardware and Software Requirements:
Hardware Requirements: CPU(Central Processing Unit): Any simple processor would be sufficient
for the execution of very basic python script and small projects.
RAM: 8GB or more RAM enhances the ability to manage the large datasets and run numerous
applications simultaneously without lag.
Storage: 100GB SSD accelerates the work considerably, allowing greater access to files and faster
processing of information, which is vital in working with large data or complex applications.
Software Requirements: Operating System: Latest version of Windows, macOS or Linux keeping
your operating system up-to-date ensures that it’s compatible with the new python versions and
development tools.
We need Python version 3.7 or higher. Download the latest version from the official Download Python |
Python.org
Jupyter Notebook: Install via Anaconda or pip.
Anaconda version – The latest version of Anaconda Navigator is 2.5.0, which is included in the
Anaconda Distribution 2023.09 release.
Downlods and install Anaconda from https://repo.anaconda.com/archive/Anaconda3-2022.05-Windows
x86_64.exe. Open "Anaconda Prompt" by finding it in the windows (start) Menu.
import tkinter as tk
from tkinter import messagebox
import json

class TodoApp:
    def __init__(self, root):
        self.root = root
        self.root.title("To-Do List Application")
        self.root.geometry("400x500")
        
        # Initialize an empty list to hold tasks
        self.tasks = []
        self.load_tasks()  # Load tasks from file on startup

        # Create UI components
        self.create_widgets()

    def create_widgets(self):
        # Label for the task entry
        self.task_label = tk.Label(self.root, text="Enter a new task:")
        self.task_label.pack(pady=10)

        # Entry for task input
        self.task_entry = tk.Entry(self.root, font=("Arial", 14))
        self.task_entry.pack(pady=10)

        # Add task button
        self.add_task_button = tk.Button(self.root, text="Add Task", command=self.add_task, width=15)
        self.add_task_button.pack(pady=5)

        # Mark task as completed button
        self.mark_task_button = tk.Button(self.root, text="Mark as Completed", command=self.mark_task_completed, width=15)
        self.mark_task_button.pack(pady=5)

        # Remove task button
        self.remove_task_button = tk.Button(self.root, text="Remove Task", command=self.remove_task, width=15)
        self.remove_task_button.pack(pady=5)

        # Listbox to display tasks
        self.task_listbox = tk.Listbox(self.root, font=("Arial", 12), selectmode=tk.SINGLE)
        self.task_listbox.pack(expand=True, fill='both', pady=10)

        # Update the Listbox with current tasks
        self.update_task_listbox()

    def add_task(self):
        """Add a task to the list."""
        task = self.task_entry.get().strip()
        if not task:
            messagebox.showerror("Input Error", "Please enter a task.")
            return
        self.tasks.append({"task": task, "completed": False})
        self.update_task_listbox()
        self.save_tasks()  # Save tasks to file
        self.task_entry.delete(0, tk.END)  # Clear input field

    def mark_task_completed(self):
        """Mark the selected task as completed."""
        try:
            selected_index = self.task_listbox.curselection()[0]
            self.tasks[selected_index]["completed"] = True
            self.update_task_listbox()
            self.save_tasks()
        except IndexError:
            messagebox.showerror("Selection Error", "Please select a task to mark as completed.")

    def remove_task(self):
        """Remove the selected task from the list."""
        try:
            selected_index = self.task_listbox.curselection()[0]
            del self.tasks[selected_index]
            self.update_task_listbox()
            self.save_tasks()  # Save tasks to file
        except IndexError:
            messagebox.showerror("Selection Error", "Please select a task to remove.")

    def update_task_listbox(self):
        """Update the Listbox to reflect the current tasks."""
        self.task_listbox.delete(0, tk.END)  # Clear current list
        for task in self.tasks:
            display_text = task["task"] + (" ✔" if task["completed"] else "")
            self.task_listbox.insert(tk.END, display_text)

    def save_tasks(self):
        """Save the current tasks to a file."""
        with open("tasks.json", "w") as f:
            json.dump(self.tasks, f)

    def load_tasks(self):
        """Load tasks from a file at startup."""
        try:
            with open("tasks.json", "r") as f:
                self.tasks = json.load(f)
        except (FileNotFoundError, json.JSONDecodeError):
            self.tasks = []

# Create the root Tkinter window
root = tk.Tk()

# Create an instance of the TodoApp
todo_app = TodoApp(root)

# Start the Tkinter main loop
root.mainloop()

Explanation of the Code
Main Application Window:

The main Tkinter window is initialized with a title and size.
UI Components:

Label: Prompts the user to enter a task.
Entry: An entry field for the user to type in new tasks.
Buttons:
"Add Task" button to add the entered task.
"Mark as Completed" button to mark a selected task as completed.
"Remove Task" button to remove the selected task.
Listbox: Displays all tasks in the list, with completed tasks visually marked.
Task Management Functions:

add_task(): Adds the task to self.tasks list and saves it.
mark_task_completed(): Marks a selected task as completed, adding a checkmark symbol next to it.
remove_task(): Deletes the selected task.
Persistent Storage:

save_tasks(): Saves tasks to a JSON file (tasks.json).
load_tasks(): Loads tasks from tasks.json at startup to keep tasks persistent.
Listbox Updating:

update_task_listbox(): Clears and refreshes the Listbox to reflect changes in self.tasks.
