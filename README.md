- 👋 Hi, I’m @Minnimuhametova
- 👀 I’m interested in ...
- 🌱 I’m currently learning ...
- 💞️ I’m looking to collaborate on ...
- 📫 How to reach me ...

<!---
Minnimuhametova/Minnimuhametova is a ✨ special ✨ repository because its `README.md` (this file) appears on your GitHub profile.
You can click the Preview link to take a look at your changes.
--->
PS C:\Users\Администратор> cd C:\OpenSSL-Win64\bin
PS C:\OpenSSL-Win64\bin> .\openssl genrsa -out root.key
Generating RSA private key, 2048 bit long modulus (2 primes)
.................+++++
.........+++++
e is 65537 (0x010001)
PS C:\OpenSSL-Win64\bin>  .\openssl req -new -x509 -days 1826 -key root.key -out root.crt
You are about to be asked to enter information that will be incorporated
into your certificate request.
What you are about to enter is what is called a Distinguished Name or a DN.
There are quite a few fields but you can leave some blank
For some fields there will be a default value,
If you enter '.', the field will be left blank.
-----
Country Name (2 letter code) [AU]:RU
State or Province Name (full name) [Some-State]:RB
Locality Name (eg, city) []:UFA
Organization Name (eg, company) [Internet Widgits Pty Ltd]:IB
Organizational Unit Name (eg, section) []:IT
Common Name (e.g. server FQDN or YOUR name) []:root
Email Address []:iwtm.demo@lab
PS C:\OpenSSL-Win64\bin> .\openssl genrsa -out iwtm.key
Generating RSA private key, 2048 bit long modulus (2 primes)
................................+++++
..................+++++
e is 65537 (0x010001)
PS C:\OpenSSL-Win64\bin>  .\openssl req -new -x509 -days 1826 -key iwtm.key -out iwtm.crt
You are about to be asked to enter information that will be incorporated
into your certificate request.
What you are about to enter is what is called a Distinguished Name or a DN.
There are quite a few fields but you can leave some blank
For some fields there will be a default value,
If you enter '.', the field will be left blank.
-----
Country Name (2 letter code) [AU]:RU
State or Province Name (full name) [Some-State]:RB
Locality Name (eg, city) []:UFA
Organization Name (eg, company) [Internet Widgits Pty Ltd]:IB
Organizational Unit Name (eg, section) []:IT
Common Name (e.g. server FQDN or YOUR name) []:iwtm
Email Address []:iwtm@demo.lab
PS C:\OpenSSL-Win64\bin>  .\openssl  req -new -key iwtm.key -out iwtm.csr
You are about to be asked to enter information that will be incorporated
into your certificate request.
What you are about to enter is what is called a Distinguished Name or a DN.
There are quite a few fields but you can leave some blank
For some fields there will be a default value,
If you enter '.', the field will be left blank.
-----
Country Name (2 letter code) [AU]:ru
State or Province Name (full name) [Some-State]:rb
Locality Name (eg, city) []:ufa
Organization Name (eg, company) [Internet Widgits Pty Ltd]:ib
Organizational Unit Name (eg, section) []:it
Common Name (e.g. server FQDN or YOUR name) []:iwtm
Email Address []:iwtm@demo.lab

Please enter the following 'extra' attributes
to be sent with your certificate request
A challenge password []:xxXX1234
An optional company name []:root

PS C:\OpenSSL-Win64\bin>  .\openssl x509 -req -days 730 -in iwtm.csr -CA root.crt -CAkey root.key -set_serial 01 -out iwtm.crt
Signature ok
subject=C = ru, ST = rb, L = ufa, O = ib, OU = it, CN = iwtm, emailAddress = iwtm@demo.lab
Getting CA Private Key
PS C:\OpenSSL-Win64\bin> .\openssl genrsa -out arm.key
Generating RSA private key, 2048 bit long modulus (2 primes)
..........................................+++++
...+++++
e is 65537 (0x010001)
PS C:\OpenSSL-Win64\bin> .\openssl  req -new -key arm.key -out arm.csr
You are about to be asked to enter information that will be incorporated
into your certificate request.
What you are about to enter is what is called a Distinguished Name or a DN.
There are quite a few fields but you can leave some blank
For some fields there will be a default value,
If you enter '.', the field will be left blank.
-----
Country Name (2 letter code) [AU]:ru
State or Province Name (full name) [Some-State]:rb
Locality Name (eg, city) []:ufa
Organization Name (eg, company) [Internet Widgits Pty Ltd]:ib
Organizational Unit Name (eg, section) []:it
Common Name (e.g. server FQDN or YOUR name) []:arm
Email Address []:arm@demo.lab

Please enter the following 'extra' attributes
to be sent with your certificate request
A challenge password []:xxXX1234
An optional company name []:arm
PS C:\OpenSSL-Win64\bin> .\openssl x509 -req -days 730 -in arm.csr -CA iwtm.crt -CAkey iwtm.key -set_serial 01 -out arm.crt
Signature ok
subject=C = ru, ST = rb, L = ufa, O = ib, OU = it, CN = iwtm, emailAddress = arm@demo.lab
Getting CA Private Key
PS C:\OpenSSL-Win64\bin>

Подключаемся к ТМ через WinCSP

Перекидываем наш сертификат


Извлекаем из сертификата публичный ключ (считаем, что pfx-ключ находится в папке root)
Извлекаем из сертификата закрытый ключ


openssl pkcs 12 -in ./iwtm.p12 -nokeys -out /opt/iw/tm5/etc/certification/iwtm.crt -password pass:xxXX1234




openssl pkcs 12 -in ./iwtm.p12 -nocerts -nodes -out /opt/iw/tm5/etc/certification/iwtm.key -password pass:xxXX1234


Переносим полученные файлы в папку сертификатов


mv /root/iwtm.cer /opt/iw/tm5/etc/certification
mv /root/iwtm.key /opt/iw/tm5/etc/certification


Изменяем файл конфигурации Nginx /etc/nginx/conf.d/iwtm.conf

Перезапускаем Nginx

systemctl restart nginx.service


nano /etc/nginx/conf.d/iwtm.conf
Service nginx restart


Нажимаем установить сертификат


Выбираем Доверенные корневые центры



Добавляем сертификат сервера в промежуточные доверенные центры сертификации


Далее также только в Личное






Видим путь сертификатов



Видим безопасное подключение к ТМ


