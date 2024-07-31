import tkinter as tk
from tkinter import messagebox, simpledialog
from tkinter import ttk
import csv

# Створення головного вікна додатка
My_window = tk.Tk()
My_window.title("Телефонна книга")
My_window.geometry("800x300")
My_window["bg"] = "#BEBEBE"

# Інструкція користувача
instructions = """
Інструкція до користування:

Звідомте про додавання контакту, введіть ПІБ, адресу,
електронну пошту та мобільний телефон, а потім клацніть
"Додати контакт" для підтвердження. Після успішного
додавання з'явиться повідомлення про успіх. Для перегляду
контактів клацніть "Переглянути контакти" та використовуйте
кнопки "Наступний" і "Попередній" для навігації. Щоб
видалити контакт, введіть ПІБ і натисніть "Видалити контакт".
Для пошуку введіть ПІБ або номер телефону та клацніть
"Пошук контакту". На кінець, щоб редагувати контакт,
введіть ПІБ, внесіть зміни і клацніть "Зберегти зміни".
Будь ласка, дотримуйтесь інструкцій для коректного використання додатку.
"""

# Функція для відображення інструкції
def show_instructions():
    instructions_window = tk.Toplevel(My_window)
    instructions_window.title("Інструкція")
    instructions_window.geometry("750x380")
    instructions_window["bg"] = "#dcdcdc"  # Сірий фон
    instructions_label = tk.Label(instructions_window, text=instructions, font=("Times New Roman", 12), bg="#dcdcdc", justify=tk.LEFT, anchor="w")
    instructions_label.pack(padx=10, pady=10)

# Функція для додавання контакту
def add_contact():
    name = name_entry.get()
    address = address_entry.get()
    email = email_entry.get()
    mobile_phone = mobile_entry.get()

    # Перевірка наявності символу "@" в електронній пошті
    if "@" not in email:
        messagebox.showerror("Помилка", "Електронна пошта має містити символ '@'")
        return

    # Запис нових даних у файл CSV
    with open('contacts.csv', mode='a', newline='', encoding='utf-8') as file:
        writer = csv.writer(file)
        writer.writerow([name, address, email, mobile_phone])

    # Виведення повідомлення про успішне додавання контакту
    messagebox.showinfo("Доданий контакт", "Контакт успішно доданий")

# Функція для перегляду контактів
def view_contacts():
    # Читання даних з файлу CSV
    with open('contacts.csv', mode='r', encoding='utf-8') as file:
        reader = csv.reader(file)
        contacts = list(reader)
        contacts.sort(key=lambda x: x[0])  # Сортування за ім'ям

    # Відображення контактів або повідомлення, якщо контактів немає
    if contacts:
        show_contact_dialog(contacts)
    else:
        messagebox.showinfo("Контакти", "Контактів не знайдено")

# Функція для відображення діалогу з контактами
def show_contact_dialog(contacts):
    current_contact = 0

    def show_contact():
        if 0 <= current_contact < len(contacts):
            contact = contacts[current_contact]
            for widget in contact_frame.winfo_children():
                widget.destroy()
            tk.Label(contact_frame, text=f"ПІБ: {contact[0]}", font=("Times New Roman", 14), bg="#f0f0f0").pack(padx=10, pady=5)
            tk.Label(contact_frame, text=f"Адреса: {contact[1]}", font=("Times New Roman", 14), bg="#f0f0f0").pack(padx=10, pady=5)
            tk.Label(contact_frame, text=f"Електронна пошта: {contact[2]}", font=("Times New Roman", 14), bg="#f0f0f0").pack(padx=10, pady=5)
            tk.Label(contact_frame, text=f"Мобільний телефон: {contact[3]}", font=("Times New Roman", 14), bg="#f0f0f0").pack(padx=10, pady=5)

    def show_next_contact():
        nonlocal current_contact
        if current_contact < len(contacts) - 1:
            current_contact += 1
            show_contact()

    def show_previous_contact():
        nonlocal current_contact
        if current_contact > 0:
            current_contact -= 1
            show_contact()

    contact_window = tk.Toplevel(My_window)
    contact_window.title("Контакти")
    contact_window.geometry("500x300")
    contact_window["bg"] = "#dcdcdc"

    contact_frame = tk.Frame(contact_window, bg="#f0f0f0")
    contact_frame.pack(expand=True, fill=tk.BOTH)

    button_frame = tk.Frame(contact_window, bg="#dcdcdc")
    button_frame.pack()

    previous_button = ttk.Button(button_frame, text="назад", command=show_previous_contact)
    previous_button.grid(row=0, column=0, padx=5, pady=5)

    next_button = ttk.Button(button_frame, text="вперед", command=show_next_contact)
    next_button.grid(row=0, column=1, padx=5, pady=5)

    show_contact()

# Функція для видалення контакту
def delete_contact():
    name_to_delete = simpledialog.askstring("Видалення контакту", "Введіть ПІБ контакту, якого бажаєте видалити:")

    # Читання контактів з файлу CSV
    with open('contacts.csv', mode='r', encoding='utf-8') as file:
        reader = csv.reader(file)
        contacts = list(reader)

    # Видалення контакту з списку
    updated_contacts = [contact for contact in contacts if contact[0] != name_to_delete]

    # Запис оновлених контактів у файл CSV
    with open('contacts.csv', mode='w', newline='', encoding='utf-8') as file:
        writer = csv.writer(file)
        writer.writerows(updated_contacts)

    messagebox.showinfo("Видалення", "Контакт успішно видалений")

# Функція для пошуку контакту
def search_contact():
    search_param = simpledialog.askstring("Пошук контакту", "Введіть ПІБ або номер телефону для пошуку:")

    # Читання контактів з файлу CSV
    with open('contacts.csv', mode='r', encoding='utf-8') as file:
        reader = csv.reader(file)
        contacts = list(reader)

    # Пошук контактів, що відповідають критеріям пошуку
    found_contacts = [contact for contact in contacts if search_param in contact]

    # Відображення знайдених контактів або повідомлення, якщо контакти не знайдено
    if found_contacts:
        show_contact_dialog(found_contacts)
    else:
        messagebox.showinfo("Пошук контакту", "Контакт не знайдений")

# Функція для редагування контакту
def edit_contact():
    name_to_edit = simpledialog.askstring("Редагування контакту", "Введіть ПІБ контакту, якого бажаєте відредагувати:")

    # Читання контактів з файлу CSV
    with open('contacts.csv', mode='r', encoding='utf-8') as file:
        reader = csv.reader(file)
        contacts = list(reader)

    # Пошук та заповнення полів для редагування контакту
    for contact in contacts:
        if contact[0] == name_to_edit:
            name_entry.delete(0, tk.END)
            name_entry.insert(0, contact[0])
            address_entry.delete(0, tk.END)
            address_entry.insert(0, contact[1])
            email_entry.delete(0, tk.END)
            email_entry.insert(0, contact[2])
            mobile_entry.delete(0, tk.END)
            mobile_entry.insert(0, contact[3])
            break
    else:
        messagebox.showinfo("Пошук контакту", "Контакт не знайдений")

    def save_changes():
        new_name = name_entry.get()
        new_address = address_entry.get()
        new_email = email_entry.get()
        new_mobile_phone = mobile_entry.get()

        updated_contacts = []
        for contact in contacts:
            if contact[0] == name_to_edit:
                updated_contacts.append([new_name, new_address, new_email, new_mobile_phone])
            else:
                updated_contacts.append(contact)

        # Запис оновлених контактів у файл CSV
        with open('contacts.csv', mode='w', newline='', encoding='utf-8') as file:
            writer = csv.writer(file)
            writer.writerows(updated_contacts)

        messagebox.showinfo("Оновлення", "Інформація про контакт успішно оновлена")

    # Створення кнопки для збереження змін
    save_button = ttk.Button(My_window, text="Зберегти зміни", style="TButton", command=save_changes)
    save_button.grid(row=9, column=2, padx=10, pady=5)

# Створення віджетів для введення даних
tk.Label(My_window, text="ПІБ:", font=("Times New Roman", 14, "bold"), fg="black", bg="#f0f0f0").grid(row=1, column=1, padx=10, pady=5)
tk.Label(My_window, text="Адреса:", font=("Times New Roman", 14, "bold"), fg="black", bg="#f0f0f0").grid(row=2, column=1, padx=10, pady=5)
tk.Label(My_window, text="Електронна пошта:", font=("Times New Roman", 14, "bold"), fg="black", bg="#f0f0f0").grid(row=3, column=1, padx=10, pady=5)
tk.Label(My_window, text="Мобільний телефон:", font=("Times New Roman", 14, "bold"), fg="black", bg="#f0f0f0").grid(row=4, column=1, padx=10, pady=5)

name_entry = tk.Entry(My_window, font=("Times New Roman", 11), width=30)
name_entry.grid(row=1, column=2, padx=10, pady=5)
address_entry = tk.Entry(My_window, font=("Times New Roman", 11), width=30)
address_entry.grid(row=2, column=2, padx=10, pady=5)
email_entry = tk.Entry(My_window, font=("Times New Roman", 11), width=30)
email_entry.grid(row=3, column=2, padx=10, pady=5)
mobile_entry = tk.Entry(My_window, font=("Times New Roman", 11), width=30)
mobile_entry.grid(row=4, column=2, padx=10, pady=5)

# Створення рамки для кнопок в лівій частині
button_frame = tk.Frame(My_window, bg="#f0f0f0")
button_frame.grid(row=0, column=0, rowspan=6, padx=10, pady=5, sticky="nw")

# Створення кнопок для різних дій
add_button = ttk.Button(button_frame, text="Додати контакт", style="TButton", command=add_contact)
add_button.grid(row=0, column=0, padx=10, pady=5)

view_button = ttk.Button(button_frame, text="Переглянути контакти", style="TButton", command=view_contacts)
view_button.grid(row=1, column=0, padx=10, pady=5)

delete_button = ttk.Button(button_frame, text="Видалити контакт", style="TButton", command=delete_contact)
delete_button.grid(row=2, column=0, padx=10, pady=5)

search_button = ttk.Button(button_frame, text="Пошук контакту", style="TButton", command=search_contact)
search_button.grid(row=3, column=0, padx=10, pady=5)

edit_button = ttk.Button(button_frame, text="Редагувати контакт", style="TButton", command=edit_contact)
edit_button.grid(row=4, column=0, padx=10, pady=5)

# Додати кнопку інструкції у правий верхній кут
instruction_button = ttk.Button(My_window, text="Довідка", style="TButton", command=show_instructions, width=16)
instruction_button.grid(row=0, column=2, padx=10, pady=5, sticky="ne")

# Оновлення стилів кнопок
style = ttk.Style()
style.configure("TButton",
                foreground="black",  # Колір тексту кнопки
                background="#d3d3d3",  # Світло-сірий колір фону кнопки
                font=("Times New Roman", 12, "bold"),  # Шрифт кнопки
                padding=4,  # Відступи кнопки
                relief=tk.RAISED,  # Стиль кнопки
                )

# Запуск головного циклу програми
My_window.mainloop()
