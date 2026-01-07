# 1
проекти
import speech_recognition as sr
from gtts import gTTS
import pygame
import time
import os
import webbrowser

# Функція голосу
def speak(text):
    tts = gTTS(text=text, lang="uk")
    tts.save("voice.mp3")

    pygame.mixer.init()
    pygame.mixer.music.load("voice.mp3")
    pygame.mixer.music.play()

    while pygame.mixer.music.get_busy():
        time.sleep(0.1)

    os.remove("voice.mp3")


# Функція слухання
def listen():
    r = sr.Recognizer()
    with sr.Microphone() as source:
        print("Слухаю...")
        audio = r.listen(source)

    try:
        command = r.recognize_google(audio, language="uk-UA")
        print("Ти сказав:", command)
        return command.lower()
    except:
        return ""


# Старт
speak("Привіт, я твій голосовий асистент. Скажи команду")

while True:
    command = listen()

    if "youtube" in command:
        speak("Відкриваю YouTube")
        webbrowser.open("https://youtube.com")

    elif "google" in command or "браузер" in command:
        speak("Відкриваю браузер")
        webbrowser.open("https://google.com")

    elif "час" in command:
        current_time = time.strftime("%H:%M")
        speak(f"Зараз {current_time}")

    elif "відкрий файл" in command:
        speak("Відкриваю файл")
        os.system("xdg-open ~/PyCharmMiscProject/data/myfile.txt &")


    elif ("код" in command or "pycharm" in
    command):
        speak("Відкриваю проєкт у PyCharm")
        os.system("pycharm-community ~/"
    "PyCharmMiscProject &")

    elif "вийти" in command or "вихід" in command:
        speak("До побачення")
        break

    else:
        speak("Я не зрозумів команду")
