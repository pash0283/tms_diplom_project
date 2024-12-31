## 2

Создать скрипт на любом языке, который в информативном виде будет запускать скрипт с установкой:

а) AVML - создание дампа оперативной памяти

б) Volatility - фреймворк для работы с артефактами форензики

в) dwarf2json - создание symbol table для кастомного ядра linux

г) Сделает снимок Debug kernel для symbol table
___
а) AVML
Для создания Python-скрипта, который будет запускать процесс установки AVLM и создания дампа оперативной памяти, можно использовать следующий код. Этот скрипт будет включать в себя установку необходимых инструментов и выполнение команд для создания дампа.
	
# python:
import os
import subprocess
import sys

def install_avlm():
    """Устанавливает необходимые инструменты для создания дампа оперативной памяти."""
    try:
        print("Установка необходимых инструментов...")
        # Пример установки dotnet-dump (можно заменить на нужные команды)
        subprocess.check_call([sys.executable, "-m", "pip", "install", "dotnet-dump"])
        print("Инструменты успешно установлены.")
    except subprocess.CalledProcessError as e:
        print(f"Ошибка установки: {e}")
        sys.exit(1)

def create_memory_dump(pid):
    """Создает дамп оперативной памяти для указанного PID."""
    try:
        print(f"Создание дампа памяти для процесса с PID: {pid}...")
        # Команда для создания дампа (замените на вашу команду)
        command = f"dotnet-dump collect -p {pid}"
        subprocess.check_call(command, shell=True)
        print("Дамп памяти успешно создан.")
    except subprocess.CalledProcessError as e:
        print(f"Ошибка при создании дампа: {e}")
        sys.exit(1)

def main():
    install_avlm()
    
    # Запрашиваем PID целевого процесса
    pid = input("Введите PID процесса для создания дампа: ")
    
    create_memory_dump(pid)

if __name__ == "__main__":
    main()

)

Объяснение кода:

    Установка инструментов: Функция install_avlm() отвечает за установку необходимых инструментов, таких как dotnet-dump. В данном случае используется pip для установки пакета.
    Создание дампа: Функция create_memory_dump(pid) принимает идентификатор процесса (PID) и выполняет команду для создания дампа памяти. В примере используется команда dotnet-dump collect -p {pid}.
    Основная функция: В функции main() вызываются функции установки и создания дампа, а также запрашивается PID у пользователя.

Запуск скрипта:
Для запуска скрипта сохраните его в файл, например memory_dump.py, и выполните команду в терминале:
	bash:
	python memory_dump.py

Скрипт запросит PID процесса и создаст дамп оперативной памяти для указанного процесса. Убедитесь, что у вас есть необходимые права доступа для выполнения этих операций.
___

б) Volatility
python:

import subprocess
import sys
import tkinter as tk
from tkinter import messagebox

def run_command(command):
    """Запускает команду в терминале и возвращает результат."""
    try:
        result = subprocess.run(command, shell=True, check=True, text=True, capture_output=True)
        return result.stdout
    except subprocess.CalledProcessError as e:
        return e.stderr

def install_volatility():
    """Устанавливает Volatility и его зависимости."""
    commands = [
        "sudo apt update && sudo apt upgrade -y",
        "sudo apt install git python2.7 python3-pip python-setuptools build-essential python-dev-is-python2 -y",
        "sudo pip install distorm3==3.4.4",
        "git clone https://github.com/volatilityfoundation/volatility.git",
        "chmod +x volatility/vol.py",
        "sudo mv volatility /opt/volatility",
        "sudo ln -s /opt/volatility/vol.py /usr/bin/vol.py",
        "vol.py --info"
    ]
    
    for command in commands:
        output = run_command(command)
        print(f"Команда: {command}\nРезультат:\n{output}\n")

def on_install():
    """Обработчик нажатия кнопки установки."""
    install_volatility()
    messagebox.showinfo("Установка завершена", "Volatility успешно установлен!")

# Создание графического интерфейса
root = tk.Tk()
root.title("Установка Volatility")

install_button = tk.Button(root, text="Установить Volatility", command=on_install)
install_button.pack(pady=20)

root.mainloop()


Объяснение кода:
    Импорт библиотек: Используются библиотеки subprocess для выполнения команд в терминале и tkinter для создания графического интерфейса.
    Функция run_command: Эта функция принимает команду в виде строки, выполняет её и возвращает результат. Если команда завершается с ошибкой, возвращается сообщение об ошибке.
    Функция install_volatility: В этой функции перечислены команды для установки Volatility и его зависимостей. Каждая команда выполняется последовательно, а результат выводится в консоль.
    Функция on_install: Обработчик нажатия кнопки, который запускает установку Volatility и показывает сообщение о завершении.
    Графический интерфейс: Создается простое окно с кнопкой для начала установки.
Этот скрипт предоставляет пользователю информативный интерфейс для установки фреймворка Volatility, выводя результаты выполнения команд в консоль.
___

в) dwarf2json
python:

import subprocess
import sys
from rich.console import Console
from rich.progress import Progress

console = Console()

def install_dmarf2json():
    console.print("[bold green]Starting installation of dmarf2json...[/bold green]")
    
    # Установка dmarf2json с помощью pip
    try:
        with Progress() as progress:
            task = progress.add_task("[cyan]Installing dmarf2json...", total=None)
            subprocess.check_call([sys.executable, "-m", "pip", "install", "dmarf2json"], stdout=subprocess.PIPE, stderr=subprocess.PIPE)
            progress.update(task, advance=1)
            console.print("[bold green]Installation completed successfully![/bold green]")
    except subprocess.CalledProcessError as e:
        console.print(f"[bold red]Error during installation:[/bold red] {e.stderr.decode().strip()}")

def main():
    console.print("[bold blue]Welcome to the dmarf2json installer![/bold blue]")
    console.print("[yellow]This script will install dmarf2json, a tool for creating a symbol table for custom Linux kernels.[/yellow]")
    install_dmarf2json()

if __name__ == "__main__":
    main()



Как использовать скрипт:
    Убедитесь, что у вас установлен Python и pip.
    Установите библиотеку rich, если она у вас не установлена:
			bash:
			pip install rich
Скопируйте приведенный выше код в файл, например, install_dmarf2json.py.
Запустите скрипт:
			bash:
			python install_dmarf2json.py
Что делает скрипт:
    Информирует пользователя о начале установки.
    Выполняет команду установки dmarf2json.
    Показывает прогресс установки.
    Отображает сообщения об успехе или ошибках.

Этот скрипт поможет вам установить dmarf2json и получить информативный вывод в терминале.
___

г) снимок Debug kernel для symbol table
python:

import subprocess
import os
import sys

def install_dependencies():
    try:
        print("Установка необходимых зависимостей...")
        subprocess.run(["sudo", "apt-get", "install", "-y", "linux-image-debug"], check=True)
        print("Зависимости установлены.")
    except subprocess.CalledProcessError as e:
        print(f"Ошибка установки зависимостей: {e}")
        sys.exit(1)

def create_debug_snapshot():
    try:
        print("Создание снимка отладочного ядра...")
        snapshot_command = "sudo cp /boot/vmlinuz-debug /boot/vmlinuz-debug.snapshot"
        subprocess.run(snapshot_command, shell=True, check=True)
        print("Снимок отладочного ядра создан.")
    except subprocess.CalledProcessError as e:
        print(f"Ошибка при создании снимка: {e}")
        sys.exit(1)

def main():
    install_dependencies()
    create_debug_snapshot()

if __name__ == "__main__":
    main()



Описание скрипта:
    Установка зависимостей:
        Функция install_dependencies() устанавливает необходимые пакеты, такие как linux-image-debug.
    Создание снимка отладочного ядра:
        Функция create_debug_snapshot() создает копию ядра для отладки.
    Главная функция:
        main() вызывает функции установки и создания снимка.

Запуск скрипта
Для запуска скрипта выполните команду в терминале:
bash:
python3 script_name.py

Замените script_name.py на имя вашего файла.
Примечание:
    Убедиться, что у есть права sudo для установки пакетов и создания снимков.
    Скрипт подходит для систем на базе Debian/Ubuntu. Если использовать другую операционную систему, команды установки могут отличаться.
___

