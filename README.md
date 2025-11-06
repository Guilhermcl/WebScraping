# WebScraping
requests
beautifulsoup4
pandas
matplotlib
seaborn
# main.py
# 📚 Projeto: Análise de Livros com Web Scraping
# Autor: Guilherme Coutinho Lopes

import requests
from bs4 import BeautifulSoup
import pandas as pd
import matplotlib.pyplot as plt
import seaborn as sns

# URL do site de exemplo (substitua pelo site real utilizado)
url = "https://books.toscrape.com/"

# Faz a requisição da página
response = requests.get(url)
soup = BeautifulSoup(response.text, "html.parser")

# Coleta os dados dos livros
books = soup.find_all("article", class_="product_pod")

titulos = []
precos = []
notas = []

for book in books:
    titulo = book.h3.a["title"]
    preco = book.find("p", class_="price_color").text.replace("£", "").strip()
    preco = float(preco)
    
    # As notas nesse site são representadas por classes (One, Two, etc.)
    nota_class = book.p["class"][1]
    notas_dict = {"One": 1, "Two": 2, "Three": 3, "Four": 4, "Five": 5}
    nota = notas_dict.get(nota_class, 0)
    
    titulos.append(titulo)
    precos.append(preco)
    notas.append(nota)

# Cria um DataFrame com os dados
df = pd.DataFrame({
    "Título": titulos,
    "Preço (£)": precos,
    "Nota": notas
})

# Exibe o resumo dos dados
print(df.head())

# Calcula a média geral de preço e nota
media_preco = df["Preço (£)"].mean()
media_nota = df["Nota"].mean()
print(f"\nMédia de Preço: £{media_preco:.2f}")
print(f"Média de Nota: {media_nota:.2f}")

# Cria gráfico de dispersão (Preço x Nota)
plt.figure(figsize=(8, 5))
sns.scatterplot(x="Preço (£)", y="Nota", data=df, s=80)
plt.title("Relação entre Preço e Nota dos Livros")
plt.xlabel("Preço (£)")
plt.ylabel("Nota")
plt.grid(True)
plt.show()
  
