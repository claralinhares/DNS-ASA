# DNS-ASA 🖥️
È preciso entrar no arquivo _/etc/hosts_ e apagar os dados anteriormente inseridos.
Atulizar os pacotes e instalar o bind9 que é um servidor DNS.
```
sudo apt-get update
sudo apt-get install bind9
```
Após isso é necessário trocar o IP manualmente e colocar o da máquina também no DNS, para que ele funcione como servidor.
 Entrar no arquivo _/etc/bind_ para ver os arquivos de configuração e utilizar um arquivo como backup
```
sudo cp named.conf.local named.conf.localOLD
```
Editar o arquivo
```
sudo nano named.conf.local
```
Colocar dentro do arquivo as configurações, criar uma zona direta que faz com que o que seja digitado no navegador, ele vai rertornar o Ip contendo as informações daquele site.
```
//zona direta
zone”ifrn.com” {
	type master;
	file “/etc/bind/db.ifrn.com”;
};
```
E  na zona reversa, coloca-se um ip e ele vai rertonar como um nome 
```
//zona reversa
zone “192.168.0.-in.addr.arpa”{
	type master;
	file “/etc/bind/db.reverso10;
};
```
Editar os arquivos .db, fazendo uma cópia dos arquivos que já existiam para a zona direta e reversa
```
sudo cp db.local db.ifrn.com
sudo cp db.127 db.reverso10
```
Entrar dentro dos arquivos e trocar o nome _localhost_ pelo nome escolhido do site, neste caso sendo _ifrn.com_. E colocar o ip do servidor.
No arquivo reverso também colocar o _ifrn.com_, porém onde tinha 1.0.0 trocar por 10.
Entrar no arquivo  _/etc/resolv.conf_ e colocar o ip escolhido

```
sudo /etc/init.d/named restart
```
Reinicia o serviço BIND para aplicar as alterções feitas.
```
nslookup ifrn.com
host ifrn.com
```
Testa se o domínio ifrn.com está sendo resolvido corretamente
```
sudo nano db.ifrn.com
```
Colocar os sites que anteriormente haviam sido criados
## Configuração de Virtual Host no Apache
Alteração/dos arquivos
```
cd /etc/apache2/sites-available/
sudo nano site1
```
Alterar as seguintes linhas
```
serverAdmin admin@site1.ifrn.com
ServerName site1.ifrn.com
ServerAlias  www.site1.ifrn.com
DocumentRoot /var/www/site1/public_html
```
Realizar a mesma configuração para o site2 (_nano site2_)
## Ativando os Sites e Reiniciando os Serviços
Reinicia o Apache 
```
sudo /etc/init.d/apache2 restart
```
Reinicia o serviço DNS para aplicar alterações 
```
sudo /etc/init.d/named restart
```
Ativação dos sites
```
sudo a2ensite site1.conf
sudo a2ensite site2.conf
```
Recarrega as configurações do Apache 
```
systemctl reload apache2
```









