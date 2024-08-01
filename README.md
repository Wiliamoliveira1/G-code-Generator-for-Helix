# G-code-Generator-for-Helix
G-code Generator for Helix

Gerador de G-code para Helicoide
Este é um gerador de G-code para a criação de helicoides personalizáveis, desenvolvido em Python com uma interface gráfica simples utilizando a biblioteca Tkinter. O programa permite que o usuário insira os parâmetros do helicoide, como diâmetro, número de voltas, altura e fator de extrusão, e gera o G-code correspondente, exibindo-o em uma área de texto na interface.

Funcionalidades
Entrada de parâmetros: Permite inserir os valores de diâmetro, número de voltas, altura e fator de extrusão.
Geração de G-code: Calcula os pontos do helicoide com base nos parâmetros fornecidos e gera o G-code correspondente.
Exibição do G-code: Exibe o G-code gerado em uma área de texto rolável para fácil visualização e cópia.
Parâmetros
Diâmetro (mm): O diâmetro do helicoide.
Voltas: O número de voltas do helicoide.
Altura (mm): A altura total do helicoide.
Extrusão Total (E): O valor total de extrusão que será distribuído ao longo do helicoide.
Requisitos
Python 2.7 ou superior
Tkinter (incluso na biblioteca padrão do Python)
Como Usar
Clone o repositório:

sh
Copiar código
git clone https://github.com/Wiliamoliveira1/G-code-Generator-for-Helix.git
cd G-code-Generator-for-Helix
Execute o programa:

sh
Copiar código
python gerador_gcode_helicoide.py
Insira os parâmetros desejados:

Diâmetro (mm)
Voltas
Altura (mm)
Extrusão Total (E)
Clique no botão "Gerar G-code" para gerar e exibir o G-code correspondente.

Exemplo de Uso
sh
Copiar código
python gerador_gcode_helicoide.py
Interface Gráfica
Campos de entrada: Permite inserir os valores de diâmetro, voltas, altura e fator de extrusão.
Botão "Gerar G-code": Gera o G-code com os parâmetros especificados e exibe na área de texto.
Área de texto: Exibe o G-code gerado.
Contribuições
Contribuições são bem-vindas! Sinta-se à vontade para abrir issues e pull requests com melhorias e correções.

Licença
Este projeto está licenciado sob a licença MIT. Veja o arquivo LICENSE para mais detalhes.

    
import math
import tkinter as tk
from tkinter import scrolledtext

# Função para gerar o G-code
        def gerar_gcode(diametro, voltas, altura, extrusion_factor):
        try:
        # Converte os valores de entrada
        
        diametro = float(diametro)
        voltas = int(voltas)
        altura = float(altura)
        extrusion_factor = float(extrusion_factor)
        
        pitch = altura / voltas
        num_pontos_por_volta = 100
        total_pontos = voltas * num_pontos_por_volta
        extrusion_factor /= total_pontos  # Distribui igualmente

        # Geração dos pontos do helicoide
        gcode = []
        gcode.append("G21 ; Set units to mm")
        gcode.append("G90 ; Absolute positioning")
        gcode.append("G1 F1200 ; Set feedrate")

        e_value = 0.0

        for i in range(total_pontos):
            angulo = 2 * math.pi * i / num_pontos_por_volta
            x = (diametro / 2) * math.cos(angulo)
            y = (diametro / 2) * math.sin(angulo)
            z = i * pitch / num_pontos_por_volta
            e_value += extrusion_factor
            gcode.append("G1 X{:.3f} Y{:.3f} Z{:.3f} E{:.4f}".format(x, y, z, e_value))
        
        return "\n".join(gcode)
        except ValueError:
        return "Erro: Verifique os valores de entrada!"

    # Função para exibir o G-code na interface
    def mostrar_gcode():
    diametro = entry_diametro.get()
    voltas = entry_voltas.get()
    altura = entry_altura.get()
    extrusion_factor = entry_extrusion_factor.get()
    gcode = gerar_gcode(diametro, voltas, altura, extrusion_factor)
    text_area.delete('1.0', tk.END)
    text_area.insert(tk.END, gcode)

    # Criação da janela principal
    window = tk.Tk()
    window.title("Gerador de G-code para Helicoide")

    # Labels e Entry widgets para os parâmetros
    tk.Label(window, text="Diâmetro (mm):").pack(pady=2)
    entry_diametro = tk.Entry(window)
    entry_diametro.pack(pady=2)

    tk.Label(window, text="Voltas:").pack(pady=2)
    entry_voltas = tk.Entry(window)
    entry_voltas.pack(pady=2)

    tk.Label(window, text="Altura (mm):").pack(pady=2)
    entry_altura = tk.Entry(window)
    entry_altura.pack(pady=2)

    tk.Label(window, text="Extrusão Total (E):").pack(pady=2)
    entry_extrusion_factor = tk.Entry(window)
    entry_extrusion_factor.pack(pady=2)

    # Criação de um campo de texto para exibir o G-code
    text_area = scrolledtext.ScrolledText(window, wrap=tk.WORD, width=80, height=20, font=("Times New Roman", 12))
    text_area.pack(padx=10, pady=10)

    # Botão para gerar o G-code
    btn_generate = tk.Button(window, text="Gerar G-code", command=mostrar_gcode)
    btn_generate.pack(pady=10)

    # Iniciar a aplicação Tkinter
    window.mainloop()
