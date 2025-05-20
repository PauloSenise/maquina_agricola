# FarmTech Solutions - CRUD System

Sistema CRUD em Python para gerenciamento de dados agrícolas do projeto FarmTech Solutions, integrando dados de sensores ESP32 (simulado via Wokwi) com banco de dados Oracle.

## Visão Geral

Este projeto implementa um sistema CRUD (Create, Read, Update, Delete) para o FarmTech Solutions, uma solução para monitoramento de culturas agrícolas. O sistema integra:

- Interface de terminal simples para operações CRUD
- Conexão com banco de dados Oracle
- Servidor HTTP para receber dados de sensores ESP32 (simulado no Wokwi)
- Armazenamento e visualização dos dados de sensores em tempo real

## Pré-requisitos

Para executar este projeto, você precisará:

- Python 3.6+
- Oracle Database (11g ou superior)
- Oracle Instant Client
- Extensão Wokwi para VSCode (para simulação do ESP32)
- Pacotes Python:
  - cx_Oracle
  - datetime
  - urllib.parse
  - http.server
  - threading

## Instalação e Configuração

### 1. Configuração do Banco de Dados Oracle

1. Instale o Oracle Database em seu sistema ou use uma instância existente
2. Execute o script `SCRIPT_DDL_Projeto_FarmTech_Solutions.SQL` para criar as tabelas necessárias:

```bash
sqlplus username/password@//host:port/service_name @SCRIPT_DDL_Projeto_FarmTech_Solutions.SQL
```

### 2. Configuração do Oracle Client

1. Baixe e instale o Oracle Instant Client correspondente à sua versão do Oracle Database
   - [Download Oracle Instant Client](https://www.oracle.com/database/technologies/instant-client/downloads.html)

2. Configure as variáveis de ambiente:
   - Windows:
     ```
     SET ORACLE_HOME=C:\path\to\instantclient
     SET PATH=%PATH%;%ORACLE_HOME%
     ```
   - Linux/Mac:
     ```
     export ORACLE_HOME=/path/to/instantclient
     export LD_LIBRARY_PATH=$LD_LIBRARY_PATH:$ORACLE_HOME
     ```

### 3. Instalação dos pacotes Python

```bash
pip install -r requirements.txt
```

### 4. Configuração do código

1. Clone este repositório ou copie o código para um arquivo Python
2. Modifique as variáveis de conexão do banco no início do script:

```python
# Parâmetros de conexão do banco de dados - substitua pelos seus dados reais
DB_USER = "seu_usuario"
DB_PASSWORD = "sua_senha"
DB_DSN = "localhost:1521/orcl"  # Modifique de acordo com sua configuração Oracle
```

### 5. Configuração do ESP32 no Wokwi

1. Instale a extensão Wokwi no VSCode
2. Configure o projeto ESP32 com o código `programa_esp32.ino`
3. Certifique-se de que o endereço IP no código ESP32 corresponde ao seu IP local:

## Estrutura do Código

O código está organizado nas seguintes seções:

1. **Configuração de Conexão** - Conexão com banco Oracle
2. **Servidor HTTP** - Recebe dados do ESP32 simulado
3. **Operações CRUD de Cultura** - Gerenciamento de culturas agrícolas
4. **Operações CRUD de Sensor** - Gerenciamento de sensores
5. **Visualização de Monitoramento** - Acesso aos dados coletados
6. **Menu Principal** - Interface de terminal com o usuário

### Principais Componentes:

- `connect_to_db()`: Estabelece conexão com o Oracle
- `ESP32Handler`: Classe que trata as requisições HTTP do ESP32
- `save_sensor_data()`: Salva os dados dos sensores no banco
- `create_cultura()`, `read_cultura()`, etc.: Operações CRUD para culturas
- `create_sensor()`, `read_sensor()`, etc.: Operações CRUD para sensores
- `show_menu()`: Exibe o menu principal
- `main()`: Função principal que coordena o programa

## Como Usar

### Executando o Programa

1. Inicie o código Python no terminal:

```bash
python farmtech_crud.py
```

2. O sistema iniciará automaticamente:
   - Conectará ao banco de dados Oracle
   - Iniciará o servidor HTTP na porta 8000
   - Exibirá o menu principal

3. Use o menu numérico para navegar:
   - Opções 1-4: Gerenciar culturas
   - Opções 5-8: Gerenciar sensores
   - Opção 9: Visualizar monitoramentos recentes
   - Opção 0: Sair do programa

### Simulando o ESP32 no Wokwi

1. Abra o VSCode com a extensão Wokwi instalada
2. Carregue o arquivo `programa_esp32.ino`
3. Inicie a simulação Wokwi
4. O ESP32 simulado enviará dados para o servidor Python
5. O servidor Python receberá, processará e salvará os dados no banco Oracle

### Exemplos de Uso

#### Cadastrar uma Nova Cultura

```
========== FarmTech Solutions CRUD ==========
1. Cadastrar Cultura
2. Listar Culturas
3. Atualizar Cultura
4. Excluir Cultura
5. Cadastrar Sensor
6. Listar Sensores
7. Atualizar Sensor
8. Excluir Sensor
9. Visualizar Últimos Monitoramentos
0. Sair
===========================================
Escolha uma opção: 1

--- Cadastrar Nova Cultura ---
Descrição da cultura: Milho
Ativo (S/N): S
Área plantada (m²): 1000
Cultura cadastrada com sucesso! ID: 1
```

#### Cadastrar um Novo Sensor

```
Escolha uma opção: 5

--- Cadastrar Novo Sensor ---

Culturas disponíveis:
ID: 1, Descrição: Milho

ID da cultura para associar ao sensor: 1
Número de série do sensor: DHT22-001
Ativo (S/N): S
Sensor cadastrado com sucesso! ID: 1
```

#### Visualizar Dados de Monitoramento

```
Escolha uma opção: 9

--- Últimos Registros de Monitoramento ---
ID    Sensor    Tipo            Valor   Unidade  Data/Hora            Cultura            
------------------------------------------------------------------------------------------
1     1         Temperatura     25.50   °C       18/05/2025 14:30:45  Milho              
2     2         Umidade         65.80   %        18/05/2025 14:30:45  Milho              
3     3         pH              6.50    pH       18/05/2025 14:30:45  Milho              
```

## Fluxo de Dados

O sistema opera com o seguinte fluxo de dados:

1. **ESP32 (Wokwi)**: Coleta dados dos sensores (temperatura, umidade, pH, etc.)
2. **Servidor HTTP**: Recebe os dados via HTTP GET requests
3. **Processamento Python**: Extrai e formata os dados recebidos
4. **Banco de Dados Oracle**: Armazena os dados nas tabelas apropriadas
5. **Interface CRUD**: Permite visualizar e gerenciar os dados armazenados

## Explicação da Integração ESP32-Oracle

### 1. Coleta de Dados (ESP32)

O ESP32 simulado no Wokwi lê dados dos sensores:
- DHT22 para temperatura e umidade
- LDR para simulação de pH
- Botões para simular presença de fósforo e potássio
- Relé para controle de bomba de água

### 2. Envio de Dados (HTTP)

O ESP32 envia dados para o servidor Python via HTTP GET:
```
http://192.168.0.12:8000/data?umidade=65.80&temperatura=25.50&ph=6.50&fosforo=presente&potassio=ausente&rele=on
```

### 3. Recepção e Processamento (Python)

O servidor HTTP Python:
- Recebe a requisição GET
- Extrai os parâmetros (umidade, temperatura, etc.)
- Processa os dados
- Salva no banco Oracle

### 4. Armazenamento no Banco (Oracle)

Os dados são salvos nas tabelas Oracle:
- `monitoramento`: Registros dos valores dos sensores
- `sensor`: Informações sobre os sensores
- `cultura`: Informações sobre as culturas monitoradas

## Resolução de Problemas

### Erro ao conectar ao banco de dados

- Verifique se o Oracle está em execução
- Confirme as credenciais (usuário/senha)
- Verifique o DSN e se o Oracle Client está corretamente configurado

### O ESP32 não consegue enviar dados

- Verifique se o IP no código ESP32 está correto
- Confirme se o servidor Python está em execução
- Verifique se não há firewalls bloqueando a porta 8000

### Erros ao salvar dados no banco

- Verifique se as tabelas foram criadas corretamente
- Confirme se os tipos de dados estão corretos
- Verifique as restrições de integridade referencial

## Personalizações e Melhorias

- **Interface Web**: Substitua a interface de terminal por uma aplicação web
- **Segurança**: Adicione autenticação ao servidor HTTP
- **Análise de Dados**: Integre algoritmos de análise para previsão de irrigação
- **Alertas**: Configure notificações para condições críticas

## Referências

- [Documentação cx_Oracle](https://cx-oracle.readthedocs.io/)
- [Oracle Database Documentation](https://docs.oracle.com/en/database/)
- [Documentação ESP32](https://docs.espressif.com/projects/esp-idf/en/latest/esp32/)
- [Wokwi Documentation](https://docs.wokwi.com/)

---

## Licença

Este projeto está licenciado sob a licença MIT - veja o arquivo LICENSE para detalhes.

## Contribuições

Contribuições são bem-vindas! Sinta-se à vontade para abrir issues ou enviar pull requests.
