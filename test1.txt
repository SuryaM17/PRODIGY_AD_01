import sys
from PyQt5.QtWidgets import QApplication, QWidget, QVBoxLayout, QGridLayout, QPushButton, QLineEdit
from PyQt5.QtCore import Qt

class Calculator(QWidget):
    def __init__(self):
        super().__init__()
        self.initUI()

    def initUI(self):
        self.setWindowTitle('Calculator')
        self.setFixedSize(400, 500)

        # Create the main layout
        main_layout = QVBoxLayout()
        main_layout.setContentsMargins(10, 10, 10, 10)
        main_layout.setSpacing(10)

        # Create the display
        self.display = QLineEdit()
        self.display.setReadOnly(True)
        self.display.setAlignment(Qt.AlignRight)
        self.display.setFixedHeight(50)
        self.display.setStyleSheet("""
            QLineEdit {
                background-color: #2e2e2e;
                color: #ffffff;
                font-size: 24px;
                border: 2px solid #4f4f4f;
                border-radius: 10px;
                padding: 10px;
            }
        """)
        main_layout.addWidget(self.display)

        # Create the grid layout for buttons
        grid_layout = QGridLayout()
        grid_layout.setSpacing(10)

        # Button labels
        buttons = [
            ('7', '#4CAF50'), ('8', '#4CAF50'), ('9', '#4CAF50'), ('/', '#FF5722'),
            ('4', '#4CAF50'), ('5', '#4CAF50'), ('6', '#4CAF50'), ('*', '#FF5722'),
            ('1', '#4CAF50'), ('2', '#4CAF50'), ('3', '#4CAF50'), ('-', '#FF5722'),
            ('0', '#4CAF50'), ('C', '#F44336'), ('=', '#FFC107'), ('+', '#FF5722')
        ]

        # Button styles
        button_style = """
            QPushButton {{
                background-color: {color};
                color: white;
                font-size: 18px;
                border: none;
                border-radius: 10px;
                padding: 15px;
            }}
            QPushButton:pressed {{
                background-color: {pressed_color};
            }}
        """

        # Create buttons and add them to the grid layout
        row, col = 0, 0
        for button_text, button_color in buttons:
            btn = QPushButton(button_text)
            btn.setStyleSheet(button_style.format(
                color=button_color,
                pressed_color=self.darken_color(button_color)
            ))
            btn.clicked.connect(self.on_click)
            grid_layout.addWidget(btn, row, col, 1, 1)
            col += 1
            if col > 3:
                col = 0
                row += 1

        main_layout.addLayout(grid_layout)
        self.setLayout(main_layout)
        self.setStyleSheet("background-color: #121212;")

    def darken_color(self, color):
        """Helper function to darken a hex color."""
        color = color.lstrip('#')
        lv = len(color)
        return '#' + ''.join(hex(max(int(color[i:i+lv//3], 16) - 40, 0))[2:].zfill(2) for i in range(0, lv, lv//3))

    def on_click(self):
        sender = self.sender().text()

        if sender == 'C':
            self.display.clear()
        elif sender == '=':
            try:
                result = eval(self.display.text())
                self.display.setText(str(result))
            except Exception as e:
                self.display.setText('Error')
        else:
            self.display.setText(self.display.text() + sender)

def main():
    app = QApplication(sys.argv)
    calc = Calculator()
    calc.show()
    sys.exit(app.exec_())

if __name__ == '__main__':
    main()
