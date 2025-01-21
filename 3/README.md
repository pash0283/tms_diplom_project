## 3

Автоматизировать процесс проверки url через virustotal

Напишите небольшой скрипт для автоматизированной проверки url.    Можно использовать любой язык программирования

___

Для автоматизации проверки URL через VirusTotal вам потребуется использовать их API. Вот пример скрипта на Python, который выполняет эту задачу:

# python:
  
    import requests
    import time
    
    # Замените 'YOUR_API_KEY' на ваш ключ API от VirusTotal
    
    API_KEY = 'YOUR_API_KEY'
    VT_URL = 'https://www.virustotal.com/vtapi/v2/url/report'
    
    def check_url(url):
        params = {
            'apikey': API_KEY,
            'resource': url,
            'allinfo': 'false'
        }
        
        response = requests.get(VT_URL, params=params)
        
        if response.status_code == 200:
            return response.json()
        else:
            print(f"Error: {response.status_code}")
            return None
    
    def main():
        urls_to_check = [
            'http://example.com',
            'http://malicious-url.com'
        ]
    
        for url in urls_to_check:
            print(f"Checking URL: {url}")
            result = check_url(url)
    
            if result:
                if result['response_code'] == 1:
                    print(f"URL: {url} - Detected: {result['positives']}/{result['total']}")
                else:
                    print(f"URL: {url} - Not found in VirusTotal database.")
            else:
                print("Failed to retrieve data.")
    
            # Задержка между запросами, чтобы не превышать лимиты API
            time.sleep(15)
    
    if __name__ == '__main__':
        main()
      
Как использовать скрипт:

Получите ключ API: Зарегистрируйтесь на VirusTotal и получите свой API-ключ.

Установите библиотеку requests: Если у вас её нет, установите с помощью команды, bash: # pip install requests
    
Добавьте свой API-ключ: Замените 'YOUR_API_KEY' на ваш ключ в коде.

Запустите скрипт: Выполните его в терминале или в среде разработки.

Примечания:

Скрипт проверяет список URL и выводит количество положительных обнаружений.

Убедитесь, что вы соблюдаете ограничения по количеству запросов к API, чтобы избежать блокировок.

___
