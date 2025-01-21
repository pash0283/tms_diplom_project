## 6

Установить SIEM систему (на ваше усмотрение Wazuh, ELK\EFK, cloud splunk)

Настроить логирование и отправку windows  логов

Настроить логирование и отправку linux syslog / auditd 

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

        systemctl deamon -reload

        systemctl enable wazuh-dashboard

        systemctl start wazuh-dashboard

        systemctl status
       
     
5. Соединение агентов:

входим http://192.168.189.132/
- логин: admin
- пароль: admin

![Снимок экрана 2025-01-21 160700](https://github.com/user-attachments/assets/09548e2c-9e47-4682-8f64-8696b237c00d)

На хосте добавляем себя как агента, deploy new agent:

- на Windows: устанавливаем программу Agent или по инструкции в PowerShell (запуск с правами администратора):

    
        Invoke-WebRequest -Uri https://packages.wazuh.com/4.x/windows/wazuh-agent-4.7.5-1.msi -OutFile ${env.tmp}\wazuh-agent; msiexec.exe /i ${env.tmp}\wazuh-agent /q WAZUH_MANAGER='192.168.189.132' WAZUH_AGENT_GROUP='default' WAZUH_REGISTRATION_SERVER='192.168.189.132'


    
        NET START WazuhSvc

 

![Снимок экрана 2025-01-21 163502](https://github.com/user-attachments/assets/3d973550-3f3b-4e9e-98a1-2bf7cc604822)



- на Linux по инструкции вводим:

 bash:

        curl -o wazuh-agent-4.7.5-1.x86_64.rpm https://packages.wazuh.com/4.x/yum/wazuh-agent-4.7.5-1.x86_64.rpm && sudo WAZUH_MANAGER='192.168.189.132' WAZUH_AGENT_GROUP='default' rpm -ihv wazuh-agent-4.7.5-1.x86_64.rpm

    
        sudo systemctl daemon-reload
        sudo systemctl enable wazuh-agent
        sudo systemctl start wazuh-agent
    

![Снимок экрана 2025-01-21 163644](https://github.com/user-attachments/assets/dcdd82d9-b551-4caf-bbdb-792485afc297)



