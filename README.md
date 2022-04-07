# Servidor HTTP/HTTPS Linux

Hoje vamos montar um servidor HTTP/HTTPS em Linux, para esse servidor vamos utilizar o famoso NGINX que é um servidor bem completo e totalmente gratuito.
Vamos utilizar um sistema de container para armazenar e isolar os processos do nosso servidor e dos sistemas necessários. Vou utilizar o DOCKER para gerenciamento dos containers e imagens utilizada neste treinamento.

## Requisitos:

Ter o Docker instalado e funcionando!

Instalação: [https://docs.docker.com/engine/install/](https://docs.docker.com/engine/install/)


## Estrutura:

	— servidornginx

		– app
			– public
				– static
					– logo.png
				– index.html
		– nginx
			– default.conf
		– docker-compose.yml







## Criando arquivos e pastas:
	
Vamos começar criando todos os arquivos e pastas que contêm nosso projeto, aqui vou nomear nossa pasta raiz como servidornginx, então criamos as pastas:

    Pastas:
      servidorngix
        app
          public
            static
        nginx


## Arquivos:
Agora vamos criar os arquivos, temos primeiro uma imagem à nossa logo e após a imagem começaremos pelo arquivo index.html que é nossa página principal, não vou estar comentando nada do código desse arquivo pois é apenas um HTML bem simples.

  ### servidorngix/app/public/static/logo.png

  ### servidorngix/app/public/index.html
  
      <!doctype html>
      <html>
      <head>
         <title> Servidor Nginx e PHP-FPM 7.4 </title>
      <head>
      <body>
      <img src='static/logo.png' style='width:10%;margin-left:45%;margin-top:5%;'/>
      <div style='float:center;margin:20%;margin-top:5%;text-align:center;'>
         <h2> Servidor para conteudo statico</h2>
         <p> Servidor em nginx em Docker, configurado para distribuir conteudo statico para site simples em html </p>
      </div>


      </body>
      </html>


Agora vem nosso arquivo de configuração do nginx, esse arquivo pode ser bem mais complexo dependendo de suas exigências e necessidades, vou estar deixando um exemplo de um modelo bem simples onde só escuta a porta 80 (HTTP) e redireciona todos os hosts names para os arquivos públicos na pasta app/public/

### servidorngix/nginx/default.conf

    server {

       listen  80 ;
       server_name _ ;
       charset UTF-8 ;
       root /var/www/html/public;

       location /statics/ {
           autoindex on;
       }

       location / {
          index index.html index.htm ;
       }

    }



Agora vamos criar nosso último arquivo ele será responsável pela configuração do nosso container esse arquivo pode contar muito mais configurações conforme a necessidade.
	
## servidorngix/docker-compose.yml

    version: "3"
    services:

       web:
           image: nginx
           ports:
             - "80:80"
           volumes:
             - "./nginx/:/etc/nginx/conf.d/"
             - "./app/public/:/var/www/html/public/"


## Iniciando Servidor:


Para iniciar nosso servidor abrimos um terminal de comandos e navegamos até a pasta do projeto servidorngix ai digitamos o comando para para dar início ao container docker.
	

Comando para iniciar:

	docker-compose up -d

Comando para finalizar:

		docker-compose down

	
Com isso temos nosso servidor NGINX funcionando em DOCKER e podemos servir arquivos estáticos em nossa pasta do servidorngix/app/public/  e servidorngix/app/public/static.


Podemos também alterar as configurações do nginx para mudar as pastas portas e também os domínios que poderão acessar nosso conteúdo, lembrando que toda vez que for alterado as configurações do nginx deverá ser reiniciado o serviço dentro do container ou o container inteiro.




## Conclusão:

Até aqui criamos um servidor simples onde podemos servir conteúdo estático como imagens, páginas html, e arquivos em geral. Um servidor em docker que pode ser rodado em qualquer sistema que tenha docker instalado, onde facilita o compartilhamento sem erros de compatibilidade e também a montagem, migração e escalonamento desse servidor, pois podemos gerenciá-lo via DOCKER SWARM.

