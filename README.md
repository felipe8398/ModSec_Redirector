# ModSec_Redirector

### Pré requisitos ###
- Ter docker instalado
- Ter uma aplicação WEB

### Executando o teste ###
- Realizar o git clone do repositorio
- Alterar o arquivo Dockerfile, e mudar o http://app:8080 pela URL da sua aplicação por exemplo http://192.168.0.10:80 
``` 
RUN echo "ProxyPass / http://app:8080/" >> /etc/apache2/sites-enabled/000-default.conf
RUN echo "ProxyPassReverse / http://app:8080/" >> /etc/apache2/sites-enabled/000-default.conf

``` 
- Adicionar suas regras do ModSecurity no arquivo regras/log4.conf (No momento ele consta com regras para proteção do Log4Shell)

- Realizar o build do Dockerfile
```sh 
docker image build -t meumodsec:0.7 .
``` 
- Subir o container do ModSecurity
```sh 
docker run -d --name modsec -p 80:80 meumodsec:0.7
``` 

### Pergunta dos universitarios ###

1- Como saber se o meu WAF e aplicação WEB vulneravel subiu ?

Execute ```docker container ls``` , e verifique se tem o container com nome "modsec"
![Captura de tela de 2022-06-19 15-34-11](https://user-images.githubusercontent.com/24979677/174495485-67be6459-f084-4e20-9ede-6cc9d27eefb6.png)

2- Como pegar log do modsec?

Execute ```docker exec -ti $(docker container ls | grep "modsec"| awk '{print $1}') cat /var/log/apache2/error.log```
![Captura de tela de 2022-06-19 15-36-33](https://user-images.githubusercontent.com/24979677/174495577-0c999d2e-77c0-4219-a8a7-ee9b3640d19d.png)

