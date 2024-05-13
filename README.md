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
1. **Comandos básicos**<br>
    - Verificar status do serviço:<br>
        `sudo systemctl status meu_servico`
    - Parando o serviço:<br>
        `sudo systemctl stop meu_servico`
    - Iniciando o serviço:<br>
        `sudo systemctl start meu_servico`
    - Habilitando o serviço para início automático:<br>
        `sudo systemctl enable meu_servico`
    - Desabilitando o início automático do serviço:<br>
        `sudo systemctl disable meu_servico`

2. **Criando Serviço Personalizado**<br>
    - Criando um script personalizado:<br>
        Primeiro, crie o script que você deseja executar como parte do serviço. Por exemplo, vamos criar um script simples chamado `meu_script.sh` que imprime uma mensagem no terminal:
        ```bash
        #!/bin/bash
        echo "Hello from meu_script.sh"
        ```
        Salve este script em um local acessível, por exemplo, em `/opt/meu_script.sh`, e torne-o executável:<br>
        ```chmod +x /opt/meu_script.sh```
    - Criando o Arquivo de Serviço do systemd:<br>
    Agora, crie um arquivo de serviço do systemd para o seu script. Vamos chamá-lo de `meu_servico.service`. Use um editor de texto para criar este arquivo (por exemplo, `sudo vi /etc/systemd/system/meu_servico.service`) e adicione o seguinte conteúdo:<br>
        ```bash
        [Unit]
        Description=Meu Serviço Executando meu_script.sh
        After=network.target

        [Service]
        Type=simple
        ExecStart=/opt/meu_script.sh
        Restart=always

        [Install]
        WantedBy=multi-user.target
        ```
        - **`Description`**: Descrição do serviço.
        - **`After`**: Especifica que o serviço deve ser iniciado após a conclusão da inicialização da rede.
        - **`Type`**: Define o tipo de serviço. Neste caso, `simple` é usado para scripts que não requerem uma estrutura de daemon.
        - **`ExecStart`**: Caminho para o script a ser executado.
        - **`Restart`**: Define o comportamento de reinicialização do serviço em caso de falha.
        - **`WantedBy`**: Especifica o alvo multiusuário como alvo para habilitar o serviço.
    - Habilitar e Iniciar o Serviço:<br>
        Depois de criar o arquivo de serviço, recarregue as unidades do systemd para reconhecer o novo serviço:<br>
            ```sudo systemctl daemon-reload```<br>
        Agora, você pode habilitar o serviço para que ele inicie automaticamente durante o inicializar do sistema e iniciar o serviço:<br>
            ```sudo systemctl enable meu_servico```<br>
            ```sudo systemctl start meu_servico```<br>
    - Verificar o Status do Serviço:<br>
        Você pode verificar o status do serviço para garantir que está em execução sem erros:<br>
        ```sudo systemctl status meu_servico```<br>
        Se tudo estiver configurado corretamente, você verá a saída indicando que o serviço está ativo e em execução.<br>
    - Testar o Serviço:<br>
        Para testar o serviço, verifique se o script está sendo executado conforme esperado:<br>
        ```journalctl -u meu_servico```

3. **Deletando um Serviço**<br>
Para apagar um serviço que você criou usando o systemd, você pode seguir os seguintes passos para desabilitar e remover o serviço:<br>
    - Parar o Serviço<br>
    Antes de remover o serviço, pare-o para garantir que ele não esteja em execução:<br>
    ```sudo systemctl stop meu_servico```<br>
    - Desabilitar o Serviço<br>
    Desabilite o serviço para que ele não seja iniciado automaticamente durante o próximo inicializar do sistema:<br>
    ```sudo systemctl disable meu_servico```<br>
    - Remover o Arquivo de Serviço<br>
    Remova o arquivo de serviço do systemd que define o seu serviço:<br>
    ```sudo rm /etc/systemd/system/meu_servico.service```
    - Recarregar as Unidades do systemd<br>
    Após remover o arquivo de serviço, recarregue as unidades do systemd para atualizar as configurações:<br>
    ```sudo systemctl daemon-reload```<br>
    - Limpar Registros do Serviço (Opcional)<br>
    Se desejar, você pode limpar os registros associados ao serviço do sistema de log do systemd:<br>
    ```sudo journalctl --vacuum-time=1d```<br>
    Isso irá limpar os registros do systemd mais antigos que tenham mais de um dia.<br>

### Consultando recursos do sistema operacional

1. **Informações Gerais do Sistema**
    - **`uname -a`**: Exibe informações do kernel e do sistema operacional.
    - **`hostname`**: Mostra o nome do host do sistema.
    - **`uptime`**: Mostra há quanto tempo o sistema está funcionando e a carga média.

2. **Informações de CPU**
    - **`lscpu`**: Lista informações detalhadas sobre a CPU.
    - **`cat /proc/cpuinfo`**: Exibe informações sobre os núcleos da CPU.
    - **`top` ou `htop`**: Monitora a utilização da CPU em tempo real.

3. **Informações de Memória**
    - **`free -h`**: Mostra o uso atual de memória, incluindo RAM e swap.
    - **`vmstat`**: Exibe estatísticas de memória virtual.
    - **`htop`**: Mostra informações de uso da memória de forma interativa.

4. **Informações de Disco**
    - **`df -h`**: Exibe o uso de espaço em disco de todas as partições.
    - **`du -h`**: Mostra o uso de espaço em disco de diretórios específicos.
    - **`lsblk`**: Lista informações sobre dispositivos de bloco (discos e partições).

5. **Informações de Rede**
    - **`ifconfig` ou `ip a`**: Mostra informações sobre interfaces de rede.
    - **`netstat -tunlp`**: Lista conexões de rede e portas abertas.
    - **`ping`**: Testa a conectividade de rede com um host.

6. **Processos e Utilização da CPU**
    - **`ps aux`**: Lista todos os processos em execução.
    - **`top` ou `htop`**: Mostra os processos em execução e sua utilização de CPU.
    - **`pidstat`**: Exibe estatísticas detalhadas sobre a utilização da CPU por processo.

7. **Logs do Sistema**
    - **`journalctl`**: Mostra mensagens de log do sistema (especialmente útil em sistemas com `systemd`).
    - **`dmesg`**: Exibe mensagens do kernel.

8. **Informações de Hardware**
    - **`lshw`**: Lista informações detalhadas sobre o hardware.
    - **`lsusb`**: Mostra dispositivos USB conectados.
    - **`lspci`**: Exibe dispositivos PCI conectados.

9. **Monitoramento de Desempenho em Tempo Real**
    - **`top`**: Monitora recursos do sistema em tempo real.
    - **`htop`**: Uma alternativa interativa ao `top` com mais recursos.
    - **`iotop`**: Mostra o uso de E/S por processos.

10. **Verificação de Temperatura**
    - **`sensors`**: Exibe informações sobre sensores de temperatura do hardware.

### IAM (Permissões/Usuários/Grupos)
### Variáveis de Ambiente
### Particionamento de Disco
### Variáveis de Ambiente
### Acesso SSH
### Lista de Comandos Essenciais para DevOps


