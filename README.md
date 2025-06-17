# Modern-GUI-Calculator 
import customtkinter as ctk

class ModernCalculator:
    def __init__(self, root):
        self.root = root
        self.root.title("Modern GUI Calculator")
        self.root.geometry("320x450")
        self.expression = ""

        self.display = ctk.CTkEntry(master=root, width=280, height=60, font=("Arial", 24), justify="right")
        self.display.pack(pady=15)


        self.create_buttons()


        self.theme_btn = ctk.CTkButton(master=root, text="Switch Theme", command=self.toggle_theme)
        self.theme_btn.pack(pady=15)

    def toggle_theme(self):
        current = ctk.get_appearance_mode()
        ctk.set_appearance_mode("dark" if current == "light" else "light")

    def create_buttons(self):
        button_frame = ctk.CTkFrame(master=self.root)
        button_frame.pack()

        buttons = [
            ["7", "8", "9", "/"],
            ["4", "5", "6", "%"],
            ["1", "2", "3", "+"],
            ["C", "0", "=", "+"]
        ]

        for row in buttons:
            row_frame = ctk.CTkFrame(master=button_frame)
            row_frame.pack(pady=5)
            for char in row:
                color = "#2ECCFA" if char in ["/", "%", "+"] else None
                btn = ctk.CTkButton(
                    master=row_frame,
                    text=char,
                    width=60,
                    height=50,
                    corner_radius=10,
                    fg_color=color,
                    command=lambda ch=char: self.on_click(ch)
                )
                btn.pack(side="left", padx=5)
    def on_click(self, char):
        if char == "C":
            self.expression = ""
        elif char == "=":
            try:
                self.expression = str(eval(self.expression.replace("%", "/100")))
            except:
                self.expression = "Error"
        else:
            self.expression += char
        self.display.delete(0, "end")
        self.display.insert(0, self.expression)

if __name__ == "__main__":
    ctk.set_appearance_mode("dark")  # Set default theme
    ctk.set_default_color_theme("blue")
    root = ctk.CTk()
    app = ModernCalculator(root)
    root.mainloop()
