import tkinter as tk
from reversi import Reversi

class ReversiGUI:
    def __init__(self, master):
        self.master = master
        self.master.title("リバーシ（オセロ）")
        self.canvas = tk.Canvas(self.master, width=640, height=640, bg='green')
        self.canvas.pack()
        self.canvas.bind("<Button-1>", self.click)
        self.reversi = Reversi()
        self.draw_board()

    def draw_board(self):
        self.canvas.delete("all")
        for i in range(8):
            for j in range(8):
                x0, y0 = i*80, j*80
                x1, y1 = x0+80, y0+80
                self.canvas.create_rectangle(x0, y0, x1, y1, outline='black')
                if self.reversi.board[j][i] == 1:
                    self.canvas.create_oval(x0+10, y0+10, x1-10, y1-10, fill='black')
                elif self.reversi.board[j][i] == -1:
                    self.canvas.create_oval(x0+10, y0+10, x1-10, y1-10, fill='white')

    def click(self, event):
        x, y = event.x // 80, event.y // 80
        if self.reversi.make_move(x, y):
            self.reversi.pass_if_needed()
            self.draw_board()
            if self.reversi.is_game_over():
                self.show_result()

    def show_result(self):
        black, white = self.reversi.count_stones()
        result = "黒:{} 白:{} → ".format(black, white)
        if black > white:
            result += "黒の勝ち！"
        elif white > black:
            result += "白の勝ち！"
        else:
            result += "引き分け！"
        self.canvas.create_text(320, 320, text=result, fill="red", font=("Arial", 32), anchor='center')

if __name__ == "__main__":
    root = tk.Tk()
    app = ReversiGUI(root)
    root.mainloop()

