
# Roteiro Detalhado: Configurando um Servidor Web Nginx e Hospedando seu Site 🚀

## Objetivo
Ao final desta atividade, você terá configurado um **servidor web Nginx** em uma instância **Amazon Linux 2023** e hospedado um site simples desenvolvido por você.  
O site ficará **publicamente acessível** através do endereço IP da sua máquina virtual (VM).

## Pré‑requisito
- Você já deve estar **conectado via SSH** à sua instância **EC2** rodando Amazon Linux 2023.  
- Todos os comandos a seguir serão executados **no terminal dessa instância**.

---

## Seção 1 — Preparando o Ambiente e Instalando o Nginx

Nesta seção você vai **atualizar** o sistema operacional da VM e **instalar** o Nginx, um servidor web conhecido por sua performance e eficiência.

### Passo 1 — Atualizar o Sistema ⚙️
Manter o sistema atualizado garante as últimas correções de segurança e versões recentes dos pacotes.

```bash
sudo dnf update -y
```

- `sudo` — executa o comando com privilégios de administrador.  
- `dnf` — gerenciador de pacotes do Amazon Linux 2023.  
- `update` — atualiza todos os pacotes instalados.  
- `-y` — confirma automaticamente qualquer pergunta do processo.

**Checkpoint ✅**  
Você deverá ver `Complete!` ou `Nothing to do`.

### Passo 2 — Instalar o Servidor Web Nginx 🌐
```bash
sudo dnf install -y nginx
```

Após a instalação, verifique a versão (opcional):

```bash
nginx -v
```

**Checkpoint ✅**  
Saída semelhante a `nginx/1.24.0`.

### Passo 3 — Iniciar e Habilitar o Nginx ▶️
Inicie o serviço e configure‑o para inicializar em todo boot:

```bash
sudo systemctl start nginx
sudo systemctl enable nginx
sudo systemctl status nginx   # ver status
```

**Checkpoint ✅**  
A linha `Active: active (running)` confirma que o Nginx está em execução.  
Pressione `q` para sair da visualização de status.

### Passo 4 — Verificar a Página Padrão do Nginx 🖥️
1. **Obtenha o IP público** da sua instância EC2 (Console AWS ou via `curl`):

   ```bash
   curl -s http://checkip.amazonaws.com
   ```

2. No seu navegador, acesse `http://<SEU_IP_PUBLICO>`.

**Checkpoint ✅**  
Se você vê *Welcome to nginx!* está tudo certo!

> **Não funcionou?**  
> - Confirme que o Nginx está ativo.  
> - Verifique se o *Security Group* permite tráfego na porta **80/TCP** (HTTP).

---

## Seção 2 — Preparando seu Site para Hospedagem

### Passo 5 — Instalar o Git SCM
Para clonar o repositório do seu site:

```bash
sudo dnf install -y git
git --version          # opcional
```

### Passo 6 — Ajustar Permissões do Diretório Web 📂
O Nginx serve arquivos de **/usr/share/nginx/html**.

1. Descubra seu usuário:
   ```bash
   whoami
   ```
2. Altere a propriedade do diretório (substitua `SEU_USUARIO`):
   ```bash
   sudo chown -R SEU_USUARIO:SEU_USUARIO /usr/share/nginx/html
   ```
3. Verifique:
   ```bash
   ls -ld /usr/share/nginx/html
   ```

---

## Seção 3 — Hospedando seu Site Pessoal

### Passo 7 — Clonar seu Site via Git 📥
```bash
cd ~                   # diretório home
git clone <URL_REPO>  <pasta_site>
cd <pasta_site>
ls
```

### Passo 8 — Publicar Arquivos no Diretório do Nginx 🚀
1. **(Opcional)** Limpe o conteúdo padrão:
   ```bash
   rm -rf /usr/share/nginx/html/*
   ```
2. Copie os arquivos do seu site:
   ```bash
   cp -R * /usr/share/nginx/html/
   # ou, se estiver fora da pasta:
   cp -R <pasta_site>/* /usr/share/nginx/html/
   ```
3. Verifique:
   ```bash
   ls /usr/share/nginx/html/
   ```

### Passo 9 — Testar o Site no Navegador 🎉
Atualize `http://<SEU_IP_PUBLICO>` — agora você deve ver a **sua** página *index.html*.

---

## Solução de Problemas Comuns 🔍

| Problema | Diagnóstico & Solução |
|----------|-----------------------|
| **Página não carrega** | - `systemctl status nginx` deve mostrar **running**.<br>- *Security Group* precisa liberar porta **80** para `0.0.0.0/0` ou seu IP.<br>- Confirme o **IP público** utilizado. |
| **Permissão negada ao copiar arquivos** | Refaça o **Passo 6** e certifique‑se de usar o usuário correto no `chown`. |
| **Site sem CSS/JS/Imagens** | - Verifique caminhos relativos no *HTML*.<br>- Confirme que pastas como `css/`, `js/`, `images/` foram copiadas. |

---

> Este roteiro foi elaborado para apoiar seu aprendizado em **redes de computadores e internet**  
> Explore, teste e resolva problemas — é assim que se aprende! 👩‍💻👨‍💻

---

