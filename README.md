# Proyecto-de-Ballenas-criptomonedas
"""
Proyecto que extrae salidas y entradas de las principales direcciones de criptomonedas
codigo escrito en python 3.7
"""
# -*- coding: utf-8 -*-
__author__ = 'Andry Valderrama'

import pandas as pd
import requests
from bs4 import BeautifulSoup
import tkinter as tk
import time
import datetime
import webbrowser
#import matplotlib.pyplot as plt


fechaActual= str(datetime.datetime.now())
horaActual= time.strftime("%X")

class Extraccion():

    def __init__(self,pagina):
        self.pagina=pagina
        self.entradas_hora= ""
        self.salidas_hora=""

    def funcionExtraccion(self,columna):    

        URL= self.pagina+".html"
        URL2= URL+"-2.html"
        URL3= URL+"-3.html"
        URL4= URL+"-4.html"

        req = requests.get(URL)
        req2 = requests.get(URL2)
        req3 = requests.get(URL3)
        req4 = requests.get(URL4)

        soup = BeautifulSoup(req.content,'lxml')
        soup2 = BeautifulSoup(req2.content,'lxml')
        soup3 = BeautifulSoup(req3.content,'lxml')
        soup4 = BeautifulSoup(req4.content,'lxml')

        table = soup.find_all('table')[2] 
        table2 = soup.find_all('table')[3]

        table3 = soup2.find_all('table')[0] 
        table4 = soup2.find_all('table')[1]

        table5 = soup3.find_all('table')[0] 
        table6 = soup3.find_all('table')[1]

        table7 = soup4.find_all('table')[0] 
        table8 = soup4.find_all('table')[1]

        df = pd.read_html(str(table))[0]
        df2 = pd.read_html(str(table2))[0]

        df3 = pd.read_html(str(table3))[0]
        df4 = pd.read_html(str(table4))[0]

        df5 = pd.read_html(str(table5))[0]
        df6 = pd.read_html(str(table6))[0]

        df7 = pd.read_html(str(table7))[0]
        df8 = pd.read_html(str(table8))[0]

        df2.columns = ['Unnamed: 0', 'Address', 'Balance △1w/△1m', '% of coins', 'First In', 'Last In', 'Number Of Ins', 'First Out', 'Last Out', 'Number Of Outs']
        data_final= pd.concat([df,df2 ], axis=0)
        df3.columns = ['Unnamed: 0', 'Address', 'Balance △1w/△1m', '% of coins', 'First In', 'Last In', 'Number Of Ins', 'First Out', 'Last Out', 'Number Of Outs']
        data_final1= pd.concat([data_final,df3 ], axis=0)
        df4.columns = ['Unnamed: 0', 'Address', 'Balance △1w/△1m', '% of coins', 'First In', 'Last In', 'Number Of Ins', 'First Out', 'Last Out', 'Number Of Outs']
        data_final2= pd.concat([data_final1,df4 ], axis=0)
        df5.columns = ['Unnamed: 0', 'Address', 'Balance △1w/△1m', '% of coins', 'First In', 'Last In', 'Number Of Ins', 'First Out', 'Last Out', 'Number Of Outs']
        data_final3= pd.concat([data_final2,df5 ], axis=0)
        df6.columns = ['Unnamed: 0', 'Address', 'Balance △1w/△1m', '% of coins', 'First In', 'Last In', 'Number Of Ins', 'First Out', 'Last Out', 'Number Of Outs']
        data_final4= pd.concat([data_final3,df6 ], axis=0)
        df7.columns = ['Unnamed: 0', 'Address', 'Balance △1w/△1m', '% of coins', 'First In', 'Last In', 'Number Of Ins', 'First Out', 'Last Out', 'Number Of Outs']
        data_final5= pd.concat([data_final4,df7 ], axis=0)
        df8.columns = ['Unnamed: 0', 'Address', 'Balance △1w/△1m', '% of coins', 'First In', 'Last In', 'Number Of Ins', 'First Out', 'Last Out', 'Number Of Outs']
        data_final6= pd.concat([data_final5,df8 ], axis=0)

        ultima_entrada = data_final6.loc[:,['Address','Last In']]

        entradas_dia =  ultima_entrada[ultima_entrada['Last In'].str.contains(fechaActual[:10], case=False)]
        entradas_hora = ultima_entrada[ultima_entrada['Last In'].str.contains(fechaActual[:13], case=False)]
        
        conteo_entradas_dia = entradas_dia['Address'].shape
        conteo_entradas_hora = entradas_hora['Address'].shape

        self.entradas_hora= entradas_hora['Address']
        
        ultima_salida = data_final6.loc[:,['Address','Last Out']]
        ultima_salida.fillna(0, inplace=True)

        no_aplica= ultima_salida['Last Out']!= 0
        ultima_salida_no_aplica= ultima_salida[no_aplica]

        salidas_dia =  ultima_salida_no_aplica[ultima_salida_no_aplica['Last Out'].str.contains(fechaActual[:10], case=False)]
        salidas_hora = ultima_salida_no_aplica[ultima_salida_no_aplica['Last Out'].str.contains(fechaActual[:13], case=False)]

        conteo_salidas_dia = salidas_dia['Address'].shape
        conteo_salidas_hora = salidas_hora['Address'].shape

        self.salidas_hora= salidas_hora['Address']
        
        print("fecha y hora:",fechaActual[:13])
        print("Entradas del dia (compras):",entradas_dia)
        print("Entradas por hora (compras):", entradas_hora)
        print("Salidas por dia (ventas)",salidas_dia)
        print("Salidas por hora (ventas)",salidas_hora)

        CriptoEntradaHoy= tk.Label(ventana, text= conteo_entradas_dia)
        CriptoEntradaHoy.grid(row=2, column=columna)

        CriptoSalidaHoy= tk.Label(ventana, text= conteo_salidas_dia)
        CriptoSalidaHoy.grid(row=3, column=columna)

        CriptoEntradaHora= tk.Label(ventana, text= conteo_entradas_hora)
        CriptoEntradaHora.grid(row=4, column=columna)

        CriptoSalidaHora= tk.Label(ventana, text= conteo_salidas_hora)
        CriptoSalidaHora.grid(row=5, column=columna)

        """
        if columna == 2:
            direccionesBTC= pd.DataFrame(entradas_hora['Address'])
            
        

        if columna == 6:
            direccionesBTC= pd.DataFrame(entradas_hora['Address'])

        botonHoraLTC= tk.Button(ventana, text="Muestrame", command=direcciones)
        botonHoraLTC.grid(row=6, column=6)
        
        
        ventana2=tk.Tk()

        CriptoEntradaHoy= tk.Label(ventana, text= conteo_entradas_dia)
        CriptoEntradaHoy.grid(row=2, column=columna)
        
        ventana2-mainloop()
        """
        ventana.title("Actualizado: " + horaActual)

    
        

def corriendo():
    extrayendoBSV= Extraccion("https://bitinfocharts.com/top-100-richest-bitcoin%20sv-addresses")
    actualizacionBSV= extrayendoBSV.funcionExtraccion(4)
    extrayendoBTC= Extraccion("https://bitinfocharts.com/top-100-richest-bitcoin-addresses")
    actualizacionBTC= extrayendoBTC.funcionExtraccion(2)
    extrayendoLTC= Extraccion("https://bitinfocharts.com/top-100-richest-litecoin-addresses")
    actualizacionLTC= extrayendoLTC.funcionExtraccion(6)
    extrayendoDash= Extraccion("https://bitinfocharts.com/top-100-richest-dash-addresses")
    actualizacionDash= extrayendoDash.funcionExtraccion(8)
    
def direccionesBTC():
    def graficaCompra():
        ventana3= tk.Tk()
        direccion=entradas_direcciones_btc.loc[int(campoIndice2.get())]
        
        etiquetaDireccion= tk.Label(ventana3, text=direccion)
        etiquetaDireccion.grid(row=1,column=1)
        print(direccion)
        ventana3.mainloop()

    def graficaCompra2():
        ventana4 = tk.Tk()
        direccion = salidas_direcciones_btc.loc[int(campoIndice3.get())]
        
        etiquetaDireccion = tk.Label(ventana4, text=direccion)
        etiquetaDireccion.grid(row=1,column=1)
        print(direccion)
        
        ventana4.mainloop()
        
    ventana2 = tk.Tk()
    extrayendoBTC= Extraccion("https://bitinfocharts.com/top-100-richest-bitcoin-addresses")
    actualizacionBTC= extrayendoBTC.funcionExtraccion(2)
    entradas_direcciones_btc= extrayendoBTC.entradas_hora
    salidas_direcciones_btc= extrayendoBTC.salidas_hora
    print(entradas_direcciones_btc)

    etiqueta_entradas= tk.Label(ventana2, text= "Entradas(Compras):")
    etiqueta_entradas.grid(row=1,column=1)

    EtiquetaDireccionesBTC= tk.Label(ventana2, text=entradas_direcciones_btc)
    EtiquetaDireccionesBTC.grid(row=2,column=1)

    campoIndice2= tk.Entry(ventana2)
    campoIndice2.grid(row=2,column=2)
    campoIndice2.get()

    botonEntradaBtc= tk.Button(ventana2, text="Seguimiento", command=graficaCompra)
    botonEntradaBtc.grid(row=2, column=3)

    etiqueta_salidas= tk.Label(ventana2, text= "Salidas(Ventas):")
    etiqueta_salidas.grid(row=3,column=1)

    EtiquetaDireccionesBTC= tk.Label(ventana2, text=salidas_direcciones_btc)
    EtiquetaDireccionesBTC.grid(row=4,column=1)

    campoIndice3= tk.Entry(ventana2)
    campoIndice3.grid(row=4,column=2)
    campoIndice3.get()

    botonSalidaBtc= tk.Button(ventana2, text="Seguimiento", command=graficaCompra2)
    botonSalidaBtc.grid(row=4, column=3)
    
    ventana2.mainloop()





ventana= tk.Tk()
"""
direccionesBTC = pd.DataFrame()
direccionesBVS = pd.DataFrame()
direccionesLTC = pd.DataFrame()
direccionesDash = pd.DataFrame()
"""

botonActualizar= tk.Button(ventana, text="Actualizar", command=corriendo)
botonActualizar.grid(row=1, column=1)

#________________________BTC_________________________________

etiquetaTitulo= tk.Label(ventana, text="Bitcoin")
etiquetaTitulo.grid(row=1, column=2)

etiquetaBitcoinEntradaHoy= tk.Label(ventana, text="Bitcoin entradas(Compras) hoy:")
etiquetaBitcoinEntradaHoy.grid(row=2, column=1)

etiquetaBitcoinSalidaHoy= tk.Label(ventana, text="Bitcoin salidas(Ventas) hoy:")
etiquetaBitcoinSalidaHoy.grid(row=3, column=1)

etiquetaBitcoinEntradaHora= tk.Label(ventana, text="Bitcoin entradas(Compras) hora:")
etiquetaBitcoinEntradaHora.grid(row=4, column=1)

etiquetaBitcoinSalidaHora= tk.Label(ventana, text="Bitcoin salidas(Ventas) hora:")
etiquetaBitcoinSalidaHora.grid(row=5, column=1)

botonHoraBTC= tk.Button(ventana, text="Muestrame", command=direccionesBTC)
botonHoraBTC.grid(row=6, column=2)

#___________________________BSV______________________________

BSVetiqueta= tk.Label(ventana, text="BSV")
BSVetiqueta.grid(row=1, column=4)

BSVetiquetaBitcoinEntradaHoy= tk.Label(ventana, text="BSV entradas(Compras) hoy:")
BSVetiquetaBitcoinEntradaHoy.grid(row=2, column=3)

BSVetiquetaBitcoinSalidaHoy= tk.Label(ventana, text="BSV salidas(Ventas) hoy:")
BSVetiquetaBitcoinSalidaHoy.grid(row=3, column=3)

BSVetiquetaBitcoinEntradaHora= tk.Label(ventana, text="BSV entradas(Compras) hora:")
BSVetiquetaBitcoinEntradaHora.grid(row=4, column=3)

BSVetiquetaBitcoinSalidaHora= tk.Label(ventana, text="BSV salidas(Ventas) hora:")
BSVetiquetaBitcoinSalidaHora.grid(row=5, column=3)

botonHoraBSV= tk.Button(ventana, text="Muestrame")
botonHoraBSV.grid(row=6, column=4)

#___________________________LTC______________________________

LTCetiquetaTitulo= tk.Label(ventana, text="LTC")
LTCetiquetaTitulo.grid(row=1, column=6)

LTCetiquetaBitcoinEntradaHoy= tk.Label(ventana, text="LTC entradas(Compras) hoy:")
LTCetiquetaBitcoinEntradaHoy.grid(row=2, column=5)

LTCetiquetaBitcoinSalidaHoy= tk.Label(ventana, text="LTC salidas(Ventas) hoy:")
LTCetiquetaBitcoinSalidaHoy.grid(row=3, column=5)

LTCetiquetaBitcoinEntradaHora= tk.Label(ventana, text="LTC entradas(Compras) hora:")
LTCetiquetaBitcoinEntradaHora.grid(row=4, column=5)

LTCetiquetaBitcoinSalidaHora= tk.Label(ventana, text="LTC salidas(Ventas) hora:")
LTCetiquetaBitcoinSalidaHora.grid(row=5, column=5)

botonHoraLTC= tk.Button(ventana, text="Muestrame")
botonHoraLTC.grid(row=6, column=6)


#___________________________Dash_____________________________

DASHetiquetaTitulo= tk.Label(ventana, text="Dash")
DASHetiquetaTitulo.grid(row=1, column=8)

DASHetiquetaBitcoinEntradaHoy= tk.Label(ventana, text="DASH entradas(Compras) hoy:")
DASHetiquetaBitcoinEntradaHoy.grid(row=2, column=7)

DASHetiquetaBitcoinSalidaHoy= tk.Label(ventana, text="DASH salidas(Ventas) hoy:")
DASHetiquetaBitcoinSalidaHoy.grid(row=3, column=7)

DASHetiquetaBitcoinEntradaHora= tk.Label(ventana, text="DASH entradas(Compras) hora:")
DASHetiquetaBitcoinEntradaHora.grid(row=4, column=7)

DASHetiquetaBitcoinSalidaHora= tk.Label(ventana, text="DASH salidas(Ventas) hora:")
DASHetiquetaBitcoinSalidaHora.grid(row=5, column=7)

botonHoraDash= tk.Button(ventana, text="Muestrame")
botonHoraDash.grid(row=6, column=8)



ventana.mainloop()


