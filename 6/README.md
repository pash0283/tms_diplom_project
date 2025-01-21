## 6

Установить SIEM систему (на ваше усмотрение Wazuh, ELK\EFK, cloud splunk)

Настроить логирование и отправку windows  логов

Настроить логирование и отправку linux syslog / auditd 

___

1. Развертывание Wazuh на виртуальную машину. 


 <img height="180em" src="https://github.com/pash0283/tms_diplom_project/blob/main/6/%D0%A1%D0%BD%D0%B8%D0%BC%D0%BE%D0%BA%20%D1%8D%D0%BA%D1%80%D0%B0%D0%BD%D0%B0%202025-01-21%20122112.png">


2. Настройка сети: вместо Bridge -> нужно NAT

   ip add

узнаем IP, в моем случае 192.168.189.132


3. Вход на сервер и его включениие:

  
