# 📊 Projeto EDA — Guia de Inicialização do Ambiente

Guia completo para garantir o **mesmo ambiente reproduzível** toda vez que ligar o PC e retomar o projeto.

---

## 🗂️ Estrutura do Projeto

```
meu-projeto-eda/
├── .venv/                  ← ambiente virtual (gerado automaticamente pelo UV, não commitar)
├── .vscode/
│   └── settings.json       ← configurações do VSCode para o projeto
├── .python-version         ← fixa a versão do Python (gerado pelo UV)
├── .gitignore
├── pyproject.toml          ← fonte da verdade das dependências
├── uv.lock                 ← lockfile com versões exatas (sempre commitar)
├── notebooks/
│   └── eda.ipynb           ← seus notebooks de análise
├── data/
│   ├── raw/                ← dados brutos (nunca modificar)
│   └── processed/          ← dados processados
└── README.md
```

---

## ⚙️ Pré-requisitos — Instalar apenas uma vez

Esses passos só precisam ser feitos **uma única vez na máquina**.

### 1. Instalar o Python 3.12

Baixe o instalador em:
```
https://www.python.org/downloads/release/python-31210/
```

Arquivo: `python-3.12.10-amd64.exe`

> ✅ Durante a instalação, marque **"Add Python to PATH"**

Verifique a instalação:
```powershell
python --version
# Python 3.12.x
```

### 2. Instalar o UV

Abra o PowerShell como administrador e execute:
```powershell
powershell -ExecutionPolicy ByPass -c "irm https://astral.sh/uv/install.ps1 | iex"
```

Feche e abra o terminal novamente, depois verifique:
```powershell
uv --version
```

---

## 🚀 Criação do Projeto — Fazer apenas uma vez

```powershell
# Cria a pasta e inicializa o projeto com Python 3.12
uv init meu-projeto-eda --python 3.12
cd meu-projeto-eda

# Fixa a versão do Python no projeto
uv python pin 3.12

# Adiciona as dependências de EDA
uv add pandas numpy matplotlib seaborn scipy

# Adiciona o Jupyter para os notebooks
uv add --dev ipykernel jupyter

# Registra o kernel do projeto no VSCode
uv run python -m ipykernel install --user --name=meu-projeto-eda --display-name "Python (meu-projeto-eda)"
```

### Criar o `.gitignore`

```
.venv/
__pycache__/
*.pyc
.DS_Store
data/raw/
*.env
```

### Criar o `.vscode/settings.json`

```json
{
  "python.defaultInterpreterPath": "${workspaceFolder}\\.venv\\Scripts\\python.exe",
  "python.terminal.activateEnvironment": true,
  "jupyter.kernelProviderSettings": {
    "ms-toolsai.jupyter-hub": {
      "serverSettings": {}
    }
  }
}
```

---

## 🔁 Rotina Diária — Toda vez que ligar o PC

Esses são os únicos comandos que você precisa rodar ao retomar o projeto.

### Passo 1 — Abra o terminal no VSCode

`Ctrl + `` ` (backtick) — abre o terminal integrado já na pasta do projeto.

### Passo 2 — Sincronize o ambiente

```powershell
uv sync
```

> Esse comando garante que o `.venv` está com **exatamente as mesmas versões** definidas no `uv.lock`.  
> É seguro rodar sempre — se nada mudou, ele termina em segundos.

### Passo 3 — Abra o notebook

No VSCode, abra seu arquivo `.ipynb` e no canto superior direito clique em **"Select Kernel"** e escolha:

```
Python (meu-projeto-eda)
```

### Passo 4 — Verifique o ambiente (opcional, mas recomendado)

Na primeira célula do notebook:

```python
import sys
print(sys.executable)        # deve apontar para .venv\Scripts\python.exe
print(sys.version)           # deve mostrar Python 3.12.x

import pandas as pd
import numpy as np
print(pd.__version__)
print(np.__version__)
```

---

## 📦 Gerenciamento de Dependências

### Adicionar um novo pacote

```powershell
uv add nome-do-pacote

# Exemplos:
uv add plotly
uv add openpyxl
uv add scikit-learn
```

### Adicionar pacote apenas para desenvolvimento

```powershell
uv add --dev nome-do-pacote
```

### Remover um pacote

```powershell
uv remove nome-do-pacote
```

### Ver todos os pacotes instalados

```powershell
uv pip list
```

### Importar de um requirements.txt existente

```powershell
uv add -r requirements.txt
```

> ⚠️ Sempre rode `uv sync` após qualquer alteração nas dependências.

---

## 🧩 Extensões Recomendadas no VSCode

Instale pelo marketplace do VSCode (`Ctrl+Shift+X`):

| Extensão | ID | Para que serve |
|---|---|---|
| Python | `ms-python.python` | Suporte geral ao Python |
| Jupyter | `ms-toolsai.jupyter` | Executar notebooks `.ipynb` |
| Pylance | `ms-python.vscode-pylance` | Intellisense e type checking |
| Ruff | `charliermarsh.ruff` | Linting e formatação |

---

## ❗ Solução de Problemas Comuns

### O kernel não aparece no notebook

```powershell
# Re-registre o kernel
uv run python -m ipykernel install --user --name=meu-projeto-eda --display-name "Python (meu-projeto-eda)"
```

Depois reinicie o VSCode (`Ctrl+Shift+P` → "Developer: Reload Window").

### O `.venv` foi deletado ou corrompido

```powershell
# Recria tudo do zero a partir do uv.lock
uv sync
```

### Erro de versão de pacote incompatível

```powershell
# Força a sincronização com o lockfile
uv sync --frozen
```

### Verificar qual Python está sendo usado

```powershell
uv run python --version
uv run python -c "import sys; print(sys.executable)"
```

---

## 🔒 Por que esse setup garante reprodutibilidade?

| Arquivo | Função |
|---|---|
| `.python-version` | Fixa a versão exata do Python (3.12) |
| `pyproject.toml` | Lista as dependências com versões mínimas |
| `uv.lock` | Fixa as versões **exatas** de todos os pacotes e suas dependências transitivas |

O `uv sync` lê o `uv.lock` e instala **exatamente** o que está registrado — não importa o dia, a máquina ou quem esteja rodando.

> 💡 **Regra de ouro:** sempre commite o `uv.lock` no Git. Nunca commite o `.venv`.

Rodar o mlflow localmente: 
```powershell
.venv\Scripts\python.exe -m mlflow ui --backend-store-uri file:///C:/mlflow --host 127.0.0.1 --port 5000
```