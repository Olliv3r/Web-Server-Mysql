# Web-Server-Mysql
Configurar phpmyadmin pra rodar com apache

## Etapas necessárias:
<a href="https://github.com/Olliv3r/Web-Server">1 - Configurar apache pra rodar com certificado</a>  
<a href="https://github.com/Olliv3r/Web-Server-Php">2 - Configurar apache pra rodar scripts PHP</a>

### Etapa 1:
#### Instalar dependencias
```
apt update && apt install apache2 php-apache phpmyadmin mariadb -y
```

### Etapa 2:
#### Criar novo usuário no mysql para acessar o phpmyadmin
Execute o serviço `mysqld_safe` como root (*não precisa de root, é so o nome de usuário que se chama `root` que ja vem no mysql) pois este usuário tem todas as permissôes no mysql incluindo a de criar usuários novos*):
```
cd && mysqld_safe -u root &
```

O comando acima irá deixar o `mysqld_safe` rodando em segundo plano, nesse momento entre no mysql como `root` (*so lembrando, não é necessário ter root, este é o nome de usuário que já vem no MySQL com todas as permissôes necessárias*):
```
mysql -u root
```

Ao executar o comando acima, abrirá o console do MySQL mariadb e execute esses passos para criar o novo usuário

Selecione o banco de dados `mysql`
```
use mysql;
```

Crie o usuário (*Substitua o `oliver` e `evilor` por dados de sua preferência*) no meu caso estou expecificando `oliver` como usuário e `evilor` como senha:
```
CREATE USER 'oliver'@'localhost' IDENTIFIED BY 'evilor';
```

Conceda todos os privilégios do banco de dados (*Substitua o `oliver` pelo seu usuário criado*):
```
GRANT ALL PRIVILEGES ON * . * TO 'oliver'@'localhost';
```

Libere imediatamente esses privilégios garantidos com o comando:
```
FLUSH PRIVILEGES;
```

E saia do console do mysql:
```
quit
```

Pare o mysqld_safe que está rodando em segundo plano:
```
pkill mariadb
```

Agora execute o serviço `mysqld_safe` com o novo usuário (*Substitua o `oliver` pelo seu usuário criado*) e der ENTER:
```
mysqld_safe -u oliver &
```

Depois de colocar o serviço `mysqld_safe` em execução, vamos garantir o acesso ao phpmyadmin, vamos fazer 2 alteraçôes nesses 2 arquivos:

Abra o arquivo `httpd.conf`:
```
nano $PREFIX/etc/apache2/httpd.conf
```

Dentro desse arquivo digite ` ctr+w` e pesquise por `<Directory> blocks below` e substitua `denied` por `granted`:

Ficando de:
![Acesso ao phpmyadmin negado](https://github.com/Olliv3r/Web-Server-Mysql/blob/main/media/mysql-denied.jpg)

Para:
![Permitir acesso ao phpmyadmin](https://github.com/Olliv3r/Web-Server-Mysql/blob/main/media/mysql-granted.jpg)

Com esse arquivo terminamos, salve o arquivo digitando `ctr+x+y` e ENTER

Pra finalizar a configuração, abra o arquivo `config.inc.php`:
```
nano $PREFIX/etc/phpmyadmin/config.inc.php
```

Digite `ctr+w`, pesquise por `localhost` e substitua por `127.0.0.1`

Com esse arquivo ja terminamos, salve o arquivo digitando `ctr+x+y` e ENTER.

Ficando de:
![Host local](https://github.com/Olliv3r/Web-Server-Mysql/blob/main/media/mysql-localhost.jpg)

Para:
![Alterando de localhost para 127.0.0.1](https://github.com/Olliv3r/Web-Server-Mysql/blob/main/media/mysql-ip.jpg)

A partir de agora o phpmyadmin já está configurando.

Reinicie o apache com se tiver rodando:
```
apachectl -k restart
```

Se tiver parado, inicie com:
```
apachectl -k start
```

acesse o seguinte link para acessá-lo:
```
termux-open-url https://localhost:8443/phpmyadmin
```

Painel phpmyadmin:
![Logando com novo usuário no phpmyadmin](https://github.com/Olliv3r/Web-Server-Mysql/blob/main/media/mysql-logando.jpg)
Logado com o novo usuário:
![Logado no phpmyadmin](https://github.com/Olliv3r/Web-Server-Mysql/blob/main/media/mysql-logado.jpg)

## Bônus:

### Rodar o servidor web:

Iniciar serviço mysqld_safe (*Substitua o `USUARIO` pelo usuário que voçê criou*):
```
mysqld_safe -u USUARIO &
```
Iniciar serviço apache:
```
apachectl -k start
```

### Parar o servidor web:

Parar serviço mysqld:
```
pkill mariadb
```
Parar serviço apache:
```
apachectl -k stop
```

## Veja o tutorial pelo youtube:  
<a href="https://youtube.com/@tioolive3144">Assistir tutorial</a>
