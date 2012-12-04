PenguinPalooza
===========

¿ Que es PenguinPalooza ?
-------------------------------------

PenguinPalooza  es un videojuego de presicion desarrollado por PNVG ( Peter and Nicholas VideoGames ) en el año 2012.

¿ En que consiste  ?
---------------------------

Este nos encierra en un tiempo invernal en el cual debemos dispararle a través de un cañón a un pingüino; el cual se alejara cada vez que la bomba impacte en el y de esta forma se avanzara de nivel.

¿ Como se Juega ?
-------------------------

En este a través del mousse y la barra espaciadora debemos direccionar el cañoñ para poder dispararle al pinguino. Con la barra espaciadora le daremos la potencia al tiro y con el mousse controlaremos la direccion y el sentido de tiro de la bomba.

Plataformas de uso
---------------------------

-Para poder correr este juego, el cual tiene requerimientos graficos minimos, debemos tener en nuestro dispositivo python y dentro de este la libreria de pilas ( forma recomendada ). 
-En caso de no tener pilas podemos correrlo importando el modulo sys, este modulo proporciona una serie de funciones y variables que se pueden utilizar para manipular diferentes partes del entorno de ejecución de Python.
.. Note::
-Aqui abajo algunos links para poder descargar Python y pilas:
http://www.python.org/download/
http://www.pilas-engine.com.ar/

Codigo de Python 
------------------------

.. code-block:: python
    :linenos:

    #!/usr/bin/env python
    # -*- coding: utf-8 -*-

    ''''Importamos los modulos'''

    import pilas
    from pilas.actores import Bomba
    from time import sleep
    pilas.iniciar()
    from pilas.escenas import Escena


   '''Representa a la escena que se desarrolla en la ayuda del juego'''

    class Ayuda(Escena):
        
        '''Define las variables de la clase'''
        def __init__(self):
            Escena.__init__(self)
        def iniciar(self) :
            self.fondo = pilas.fondos.Fondo("nubes.png")
            self.crear_texto_ayuda()
       
        
        def crear_texto_ayuda(self):
            titulo = pilas.actores.Texto("Controles", magnitud=30, y=230)
            titulo.color = pilas.colores.rojo
            texto1 = pilas.actores.Texto('Presione la barra espaciadora para poder darle potencia a nuestro disparo', magnitud=13, y=-150)
            texto2 = pilas.actores.Texto('Utilize el raton para poder elegir la direccion de tiro', magnitud=13, y=30)
            def iniciar_juego():
                Menu()


            self.menu = pilas.actores.Menu([("Volver", iniciar_juego),
         
                                        ])
            self.menu.x = -200
            self.menu.y = 200        					
    





    '''Escena que contiene los Creditos y Agradecimientos'''
    class Creditos(Escena):

        def __init__(self):
            Escena.__init__(self)
        def iniciar_juego1(self):
            Menu()
     '''Fondo de Creditos y Texto''' 
        def iniciar(self):
            self.fondo = pilas.fondos.Fondo("fondo_creditos.png")
            self.texto = pilas.actores.Texto("""
    Creado por: Pedro Cabezas y Nicolas Ordonez
    <<<<<<<<<<<<<<<<<<<>>>>>>>>>>>>>>>>>>>>>>
    
    Agradecimientos: A Marcos, Franco, Jair, Lucho y los Profes.
    <<<<<<<<<<<<<<<<<<<>>>>>>>>>>>>>>>>>>>>>>
            """)
            self.texto.color = pilas.colores.negro
            self.texto.x, self.texto.y = 0, 250
            self.texto.escala=0.70
            self.texto.y=[-0,5]
        
            self.menu1 = pilas.actores.Menu([("Volver", self.iniciar_juego1),
         
                                        ])
        
            self.menu1.x = -200
            self.menu1.y = 200
        



    '''Este es el menu principal que contiene Jugar, Ayuda, Creditos y Salir'''   
    class Menu(Escena):

        def __init__(self):
            Escena.__init__(self)

        def iniciar(self):
            self.fondo = pilas.fondos.Fondo('fondo_menu.png')
            self.crear_menu()
        
        def crear_menu(self):
            def iniciar_juego():
                Juego()
            def creditos():
                Creditos()
            def ayuda_iniciar():
                Ayuda()
            def salir_del_juego():
                pilas.terminar()

            self.menu = pilas.actores.Menu([("Iniciar Juego", iniciar_juego),
                                            ("Ayuda", ayuda_iniciar),
                                            ("Creditos", creditos),
                                            ("Salir", salir_del_juego),
                                            ])



    '''Escena donde desarrolla el juego'''
    class Juego(Escena):
    
        def __init__(self, x=180):
            Escena.__init__(self)

            self.x = x

            self.fondo = pilas.fondos.Fondo("frio.png")
            self.pelota = self.crear_pelota()
            self.pingui = self.crear_pingui()
            self.crear_colisiones()
    
        def crear_pelota(self):
            pelota = Pelota(x=-200, y=-20)
            return pelota

        def crear_pingui(self):
            pingui = pilas.actores.Pingu(x=self.x, y=-240)
            pingui.espejado = True
            return pingui

        def crear_colisiones(self):
            def colision(pingui, pelota):
                pingui.eliminar()
                pelota.eliminar()
                def nivel():
                    Juego(self.x+20)
                def menu():
                    Menu()
                def salir():
                    pilas.terminar()

                menu = pilas.actores.Menu([("Siguiente Nivel", nivel),
                                           ("Volver al menu", menu),
                                           ("Salir", salir),
                                         ])
            pilas.mundo.colisiones.agregar(self.pingui, self.pelota, colision)

   
    Menu() 




    class Pelota(Bomba): 

        def __init__(self, x=0, y=0):
            Bomba.__init__(self)
            self.circulo = pilas.fisica.Circulo(x, y, 20)
            self.imitar(self.circulo)
            self.valueImpulse = 0
            self.sentido = 100
            self.barra = pilas.actores.Energia()
            self.barra.rotacion = -90
            self.barra.x, self.barra.y = -270,-110
            self.barra.progreso = 0

        def actualizar(self):
            if pilas.mundo.control.boton:
                if self.valueImpulse >= 100 :
                    self.sentido = -1
                if self.valueImpulse <= 0 :
                   self.sentido = 1
            self.valueImpulse += self.sentido
            else:
                self.impulsar()
                if self.valueImpulse > 0 :
                    self.valueImpulse -= 10

            self.barra.progreso = (self.valueImpulse)
    
        def impulsar(self):
                if (self.valueImpulse>0):
                    self.circulo.impulsar(10000*self.valueImpulse,10000*self.valueImpulse)



    """Ejecutamos el programa"
    pilas.ejecutar()




  
   
