# **Estudos Linux**
Essa documentação tem o objetivo de ser uma referência para a administração do sistema operacional linux, fazendo um compilado de vários tópicos importantes para executarmos nossas tarefas no dia a dia.

## **Tópicos**
- Estruturas dos Diretórios
- Controle de logs
- Manipulando arquivos e Diretórios
- Consultando arquivos e Diretórios (Avançado)
- Instalação e remoção de pacotes
- Trabalhando com serviços
- Consultando recursos do sistema operacional
- IAM (Permissões/Usuários/Grupos)
- Variáveis de Ambiente
- Particionamento de Disco
- Variáveis de Ambiente
- Acesso SSH
- Lista de Comandos Essenciais para DevOps

### **Estruturas dos Diretórios**
![Mapa Mental Estrutura de Diretórios](./img/markmap-estrutura-dir.png)

### **Controle de logs**
O controle de logs no Linux é fundamental para monitorar e diagnosticar o sistema operacional e os aplicativos em execução. Nesta tópico serão abordados os conceitos fundamentais do controle de logs no Linux, incluindo os tipos de logs, onde eles são armazenados, como visualizá-los e gerenciá-los.

#### **Conceitos Básicos**

1. **Tipos de Logs**<br>
Existem vários tipos de logs no Linux, incluindo:
    - **Syslog**: Logs do sistema que abrangem eventos do kernel, serviços e aplicativos.
    - **Logs de aplicativos**: Logs gerados por aplicativos específicos, como servidores web, bancos de dados, e-mails, etc.
    - **Logs de segurança**: Registros de atividades de segurança, como tentativas de login, alterações de permissões, etc.
    - **Logs de autenticação**: Informações sobre autenticação de usuários.
    - **Logs do sistema**: Mensagens do kernel, eventos de hardware, etc.

2. **Localização dos Arquivos de Log**<br>
![Estrutura de logs](./img/markmap-logs.png)<br>
Os arquivos de log geralmente estão localizados no diretório `/var/log`. Os principais arquivos de log incluem:
    - `/var/log/syslog`
        - Log do sistema que contém mensagens do kernel e serviços.
    - `/var/log/auth.log`
        - Registros de autenticação e informações de login.
    - `/var/log/messages`
        - Mensagens gerais do sistema.
    - Outros (Exemplo)
        - Outros arquivos específicos de aplicativos ou serviços
            - `/var/log/apache2/access.log`
                - Para logs do servidor Apache.

#### **Comandos e Ferramentas**

1. **Visualização de Logs**
    - `cat` ou `less`: Exibir o conteúdo completo de um arquivo de log.<br>
        `cat /var/log/syslog`
    - `tail`: Exibir as últimas linhas de um arquivo de log (útil para logs em tempo real).<br>
        `tail -f /var/log/syslog`

2. **Filtragem e Análise de Logs**
    - `grep`: Filtrar linhas que correspondam a um padrão específico.<br>
        `grep "Failed password" /var/log/auth.log`
    - `awk`: Manipular e analisar dados em logs.<br>
        `awk '{print $1, $5, $6}' /var/log/apache2/access.log`

#### **Ferramentas Avançadas**

1. **journalctl**<br>
O `journalctl` é uma ferramenta para acessar os logs do sistema gerenciados pelo `systemd`.
    - Exibir logs do sistema:<br>
        `journalctl`
    - Filtrar logs por unidade de serviço:<br>
        `journalctl -u nginx.service`
2. **Configuração de Rotação de Logs**<br>
O Linux utiliza o `logrotate` para gerenciar e rotacionar os arquivos de log para evitar que eles cresçam indefinidamente.`
    - Arquivo de configuração: `/etc/logrotate.conf` e arquivos de configuração específicos em `/etc/logrotate.d/`.

#### **Gerenciamento de Logs em Ambientes de Produção**
- **Centralização de Logs**:<br>
    Usar ferramentas como Elasticsearch, Logstash e Kibana (ELK Stack) para coletar, armazenar e analisar logs de vários servidores.
- **Monitoramento de Logs em Tempo Real**:<br> 
    Configurar alertas para eventos importantes usando ferramentas como `syslog-ng`, `rsyslog`, ou soluções de monitoramento como Prometheus com Grafana.

### Manipulando arquivos e Diretórios
1. **Manipulando pastas**
    - `mkdir`<br>
        - Comando de criação básico:<br>
            `mkdir folder1`<br>
        - Criando as pastas recursivamente:<br>
            `mkdir -p folder1\subfolder1`<br>
        - Criando as pastas recursivamente e aplicando permissões<br>
            `mkdir -m 775 -p folder1\subfolder1`<br>
    - `rm`<br>
        - No cenário de folders o rm deve ser utilizado para folders que não estão vazios.<br>
        - Comando para excluir uma pasta:<br>
            `rm folder1`
        - Excluindo uma pasta de forma recursiva:<br>
            `rm -rf folder1`
    - `rmdir`<br>
        - Utilizado para deletar folder vazios.<br>
        - Deletando o dir:<br>
            `rmdir meu_diretorio_vazio`

### Consultando arquivos e Diretórios (Avançado)
1. **Pesquisando qualquer file com determinado conteúdo**
    - No diretório / pesquisar qualquer arquivo com "teste*" recursivamente:<br>
        `sudo find / -type f -name "test*"`<br>
    - No diretório / pesquisar qualquer folder com "teste*" recursivamente:<br>
        `sudo find / -type d -name "test*"`<br>
    - Join com as duas funções anteriores:<br>
        `sudo find / -type f,d -name "test*"`<br>

2. **Utilizando o comando grep para pesquisar o conteúdo do arquivo**
    - No diretório / pesquisar qualquer arquivo com o conteúdo "exemplo" recursivamente:<br>
        `sudo find / -type f -exec grep -l "exemplo" {} +`

### Instalação e remoção de pacotes
1. **Atualizando o repositório**<br>
    `sudo apt update`
2. **Pesquisando o pacote no repositório**<br>
    `sudo apt search openjdk-17-jdk`
3. **Instalando o pacote**<br>
    `sudo apt-get install openjdk-17-jdk -y`
4. **Removendo um pacote**
    - **Removação simples:**<br>
        `sudo apt-get remove openjdk-17-jdk`
    - **Removendo o pacote, seus arquivos e depências:**<br>
        `sudo apt-get autoremove openjdk-17-jdk`

### Trabalhando com serviços
### Consultando recursos do sistema operacional
### IAM (Permissões/Usuários/Grupos)
### Variáveis de Ambiente
### Particionamento de Disco
### Variáveis de Ambiente
### Acesso SSH
### Lista de Comandos Essenciais para DevOps


