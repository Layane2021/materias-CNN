# MATÉRIAS NA CNN:
Esse repositório contém um código de busca de todas as matérias que foram produzidas por mim para o site da CNN Brasil. 

Por meio das bibliotecas BeautifulSoup e requests, fiz a raspagem de duas páginas do site da CNN que englobam matérias apenas com o meu nome:

_Produzido por Layane Serrano, da CNN São Paulo_
https://www.cnnbrasil.com.br/autor/produzido-por-layane-serrano/pagina/2/

_Textos de Layane Serrano_
https://www.cnnbrasil.com.br/autor/layane-serrano/

Para exportar para o Excel, utilizei a biblioteca pandas:


**CÓGIGO PYTHON**

import requests #permite baixar arquivos usando Python
from bs4 import BeautifulSoup as bs #permite fazer uma leitura da página estruturada da página html
import pandas as pd 

# Raspando o resultado da página de busca do site da CNN Brasil - página - por Layane Serrano

termo_de_busca = 'Layane Serrano'

url = 'https://www.cnnbrasil.com.br/autor/layane-serrano/'

resposta = requests.get(url)

html = resposta.text



soup = bs(resposta.text, 'html.parser') # carregando o html no BeautifulSoup

tags_de_links = soup.find_all('a', {'class': 'home__list__tag'}) # examinando o html, vemos que os links da busca tem class="homelist_tag"

tags_de_datas = soup.find_all('span', {'class': 'home__title__date'})


noticias = []

for tag, data in zip(tags_de_links, tags_de_datas):

    link = tag.attrs['href'] # aqui pegamos o atributo href de cada tag a, q é onde está a url do link
    
    title = tag.attrs["title"]  # -----------------> adicionei
    
    date = data.text
    
    noticias.append([title, link, date])
    
    print(link, title, date)
    

news = pd.DataFrame(noticias, columns=['Title', 'Link', 'Date'])

news.to_excel('news-de-lay.xlsx', index=False)
