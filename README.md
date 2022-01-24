# kong-konga-external-postgresql-kubernetes
Manual ini memuat instalasi KONG PI gateway dan KONGA sebagai web management, sedangkan database di SUMSIKAN telah memiliki server database PostgreSQL terpisah yang sudah SIAP, sehingga tinggal membuat database berserta aksesnya, seperti berikut:


```
IP server PostgreSQL: 12.34.56.78
Port		: 5432
Username	: wkongdbuserk8s
Password	: R4ha51iaD0n9!KONG
Database	: wkongdk8s

create database wkongdk8s;
create user wkongdbuserk8s with encrypted password 'R4ha51iaD0n9!KONG';
grant all privileges on database wkongdk8s to wkongdbuserk8s;


IP server PostgreSQL: 12.34.56.78
Port		: 5432
Username	: wkongadbuserk8s
Password	: R4ha51iaD0n9!KONGA
Database	: wkongadbk8s

create database wkongadbk8s;
create user wkongadbuserk8s with encrypted password 'R4ha51iaD0n9!KONGA';
grant all privileges on database wkongadbk8s to wkongadbuserk8s;
```
