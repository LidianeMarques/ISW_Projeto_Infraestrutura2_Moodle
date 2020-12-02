# <h1 align="center"> Infraestrutura para Sistemas Web <h1>

## <h2 align="center">Projeto em Infraestrutura 2 - AVA_Moodle <h2>
 <hr/>
  <div align="justify">
  
  <p>Esse projeto consiste na instalação de um: Ambiente Virtual de Aprendizagem Existem vários sistemas AVA no mercado. Abordaremos o Moodle. </p>
  <p>Requisitos:</p>  
  
  1. Você deverá instalar e configurar o Moodle em sua infraestrutura;
  2. Além da instalação e configuração, você deverá redigir um relatório descrevendo o processo;
  3. O relatório deverá ser escrito em Markdown e postado no seu github.
 <hr/>
  
  ## 1.__Instalação do Moodle no Ubuntu Server 20.04.__ <h2>  
  
  <h4>Pré-requisitos:</h4>
  
* __Criar um usuário sudo no seu servidor__: vamos realizar os passos neste guia usando um usuário não raiz com privilégios sudo. 
* __Instalar uma pilha LAMP:__ o Moodle precisará de um servidor Web, um banco de dados e um PHP para funcionar corretamente. Configurar uma pilha LAMP (Linux, Apache, MySQL, e PHP) cumpre todos esses requisitos. 
<hr/>
  
   ### 1.1. Instalar software adicional. <h3>
  <p>Para o Moodle funcionar como esperado, precisamos instalar pacotes de software extras usando o comando abaixo</p>
 
 ~~~shell
sudo apt install graphviz aspell ghostscript clamav php7.4-pspell php7.4-curl php7.4-gd php7.4-intl php7.4-mysql php7.4-xml php7.4-xmlrpc php7.4-ldap php7.4-zip php7.4-soap php7.4-mbstring
 ~~~
 
<hr/>

> :warning:**Aviso!:** Se você usar <b>versões mais novas do MariaDB no Ubuntu 20.04</b>, essas alterações no arquivo de configuração surgirão e ocorrerão erros (mysql unknown variable 'innodb_file_format = barracuda'), portanto, comente ou <b>não faça essas alterações</b>, esses valores são obtidos por padrão. em MariaDB 10.2 e removido em MariaDB.  
  ### 1.2. Configurar o arquivo MySql padrão (Caso esteja fazendo em versões anteriores ao Ubuntu Server 20.04). <h3>

<p>Agora precisamos fazer algumas modificações no arquivo de configuração mysql padrão. Usando seu editor de texto preferido, abra o arquivo abaixo.</p>

~~~shell
sudo nano /etc/mysql/mysql.conf.d/mysqld.cnf
~~~

<p>Na seção Mysqld , anexe as linhas abaixo:</p>

~~~shell
default_storage_engine = innodb
innodb_file_per_table = 1
innodb_file_format = Barracuda
innodb_large_prefix = 1
~~~

<p>Depois de acrescentar essas linhas, reinicie o banco de dados MySQL para efetuar as alterações</p>

~~~shell
sudo service apache2 restart
~~~

  ### 1.3. Criar o banco de dados Moodle e o usuário Moodle MySQL com as permissões corretas. <h3>

<p> Para começar, faça login na conta raiz (administrativa) do MySQL, inserindo este comando:</p>

~~~shell
sudo mysql -u root -p
~~~

<p>Será solicitada a senha que você configurou para a conta raiz do MySQL quando instalou o software.</p>
<p> Primeiramente, criamos um banco de dados separado que o moodle irá controlar. Você pode dar o nome que quiser ao banco de dados, mas neste guia nós o chamaremos de moodle para ficar mais simples. Crie o banco de dados para o Moodle digitando:</p>

~~~mysql
CREATE DATABASE moodle DEFAULT CHARACTER SET utf8mb4 COLLATE utf8mb4_unicode_ci;
~~~

<p>Em seguida, vamos criar uma conta de usuário do MySQL separada que vamos usar exclusivamente para operar no nosso novo banco de dados. </p>

>:information_source: __Nota:__ Criar bancos de dados e contas para uma função é uma boa ideia sob o ponto de vista de gerenciamento e segurança. 

<p> Vamos usar o nome <b>moodleuser</b> neste guia. Sinta-se à vontade para alterar isso se quiser.</p>
<p> Vamos criar essa conta, definir uma senha e conceder o acesso ao banco de dados que criamos. Podemos fazer isso digitando o seguinte comando: Lembre-se de escolher uma senha forte para o usuário do seu banco de dados:</p>

~~~mysql
CREATE USER 'moodleuser'@'localhost' IDENTIFIED BY 'suasenha';
~~~

~~~mysql
GRANT ALL PRIVILEGES ON *.* TO 'moodleuser'@'localhost' WITH GRANT OPTION;
~~~

<p>Agora, você tem um banco de dados e uma conta de usuário, criados especificamente para o moodle. Precisamos atualizar os privilégios para que a instância atual do MySQL saiba sobre as alterações recentes que fizemos:</p>

~~~mysql
FLUSH PRIVILEGES;
~~~

<p>Feito isso é só sair do banco usando o comando exit.</p>
<hr/>

  ### 1.4. Instalação do Moodle. <h4>

Faça o download da última versão do moodle em www.moodle.org no link [Downloads Moodle](http://download.moodle.org/)

<p>A versão mais recente do moodle usando no presente tutorial foi a 3.10+</p>
<p>Baixe o arquivo, no diretório /var/www , usando o código abaixo</p>

~~~shell
cd /var/www
~~~

~~~shell
sudo wget https://download.moodle.org/download.php/stable310/moodle-3.10.tgz
~~~

<p>Depois de baixar o arquivo, descompacte usando o comando:</p>

~~~shell
sudo tar xvfz moodle-latest-310.tgz
~~~
  
<p>Se for outra versão, o nome pode ser diferente</p>
<p>Agora para terminar a instalação do moodle vamos precisar usar um navegar web</p>
<p>Em seu navegador padrão digite na url:</p>

~~~http
http://seu_ip_localhost/moodle
~~~



  
  </div>
