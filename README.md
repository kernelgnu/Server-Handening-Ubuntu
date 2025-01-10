# Projeto de Hardening de Servidor Ubuntu

## Descrição
Este projeto implementa práticas de hardening em servidores Ubuntu para melhorar a segurança, reduzindo a superfície de ataque e mitigando possíveis vulnerabilidades. Inclui configurações de firewall, permissões de usuários, autenticação e monitoramento.

---

## Funcionalidades
- Configuração de firewall com UFW.
- Bloqueio de acessos desnecessários.
- Configuração de SSH segura (autenticação por chave pública).
- Limitação de privilégios de usuários e serviços.
- Instalação de ferramentas de monitoramento e auditoria.
- Aplicação de atualizações automáticas de segurança.

---

## Tecnologias Usadas
- **Ubuntu Server**: Sistema operacional base.
- **UFW (Uncomplicated Firewall)**: Configuração de firewall.
- **Fail2Ban**: Proteção contra tentativas de login bruteforce.
- **Auditd**: Ferramenta de auditoria.
- **Lynis**: Ferramenta de análise de segurança do sistema.

---

## Como Usar

### Pré-requisitos
Certifique-se de ter:
- Um servidor Ubuntu configurado (18.04 ou superior).
- Acesso root ou privilégios sudo.

### Passos para Configuração

1. **Atualização do Sistema**:
   ```bash
   sudo apt update && sudo apt upgrade -y
   ```

2. **Configuração de Firewall (UFW)**:
   - Permita apenas serviços essenciais:
     ```bash
     sudo ufw allow ssh
     sudo ufw allow http
     sudo ufw allow https
     sudo ufw enable
     ```
   - Verifique o status do firewall:
     ```bash
     sudo ufw status
     ```

3. **Fortalecimento do SSH**:
   - Edite o arquivo de configuração do SSH:
     ```bash
     sudo nano /etc/ssh/sshd_config
     ```
   - Ajuste as seguintes configurações:
     ```plaintext
     PermitRootLogin no
     PasswordAuthentication no
     AllowUsers seu-usuario
     ```
   - Reinicie o serviço SSH:
     ```bash
     sudo systemctl restart sshd
     ```

4. **Instale o Fail2Ban**:
   ```bash
   sudo apt install fail2ban -y
   ```
   - Configure uma regra básica de proteção SSH:
     ```bash
     sudo cp /etc/fail2ban/jail.conf /etc/fail2ban/jail.local
     sudo nano /etc/fail2ban/jail.local
     ```
     - Exemplo de configuração para SSH:
       ```plaintext
       [sshd]
       enabled = true
       port = ssh
       filter = sshd
       logpath = /var/log/auth.log
       maxretry = 3
       ```
   - Inicie o serviço:
     ```bash
     sudo systemctl enable fail2ban
     sudo systemctl start fail2ban
     ```

5. **Habilite Atualizações Automáticas**:
   ```bash
   sudo apt install unattended-upgrades -y
   sudo dpkg-reconfigure --priority=low unattended-upgrades
   ```

6. **Auditoria com Auditd**:
   - Instale o Auditd:
     ```bash
     sudo apt install auditd -y
     ```
   - Ative o serviço:
     ```bash
     sudo systemctl enable auditd
     sudo systemctl start auditd
     ```
   - Visualize relatórios:
     ```bash
     sudo ausearch -m USER_LOGIN
     ```

7. **Análise com Lynis**:
   - Instale o Lynis:
     ```bash
     sudo apt install lynis -y
     ```
   - Execute uma análise de segurança:
     ```bash
     sudo lynis audit system
     ```

---

## Exemplo de Uso
- O firewall bloqueia acessos não autorizados a portas críticas.
- Tentativas de login bruteforce são mitigadas pelo Fail2Ban.
- Auditorias e logs detalhados são gerados pelo Auditd para análises de segurança.

---

## Estrutura do Projeto
```plaintext
hardening-ubuntu/
├── scripts/            # Scripts automatizados de configuração (opcional)
├── configs/            # Arquivos de configuração para serviços
├── logs/               # Logs gerados durante as auditorias
├── docs/               # Documentação adicional
└── README.md           # Este arquivo
```

---

## Aprendizados
- Configuração de firewalls e segurança SSH.
- Uso de ferramentas de monitoramento e auditoria em servidores Linux.
- Práticas recomendadas para hardening de servidores.

---

## Próximos Passos
- Automatizar o processo de hardening com Ansible.
- Adicionar notificações para eventos críticos no servidor.
- Implementar controles de acesso baseados em funções (RBAC).
