# Relatório de Auditoria de Senhas: Brute Force & Password Spraying

## 📋 Descrição do Desafio
Este repositório documenta a execução de testes de intrusão focados em autenticação. O projeto explora técnicas de Brute Force (Dicionário) e Password Spraying contra serviços vulneráveis (FTP, SMB) e formulários web (DVWA), utilizando ferramentas nativas do Kali Linux como Medusa e Hydra em ambiente controlado (Metasploitable 2).

## 🛠️ Ferramentas & Ambiente
* **Kali Linux:** Sistema Operacional Ofensivo (VirtualBox).
* **Metasploitable 2:** Alvo (Target) rodando em Rede Interna (VirtualBox).
* **Nmap:** Reconhecimento e enumeração de serviços.
* **Medusa:** Ferramenta modular para brute force em serviços.
* **Hydra:** Ferramenta especializada em ataques a formulários web.

## 🚀 Execução dos Testes

### 1. Ataque ao Serviço FTP (Porta 21)
* **Técnica:** Brute Force (Dicionário).
* **Ferramenta:** Medusa.
* **Contexto:** Uso da wordlist `rockyou.txt` e lista personalizada para validar credenciais padrão.
* **Comando:**
    ```bash
    medusa -h <IP_ALVO> -u msfadmin -P alvo.txt -M ftp
    ```
* **Resultado:** Credencial `msfadmin` encontrada com sucesso (SUCCESS).

### 2. Ataque ao Serviço SMB (Porta 445)
* **Técnica:** Password Spraying.
* **Ferramenta:** Medusa.
* **Contexto:** Enumeração prévia de usuários via Nmap (`smb-enum-users`). Teste de uma única senha comum contra múltiplos usuários para evitar bloqueio de conta.
* **Comando:**
    ```bash
    medusa -h <IP_ALVO> -U usuarios.txt -p msfadmin -M smbnt
    ```
* **Resultado:** Acesso validado apenas para o usuário `msfadmin`.

### 3. Ataque à Aplicação Web (DVWA)
* **Técnica:** Brute Force em Formulário HTTP POST.
* **Ferramenta:** Hydra.
* **Nota Técnica:** Optou-se pelo uso do Hydra nesta etapa devido à sua maior robustez e precisão na tratativa de formulários web e redirecionamentos em comparação ao módulo web-form do Medusa.
* **Comando:**
    ```bash
    hydra -l admin -P alvo.txt <IP_ALVO> http-post-form "/dvwa/login.php:username=^USER^&password=^PASS^&Login=Login:Login failed"
    ```
* **Resultado:** Senha `password` encontrada para o usuário `admin`.

## 🛡️ Recomendações de Mitigação
1.  **Política de Senhas:** Impor complexidade mínima e rotação periódica.
2.  **Bloqueio de Conta (Account Lockout):** Bloquear o usuário após 3-5 tentativas falhas (mitiga Brute Force, mas exige cuidado contra DoS).
3.  **Monitoramento:** Alertas para múltiplos logins falhos em curto período (detecção de Spraying).
4.  **MFA:** Implementar Autenticação Multifator sempre que possível.

---
*Desenvolvido por Urkyn para o Desafio de Cibersegurança da DIO.*
