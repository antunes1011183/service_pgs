# **Sincronização de Dados CSV com Supabase**

Este projeto implementa um serviço em Python para sincronizar dados de um arquivo CSV com tabelas em um banco de dados Supabase. Ele foi projetado para evitar duplicação de dados e permite a configuração dinâmica de caminho do arquivo, credenciais do banco e intervalo de sincronização.

---

## **Recursos**
- Leitura de arquivos CSV com suporte a diferentes codificações.
- Detecção de duplicatas antes da inserção no banco.
- Sincronização de dados com as tabelas:
  - `cliente`
  - `contrato`
  - `produto`
- Logs detalhados para monitorar a execução e depurar problemas.
- Configuração dinâmica via arquivo `config.json`.

---

## **Instalação**

### **Pré-requisitos**
- **Python 3.8+**
- Instale as dependências do projeto:
  ```bash
  pip install -r requirements.txt
  ```

### **Configuração**
1. **Configurar o arquivo `config.json`**:
   Crie um arquivo `config.json` com o seguinte formato:
   ```json
   {
       "update_interval_minutes": 60,
       "csv_file_path": "path/to/your/file.csv",
       "supabase_url": "https://seu-supabase-url.supabase.co",
       "supabase_anon_key": "sua-chave-anon",
       "supabase_service_role_key": "sua-chave-service-role"
   }
   ```
   - **`update_interval_minutes`**: Intervalo de execução da sincronização, em minutos.
   - **`csv_file_path`**: Caminho absoluto do arquivo CSV.
   - **`supabase_url`**: URL do seu projeto Supabase.
   - **`supabase_anon_key`** e **`supabase_service_role_key`**: Chaves de autenticação do Supabase.

2. **Configurar variáveis de ambiente (opcional)**:
   Em vez de usar `config.json`, você pode definir as chaves diretamente no ambiente:
   ```bash
   export SUPABASE_URL="https://seu-supabase-url.supabase.co"
   export SUPABASE_ANON_KEY="sua-chave-anon"
   export SUPABASE_SERVICE_ROLE_KEY="sua-chave-service-role"
   ```

---

## **Uso**

### **Executar o Serviço**
1. Execute o script principal:
   ```bash
   python service.py
   ```
2. O serviço sincronizará automaticamente os dados com o Supabase no intervalo configurado.

---

## **Estrutura do Projeto**

```plaintext
.
├── service.py            # Script principal
├── config.json           # Configurações do sistema (adicionado pelo usuário)
├── requirements.txt      # Dependências do projeto
├── service.log           # Arquivo de logs gerado automaticamente
└── README.md             # Este arquivo
```

---

## **Tabelas no Supabase**

### **Tabela: `cliente`**
| Campo               | Tipo       | Descrição                   |
|---------------------|------------|-----------------------------|
| `id_siger_cliente`  | `bigint`   | ID do cliente no SIGER      |
| `nome_cliente`      | `text`     | Nome do cliente             |
| `cnpj`              | `text`     | CNPJ do cliente             |
| `deletado`          | `boolean`  | Status de exclusão lógica   |

### **Tabela: `contrato`**
| Campo               | Tipo       | Descrição                   |
|---------------------|------------|-----------------------------|
| `id_contrato`       | `integer`  | ID do contrato no SIGER     |
| `id_siger_cliente`  | `bigint`   | Relacionado ao cliente      |
| `dt_inic_cont`      | `date`     | Data de início do contrato  |
| `dt_vig_inic`       | `date`     | Data de início da vigência  |
| `dt_vig_final`      | `date`     | Data de término da vigência |
| `deletado`          | `boolean`  | Status de exclusão lógica   |

### **Tabela: `produto`**
| Campo               | Tipo       | Descrição                   |
|---------------------|------------|-----------------------------|
| `id_produto_siger`  | `bigint`   | ID do produto no SIGER      |
| `nome_produto`      | `text`     | Nome do produto             |
| `tipo_produto`      | `text`     | Tipo do produto             |
| `id_cliente_siger`  | `bigint`   | Relacionado ao cliente      |
| `id_contrato`       | `integer`  | Relacionado ao contrato     |
| `num_serie`         | `text`     | Número de série (opcional)  |
| `num_lote`          | `text`     | Número do lote (opcional)   |
| `deletado`          | `boolean`  | Status de exclusão lógica   |

---

## **Logs**

- Os logs são armazenados no arquivo `service.log` e incluem informações como:
  - Detecção de codificação do CSV.
  - Inserções bem-sucedidas no banco.
  - Detecção de duplicatas.
  - Erros e exceções.

### Exemplo de Log
```plaintext
2025-01-10 15:00:00 [INFO] Serviço iniciado. Atualização programada a cada 60 minutos.
2025-01-10 15:00:01 [INFO] Carregando arquivo de configuração.
2025-01-10 15:00:02 [INFO] Conectando ao Supabase.
2025-01-10 15:00:03 [INFO] Lendo dados do arquivo CSV: /path/to/file.csv
2025-01-10 15:00:03 [INFO] Carregados 4554 registros do CSV.
2025-01-10 15:00:04 [INFO] Sincronizando clientes.
2025-01-10 15:00:05 [INFO] Cliente inserido: {'id_siger_cliente': 1, 'nome_cliente': 'Cliente A', 'cnpj': '12345678901234'}
```

---

## **Contribuição**

1. Faça um fork do repositório.
2. Crie um branch para a sua feature/correção:
   ```bash
   git checkout -b minha-feature
   ```
3. Faça as alterações e envie um pull request.

---

## **Licença**

Este projeto está licenciado sob a licença MIT. Consulte o arquivo `LICENSE` para mais informações.

---

### Se precisar de mais ajustes ou algum outro conteúdo no README, é só avisar! 😊
