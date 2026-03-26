#Quiz_Game
#language swedish
#tkinter — Python interface to Tcl/Tk
import tkinter as tk
from tkinter import messagebox
import random

# Frågor och svar
questions = {
    "Historia": [
        {"question": "Vilket år startade andra världskriget?", "answer": "1939"},
        {"question": "Vem var den första presidenten i USA?", "answer": "George Washington"},
        {"question": "Vilken händelse markerade början på franska revolutionen?", "answer": "Stormningen av Bastiljen"},
        {"question": "Vilken kung av Sverige dog i slaget vid Lützen 1632?", "answer": "Gustav II Adolf"},
        {"question": "Vem var den första kvinnliga premiärministern i Storbritannien?", "answer": "Margaret Thatcher"},
    ],
    "Geografi": [
        {"question": "Vilken är världens största ö?", "answer": "Grönland"},
        {"question": "Genom vilket land rinner floden Nilen?", "answer": "Egypten"},
        {"question": "Vilken är huvudstaden i Japan?", "answer": "Tokyo"},
        {"question": "I vilket hav ligger Maldiverna?", "answer": "Indiska oceanen"},
        {"question": "Vilket land har flest tidszoner?", "answer": "Frankrike"},
    ],
    "Vetenskap": [
        {"question": "Vad är den kemiska beteckningen för vatten?", "answer": "H2O"},
        {"question": "Vem formulerade teorin om relativitet?", "answer": "Albert Einstein"},
        {"question": "Vilken planet är närmast solen?", "answer": "Merkurius"},
        {"question": "Vilken gas utgör cirka 78% av jordens atmosfär?", "answer": "Kväve"},
        {"question": "Vad heter grundämnet med den kemiska beteckningen Au?", "answer": "Guld"},
    ],
    "Litteratur": [
        {"question": "Vem skrev 'Romeo och Julia'?", "answer": "William Shakespeare"},
        {"question": "Vilken bok börjar med orden 'Kalla mig Ismael'?", "answer": "Moby Dick"},
        {"question": "Vem är författaren till trilogin 'Sagan om Ringen'?", "answer": "J.R.R. Tolkien"},
        {"question": "Vilken svensk författare skrev 'Utvandrarna'?", "answer": "Vilhelm Moberg"},
        {"question": "Vad heter huvudkaraktären i 'Don Quijote'?", "answer": "Don Quijote de la Mancha"},
    ]
}

class QuizGame:
    def __init__(self, root):
        self.root = root
        self.root.title("Frågesport")
        self.root.geometry("550x450")
        
        self.score = 0
        self.total_questions = 0
        self.current_category = None
        self.current_question = None
        self.question_index = 0
        
        # Välkomstetikett
        self.welcome_label = tk.Label(root, text="Welcome to the Quiz!", font=("Helvetica", 18))
        self.welcome_label.pack(pady=20)
        
        # Kategoriknappar
        self.buttons_frame = tk.Frame(root)
        self.buttons_frame.pack(pady=30)
        
        for category in questions:
            btn = tk.Button(self.buttons_frame, text=category, command=lambda c=category: self.start_category(c))
            btn.pack(side=tk.LEFT, padx=20)
        
        # Fråga etikett
        self.question_label = tk.Label(root, text="", font=("Helvetica", 14))
        self.question_label.pack(pady=20)
        
        # Svarsbox
        self.answer_entry = tk.Entry(root, font=("Helvetica", 14))
        self.answer_entry.pack(pady=10)
        
        # Svarsknapp
        self.submit_button = tk.Button(root, text="Svara", command=self.check_answer)
        self.submit_button.pack(pady=20)
        
        # Resultat etikett
        self.result_label = tk.Label(root, text="", font=("Helvetica", 14))
        self.result_label.pack(pady=10)

        # Poäng etikett
        self.score_label = tk.Label(root, text="Poäng: 0", font=("Helvetica", 14))
        self.score_label.pack(pady=10)
        
    def start_category(self, category):
        self.current_category = category
        self.question_index = 0
        self.score = 0
        self.total_questions = len(questions[category])
        
        # Slumpmässigt ordna frågorna i kategorin (random)
        random.shuffle(questions[category])
        self.show_question()
        
    def show_question(self):
        if self.question_index < self.total_questions:
            self.current_question = questions[self.current_category][self.question_index]
            self.question_label.config(text=self.current_question["question"])
            self.answer_entry.delete(0, tk.END)
            self.result_label.config(text="")
        else:
            messagebox.showinfo("Frågesport", f"Du har slutfört {self.current_category}!\nDu fick {self.score} av {self.total_questions} rätt!")
            self.reset_game()
    
    def check_answer(self):
        user_answer = self.answer_entry.get().strip().lower()
        correct_answer = self.current_question["answer"].strip().lower()
        if user_answer == correct_answer:
            self.score += 1
            self.result_label.config(text="Rätt!", fg="green")
        else:
            self.result_label.config(text=f"Fel. Rätt svar är: {self.current_question['answer']}", fg="red")
        
        #score (module random)
        self.score_label.config(text=f"Poäng: {self.score}")
        self.question_index += 1
        self.root.after(2000, self.show_question)
    
    def reset_game(self):
        self.current_category = None
        self.current_question = None
        self.question_index = 0
        self.question_label.config(text="")
        self.answer_entry.delete(0, tk.END)
        self.result_label.config(text="")
        self.score_label.config(text="Poäng: 0")

if __name__ == "__main__":
    root = tk.Tk()
    quiz_game = QuizGame(root)
    root.mainloop()
