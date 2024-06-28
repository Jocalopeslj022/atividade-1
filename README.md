# atividade-1
atividade linguagem de programação

import random
import tkinter as tk
from tkinter import messagebox

# Lista de palavras por categoria
conjuntos = {
    'nome': ['nicolas', 'isadora', 'joaquim', 'samuel', 'matheus', 'vitor', 'gabriel', 'lucas', 'henrique', 'henry', 'luiz', 'gabriela', 'maria', 'ana', 'mariana', 'julia', 'juliana', 'marcela'],
    'fruta': ['banana', 'maca', 'pera', 'limao', 'mamao', 'laranja', 'mirtilo', 'pessego', 'uva', 'abacate', 'melancia', 'caqui', 'melao', 'jaca', 'caju', 'goiaba', 'manga', 'abacaxi', 'amora', 'ameixa', 'acerola', 'bergamota', 'framboesa', 'tangerina', 'acai'],
    'animal': ['aguia', 'baleia', 'cavalo', 'doninha', 'elefante', 'foca', 'gato', 'hipopotamo', 'iguana', 'jacare', 'leao', 'macaco', 'naja', 'onca', 'panda', 'queimado', 'rato', 'sapo', 'tartaruga', 'urso', 'vaca', 'xexu', 'yak', 'zebra']
}

class JogoDaForca:
    def __init__(self, root):
        self.root = root
        self.root.title("Jogo da Forca")

        self.canvas = tk.Canvas(root, width=200, height=200)
        self.canvas.pack()

        self.label_palavra = tk.Label(root, text="")
        self.label_palavra.pack()

        self.label_letras_erradas = tk.Label(root, text="")
        self.label_letras_erradas.pack()

        self.entry_letra = tk.Entry(root)
        self.entry_letra.pack()

        self.button_submeter = tk.Button(root, text="Submeter letra", command=self.submeter_letra)
        self.button_submeter.pack()

        self.max_tentativas = 10
        self.tentativas = 0
        self.letras_certas = []
        self.letras_erradas = []
        self.conjunto = ""
        self.palavra = ""

        self.novo_jogo()

    def novo_jogo(self):
        self.conjunto, self.palavra = self.escolher_palavra()
        self.letras_certas = []
        self.letras_erradas = []
        self.tentativas = 0
        self.desenhar_forca()
        self.atualizar_interface()

    def escolher_palavra(self):
        conjunto_escolhido = random.choice(list(conjuntos.keys()))
        palavra_escolhida = random.choice(conjuntos[conjunto_escolhido])
        return conjunto_escolhido, palavra_escolhida

    def submeter_letra(self):
        letra = self.entry_letra.get().lower()
        self.entry_letra.delete(0, tk.END)

        if letra in self.letras_certas or letra in self.letras_erradas:
            messagebox.showwarning("Letra repetida", "Você já tentou essa letra.")
            return

        if letra in self.palavra:
            self.letras_certas.append(letra)
        else:
            self.letras_erradas.append(letra)
            self.tentativas += 1

        self.atualizar_interface()

        if self.tentativas == self.max_tentativas:
            messagebox.showinfo("Fim de jogo", f"Você perdeu! A palavra era: {self.palavra}")
            self.novo_jogo()

        if '_' not in ''.join(letra if letra in self.letras_certas else '_' for letra in self.palavra):
            messagebox.showinfo("Parabéns!", f"Você acertou a palavra: {self.palavra}")
            self.novo_jogo()

    def desenhar_forca(self):
        # Limpar o desenho anterior
        self.canvas.delete("all")

        # Desenhos da forca
        desenhos_forca = [
            self.canvas.create_line(50, 180, 150, 180),  # Base
            self.canvas.create_line(100, 180, 100, 20),  # Poste
            self.canvas.create_line(100, 20, 150, 20),   # Barra superior
        ]

        if self.tentativas > 0:
            # Cabeça
            if self.tentativas >= 1:
                self.canvas.create_oval(135, 20, 165, 50)
            # Olhos esquerdo
            if self.tentativas >= 2:
                self.canvas.create_oval(140, 30, 145, 35)
            # Olhos direito
            if self.tentativas >= 3:
                self.canvas.create_oval(155, 30, 160, 35)
            # Boca
            if self.tentativas >= 4:
                self.canvas.create_line(145, 45, 155, 45)
            # Corpo
            if self.tentativas >= 5:
                self.canvas.create_line(150, 50, 150, 100)
            # Braço direito
            if self.tentativas >= 6:
                self.canvas.create_line(150, 60, 130, 80)
            # Braço esquerdo
            if self.tentativas >= 7:
                self.canvas.create_line(150, 60, 170, 80)
            # Perna direita
            if self.tentativas >= 8:
                self.canvas.create_line(150, 100, 130, 130)
            # Perna esquerda
            if self.tentativas >= 9:
                self.canvas.create_line(150, 100, 170, 130)
            # Enforcado
            if self.tentativas >= 10:
                self.canvas.create_line(150, 50, 150, 20)
                self.canvas.create_line(150, 20, 130, 55)
                self.canvas.create_line(150, 20, 170, 55)
                

    def atualizar_interface(self):
        palavra_mascarada = ''.join(letra if letra in self.letras_certas else '_' for letra in self.palavra)
        self.label_palavra.config(text=f"Palavra ({self.conjunto}): {palavra_mascarada}")
        self.label_letras_erradas.config(text=f"Letras erradas: {', '.join(self.letras_erradas)}")
        self.desenhar_forca()

# Função principal para iniciar o jogo
def main():
    root = tk.Tk()
    jogo = JogoDaForca(root)
    root.mainloop()

if __name__ == "__main__":
    main()
