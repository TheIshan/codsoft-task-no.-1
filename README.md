import tkinter as tk
from tkinter import messagebox
import json
import os

TASKS_FILE = 'tasks.json'

class TodoApp:
    def __init__(self, root):
        self.root = root
        self.root.title("To-Do List Application")

        self.tasks = self.load_tasks()

        self.frame = tk.Frame(self.root)
        self.frame.pack(pady=10)

        self.task_listbox = tk.Listbox(self.frame, width=50, height=10)
        self.task_listbox.pack(side=tk.LEFT, fill=tk.BOTH)

        self.scrollbar = tk.Scrollbar(self.frame, orient=tk.VERTICAL)
        self.scrollbar.config(command=self.task_listbox.yview)
        self.scrollbar.pack(side=tk.RIGHT, fill=tk.Y)

        self.task_listbox.config(yscrollcommand=self.scrollbar.set)

        self.entry = tk.Entry(self.root, width=50)
        self.entry.pack(pady=10)

        self.add_button = tk.Button(self.root, text="Add Task", command=self.add_task)
        self.add_button.pack(pady=5)

        self.update_button = tk.Button(self.root, text="Update Task", command=self.update_task)
        self.update_button.pack(pady=5)

        self.populate_listbox()

    def load_tasks(self):
        if os.path.exists(TASKS_FILE):
            with open(TASKS_FILE, 'r') as file:
                return json.load(file)
        return []

    def save_tasks(self):
        with open(TASKS_FILE, 'w') as file:
            json.dump(self.tasks, file, indent=4)

    def add_task(self):
        task_description = self.entry.get()
        if task_description:
            self.tasks.append({'description': task_description, 'completed': False})
            self.save_tasks()
            self.populate_listbox()
            self.entry.delete(0, tk.END)
        else:
            messagebox.showwarning("Warning", "You must enter a task description.")

    def update_task(self):
        selected_task_index = self.task_listbox.curselection()
        if selected_task_index:
            task_index = selected_task_index[0]
            self.tasks[task_index]['completed'] = not self.tasks[task_index]['completed']
            self.save_tasks()
            self.populate_listbox()
        else:
            messagebox.showwarning("Warning", "You must select a task to update.")

    def populate_listbox(self):
        self.task_listbox.delete(0, tk.END)
        for task in self.tasks:
            status = 'Completed' if task['completed'] else 'Pending'
            self.task_listbox.insert(tk.END, f"{task['description']} - {status}")

if __name__ == "__main__":
    root = tk.Tk()
    app = TodoApp(root)
    root.mainloop()
