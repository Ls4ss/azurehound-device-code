# AzureHound Device Code Automation Script

Este script em Bash automatiza o processo de coleta de dados do Entra ID (Azure AD) e AzureRM utilizando o **AzureHound Community Edition** através do fluxo de autenticação por **Device Code**. 

Ele foi projetado especificamente para cenários onde o usuário possui restrições de **MFA (Multi-Factor Authentication)** ou **Políticas de Acesso Condicional (CAP)**, eliminando a necessidade de interações manuais complexas via PowerShell para obter e gerenciar tokens de atualização (*Refresh Tokens*).

## 🚀 Como Funciona

1. **Solicitação Automatizada:** O script faz uma requisição HTTP para os endpoints da Microsoft solicitando um código de dispositivo utilizando o Client ID do Azure PowerShell (`1950a258-227b-4e31-a9cf-717495945fc2`).
2. **Polling de Autenticação:** Ele entra em um loop aguardando que você insira o código gerado no navegador e conclua a autenticação.
3. **Coleta Automática:** Assim que o login é confirmado, o script captura o *Refresh Token* dinamicamente e dispara o `azurehound` de forma imediata, gerando o arquivo JSON final pronto para importação no BloodHound.

## 📋 Pré-requisitos

Certifique-se de ter as seguintes ferramentas instaladas no seu ambiente Linux/macOS:

### Debian / Ubuntu / Parrot OS / Kali
sudo apt install curl jq -y

### RHEL / CentOS / Fedora
sudo dnf install curl jq -y
O binário do azurehound também deve estar presente no diretório (ou o caminho configurado corretamente nas variáveis do script).

⚙️ Configuração
Antes de rodar, abra o script e configure as variáveis no topo do arquivo:

Bash
TENANT="SEU_TENANT_ID_AQUI"         # Use preferencialmente o GUID do Tenant alvo
AZUREHOUND_PATH="./azurehound"      # Caminho para o executável do AzureHound
OUTPUT_FILE="output.json"           # Nome do arquivo de saída para o BloodHound
Dica: Utilizar o ID do Tenant em formato GUID (ex: 0fe1c33c-50ee-467f-9405-8396b8b74e3d) em vez do domínio evita erros de escopo de usuário caso a conta seja um usuário convidado (Guest User) no ambiente.

📖 Modo de Uso
Dê permissão de execução ao script:

Bash
chmod +x azhound-dc
Execute o script:

Bash
./azhound-dc
O terminal exibirá uma mensagem destacada. Abra o navegador indicado, digite o código gerado e realize o processo de autenticação com a conta correspondente.

Volte ao terminal. O script detectará a conclusão do login e gerará o arquivo output.json. Basta fazer o upload desse arquivo diretamente na interface do BloodHound CE.

🔗 Referências
Este script foi desenvolvido com base na documentação oficial do SpecterOps para contorno de MFA e Acesso Condicional no AzureHound:

BloodHound Documentation - [Dealing with Multi-Factor Auth and Conditional Access Policies](https://bloodhound.specterops.io/collect-data/ce-collection/azurehound#dealing-with-multi-factor-auth-and-conditional-access-policies)
