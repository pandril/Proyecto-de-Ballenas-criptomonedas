# Proyecto-de-Ballenas-criptomonedas
Proyecto que extrae salidas y entradas de las principales direcciones de criptomonedas

leguaje de programacion python

import tkinter as tk
from bs4 import BeautifulSoup
import requests
import datatime


class Calculo():
    
    def __init__(self,pagina,columna):
        self.columna= columna
        self.pagina=pagina
        
    def calculo(self):
        URL=self.pagina
        req = requests.get(URL)
        
        html = BeautifulSoup(req.text, "html.parser")
        entradas = html.find_all('table {'id': 'tblOne'})

URL= https://bitinfocharts.com/top-100-richest-bitcoin-addresses.html

"""
# -*- coding: utf-8 -*-
__author__ = 'RicardoMoya'

from bs4 import BeautifulSoup
import requests

URL = "http://jarroba.com/"

# Realizamos la petición a la web
req = requests.get(URL)

# Comprobamos que la petición nos devuelve un Status Code = 200
status_code = req.status_code
if status_code == 200:

    # Pasamos el contenido HTML de la web a un objeto BeautifulSoup()
    html = BeautifulSoup(req.text, "html.parser")

    # Obtenemos todos los divs donde están las entradas
    entradas = html.find_all('div', {'class': 'col-md-4 col-xs-12'})

    # Recorremos todas las entradas para extraer el título, autor y fecha
    for i, entrada in enumerate(entradas):
        # Con el método "getText()" no nos devuelve el HTML
        titulo = entrada.find('span', {'class': 'tituloPost'}).getText()
        # Sino llamamos al método "getText()" nos devuelve también el HTML
        autor = entrada.find('span', {'class': 'autor'})
        fecha = entrada.find('span', {'class': 'fecha'}).getText()

        # Imprimo el Título, Autor y Fecha de las entradas
        print "%d - %s  |  %s  |  %s" % (i + 1, titulo, autor, fecha)

else:
    print "Status Code %d" % status_code
"""



ventana=tk.Tk()

botonActualizar= tk.Button(ventana, text="Actualizar")# falta comando
botonActualizar.grid(row=1, column=1)


primeraEtiqueta=tk.Label(ventana, text="inicio")
primeraEtiqueta.grid(row=1,column=2)

#__________________BITCOIN__________________
bitcoin=tk.Label(ventana, text="Bitcoin")
bitcoin.grid(row=2,column=2)

enviadasbitcoinDia=tk.Label(ventana, text="Ventas por dia:")
enviadasbitcoinDia.grid(row=3,column=1)

recibidasbitcoinDia=tk.Label(ventana, text="Compras por dia:")
recibidasbitcoinDia.grid(row=4,column=1)

enviadasbitcoinHora=tk.Label(ventana, text="Ventas por hora:")
enviadasbitcoinHora.grid(row=5,column=1)

recibidasbitcoinHora=tk.Label(ventana, text="Compras por hora:")
recibidasbitcoinHora.grid(row=6,column=1)


#____________________LTC___________________
litecoin=tk.Label(ventana, text="Ltc")
litecoin.grid(row=2,column=4)

enviadasLTCdia=tk.Label(ventana, text="Ventas por dia:")
enviadasLTCdia.grid(row=3,column=3)

recibidasLTCdia=tk.Label(ventana, text="Compras por dia:")
recibidasLTCdia.grid(row=4,column=3)

enviadasLTChora=tk.Label(ventana, text="Ventas por hora:")
enviadasLTChora.grid(row=5,column=3)

recibidasLTChora=tk.Label(ventana, text="Compras por hora:")
recibidasLTChora.grid(row=6,column=3)

#__________________Dash__________________
bitcoin=tk.Label(ventana, text="Dash")
bitcoin.grid(row=2,column=6)

enviadasDASHdia=tk.Label(ventana, text="Ventas por dia:")
enviadasDASHdia.grid(row=3,column=5)

recibidasDASHdia=tk.Label(ventana, text="Compras por dia:")
recibidasDASHdia.grid(row=4,column=5)

enviadasDASHhora=tk.Label(ventana, text="Ventas por hora:")
enviadasDASHhora.grid(row=5,column=5)

recibidasDASHhora=tk.Label(ventana, text="Compras por hora:")
recibidasDASHhora.grid(row=6,column=5)


ventana.mainloop()
