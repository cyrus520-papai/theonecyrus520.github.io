import json
from pathlib import Path

FILE = Path("tasks.json")

def load_tasks():
    if not FILE.exists():
        return []
    try:
        with FILE.open("r", encoding="utf-8") as f:
            data = json.load(f)
            return data if isinstance(data, list) else []
    except json.JSONDecodeError:
        return []

def save_tasks(tasks):
    with FILE.open("w", encoding="utf-8") as f:
        json.dump(tasks, f, indent=2)

def add_task(title):
    tasks = load_tasks()
    tasks.append({"title": title, "done": False})
    save_tasks(tasks)
    print(f"Added: {title}")

def list_tasks():
    tasks = load_tasks()
    if not tasks:
        print("No tasks yet.")
        return

    for i, task in enumerate(tasks, start=1):
        status = "✓" if task.get("done") else " "
        print(f"{i}. [{status}] {task['title']}")

def toggle_task(index):
    tasks = load_tasks()
    if 1 <= index <= len(tasks):
        tasks[index - 1]["done"] = not tasks[index - 1]["done"]
        save_tasks(tasks)
        print("Updated task status.")
    else:
        print("Invalid task number.")

def remove_task(index):
    tasks = load_tasks()
    if 1 <= index <= len(tasks):
        removed = tasks.pop(index - 1)
        save_tasks(tasks)
        print(f"Removed: {removed['title']}")
    else:
        print("Invalid task number.")

def menu():
    while True:
        print("\nTo-Do List")
        print("1. Add task")
        print("2. List tasks")
        print("3. Mark done / undo")
        print("4. Remove task")
        print("5. Exit")

        choice = input("Choose an option: ").strip()

        if choice == "1":
            title = input("Task title: ").strip()
            if title:
                add_task(title)
            else:
                print("Task title cannot be empty.")

        elif choice == "2":
            list_tasks()

        elif choice == "3":
            index = int(input("Task number: "))
            toggle_task(index)

# theonecyrus520.github.io
