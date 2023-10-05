import sqlite3
from PySide6 import QtCore, QtWidgets
from PySide6.QtUiTools import QUiLoader
from PySide6.QtWidgets import QApplication, QMainWindow, QPushButton, QListWidgetItem, QMessageBox

loader = QUiLoader()


class UserInterface(QtCore.QObject): #An object wrapping around our ui
    def __init__(self,):
        super().__init__()
        self.ui = loader.load("task.ui", None)
        self.ui.setWindowTitle("Daily Planner")
        self.ui.calendarWidget.selectionChanged.connect(self.calendarDateChanged)
        self.ui.save_button.clicked.connect(self.saveChanges)
        self.ui.addnew_button.clicked.connect(self.addNewTask)
        self.ui.save_button_2.clicked.connect(self.saveChangesTwo)

    def saveChangesTwo(self):
        db = sqlite3.connect("newdb.db")
        cursor = db.cursor()
        newTasks = str(self.ui.text_Edit.text())
        query = ("INSERT INTO Tasks(Task) VALUES (?)")
        row = (newTasks)
        cursor.execute(query, row)
        db.commit()
        # self.updateNotesList()

    # def updateNotesList(self):
    #     db = sqlite3.connect("newdb.db")
    #     cursor = db.cursor()
    #     query = "SELECT Task, Completed FROM Tasks WHERE Date = ?"
    #     results = cursor.execute(query).fetchall()
    #     for result in results:
    #         item = QListWidgetItem(str(result[0]))
    #         self.ui.text_Edit.addItem(item)





    def addNewTask(self):
        db = sqlite3.connect("newdb.db")
        cursor = db.cursor()
        newTask = str(self.ui.lineEdit.text())
        date = self.ui.calendarWidget.selectedDate().toPython()

        query = "INSERT INTO Tasks(Task, Completed, Date) VALUES (?,?,?)"

        row = (newTask, "NO", date, )

        
        cursor.execute(query, row)
        db.commit()
        self.updateTaskList(date)
        self.ui.lineEdit.clear()

        messageBox = QMessageBox()
        messageBox.setWindowTitle("Task Added.")
        messageBox.setText("Task Added.")
        messageBox.setStandardButtons(QMessageBox.Ok)
        messageBox.exec()
        




    def saveChanges(self):
        db = sqlite3.connect("newdb.db")
        cursor = db.cursor()
        date = self.ui.calendarWidget.selectedDate().toPython()
        

        for i in range(self.ui.tasks_list.count())   :
            item = self.ui.tasks_list.item(i)
            Task = item.text()
            if item.checkState() == QtCore.Qt.Checked:   
                query = "UPDATE Tasks SET Completed = 'Yes' WHERE Task = ? AND Date = ?"
            else: 
                query = "UPDATE Tasks SET Completed = 'No' WHERE Task = ? AND Date = ?"
            row = (Task, date,)
            cursor.execute(query, row)
        db.commit()  

        messageBox = QMessageBox()
        messageBox.setWindowTitle("Changes Saved.")
        messageBox.setText("Changes Saved.")
        messageBox.setStandardButtons(QMessageBox.Ok)
        messageBox.exec()

        

        
    def show(self):
        self.ui.show()

    def calendarDateChanged(self):
        dateSelected = self.ui.calendarWidget.selectedDate().toPython()
        print(dateSelected)   
        self.updateTaskList(dateSelected)

    def updateTaskList(self, date):
        self.ui.tasks_list.clear()
        db = sqlite3.connect("newdb.db")
        cursor = db.cursor()

        query = "SELECT Task, Completed FROM Tasks WHERE Date = ?"
        row = (date,)
        results = cursor.execute(query, row).fetchall()
        for result in results:
            item = QListWidgetItem(str(result[0]))
            item.setFlags(item.flags() | QtCore.Qt.ItemIsUserCheckable)
            item.setCheckState(QtCore.Qt.Unchecked)
            if result[1] == "Yes":
                item.setCheckState(QtCore.Qt.Checked)
            elif result[1] == "No":
                item.setCheckState(QtCore.Qt.Unchecked)
            self.ui.tasks_list.addItem(item)
       
