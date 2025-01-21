## 6

Установить SIEM систему (на ваше усмотрение Wazuh, ELK\EFK, cloud splunk)

- Настроить логирование и отправку windows  логов
- Настроить логирование и отправку linux syslog / auditd 

___

1. Развертывание Wazuh на виртуальную машину. 
- логин: wazuh-user
- пароль: wazuh

![Снимок экрана 2025-01-21 122112](https://github.com/user-attachments/assets/cb2f90c3-0770-42f3-ae67-98fbb11ce642)


2. Настройка сети: вместо Bridge -> нужно NAT

        ip add
    

узнаем IP, в моем случае 192.168.189.132


3. Вход на сервер и его включениие:

        sudo -i

        systemctl start wazuh-manager

        systemctl enable wazuh-indexer

        systemctl start wazuh-indexer

        systemctl deamon-reload

        systemctl enable wazuh-dashboard

        systemctl start wazuh-dashboard

        systemctl status
       
     
4. Установка Wazuh Agent:

входим "http://192.168.189.132/"
- логин: admin
- пароль: admin

![Снимок экрана 2025-01-21 160700](https://github.com/user-attachments/assets/09548e2c-9e47-4682-8f64-8696b237c00d)


5a. Установка Wazuh Agent на Linux:

Если агент еще не установлен, выполните следующие шаги:
bash:

	# Для Debian/Ubuntu
	curl -s https://packages.wazuh.com/4.x/apt/doc/install.sh | bash
	apt-get install wazuh-agent

	# Для CentOS/RHEL
	curl -s https://packages.wazuh.com/4.x/yum/doc/install.sh | bash
	yum install wazuh-agent


На хосте добавляем себя как агента, deploy new agent:

- на Linux по инструкции вводим,   bash:

    
        curl -o wazuh-agent-4.7.5-1.x86_64.rpm https://packages.wazuh.com/4.x/yum/wazuh-agent-4.7.5-1.x86_64.rpm && sudo WAZUH_MANAGER='192.168.189.132' WAZUH_AGENT_GROUP='default' rpm -ihv wazuh-agent-4.7.5-1.x86_64.rpm


        sudo systemctl daemon-reload
        sudo systemctl enable wazuh-agent
        sudo systemctl start wazuh-agent
          
    
![Снимок экрана 2025-01-21 163644](https://github.com/user-attachments/assets/dcdd82d9-b551-4caf-bbdb-792485afc297)


5a. Учтановка Wazuh Agent на Windows:

- на Windows: устанавливаем программу Agent (с официального сайта https://wazuh.com/downloads/) или по инструкции в PowerShell (запуск с правами администратора):

    
        Invoke-WebRequest -Uri https://packages.wazuh.com/4.x/windows/wazuh-agent-4.7.5-1.msi -OutFile ${env.tmp}\wazuh-agent; msiexec.exe /i ${env.tmp}\wazuh-agent /q WAZUH_MANAGER='192.168.189.132' WAZUH_AGENT_GROUP='default' WAZUH_REGISTRATION_SERVER='192.168.189.132'


    
        NET START WazuhSvc

 

![Снимок экрана 2025-01-21 163502](https://github.com/user-attachments/assets/3d973550-3f3b-4e9e-98a1-2bf7cc604822)



6. Настройка Wazuh Agent(Linux):

Отредактируйте конфигурационный файл агента, bash:

	nano /var/ossec/etc/ossec.conf


Добавьте параметры для syslog и auditd:

	xml
	<localfile>
	  <log_format>syslog</log_format>
	  <location>/var/log/syslog</location>
	</localfile>
	
	<localfile>
	  <log_format>auditd</log_format>
	  <location>/var/log/audit/audit.log</location>
	</localfile>
	

6a. Настройка Wazuh Agent(Windows):

Отредактируйте конфигурационный файл:

Откройте файл конфигурации ossec.conf, который находится в директории установки агента (обычно C:\Program Files (x86)\ossec-agent\ossec.conf).

Пример настройки для подключения к Wazuh Manager:

        xml
          <ossec_config>
            <global>
                <agent>
                    <name>Windows-Agent</name>
                    <ip>IP_вашего_Wazuh_Manager</ip>
                </agent>
            </global>
            ...
        </ossec_config>


- Настройка логирования:
Добавьте или измените настройки для отправки логов Windows:

        xml
		 <localfile>
		    <location>EventChannel://Security</location>
		</localfile>
		<localfile>
		    <location>EventChannel://System</location>
		</localfile>
		<localfile>
		    <location>EventChannel://Application</location>
		</localfile>


7. Настройка отправки логов:

8. Перезапуск Wazuh Agent:

9. Настройка auditd(если надо):

10. Проверка состояния:

11. Настройка правил в Wazuh:

Заключение:

1. Установка Wazuh Agent на Windows
1.	Скачайте Wazuh Agent:
Перейдите на официальный сайт Wazuh и скачайте последнюю версию агента для Windows.
2.	Установите Wazuh Agent:
Запустите установщик и следуйте инструкциям на экране.
2. 
3. Запуск и проверка агента
1.	Запустите Wazuh Agent:
После внесения изменений, перезапустите службу Wazuh Agent через Панель управления или с помощью командной строки:
bash
•  net start wazuh-agent
•  Проверьте статус:
Убедитесь, что агент успешно подключен к Wazuh Manager. Это можно сделать через интерфейс Kibana или команду:
bash
2.	wazuh-logtest
3.	
4. Настройка Wazuh Manager
1.	Добавьте нового агента:
Откройте файл ossec.conf на Wazuh Manager и добавьте новую секцию агента:
xml
1.	<agent>
2.	    <name>Windows-Agent</name>
3.	    <ip>IP_вашего_Windows_Agent</ip>
4.	</agent>
5.	
6.	Перезапустите Wazuh Manager:
После внесения изменений, перезапустите службу Wazuh Manager.
5. Проверка логов
После настройки, вы сможете видеть логи Windows в интерфейсе Wazuh. Проверьте, что данные поступают корректно, и настроены необходимые правила для их обработки.

Заключение:

Теперь вы успешно настроили логирование и отправку логов Windows в Wazuh. Если возникнут проблемы, проверьте логи агента и Wazuh Manager для получения дополнительной информации о возможных ошибках.

13. 
