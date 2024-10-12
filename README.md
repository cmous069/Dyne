import machine
import utime

# Initialisation de l'UART
uart = machine.UART(0, baudrate=9600, tx=machine.Pin(0), rx=machine.Pin(1))

# Initialisation des LEDs
led_rouge = machine.Pin(18, machine.Pin.OUT)
led_jaune = machine.Pin(19, machine.Pin.OUT)
led_verte = machine.Pin(20, machine.Pin.OUT)

# Fonction pour changer l'état des LEDs
def set_lights(rouge, jaune, vert):
    led_rouge.value(rouge)
    led_jaune.value(jaune)
    led_verte.value(verte)

# Si j’envoie le message voici ma boucle au picot récepteur de mon message . Tant que la condition sera vérifiée 

while True:
    # Feu vert pendant 5 secondes
    set_lights(0, 0, 1)
    uart.write('VERT\n')
    utime.sleep(5)

    # Feu jaune pendant 3 secondes
    set_lights(0, 1, 0)
    uart.write('JAUNE\n')
    utime.sleep(3)
    
    # Feu rouge pendant 5 secondes
    set_lights(1, 0, 0)
    uart.write('ROUGE\n')
    utime.sleep(5)

# Si je reçois le message de la part d’un autre pico voici ma boucle qui viendra vérifier s’il y’a bien un message et s’il y en a un le lire, puis le décoder 

while True:
    if uart.any():
        message = uart.read().decode('utf-8').strip()
        if message == 'VERT':
            set_lights(0, 0, 1)  # Feu vert
        elif message == 'JAUNE':
            set_lights(0, 1, 0)  # Feu jaune
        elif message == 'ROUGE':
            set_lights(1, 0, 0)  # Feu rouge
    utime.sleep(1)

