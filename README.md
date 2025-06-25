import tkinter as tk
from tkinter import messagebox, simpledialog, ttk

# --- royal color palette ---
ROYAL_BLUE = "#000080"      # Navy Blue
ROYAL_PURPLE = "#800080"    # Purple
GOLD_ACCENT = "#FFD700"     # Gold
LIGHT_GOLD = "#DAA520"      # Goldenrod (For lighter accents/text)
WHITE = "#FFFFFF"           # White
LIGHT_GREY = "#D3D3D3"      # Light Grey (for some backgrounds/borders)
DARK_GREY = "#36454F"       # Charcoal (for text on light backgrounds)
BLACK = "#000000"           # Black

# --- Simulated In-Memory "Database" ---
# Data will be lost when the application closes

student_data = []
employee_data = []
fees_data = []
attendance_data = []
school_data = []

# --- Helper Functions for Simulated Data Operations (No Change from previous version) ---

def get_students():
    return student_data

def add_student(name, admno, clas, city, dob):
    if any(s['admno'] == admno for s in student_data):
        messagebox.showerror("Error", f"Student with Admission No. {admno} already exists.")
        return False
    student_data.append({'name': name, 'admno': admno, 'class': clas, 'city': city, 'dob': dob})
    return True

def update_student(admno, new_class):
    for student in student_data:
        if student['admno'] == admno:
            student['class'] = new_class
            return True
    return False

def delete_student(admno):
    global student_data
    initial_len = len(student_data)
    student_data = [s for s in student_data if s['admno'] != admno]
    return len(student_data) < initial_len

def get_employees():
    return employee_data

def add_employee(empno, name, job, hiredate):
    if any(e['empno'] == empno for e in employee_data):
        messagebox.showerror("Error", f"Employee with Employee No. {empno} already exists.")
        return False
    employee_data.append({'empno': empno, 'name': name, 'job': job, 'hiredate': hiredate})
    return True

def update_employee(empno, new_job):
    for employee in employee_data:
        if employee['empno'] == empno:
            employee['job'] = new_job
            return True
    return False

def delete_employee(empno):
    global employee_data
    initial_len = len(employee_data)
    employee_data = [e for e in employee_data if e['empno'] != empno]
    return len(employee_data) < initial_len

def get_fees():
    return fees_data

def add_fee(admno, fees, monthunpaid):
    if any(f['admno'] == admno for f in fees_data):
        messagebox.showerror("Error", f"Fees for Admission No. {admno} already exist. Please update existing record.")
        return False
    fees_data.append({'admno': admno, 'fees': fees, 'monthunpaid': monthunpaid})
    return True

def update_fee(admno, new_monthunpaid):
    for fee in fees_data:
        if fee['admno'] == admno:
            fee['monthunpaid'] = new_monthunpaid
            return True
    return False

def delete_fee(admno):
    global fees_data
    initial_len = len(fees_data)
    fees_data = [f for f in fees_data if f['admno'] != admno]
    return len(fees_data) < initial_len

def get_attendance():
    return attendance_data

def add_attendance(admno, name, present, totalpresent, per):
    if any(a['admno'] == admno for a in attendance_data):
        messagebox.showerror("Error", f"Attendance for Admission No. {admno} already exists. Please update existing record.")
        return False
    attendance_data.append({'admno': admno, 'name': name, 'present': present, 'totalpresent': totalpresent, 'per': per})
    return True

def update_attendance(admno, new_present, new_per):
    for att in attendance_data:
        if att['admno'] == admno:
            att['present'] = new_present
            att['per'] = new_per
            return True
    return False

def delete_attendance(admno):
    global attendance_data
    initial_len = len(attendance_data)
    attendance_data = [a for a in attendance_data if a['admno'] != admno]
    return len(attendance_data) < initial_len

def get_school_details():
    return school_data

def add_school_detail(sid, sname, noofstudent, noofemployee, nooflabs):
    if any(s['id'] == sid for s in school_data):
        messagebox.showerror("Error", f"School details for ID {sid} already exist. Please update existing record.")
        return False
    school_data.append({'id': sid, 'sname': sname, 'noofstudent': noofstudent, 'noofemployee': noofemployee, 'nooflabs': nooflabs})
    return True

def update_school_detail(sid, noofstudent, noofemployee, nooflabs):
    for school in school_data:
        if school['id'] == sid:
            school['noofstudent'] = noofstudent
            school['noofemployee'] = noofemployee
            school['nooflabs'] = nooflabs
            return True
    return False

def delete_school_detail(sid):
    global school_data
    initial_len = len(school_data)
    school_data = [s for s in school_data if s['id'] != sid]
    return len(school_data) < initial_len

# --- GUI Functions with Royal Color Grading ---

class SchoolManagementGUI:
    def __init__(self, master):
        self.master = master
        master.title("Royal School Management System")
        master.geometry("800x600")
        master.config(bg=ROYAL_BLUE) # Main window background

        # Style for buttons
        s = ttk.Style()
        s.theme_use('default') # Use 'default' or 'clam' for more customization
        s.configure('TButton',
                    background=ROYAL_PURPLE,
                    foreground=GOLD_ACCENT,
                    font=("Arial", 12, "bold"),
                    padding=10,
                    borderwidth=2,
                    relief="raised")
        s.map('TButton',
              background=[('active', LIGHT_GOLD)],
              foreground=[('active', ROYAL_BLUE)]) # Change text color on hover

        # Style for labels
        s.configure('TLabel',
                    background=ROYAL_BLUE,
                    foreground=GOLD_ACCENT,
                    font=("Arial", 16, "bold"))

        self.label = ttk.Label(master, text="WELCOME TO ROYAL SCHOOL MANAGEMENT")
        self.label.pack(pady=20)

        # Using ttk.Button for better styling control
        self.student_button = ttk.Button(master, text="1. Student Management", command=self.open_student_management, width=30)
        self.student_button.pack(pady=5)

        self.employee_button = ttk.Button(master, text="2. Employee Management", command=self.open_employee_management, width=30)
        self.employee_button.pack(pady=5)

        self.fee_button = ttk.Button(master, text="3. Display Fee", command=self.open_fee_management, width=30)
        self.fee_button.pack(pady=5)

        self.attendance_button = ttk.Button(master, text="4. Attendance Management", command=self.open_attendance_management, width=30)
        self.attendance_button.pack(pady=5)

        self.school_details_button = ttk.Button(master, text="5. Display School Details", command=self.open_school_details_management, width=30)
        self.school_details_button.pack(pady=5)

        self.exit_button = ttk.Button(master, text="Exit", command=master.quit, width=30,
                                     style='Exit.TButton') # Custom style for Exit button

        s.configure('Exit.TButton',
                    background=DARK_GREY,
                    foreground=WHITE,
                    font=("Arial", 12, "bold"))
        s.map('Exit.TButton',
              background=[('active', BLACK)],
              foreground=[('active', GOLD_ACCENT)])

        self.exit_button.pack(pady=20)

    def open_student_management(self):
        StudentManagementWindow(self.master)

    def open_employee_management(self):
        EmployeeManagementWindow(self.master)

    def open_fee_management(self):
        FeeManagementWindow(self.master)

    def open_attendance_management(self):
        AttendanceManagementWindow(self.master)

    def open_school_details_management(self):
        SchoolDetailsManagementWindow(self.master)

# Base class for common window configurations
class RoyalToplevel(tk.Toplevel):
    def __init__(self, master, title):
        super().__init__(master)
        self.title(title)
        self.geometry("800x600")
        self.config(bg=LIGHT_GREY) # Consistent background for sub-windows

        # Style for general labels in sub-windows
        s = ttk.Style()
        s.configure('WindowLabel.TLabel',
                    background=LIGHT_GREY,
                    foreground=ROYAL_BLUE,
                    font=("Arial", 14, "bold"))

        # Style for Treeview headings and rows
        s.configure('Treeview.Heading',
                    background=ROY_BLUE, # Using ROYAL_BLUE for headings
                    foreground=GOLD_ACCENT,
                    font=("Arial", 10, "bold"))
        s.configure('Treeview',
                    background=WHITE,
                    foreground=DARK_GREY,
                    rowheight=25,
                    fieldbackground=WHITE)
        s.map('Treeview',
              background=[('selected', ROYAL_PURPLE)],
              foreground=[('selected', WHITE)])

        # Style for Entry fields (for dialogs)
        s.configure('TEntry',
                    fieldbackground=WHITE,
                    foreground=DARK_GREY,
                    font=("Arial", 10))

        self.label = ttk.Label(self, text=f"WELCOME TO {title.upper()}", style='WindowLabel.TLabel')
        self.label.pack(pady=10)

        self.setup_buttons()
        self.setup_treeview()
        self.refresh_display()

    def setup_buttons(self):
        # Override in subclasses
        pass

    def setup_treeview(self):
        # Override in subclasses
        pass

    def refresh_display(self):
        # Override in subclasses
        pass

# Inherit from RoyalToplevel to apply common styles
class StudentManagementWindow(RoyalToplevel):
    def __init__(self, master):
        super().__init__(master, "Student Management")

    def setup_buttons(self):
        self.add_button = ttk.Button(self, text="New Admission", command=self.add_student_gui)
        self.add_button.pack(pady=5)

        self.update_button = ttk.Button(self, text="Update Details", command=self.update_student_gui)
        self.update_button.pack(pady=5)

        self.delete_button = ttk.Button(self, text="Issue TC (Delete Student)", command=self.delete_student_gui)
        self.delete_button.pack(pady=5)

    def setup_treeview(self):
        self.tree = ttk.Treeview(self, columns=("Adm No", "Name", "Class", "City", "DOB"), show="headings")
        self.tree.heading("Adm No", text="Admission No.")
        self.tree.heading("Name", text="Name")
        self.tree.heading("Class", text="Class")
        self.tree.heading("City", text="City")
        self.tree.heading("DOB", text="Date of Birth")
        self.tree.pack(pady=10, fill=tk.BOTH, expand=True)

    def refresh_display(self):
        for i in self.tree.get_children():
            self.tree.delete(i)
        for student in get_students():
            self.tree.insert("", "end", values=(student['admno'], student['name'], student['class'], student['city'], student['dob']))

    def add_student_gui(self):
        dialog = tk.Toplevel(self)
        dialog.title("New Admission")
        dialog.geometry("300x250")
        dialog.config(bg=LIGHT_GREY)

        tk.Label(dialog, text="Name:", bg=LIGHT_GREY, fg=DARK_GREY).pack(pady=2)
        name_entry = ttk.Entry(dialog, style='TEntry')
        name_entry.pack(pady=2)

        tk.Label(dialog, text="Admission No.:", bg=LIGHT_GREY, fg=DARK_GREY).pack(pady=2)
        admno_entry = ttk.Entry(dialog, style='TEntry')
        admno_entry.pack(pady=2)

        tk.Label(dialog, text="Class:", bg=LIGHT_GREY, fg=DARK_GREY).pack(pady=2)
        class_entry = ttk.Entry(dialog, style='TEntry')
        class_entry.pack(pady=2)

        tk.Label(dialog, text="City:", bg=LIGHT_GREY, fg=DARK_GREY).pack(pady=2)
        city_entry = ttk.Entry(dialog, style='TEntry')
        city_entry.pack(pady=2)

        tk.Label(dialog, text="Date of Birth (YYYY-MM-DD):", bg=LIGHT_GREY, fg=DARK_GREY).pack(pady=2)
        dob_entry = ttk.Entry(dialog, style='TEntry')
        dob_entry.pack(pady=2)

        def save_student():
            try:
                name = name_entry.get()
                admno = int(admno_entry.get())
                clas = class_entry.get()
                city = city_entry.get()
                dob = dob_entry.get()
                if add_student(name, admno, clas, city, dob):
                    messagebox.showinfo("Success", "Student added successfully!")
                    self.refresh_display()
                    dialog.destroy()
                else:
                    messagebox.showerror("Error", "Failed to add student. Admission number might exist.")
            except ValueError:
                messagebox.showerror("Error", "Please enter valid data. Admission number must be an integer.")

        ttk.Button(dialog, text="Save", command=save_student).pack(pady=10)

    def update_student_gui(self):
        admno_str = simpledialog.askstring("Update Student", "Enter Admission No. to update:",
                                           parent=self, initialvalue="") # Use initialvalue to prevent None
        if admno_str:
            try:
                admno = int(admno_str)
                new_class = simpledialog.askstring("Update Student", "Enter New Class:",
                                                  parent=self, initialvalue="")
                if new_class:
                    if update_student(admno, new_class):
                        messagebox.showinfo("Success", "Student details updated successfully!")
                        self.refresh_display()
                    else:
                        messagebox.showerror("Error", "Student with given Admission No. not found.")
                else:
                    messagebox.showwarning("Warning", "New Class cannot be empty.")
            except ValueError:
                messagebox.showerror("Error", "Invalid Admission No. Please enter an integer.")

    def delete_student_gui(self):
        admno_str = simpledialog.askstring("Delete Student", "Enter Admission No. to delete:",
                                           parent=self, initialvalue="")
        if admno_str:
            try:
                admno = int(admno_str)
                if messagebox.askyesno("Confirm Deletion", f"Are you sure you want to delete student with Admission No. {admno}?"):
                    if delete_student(admno):
                        messagebox.showinfo("Success", "Student deleted successfully!")
                        self.refresh_display()
                    else:
                        messagebox.showerror("Error", "Student with given Admission No. not found.")
            except ValueError:
                messagebox.showerror("Error", "Invalid Admission No. Please enter an integer.")

class EmployeeManagementWindow(RoyalToplevel):
    def __init__(self, master):
        super().__init__(master, "Employee Management")

    def setup_buttons(self):
        self.add_button = ttk.Button(self, text="New Employee", command=self.add_employee_gui)
        self.add_button.pack(pady=5)

        self.update_button = ttk.Button(self, text="Update Staff Details", command=self.update_employee_gui)
        self.update_button.pack(pady=5)

        self.delete_button = ttk.Button(self, text="Delete Employee", command=self.delete_employee_gui)
        self.delete_button.pack(pady=5)

    def setup_treeview(self):
        self.tree = ttk.Treeview(self, columns=("Emp No", "Name", "Job", "Hire Date"), show="headings")
        self.tree.heading("Emp No", text="Employee No.")
        self.tree.heading("Name", text="Name")
        self.tree.heading("Job", text="Designation")
        self.tree.heading("Hire Date", text="Hire Date")
        self.tree.pack(pady=10, fill=tk.BOTH, expand=True)

    def refresh_display(self):
        for i in self.tree.get_children():
            self.tree.delete(i)
        for employee in get_employees():
            self.tree.insert("", "end", values=(employee['empno'], employee['name'], employee['job'], employee['hiredate']))

    def add_employee_gui(self):
        dialog = tk.Toplevel(self)
        dialog.title("New Employee")
        dialog.geometry("300x250")
        dialog.config(bg=LIGHT_GREY)

        tk.Label(dialog, text="Employee No.:", bg=LIGHT_GREY, fg=DARK_GREY).pack(pady=2)
        empno_entry = ttk.Entry(dialog, style='TEntry')
        empno_entry.pack(pady=2)

        tk.Label(dialog, text="Name:", bg=LIGHT_GREY, fg=DARK_GREY).pack(pady=2)
        name_entry = ttk.Entry(dialog, style='TEntry')
        name_entry.pack(pady=2)

        tk.Label(dialog, text="Designation:", bg=LIGHT_GREY, fg=DARK_GREY).pack(pady=2)
        job_entry = ttk.Entry(dialog, style='TEntry')
        job_entry.pack(pady=2)

        tk.Label(dialog, text="Hire Date (YYYY-MM-DD):", bg=LIGHT_GREY, fg=DARK_GREY).pack(pady=2)
        hiredate_entry = ttk.Entry(dialog, style='TEntry')
        hiredate_entry.pack(pady=2)

        def save_employee():
            try:
                empno = int(empno_entry.get())
                name = name_entry.get()
                job = job_entry.get()
                hiredate = hiredate_entry.get()
                if add_employee(empno, name, job, hiredate):
                    messagebox.showinfo("Success", "Employee added successfully!")
                    self.refresh_display()
                    dialog.destroy()
                else:
                    messagebox.showerror("Error", "Failed to add employee. Employee number might exist.")
            except ValueError:
                messagebox.showerror("Error", "Please enter valid data. Employee number must be an integer.")

        ttk.Button(dialog, text="Save", command=save_employee).pack(pady=10)

    def update_employee_gui(self):
        empno_str = simpledialog.askstring("Update Employee", "Enter Employee No. to update:",
                                           parent=self, initialvalue="")
        if empno_str:
            try:
                empno = int(empno_str)
                new_job = simpledialog.askstring("Update Employee", "Enter New Designation:",
                                                parent=self, initialvalue="")
                if new_job:
                    if update_employee(empno, new_job):
                        messagebox.showinfo("Success", "Employee details updated successfully!")
                        self.refresh_display()
                    else:
                        messagebox.showerror("Error", "Employee with given Employee No. not found.")
                else:
                    messagebox.showwarning("Warning", "New Designation cannot be empty.")
            except ValueError:
                messagebox.showerror("Error", "Invalid Employee No. Please enter an integer.")

    def delete_employee_gui(self):
        empno_str = simpledialog.askstring("Delete Employee", "Enter Employee No. to delete:",
                                           parent=self, initialvalue="")
        if empno_str:
            try:
                empno = int(empno_str)
                if messagebox.askyesno("Confirm Deletion", f"Are you sure you want to delete employee with Employee No. {empno}?"):
                    if delete_employee(empno):
                        messagebox.showinfo("Success", "Employee deleted successfully!")
                        self.refresh_display()
                    else:
                        messagebox.showerror("Error", "Employee with given Employee No. not found.")
            except ValueError:
                messagebox.showerror("Error", "Invalid Employee No. Please enter an integer.")

class FeeManagementWindow(RoyalToplevel):
    def __init__(self, master):
        super().__init__(master, "Fees Management")

    def setup_buttons(self):
        self.add_button = ttk.Button(self, text="Insert Fees", command=self.add_fee_gui)
        self.add_button.pack(pady=5)

        self.update_button = ttk.Button(self, text="Update Fees", command=self.update_fee_gui)
        self.update_button.pack(pady=5)

        self.delete_button = ttk.Button(self, text="Exempt Fees (Delete)", command=self.delete_fee_gui)
        self.delete_button.pack(pady=5)

    def setup_treeview(self):
        self.tree = ttk.Treeview(self, columns=("Adm No", "Fees Amount", "Month Unpaid"), show="headings")
        self.tree.heading("Adm No", text="Admission No.")
        self.tree.heading("Fees Amount", text="Fees Amount")
        self.tree.heading("Month Unpaid", text="Month Unpaid")
        self.tree.pack(pady=10, fill=tk.BOTH, expand=True)

    def refresh_display(self):
        for i in self.tree.get_children():
            self.tree.delete(i)
        for fee in get_fees():
            self.tree.insert("", "end", values=(fee['admno'], fee['fees'], fee['monthunpaid']))

    def add_fee_gui(self):
        dialog = tk.Toplevel(self)
        dialog.title("Insert Fees")
        dialog.geometry("300x200")
        dialog.config(bg=LIGHT_GREY)

        tk.Label(dialog, text="Admission No.:", bg=LIGHT_GREY, fg=DARK_GREY).pack(pady=2)
        admno_entry = ttk.Entry(dialog, style='TEntry')
        admno_entry.pack(pady=2)

        tk.Label(dialog, text="Fees Amount:", bg=LIGHT_GREY, fg=DARK_GREY).pack(pady=2)
        fees_entry = ttk.Entry(dialog, style='TEntry')
        fees_entry.pack(pady=2)

        tk.Label(dialog, text="Month Unpaid:", bg=LIGHT_GREY, fg=DARK_GREY).pack(pady=2)
        month_entry = ttk.Entry(dialog, style='TEntry')
        month_entry.pack(pady=2)

        def save_fee():
            try:
                admno = int(admno_entry.get())
                fees = float(fees_entry.get())
                monthunpaid = month_entry.get()
                if add_fee(admno, fees, monthunpaid):
                    messagebox.showinfo("Success", "Fees added successfully!")
                    self.refresh_display()
                    dialog.destroy()
                else:
                    messagebox.showerror("Error", "Failed to add fees. Admission number might exist.")
            except ValueError:
                messagebox.showerror("Error", "Please enter valid data. Admission No. must be integer, Fees must be a number.")

        ttk.Button(dialog, text="Save", command=save_fee).pack(pady=10)

    def update_fee_gui(self):
        admno_str = simpledialog.askstring("Update Fees", "Enter Admission No. to update fees:",
                                           parent=self, initialvalue="")
        if admno_str:
            try:
                admno = int(admno_str)
                new_month = simpledialog.askstring("Update Fees", "Enter New Month of Unpaid Fees:",
                                                  parent=self, initialvalue="")
                if new_month:
                    if update_fee(admno, new_month):
                        messagebox.showinfo("Success", "Fees details updated successfully!")
                        self.refresh_display()
                    else:
                        messagebox.showerror("Error", "Fees record for given Admission No. not found.")
                else:
                    messagebox.showwarning("Warning", "New Month cannot be empty.")
            except ValueError:
                messagebox.showerror("Error", "Invalid Admission No. Please enter an integer.")

    def delete_fee_gui(self):
        admno_str = simpledialog.askstring("Exempt Fees", "Enter Admission No. to exempt fees:",
                                           parent=self, initialvalue="")
        if admno_str:
            try:
                admno = int(admno_str)
                if messagebox.askyesno("Confirm Exemption", f"Are you sure you want to exempt fees for Admission No. {admno}?"):
                    if delete_fee(admno):
                        messagebox.showinfo("Success", "Fees exempted successfully!")
                        self.refresh_display()
                    else:
                        messagebox.showerror("Error", "Fees record for given Admission No. not found.")
            except ValueError:
                messagebox.showerror("Error", "Invalid Admission No. Please enter an integer.")

class AttendanceManagementWindow(RoyalToplevel):
    def __init__(self, master):
        super().__init__(master, "Attendance Management")

    def setup_buttons(self):
        self.add_button = ttk.Button(self, text="Insert Details", command=self.add_attendance_gui)
        self.add_button.pack(pady=5)

        self.update_button = ttk.Button(self, text="Update Details", command=self.update_attendance_gui)
        self.update_button.pack(pady=5)

        self.delete_button = ttk.Button(self, text="Delete Details", command=self.delete_attendance_gui)
        self.delete_button.pack(pady=5)

    def setup_treeview(self):
        self.tree = ttk.Treeview(self, columns=("Adm No", "Name", "Present", "Total Present", "Percentage"), show="headings")
        self.tree.heading("Adm No", text="Admission No.")
        self.tree.heading("Name", text="Name")
        self.tree.heading("Present", text="Classes Attended")
        self.tree.heading("Total Present", text="Total Classes")
        self.tree.heading("Percentage", text="Percentage (%)")
        self.tree.pack(pady=10, fill=tk.BOTH, expand=True)

    def refresh_display(self):
        for i in self.tree.get_children():
            self.tree.delete(i)
        for att in get_attendance():
            self.tree.insert("", "end", values=(att['admno'], att['name'], att['present'], att['totalpresent'], f"{att['per']:.2f}"))

    def add_attendance_gui(self):
        dialog = tk.Toplevel(self)
        dialog.title("Insert Attendance")
        dialog.geometry("300x300")
        dialog.config(bg=LIGHT_GREY)

        tk.Label(dialog, text="Admission No.:", bg=LIGHT_GREY, fg=DARK_GREY).pack(pady=2)
        admno_entry = ttk.Entry(dialog, style='TEntry')
        admno_entry.pack(pady=2)

        tk.Label(dialog, text="Student Name:", bg=LIGHT_GREY, fg=DARK_GREY).pack(pady=2)
        name_entry = ttk.Entry(dialog, style='TEntry')
        name_entry.pack(pady=2)

        tk.Label(dialog, text="Classes Attended:", bg=LIGHT_GREY, fg=DARK_GREY).pack(pady=2)
        present_entry = ttk.Entry(dialog, style='TEntry')
        present_entry.pack(pady=2)

        tk.Label(dialog, text="Total Classes:", bg=LIGHT_GREY, fg=DARK_GREY).pack(pady=2)
        total_present_entry = ttk.Entry(dialog, style='TEntry')
        total_present_entry.pack(pady=2)

        tk.Label(dialog, text="Percentage:", bg=LIGHT_GREY, fg=DARK_GREY).pack(pady=2)
        per_entry = ttk.Entry(dialog, style='TEntry')
        per_entry.pack(pady=2)

        def save_attendance():
            try:
                admno = int(admno_entry.get())
                name = name_entry.get()
                present = int(present_entry.get())
                totalpresent = int(total_present_entry.get())
                per = float(per_entry.get())
                if add_attendance(admno, name, present, totalpresent, per):
                    messagebox.showinfo("Success", "Attendance added successfully!")
                    self.refresh_display()
                    dialog.destroy()
                else:
                    messagebox.showerror("Error", "Failed to add attendance. Admission number might exist.")
            except ValueError:
                messagebox.showerror("Error", "Please enter valid data. Numbers must be integers/floats.")

        ttk.Button(dialog, text="Save", command=save_attendance).pack(pady=10)

    def update_attendance_gui(self):
        admno_str = simpledialog.askstring("Update Attendance", "Enter Admission No. to update attendance:",
                                           parent=self, initialvalue="")
        if admno_str:
            try:
                admno = int(admno_str)
                new_present_str = simpledialog.askstring("Update Attendance", "Enter New Classes Attended:",
                                                          parent=self, initialvalue="")
                new_per_str = simpledialog.askstring("Update Attendance", "Enter New Percentage:",
                                                      parent=self, initialvalue="")
                if new_present_str and new_per_str:
                    new_present = int(new_present_str)
                    new_per = float(new_per_str)
                    if update_attendance(admno, new_present, new_per):
                        messagebox.showinfo("Success", "Attendance details updated successfully!")
                        self.refresh_display()
                    else:
                        messagebox.showerror("Error", "Attendance record for given Admission No. not found.")
                else:
                    messagebox.showwarning("Warning", "New Classes Attended and Percentage cannot be empty.")
            except ValueError:
                messagebox.showerror("Error", "Invalid input. Please enter valid numbers.")

    def delete_attendance_gui(self):
        admno_str = simpledialog.askstring("Delete Attendance", "Enter Admission No. to delete attendance:",
                                           parent=self, initialvalue="")
        if admno_str:
            try:
                admno = int(admno_str)
                if messagebox.askyesno("Confirm Deletion", f"Are you sure you want to delete attendance for Admission No. {admno}?"):
                    if delete_attendance(admno):
                        messagebox.showinfo("Success", "Attendance deleted successfully!")
                        self.refresh_display()
                    else:
                        messagebox.showerror("Error", "Attendance record for given Admission No. not found.")
            except ValueError:
                messagebox.showerror("Error", "Invalid Admission No. Please enter an integer.")

class SchoolDetailsManagementWindow(RoyalToplevel):
    def __init__(self, master):
        super().__init__(master, "School Details Management")

    def setup_buttons(self):
        self.add_button = ttk.Button(self, text="Insert Details", command=self.add_school_detail_gui)
        self.add_button.pack(pady=5)

        self.update_button = ttk.Button(self, text="Update Details", command=self.update_school_detail_gui)
        self.update_button.pack(pady=5)

        self.delete_button = ttk.Button(self, text="Delete Details", command=self.delete_school_detail_gui)
        self.delete_button.pack(pady=5)

    def setup_treeview(self):
        self.tree = ttk.Treeview(self, columns=("School ID", "School Name", "No. of Students", "No. of Employees", "No. of Labs"), show="headings")
        self.tree.heading("School ID", text="School ID")
        self.tree.heading("School Name", text="School Name")
        self.tree.heading("No. of Students", text="No. of Students")
        self.tree.heading("No. of Employees", text="No. of Employees")
        self.tree.heading("No. of Labs", text="No. of Labs")
        self.tree.pack(pady=10, fill=tk.BOTH, expand=True)

    def refresh_display(self):
        for i in self.tree.get_children():
            self.tree.delete(i)
        for school in get_school_details():
            self.tree.insert("", "end", values=(school['id'], school['sname'], school['noofstudent'], school['noofemployee'], school['nooflabs']))

    def add_school_detail_gui(self):
        dialog = tk.Toplevel(self)
        dialog.title("Insert School Details")
        dialog.geometry("300x280")
        dialog.config(bg=LIGHT_GREY)

        tk.Label(dialog, text="School ID:", bg=LIGHT_GREY, fg=DARK_GREY).pack(pady=2)
        sid_entry = ttk.Entry(dialog, style='TEntry')
        sid_entry.pack(pady=2)

        tk.Label(dialog, text="School Name:", bg=LIGHT_GREY, fg=DARK_GREY).pack(pady=2)
        sname_entry = ttk.Entry(dialog, style='TEntry')
        sname_entry.pack(pady=2)

        tk.Label(dialog, text="No. of Students:", bg=LIGHT_GREY, fg=DARK_GREY).pack(pady=2)
        students_entry = ttk.Entry(dialog, style='TEntry')
        students_entry.pack(pady=2)

        tk.Label(dialog, text="No. of Employees:", bg=LIGHT_GREY, fg=DARK_GREY).pack(pady=2)
        employees_entry = ttk.Entry(dialog, style='TEntry')
        employees_entry.pack(pady=2)

        tk.Label(dialog, text="No. of Labs:", bg=LIGHT_GREY, fg=DARK_GREY).pack(pady=2)
        labs_entry = ttk.Entry(dialog, style='TEntry')
        labs_entry.pack(pady=2)

        def save_school_detail():
            try:
                sid = int(sid_entry.get())
                sname = sname_entry.get()
                noofstudent = int(students_entry.get())
                noofemployee = int(employees_entry.get())
                nooflabs = int(labs_entry.get())
                if add_school_detail(sid, sname, noofstudent, noofemployee, nooflabs):
                    messagebox.showinfo("Success", "School details added successfully!")
                    self.refresh_display()
                    dialog.destroy()
                else:
                    messagebox.showerror("Error", "Failed to add school details. School ID might exist.")
            except ValueError:
                messagebox.showerror("Error", "Please enter valid data. Numbers must be integers.")

        ttk.Button(dialog, text="Save", command=save_school_detail).pack(pady=10)

    def update_school_detail_gui(self):
        sid_str = simpledialog.askstring("Update School Details", "Enter School ID to update:",
                                         parent=self, initialvalue="")
        if sid_str:
            try:
                sid = int(sid_str)
                new_students_str = simpledialog.askstring("Update School Details", "Enter New No. of Students:",
                                                           parent=self, initialvalue="")
                new_employees_str = simpledialog.askstring("Update School Details", "Enter New No. of Employees:",
                                                            parent=self, initialvalue="")
                new_labs_str = simpledialog.askstring("Update School Details", "Enter New No. of Labs:",
                                                       parent=self, initialvalue="")

                if new_students_str and new_employees_str and new_labs_str:
                    new_students = int(new_students_str)
                    new_employees = int(new_employees_str)
                    new_labs = int(new_labs_str)
                    if update_school_detail(sid, new_students, new_employees, new_labs):
                        messagebox.showinfo("Success", "School details updated successfully!")
                        self.refresh_display()
                    else:
                        messagebox.showerror("Error", "School details for given ID not found.")
                else:
                    messagebox.showwarning("Warning", "All fields must be filled.")
            except ValueError:
                messagebox.showerror("Error", "Invalid input. Please enter valid numbers.")

    def delete_school_detail_gui(self):
        sid_str = simpledialog.askstring("Delete School Details", "Enter School ID to delete:",
                                         parent=self, initialvalue="")
        if sid_str:
            try:
                sid = int(sid_str)
                if messagebox.askyesno("Confirm Deletion", f"Are you sure you want to delete school details for ID {sid}?"):
                    if delete_school_detail(sid):
                        messagebox.showinfo("Success", "School details deleted successfully!")
                        self.refresh_display()
                    else:
                        messagebox.showerror("Error", "School details for given ID not found.")
            except ValueError:
                messagebox.showerror("Error", "Invalid School ID. Please enter an integer.")


if __name__ == "__main__":
    root = tk.Tk()
    app = SchoolManagementGUI(root)
    root.mainloop()
