## 6

Установить SIEM систему (на ваше усмотрение Wazuh, ELK\EFK, cloud splunk)

- Настроить логирование и отправку windows  логов
- Настроить логирование и отправку linux syslog / auditd 

___

1. Развертывание Wazuh на виртуальную машину. 
- логин: wazuh-user
- пароль: wazuh

![1](https://github.com/user-attachments/assets/24889b66-45ef-435c-b558-db35f0cfa52a)

___
2. Настройка сети: вместо Bridge -> нужно NAT

        ip add
    

узнаем IP, в моем случае 192.168.189.132

___
3. Вход на сервер и его включениие:

        sudo -i

        systemctl start wazuh-manager

        systemctl enable wazuh-indexer

        systemctl start wazuh-indexer

        systemctl deamon-reload

        systemctl enable wazuh-dashboard

        systemctl start wazuh-dashboard

        systemctl status
       
___     
4. Установка Wazuh Agent:

входим "http://192.168.189.132/"
- логин: admin
- пароль: admin

![2](https://github.com/user-attachments/assets/e42fb17b-0720-49e7-9529-61b8ab99b387)
![4](https://github.com/user-attachments/assets/ebfd3055-5d25-4d49-923b-bc34f17783a0)
___
5a. Установка Wazuh Agent на Linux:

Если агент еще не установлен, выполните следующие шаги:

bash:

 Для Debian/Ubuntu

 	curl -s https://packages.wazuh.com/4.x/apt/doc/install.sh | bash
	apt-get install wazuh-agent

 Для CentOS/RHEL

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
___
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

![6](https://github.com/user-attachments/assets/4941029f-bd77-481d-8af4-9cb17b2a5cf6)
___
7. Настройка отправки логов:

Убедитесь, что Wazuh Agent настроен для отправки логов на сервер Wazuh:


	xml
	<server>
	  <address>wazuh-manager-ip</address>
	  <port>1514</port>
	</server>

![7++](https://github.com/user-attachments/assets/b437553a-6552-4180-8c79-e95f11dc6e8e)
![7+](https://github.com/user-attachments/assets/6e9524b1-263a-4cd9-aaa4-8f4aee26b214)
___
8. Перезапуск Wazuh Agent:

После внесения изменений перезапустите агент, bash:

Для Debian/Ubuntu

	systemctl restart wazuh-agent
	

Для CentOS/RHEL

	systemctl restart wazuh-agent
	

Перезапустить службу Wazuh Agent с помощью командной строки:

	net start wazuh-agent


Проверьте статус:
Убедитесь, что агент успешно подключен к Wazuh Manager, bash:

	wazuh-logtest


___
9. Настройка auditd(если надо):

Если auditd не установлен, его можно установить и настроить, 
bash:

Установка auditd

	apt-get install auditd  # Для Debian/Ubuntu
	yum install audit  # Для CentOS/RHEL
	

Отредактировать конфигурацию auditd, если необходимо, и убедитесь, что он запущен, 
bash:

	systemctl start auditd
	systemctl enable auditd
![auditd](https://github.com/user-attachments/assets/dc4b4bc2-3522-4e06-952e-3e2156bc0b6f)

___
10. Проверка состояния:

Убедитесь, что агент работает и передает логи на Wazuh Manager, 
bash:

	/var/ossec/bin/agent_control -l

Dashboard (пример):
![dashboard](https://github.com/user-attachments/assets/1d18973f-07f0-4e74-8fa9-956a6ae84760)
___
11. Настройка правил в Wazuh:

В Wazuh Manager можно дополнительно настроить правила для обработки логов syslog и auditd, если необходимо.
![rules](https://github.com/user-attachments/assets/e73a6306-584b-4a9d-b6cd-c9883f473b03)

После настройки, вы сможете видеть логи Windows в интерфейсе Wazuh. Проверьте, что данные поступают корректно, и настроены необходимые правила для их обработки.
___
Заключение:

Теперь Wazuh должен успешно собирать логи из syslog и auditd на вашем Linux-сервере. Убедиться, что логи Wazuh проверяются для диагностики и мониторинга.

Логирование и отправка логов Windows в Wazuh настроено. Если возникнут проблемы, проверить логи агента и Wazuh Manager для получения дополнительной информации о возможных ошибках.
___

