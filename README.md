# Synapse-ODT-2025
#From Idea to Impact- Open Design &amp; Technology Final Showcase by Arya and Anoushka

# Write your code here :-)
''' ODT Final '''
#Importing Libraries and Classes
from machine import Pin, PWM, TouchPad
import time
import neopixel
import random

#Defining Circuit components
#1-Motors
m1= PWM(Pin(5)) #happy, anxious
m1.freq(50)
m2= PWM(Pin(18)) #sad
m2.freq(50)
m3= PWM(Pin(19)) #bored
m3.freq(50)

motors=[m1, m2, m3]

#2-Touchpins
happy_pin= TouchPad(Pin(13))
sad_pin= TouchPad(Pin(12))
bored_pin= TouchPad(Pin(4)) #red
anxious_pin= TouchPad(Pin(15)) #green

#3-LEDs
line_1= neopixel.NeoPixel(Pin(22), 23)
line_2= neopixel.NeoPixel(Pin(23), 17)
line_3= neopixel.NeoPixel(Pin(32), 22)
line_4= neopixel.NeoPixel(Pin(33), 23)
line_5= neopixel.NeoPixel(Pin(25), 23)
line_6= neopixel.NeoPixel(Pin(26), 23)
line_7= neopixel.NeoPixel(Pin(14), 19)
line_8= neopixel.NeoPixel(Pin(27), 20)

#Categorising words according to grammar
subject= ['you', line_1, [0,3]]
modal_verb= [['must', line_1, [4, 9]], ['should', line_1, [10,17]], ['can', line_1, [18, 22]], ['could', line_2, [0, 5]], ['would', line_2, [6, 12]]]
negative= [['not',line_2, [13, 16]], ['unrequired']]
verb= [['believe',line_3, [0, 7]], ['live',line_3, [8, 13]], ['create', line_3, [14, 21]], ['stop',line_4, [0, 3]], ['start',line_4, [4, 10]], ['slow down',line_4, [12, 22]], ['breathe',line_5, [0, 6]], ['rest',line_5, [7, 12]], ['run', line_5, [13, 17]], ['fly', line_5, [18, 22]], ['remember', line_6, [0, 8]], ['forget', line_6, [9, 17]], ['love', line_6, [18, 22]]]
objectt= [['now', line_7, [0, 2]], ['always', line_7, [3, 11]], ['right', line_7, [12, 18]], ['freely', line_8, [0, 6]], ['first', line_8, [7, 13]], ['last', line_8, [14, 19]]]






def sentence_maker():
    sub= subject[0]
    sub_line= subject[1]
    sub_led_start= subject[2][0]
    sub_led_stop= subject[2][1]
    for i in range(sub_led_start, sub_led_stop+1):
        sub_line[i]=(255,255,255)
        sub_line.write()

    mv_choice= random.randint(0, len(modal_verb)-1)
    mv= modal_verb[mv_choice][0]
    mv_line=modal_verb[mv_choice][1]
    mv_led_start= modal_verb[mv_choice][2][0]
    mv_led_stop= modal_verb[mv_choice][2][1]
    for i in range(mv_led_start, mv_led_stop+1):
        mv_line[i]=(255,255,255)
        mv_line.write()

    neg_choice= random.randint(0, len(negative)-1)
    neg= negative[neg_choice][0]
    if neg=='not':
        neg_line=negative[neg_choice][1]
        neg_led_start= negative[neg_choice][2][0]
        neg_led_stop= negative[neg_choice][2][1]
        for i in range(neg_led_start, neg_led_stop+1):
            neg_line[i]=(255,255,255)
            neg_line.write()

    verb_choice= random.randint(0, len(verb)-1)
    vb= verb[verb_choice][0]
    vb_line=verb[verb_choice][1]
    vb_led_start= verb[verb_choice][2][0]
    vb_led_stop= verb[verb_choice][2][1]
    for i in range(vb_led_start, vb_led_stop+1):
        vb_line[i]=(255,255,255)
        vb_line.write()


    obj_choice= random.randint(0, len(objectt)-1)
    obj= objectt[obj_choice][0]
    obj_line=objectt[obj_choice][1]
    obj_led_start= objectt[obj_choice][2][0]
    obj_led_stop= objectt[obj_choice][2][1]
    for i in range(obj_led_start, obj_led_stop+1):
        obj_line[i]=(255,255,255)
        obj_line.write()


def reset_leds():
    for line in [line_1, line_2, line_3, line_4, line_5, line_6, line_7, line_8]:
        for i in range(len(line)):
            line[i] = (0, 0, 0)
            line.write()

def run_emotion(primary_motor):
    sentence_maker()

    for i in range(26, 80):
        primary_motor.duty(i)
        time.sleep(0.05)

    time.sleep(5)
    reset_leds()



happy_threshold= 100
sad_threshold= 100
anxious_threshold= 100
bored_threshold= 270


while True:
    val_happy= happy_pin.read()
    time.sleep(0.05)
    val_sad= sad_pin.read()
    time.sleep(0.05)
    val_bored= bored_pin.read()
    time.sleep(0.05)
    val_anxious= anxious_pin.read()
    time.sleep(0.05)

    if val_happy<happy_threshold:
        run_emotion(m1)

    if val_sad<sad_threshold:
        run_emotion(m2)

    if val_bored<bored_threshold:
        run_emotion(m3)

    if val_anxious<anxious_threshold:
         run_emotion(m1)
