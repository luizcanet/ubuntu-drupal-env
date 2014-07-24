Ambiente de Desenvolvimento Drupal no Ubuntu
=============================================

Documentação para auxiliar na configuração do ambiente de desenvolvimento no Ubuntu

Git
---

	$ sudo apt-get install git

MySQL
-----

	$ sudo apt-get install mysql-server

Opcional:

	$ sudo apt-get install mysql-workbench

Crie um usuário MySQL para você:

	$ sudo su
	# mysql -p
	mysql> CREATE USER 'seu_usuario'@'localhost' IDENTIFIED BY 'sua_senha';
	mysql> GRANT ALL PRIVILEGES ON * . * TO 'seu_usuario'@'localhost';
	mysql> FLUSH PRIVILEGES;
	mysql> quit
	# exit

PHP
---

	$ sudo apt-get install php5-fpm php5-mysql php5-gd php5-curl

Dnsmasq
-------

	$ sudo apt-get install dnsmasq
	$ sudo vi /etc/dnsmasq.conf

Descomente a linha que contém (provavelmente a última do arquivo):

conf-dir=/etc/dnsmasq.d

	$ sudo vi /etc/dnsmasq.d/localdomain

Adicione a seguinte linha:

address=/localdomain/127.0.0.1

	$ sudo service dnsmasq restart

Nginx
-----

	$ sudo apt-get install nginx
	$ sudo cp /etc/php5/fpm/pool.d/www.conf /etc/php5/fpm/pool.d/seu_usuario.conf
	$ sudo vi /etc/php5/fpm/pool.d/seu_usuario.conf

Mude a linha:

	[www]

Para:

	[seu_usuario]

As linhas:

	user = www-data
	group = www-data

Para:

	user = seu_usuario
	group = seu_usuario

E a linha:

	listen = /var/run/php5-fpm.socket

Para:

	listen = /var/run/php5-fpm-seu_usuario.socket


	$ sudo service php5-fpm stop
	$ sudo service php5-fpm start

(Melhor reiniciar o PC)

Composer
--------

	$ curl -sS https://getcomposer.org/installer | php
	$ sudo mv composer.phar /usr/local/bin/composer

Drush
-----

	$ sed -i '1i export PATH="$HOME/.composer/vendor/bin:$PATH"' $HOME/.bashrc
	$ source $HOME/.bashrc
	$ composer global require drush/drush:dev-master
	$ sudo cp .composer/vendor/drush/drush/drush.complete.sh /etc/bash_completion.d/
	$ cp ~/.composer/vendor/drush/drush/examples/example.bashrc ~/.drush.bashrc
	$ vi ~/.bashrc
	
Adicione ao final do arquivo:

	if [ -f ~/.drush_bashrc ] ; then
	  . ~/.drush_bashrc
	fi

		
	if [ "\$(type -t __git_ps1)" ] && [ "\$(type -t __drush_ps1)" ]; then
	  PS1='\u@\h \w$(__git_ps1 " (%s)")$(__drush_ps1 "[%s]")\$ '
	fi

Iniciando um Novo Projeto
-------------------------

	$ mysql -p
	mysql> CREATE DATABASE nome_do_projeto;
	mysql> quit

Opção 1 (Via Composer):

	$ composer create-project drupal/drupal nome_do_projeto versao_do_drupal

Opção 2 (Via Git):

	$ git clone --branch versao_do_drupal http://git.drupal.org/project/drupal.git nome_do_projeto

Opção 3 (Via .tar.gz ou Zip):
Baixar o pacote e descompactar no local desejado, renomeando a pasta para nome_do_projeto.



