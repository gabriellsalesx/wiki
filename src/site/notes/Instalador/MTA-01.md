---
{"title":"MTA-01","subtitle":"De Zero a Herói: Instale, Configure e Otimize como um Profissional!","author":"Gabriell Sales","share_link":"https://share.note.sx/5mfvvld1#4UMe8lgOUxfJpGdlEfgkDP6uOKaGu4IvRsolGbPGdcs","share_updated":"2025-02-05T00:02:56-03:00","dg-publish":true,"dg-home":null,"Criado":"2025-02-09","permalink":"/instalador/mta-01/","dgPassFrontmatter":true}
---


<div align="center" style="margin-top:50px; padding:20px; border: 2px dotted #8A5CF4; border-radius: 8px;">
  <p>Mais conteúdos exclusivos e como esse, você encontra na <a href="https://comunidade.gabriellsales.com.br" target="_blank">comunidade.</a></p>
</div>

<div align="center"> <h1>O Guia Definitivo de Analytics Self-Hosted</h1> <p>Pronto para revolucionar como você analisa dados?</p> <h2>🚀 Vamos Começar! → Role para Baixo</h2> </div>

<div align="center">
  <img src="https://media.giphy.com/media/LpiVeIRgrqVsZJpM5H/giphy.gif" width="200">
  <p><em>"Não rastreamos você... ensinamos você a rastrear o que importa!"</em></p>
</div>

---

## ⚡ **O Que Você Vai Dominar Hoje?**

<div class="features-grid">

| 🛠️ **Ferramentas**          | 🚀 **Benefícios**                  |
|-----------------------------|-----------------------------------|
| ✔️ Instalação Debian 12     | ✔️ 3x Mais Rápido que o Padrão    |
| ✔️ Configuração Apache      | ✔️ 100% Livre de Erros Comuns     |
| ✔️ Otimização MySQL         | ✔️ Relatórios em Tempo Real       |
| ✔️ SSL Gratuito             | ✔️ Proteção Contra Ataques        |
</div>

---

## 📊 Instalação do Matomo no Debian: Guia Passo a Passo

> **Para quem é este guia?**  
> Iniciantes, administradores de sistemas ou curiosos que querem instalar o Matomo (alternativa ao Google Analytics) em um servidor Debian **do zero**, mesmo sem experiência prévia!

---

## 📋 Pré-requisitos

1. **Servidor Debian 12** (ou versão recente).
2. Acesso como **root** ou usuário com `sudo`.
3. Conexão à internet.

---

## 🛠️ Passo 1: Configuração Inicial do Servidor

```bash
# 1.1 Atualize o Sistema
apt update
```

```bash
# 1.2 Instale o Sudo
apt install sudo
```

## 🌐 Passo 2: Instale o Apache, PHP e MySQL


```bash
# 2.1 Instale os Pacotes
sudo apt install -y apache2 mariadb-server php php-cli php-curl php-gd php-mysql php-xml php-mbstring php-intl php-zip unzip wget
```

```bash
# 2.2 Configure o MySQL/MariaDB
sudo mysql_secure_installation
```

> *Siga as perguntas (defina senha para o root do MySQL, remova usuários anônimos, etc.).*

## 🗃️ Passo 3: Crie o Banco de Dados do Matomo

```bash
# 3.1 Acesse o MySQL
sudo mysql -u root -p  # Use a senha definida no passo anterior
```

```sql
# 3.2 Execute no Prompt do MySQL
CREATE DATABASE matomo_db;
CREATE USER 'matomo_user'@'localhost' IDENTIFIED BY 'ColoqueUmaSenhaForteAqui';
GRANT ALL PRIVILEGES ON matomo_db.* TO 'matomo_user'@'localhost';
FLUSH PRIVILEGES;
EXIT;
```

## 📥 Passo 4: Instale o Matomo

```bash
# 4.1 Baixe e Extraia o Matomo
cd /var/www/html
sudo wget https://builds.matomo.org/matomo.zip
sudo unzip matomo.zip
sudo rm matomo.zip
sudo mv matomo analytics
```

```bash
# 4.2 Configure Permissões
sudo chown -R www-data:www-data /var/www/html/analytics
sudo chmod -R 755 /var/www/html/analytics
```


## 🔒 Passo 5: SSL com Let's Encrypt

```bash
# 5.1 Instale o Certbot
sudo apt install -y certbot python3-certbot-apache
```

```bash
# 5.2 Obtenha o Certificado
sudo certbot --apache
```

## 🔧 Passo 6: Configure o Apache

```bash
# 6.1 Crie um Arquivo de Configuração
sudo nano /etc/apache2/sites-available/matomo.conf
```

```apache
# 6.2 Cole o Conteúdo (Substitua seusite.com):

<VirtualHost *:80>
ServerName seusite.com
DocumentRoot /var/www/html/analytics
<Directory /var/www/html/analytics>
Options -Indexes +FollowSymLinks
AllowOverride All
DirectoryIndex index.php
Require all granted
</Directory>
ErrorLog ${APACHE_LOG_DIR}/matomo_error.log
CustomLog ${APACHE_LOG_DIR}/matomo_access.log combined
</VirtualHost>
```

```bash
# 6.3 Ative o Site e Reinicie o Apache
sudo a2dissite 000-default.conf
sudo a2ensite matomo.conf
sudo a2enmod rewrite
sudo systemctl restart apache2
```

## 🚀 Passo 7: Instalação Web do Matomo

1. Acesse no navegador: `https://seu-ip-ou-dominio/`.
    
2. Siga o assistente de instalação:
    
    - **Banco de dados**: Use `matomo_db`, `matomo_user` e a senha definida.
        
3. Após instalar, remova a pasta `tmp`:

```bash
sudo rm -rf /var/www/html/analytics/tmp/*
```

## ⚙️ Passo 8: Otimizações e Configurações

```bash
# 8.1 Configure o Cron para Relatórios Automáticos
sudo crontab -u www-data -e
```


```cron
# 8.2 Adicione o cron

*/30 * * * * /usr/bin/php /var/www/html/analytics/console core:archive --url=http://seusite.com > /dev/null 2>&1
```

### 8.3 Desative o Acionamento por Navegador

- No Matomo: **Administração → Sistema → Configurações Gerais** → Desmarque _"Ativar arquivamento via navegador"_.
### 8.4 Force SSL (HTTPS)

```bash
# 8.4 Force SSL (HTTPS)
sudo nano /var/www/html/analytics/config/config.ini.php
```

Adicione:

```ini
[General]
force_ssl = 1
```

## 🎉 Parabéns! Você Concluiu a Instalação do Matomo Agora!

<div align="center"> <img src="https://media.giphy.com/media/3o7abKhOpu0NwenH3O/giphy.gif" width="300"> </div>

> Este material é **100% gratuito para uso pessoal**!
> - Permite modificações desde que mantenha os créditos
> - Proibida comercialização sem autorização
> - Dúvidas? Contato@gabriellsales.com.br

<div align="center" style="margin-top:50px">
  <p>Feito com ❤️ por <a href="https://gabriellsales.com.br" target="_blank">Gabriell Sales</a></p>
</div>