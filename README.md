# Tinycore Conexao RDP
Configuração do SO Tinycore para conexão RDP com redirecionamento da porta COM1 e compartilhamento de impressora via samba

# Instalação

`tce-load -wi bash freerdp2 firefox cups samba`

Faça download de uma imagem e coloque no repositorio `/usr/local/share/pixmaps/`

# Configuração RDP

Faça o download do arquivo `remote.sh` e copie para o diretorio `/home/tc` forneça ao arquivo permissão de execução

Edite o arquivo `remote.sh`adicionando o endereço da conexão em <servidor> e o dominio em <dominio>

Faça o download do arquivo `menu` e copie para o diretorio `/home/tc/.X.d/` forneça ao arquivo permissão de execução

Edite o arquivo `menu` adicione o nome da imagem que foi feita o download no campo `<imagem.png>` e adicione o nome para o atalho da conexão remota no campo `<remoto>`

# Configuração do compartilhamento da impressora via samba

Adicione a impressora via Cups no endereço `localhost:631` em seu navegador (firefox)

Crie diretorio `samba` em `/var/spool/` e dê permissão total a ele

Edite o arquivo `/usr/local/etc/samba/smb.conf` no seguintes campos:

```
Security = share
Host allow = all
Printing = cups


[printers]
comment = All Printers
patch = /var/spool/samba
browseable = no
public = yes
guest ok = yes
print ok = yes
writable = yes
read only = yes
printable = yes
```

Edite o arquivo  `usr/local/etc/cups/cupsd.conf` para compartilhamento de impressora

Adicione as seguintes linhas no arquivo `/opt/.filetool.lst`

```
usr/local/share/pixmaps/<imagem.png>

usr/local/tce.icons

usr/local/etc/cups/ppd/<nomeimpressora>.ppd

usr/local/etc/cups/printers.conf

usr/local/etc/samba/smb.conf

usr/local/etc/cups/cupsd.conf

Var/spool/samba
```

Adicione as seguintes linhas no arquivo `/opt/bootlocal.sh`

```
sudo /usr/local/share/init.d/cups start

sleep 10s

sudo /usr/local/share/init.d/samba start
```

Faça o backup `filetool.sh –b` e renicie a maquina `reboot`
