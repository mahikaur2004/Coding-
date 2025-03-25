import tkinter as tk
from tkinter import ttk

# Function to toggle checkboxes
def toggle_task(index):
    if task_status[index].get() == 1:
        checkboxes[index].config(style="Checked.TCheckbutton")
    else:
        checkboxes[index].config(style="Unchecked.TCheckbutton")

# Function to retrieve and print selected data
def retrieve_data():
    print("\nRetrieving Data...")
    for i, task in enumerate(tasks):
        status = "✔ Done" if task_status[i].get() == 1 else "❌ Not Done"
        selected_time = time_vars[i].get() or "No Time Set"
        print(f"{task}: {status} | Time: {selected_time}")

# Create the main window
root = tk.Tk()
root.title("To-Do List with Time Selector")
root.geometry("400x350")
root.configure(bg="#141414")  # Dark background

# Define styles
style = ttk.Style()
style.configure("TFrame", background="#141414")
style.configure("TLabel", background="#141414", foreground="white", font=("Arial", 12, "bold"))
style.configure("Checked.TCheckbutton", background="#141414", foreground="white")
style.configure("Unchecked.TCheckbutton", background="#141414", foreground="white")
style.configure("TCombobox", fieldbackground="#333", background="#222", foreground="white")

# Frame for To-Do List
frame = ttk.Frame(root, padding=10, style="TFrame")
frame.pack(pady=10)

# Title
title_label = ttk.Label(frame, text="TO - DO LIST", style="TLabel", font=("Arial", 14, "bold"))
title_label.grid(row=0, column=1, columnspan=2, pady=10)

# Tasks List
tasks = ["DRINK COFFEE", "MAKE THE BED", "TAKE A SHOWER", "CHECK E-MAILS"]
task_status = [tk.IntVar() for _ in tasks]
time_vars = [tk.StringVar() for _ in tasks]

checkboxes = []
time_selectors = []

# Creating checkboxes and time selection
for i, task in enumerate(tasks):
    task_status[i].set(0)  # Initially unchecked

    # Checkbox for task completion
    checkbox = ttk.Checkbutton(frame, text=task, variable=task_status[i],
                               command=lambda i=i: toggle_task(i),
                               style="Unchecked.TCheckbutton")
    checkbox.grid(row=i + 1, column=0, padx=5, pady=5, sticky="w")
    checkboxes.append(checkbox)

    # Clock icon label (Added inside the grid properly)
    clock_label = ttk.Label(frame, text="🕒", background="#141414", foreground="white", font=("Arial", 12))
    clock_label.grid(row=i + 1, column=1, padx=5, pady=5)

    # Time selection dropdown (Adding a larger width for better visibility)
    time_options = [f"{h:02d}:{m:02d}" for h in range(24) for m in (0, 30)]  # Every 30 minutes
    time_selector = ttk.Combobox(frame, textvariable=time_vars[i], values=time_options, width=7, state="readonly")
    time_selector.grid(row=i + 1, column=2, padx=5, pady=5)
    time_selectors.append(time_selector)

# Retrieve Data Button
retrieve_button = tk.Button(root, text="RETRIEVE DATA", bg="#141414", fg="white",
                            font=("Arial", 10, "bold"), borderwidth=0, command=retrieve_data)
retrieve_button.pack(pady=10)

# Run the main loop
root.mainloop()
