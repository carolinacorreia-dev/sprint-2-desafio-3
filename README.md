# sprint-2-desafio-3
<h1><strong>Instalar o docker no linux, criar uma imagem do wordpress com banco de dados e persistir os dados usando docker-compose</strong></h1>
<br>
<strong>Passo 1: Instalar o Docker no Oracle Linux 8</strong>
<br><br>
	1. Atualize seu sistema Linux:
 <br>
	sudo yum update
 <br><br>
	2. Instale o repositório EPEL (Extra Packages for Enterprise Linux) para acessar o Docker:
 <br>
	sudo yum install epel-release
 <br><br>
	3. Instale o Docker Community Edition (CE) e habilite o serviço Docker para iniciar automaticamente:
 <br>
	sudo yum install docker-ce
	sudo systemctl start docker
	sudo systemctl enable docker
 <br><br>
<strong>Passo 2: Instalar o Docker Compose</strong>
<br>
	1. Baixe a versão mais recente do Docker Compose:
 <br>
	sudo curl -L "https://github.com/docker/compose/releases/latest/download/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose
 <br>
	2. Dê permissões de execução ao Docker Compose:
 <br>
	sudo chmod +x /usr/local/bin/docker-compose
 <br><br>
<strong>Passo 3: Criar um diretório para o projeto Docker Compose</strong>
<br>
	Crie um diretório para o projeto Docker Compose. Por exemplo:
	mkdir docker-wordpress
	cd docker-wordpress
 <br><br>
<strong>Passo 4: Criar um arquivo docker-compose.yml</strong>
<br>
	Crie um arquivo chamado docker-compose.yml neste diretório com o seguinte conteúdo:
 <br><br>
services:
<br>
  &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;db:
	<br>
    &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;image: mysql:8.0.27
		<br>
    &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;command: '--default-authentication-plugin=mysql_native_password'
		<br>
    &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;volumes:
		<br>
      &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;- db_data:/var/lib/mysql
			<br>
    &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;restart: always
		<br>
    &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;environment:
		<br>
      &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;- MYSQL_ROOT_PASSWORD=somewordpress
			<br>
      &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;- MYSQL_DATABASE=wordpress
			<br>
      &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;- MYSQL_USER=wordpress
			<br>
      &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;- MYSQL_PASSWORD=wordpress
			<br>
    &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;expose:
		<br>
      &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;- 3306
			<br>
      &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;- 33060
			<br>
  &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;wordpress:
	<br>
    &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;image: wordpress:latest
		<br>
    &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;volumes:
		<br>
      &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;- wp_data:/var/www/html
			<br>
    &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;ports:
		<br>
      &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;- 80:80
			<br>
    restart: always
		<br>
    environment:
		<br>
      - WORDPRESS_DB_HOST=db
			<br>
      - WORDPRESS_DB_USER=wordpress
			<br>
      - WORDPRESS_DB_PASSWORD=wordpress
			<br>
      - WORDPRESS_DB_NAME=wordpress
			<br>
volumes:
<br>
  db_data:
	<br>
  wp_data:
	<br><br>
Isso cria um serviço WordPress e um serviço de banco de dados MySQL com volumes para persistência.
<br><br>
<strong>Passo 5: Iniciar os contêineres</strong>
<br>
	Dentro do diretório onde você criou o arquivo docker-compose.yml, execute o seguinte comando para iniciar os contêineres:
 <br>
	docker-compose up -d
 <br><br>
<strong>Passo 6: Acessar o WordPress</strong>
<br>
	Após iniciar os contêineres, você pode acessar o WordPress no seu navegador em http://localhost.
