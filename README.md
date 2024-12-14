import requests
import tkinter
import customtkinter

customtkinter.set_appearance_mode('dark')
customtkinter.set_default_color_theme('blue')

app = customtkinter.CTk()
app.title('Ten currency converter')
app.geometry('530x340')
app.resizable(False, False)

base_currency = 'USD'
target_currency = 'RUB'
url = "https://v6.exchangerate-api.com/v6/{}/pair/{}/{}"
api = '6a647b4f875e4a14d3fdfdfe'

bcl = customtkinter.CTkLabel(master=app, text='').place(relx=0.45, rely=0.04)
tcl = customtkinter.CTkLabel(master=app, text='').place(relx=0.94, rely=0.04)

frame = customtkinter.CTkFrame(master=app, width=500, height=170, fg_color="gray").place(relx=0.02, rely=0.15)

text_field = customtkinter.CTkTextbox(master=frame, width=140, height=37)
text_field.insert(index='0.0', text='')
text_field.place(relx=0.20, rely=0.3)

def convert_currency():
    global response
    global result
    global rate
    
    # Запрос обменного курса между базовой и целевой валютой
    response = requests.get(url.format(api, base_currency, target_currency))
    result = response.json()  # Исправлено: вызов json как метода

    # Извлечение значения курса из ответа
    if response.status_code == 200:  # Проверка успешности запроса
        rate = result['conversion_rate']  # Курс конверсии
        amount = float(text_field.get(0.0, tkinter.END))  # Получаем сумму для конвертации
        
        # Конвертация валюты
        converted_amount = amount * rate  # Рассчитываем конвертированную сумму
        result_label.configure(text=str(round(converted_amount, 2)))  # Отображаем результат на интерфейсе
    else:
        result_label.configure(text="Ошибка!")

def main():
    base_currency = 'USD'
    target_currency = 'RUB'
    amount = text_field.get(0.0, tkinter.END)
    convert_currency()  # Исправлено: вызов без параметров

# Функции выбора базовой валюты
def base_currency1():
    global base_currency
    base_currency = 'USD'
    global bcl
    del bcl
    bcl = customtkinter.CTkLabel(master=app, text='USD', width=30).place(relx=0.45, rely=0.04)

def base_currency2():
    global base_currency
    base_currency = 'EUR'
    global bcl
    del bcl
    bcl = customtkinter.CTkLabel(master=app, text='EUR', width=30).place(relx=0.45, rely=0.04)

def base_currency3():
    global base_currency
    base_currency = 'RUB'
    global bcl
    del bcl
    bcl = customtkinter.CTkLabel(master=app, text='RUB', width=30).place(relx=0.45, rely=0.04)

from_label = customtkinter.CTkLabel(master=app, text='Из:').place(relx=0.1, rely=0.04)

bc_usd = customtkinter.CTkButton(master=app, width=30, height=30, text='USD', command=base_currency1).place(relx=0.15, rely=0.04)
bc_eur = customtkinter.CTkButton(master=app, width=30, height=30, text='EUR', command=base_currency2).place(relx=0.25, rely=0.04)
bc_ru = customtkinter.CTkButton(master=app, width=30, height=30, text='RUB', command=base_currency3).place(relx=0.35, rely=0.04)

to_label = customtkinter.CTkLabel(master=app, text='В:').place(relx=0.6, rely=0.04)

# Функции выбора целевой валюты
def target_currency1():
    global target_currency
    target_currency = 'USD'
    global tcl
    del tcl
    tcl = customtkinter.CTkLabel(master=app, text='USD', width=30).place(relx=0.94, rely=0.04)

def target_currency2():
    global target_currency
    target_currency = 'EUR'
    global tcl
    del tcl
    tcl = customtkinter.CTkLabel(master=app, text='EUR', width=30).place(relx=0.94, rely=0.04)

def target_currency3():
    global target_currency
    target_currency = 'RUB'
    global tcl
    del tcl
    tcl = customtkinter.CTkLabel(master=app, text='RUB', width=30).place(relx=0.94, rely=0.04)

tc_usd = customtkinter.CTkButton(master=app, width=30, height=30, text='USD', command=target_currency1).place(relx=0.65, rely=0.04)
tc_eur = customtkinter.CTkButton(master=app, width=30, height=30, text='EUR', command=target_currency2).place(relx=0.75, rely=0.04)
tc_ru = customtkinter.CTkButton(master=app, width=30, height=30, text='RUB', command=target_currency3).place(relx=0.85, rely=0.04)

def clear():
    text_field.delete(0.0, tkinter.END)
    result_label.configure(text=str(''))

clear_button = customtkinter.CTkButton(master=app, width=40, height=30, command=clear, text='Очистить')
clear_button.place(relx=0.45, rely=0.45)

cal_button = customtkinter.CTkButton(master=app, width=40, height=30, text='Конвертировать', command=convert_currency)
cal_button.place(relx=0.415, rely=0.18)

result_label = customtkinter.CTkLabel(master=frame, text='DELETE ME', width=140, height=37)
result_label.place(relx=0.57, rely=0.30)

app.mainloop()
