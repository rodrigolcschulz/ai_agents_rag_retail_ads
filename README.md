# RAG - Assistente de Marketplace

Agente de IA especializado em Product Ads

## 🚀 Funcionalidades

- **RAG (Retrieval-Augmented Generation)** com base de conhecimento sobre Retail Media
- **Scraping automático** de conteúdo do blog Conecta Ads
- **Interface web** interativa com Gradio
- **Pipeline ETL** completo para processamento de dados
- **Análise de conteúdo** e geração de relatórios
- **Fine-Tuning de LLMs** com QLoRA (4-bit quantization)
- **Evaluation Loops** automatizados com múltiplas métricas
- **MLflow** para tracking de experimentos

## 🛠️ Instalação

### Pré-requisitos

- Python 3.8+
- OpenAI API Key

### Setup Local

```bash
# Clone o repositório
git clone https://github.com/rodrigolcschulz/ai_agents_rag_retail_ads.git
cd melancia-ai-rag

# Crie e ative o ambiente virtual
python -m venv venv
venv\Scripts\activate  # Windows
# source venv/bin/activate  # Linux/Mac

# Instale as dependências
pip install -r requirements.txt

# Configure a API Key
echo "OPENAI_API_KEY=sua_api_key_aqui" > .env
```

## 🎯 Como Usar

### Interface Web

```bash
python src/agent/web_interface.py
```

Acesse: http://localhost:8000

### Terminal

```bash
python src/agent/main.py
```

### Pipeline ETL

```bash
# Scraping + Análise
python src/etl/run_etl.py

# Apenas scraping
python src/etl/run_etl.py --no-analyze

# Limitar artigos
python src/etl/run_etl.py --max-articles 10
```

## 🐳 Docker

```bash
docker compose up -d
```

## 📁 Estrutura do Projeto

```text
melancia-ai-rag/
├── src/
│   ├── agent/          # Agente RAG principal
│   │   ├── main.py              # CLI do agente
│   │   ├── web_interface_with_eval.py  # Interface web com evaluation
│   │   ├── config.py            # Configurações gerais
│   │   ├── keywords.py          # 🆕 120+ keywords organizadas
│   │   ├── retriever.py         # 🆕 Retriever com MMR
│   │   ├── prompt.py            # Templates de prompts
│   │   ├── memory.py            # Gestão de memória
│   │   └── utils.py             # Funções auxiliares
│   ├── llm/            # 🆕 Gerenciamento de LLMs
│   │   ├── __init__.py
│   │   └── manager.py           # MultiLLMManager (OpenAI, Ollama)
│   ├── etl/            # Pipeline ETL
│   │   ├── scraper_blog_conecta.py
│   │   ├── analyzer.py          # Análise de conteúdo
│   │   └── populate_vector_db.py
│   ├── experiments/    # 🔬 Experimentação e Benchmarks
│   │   ├── benchmark.py         # Sistema de benchmark
│   │   ├── benchmark_models_mlflow.py  # 🆕 Benchmark com MLflow
│   │   └── run_experiments.py   # Script principal
│   ├── evaluation/     # 🆕 Sistema de avaliação
│   │   ├── rag_evaluator.py     # Métricas de qualidade RAG
│   │   └── interaction_logger.py # Log de interações
│   ├── mlops/          # 🆕 MLOps e tracking
│   │   ├── tracking.py          # MLflow tracking
│   │   ├── registry.py          # Model registry
│   │   └── model_router.py      # Roteamento Ollama/OpenAI
│   └── monitoring/     # 🆕 Monitoramento de Performance
│       ├── __init__.py
│       └── latency_monitor.py   # Monitor de latência
├── data/
│   ├── input/          # Arquivos markdown
│   │   ├── blog_conecta/         # Blog da Conecta Ads
│   │   └── central_vendedores/   # Central do Mercado Livre
│   ├── output/         # Relatórios e análises
│   ├── vector_db/      # Base vetorial (ChromaDB)
│   ├── evaluation/     # 🆕 Logs de interações (SQLite)
│   ├── experiments/    # 🆕 Resultados de benchmarks
│   └── models/         # 🆕 Modelos treinados
├── docs/               # Documentação detalhada
│   ├── AGENT_ARCHITECTURE.md     # Arquitetura do agente
│   ├── FINE_TUNING_GUIDE.md
│   ├── EVALUATION_GUIDE.md
│   ├── HARDWARE_SETUP.md
│   ├── MLOPS_REPORT.md
│   ├── MONITORING_BEST_PRACTICES.md  # 🆕 Monitoramento
│   ├── RAG_MEMORY_VS_CACHE.md        # 🆕 Como funciona memória
│   └── MODEL_BENCHMARK_GUIDE.md      # 🆕 Guia de benchmark
├── notebooks/          # 🆕 Jupyter notebooks
│   └── experimentacao_llms.ipynb
├── mlruns/             # 🆕 MLflow tracking data
├── logs/               # Logs do sistema
└── requirements.txt    # Dependências
```

## 📝 Sistema de Logs

O MelancIA mantém um sistema completo de logs:

- **`logs/chat_history.txt`** - Histórico completo de conversas
- **`data/output/chat_history.pkl`** - Memória da conversa (últimas 5 interações)
- **`logs/etl_pipeline.log`** - Logs do pipeline ETL
- **`logs/scraper.log`** - Logs do web scraping
- **`logs/vector_db.log`** - Logs da base vetorial

> **Nota**: Todos os arquivos de log são ignorados pelo Git (`.gitignore`)

## 🧠 Como Funciona o RAG

O Agente utiliza uma arquitetura RAG (Retrieval-Augmented Generation) sofisticada:

1. **📚 Base de Conhecimento**: Conteúdo do blog Conecta Ads + Central de Vendedores do Mercado Livre
2. **🔍 Embeddings**: Transforma o conteúdo em vetores usando OpenAI embeddings (text-embedding-3-small)
3. **💾 Banco Vetorial**: Armazena os vetores no ChromaDB para busca rápida
4. **🎯 Busca Inteligente (MMR)**: Maximum Marginal Relevance para retornar documentos relevantes E diversos
5. **🤖 LLM**: GPT-4o-mini gera respostas baseadas no contexto recuperado
6. **🔄 Memória**: Mantém contexto das últimas 5 conversas
7. **🛡️ Filtros Contextuais**: Sistema de keywords para validar relevância das perguntas

### 🔑 Sistema de Keywords Expandido

O MelancIA agora possui mais de **120+ keywords** organizadas em categorias:

- **Retail Media & Publicidade**: product ads, anúncios patrocinados, display ads
- **Métricas**: ACOS, TACOS, ROAS, CTR, CPC, visibilidade, ranking
- **Catálogo**: ficha técnica, características, variações, compatibilidade
- **Categorias Específicas**: autopeças, veículos, compatibilidade
- **Logística**: envio flex, fulfillment, ME1, mesmo dia
- **Reputação**: avaliações, reclamações, tempo de resposta
- **Ferramentas**: excel, editor, planilha, anunciador em massa
- **Branding**: loja oficial, marca, INPI
- **Financeiro**: mercado pago, crédito, taxas, custos
- **Eventos**: black friday, sazonalidade

Ver lista completa em: `src/agent/keywords.py`

### ⚙️ Configurações do RAG

O sistema permite ajustes finos de performance:

```python
# src/agent/config.py

RETRIEVER_K = 15  # Número de documentos recuperados
RETRIEVER_SEARCH_TYPE = "mmr"  # Tipo de busca
# Opções: "similarity", "mmr", "similarity_score_threshold"
```

**MMR (Maximum Marginal Relevance)**: Balanceia relevância e diversidade para evitar documentos redundantes.

## 🤖 Sobre o MelancIA

MelancIA é um assistente especializado em:

- **Retail Media** e estratégias de anúncios
- **E-commerce** e marketplaces (Mercado Livre, Shopee, Amazon)
- **Product Ads** e anúncios patrocinados
- **Métricas de performance** (ACOS, ROAS, CTR, CPC, ROI)
- **Catálogo de produtos**: ficha técnica, características, variações
- **Categorias específicas**: autopeças, compatibilidade de veículos
- **Logística** e fulfillment (Envio Flex, Full, ME1)
- **Reputação e atendimento**: mensagens, avaliações, tempo de resposta
- **Ferramentas de gestão**: Excel, Editor, Planilha, Anunciador em Massa
- **Análise de concorrência** e tendências
- **Black Friday** e sazonalidade

### 📊 Base de Conhecimento

O assistente possui conhecimento sobre:
- **177+ documentos** do blog Conecta Ads e Central de Vendedores
- Conteúdo categorizado por temas (Retail Media, Logística, Catálogo, etc.)
- Atualizações automáticas via scraping

## 🧪 Experimentação com LLMs Open Source

O MelancIA agora suporta múltiplos provedores de LLM para experimentação e comparação!

### 💻 Hardware Testado

Este projeto foi otimizado para rodar em:
- **CPU**: AMD Ryzen 5 3600X (6 cores, 12 threads)
- **RAM**: 15 GB
- **Disco**: 439 GB (346 GB disponíveis)
- **GPU**: AMD Radeon RX 570/580 (CPU inference recomendado)

✅ **Viável**: Modelos 2-7B quantizados (Phi-3, Llama 3.2, Gemma)
⚠️ **Não recomendado**: Modelos 13B+, GPU training

📖 **Ver**: [docs/HARDWARE_SETUP.md](docs/HARDWARE_SETUP.md) para análise completa

### Provedores Suportados

**🤖 OpenAI** (Pago - Alta Qualidade)
- gpt-4o-mini
- gpt-4o
- gpt-3.5-turbo

**🦙 Ollama** (Gratuito - Local)
- llama3.1:8b / llama3.1:70b
- mistral:7b
- phi3:mini
- gemma2:9b
- qwen2.5:7b

**🤗 HuggingFace** (Gratuito - API)
- mistralai/Mistral-7B-Instruct-v0.2
- meta-llama/Llama-2-7b-chat-hf
- tiiuae/falcon-7b-instruct

### 🚀 Quick Start - Experimentos

#### 1. Instalar Ollama (Recomendado para testes locais)

```bash
# Instalar Ollama
curl -fsSL https://ollama.ai/install.sh | sh

# Baixar modelo
ollama pull llama3.1:8b

# Verificar se está rodando
ollama list
```

#### 2. Instalar dependências de experimentação

```bash
pip install -r requirements.txt
```

#### 3. Executar teste rápido

```bash
# Teste rápido (OpenAI + Ollama se disponível)
python src/experiments/run_experiments.py --mode quick

# Benchmark completo (todos os modelos)
python src/experiments/run_experiments.py --mode full

# Abrir MLflow UI
python src/experiments/run_experiments.py --mode ui
```

#### 4. Experimentação no Jupyter

```bash
# Iniciar Jupyter
jupyter lab

# Abrir: notebooks/experimentacao_llms.ipynb
```

### 📊 Comparação de Modelos

O sistema de benchmark avalia automaticamente:
- ⚡ **Latência** - Tempo de resposta
- ⭐ **Qualidade** - Score de qualidade da resposta
- 🎯 **Relevância** - Pertinência ao contexto
- 💰 **Custo** - Custo por pergunta (USD)
- 📝 **Tokens** - Uso de tokens

**Exemplo de uso programático:**

```python
from src.experiments.multi_llm import MultiLLMManager
from src.experiments.benchmark import ModelBenchmark

# Criar LLMs
llm_openai = MultiLLMManager.create_llm("openai", "gpt-4o-mini")
llm_ollama = MultiLLMManager.create_llm("ollama", "llama3.1:8b")

# Comparar modelos
benchmark = ModelBenchmark(retriever, memory)
benchmark.add_model("openai", "gpt-4o-mini", llm_openai)
benchmark.add_model("ollama", "llama3.1:8b", llm_ollama)

results = benchmark.run(test_questions)
benchmark.print_report()
```

### 🔬 MLflow Tracking

Todos os experimentos são rastreados automaticamente com MLflow:

```bash
# Visualizar experimentos
mlflow ui --port 5000

# Abrir navegador em: http://localhost:5000
```

**Métricas rastreadas:**
- Parâmetros do modelo (temperatura, tokens, etc)
- Métricas de performance (latência, qualidade)
- Comparação entre runs
- Versionamento de modelos

## 🎓 Fine-Tuning e Evaluation Loops

### Workflow Completo

```
1. Preparar Dados     → notebooks/prepare_finetuning_data.ipynb
2. Fine-Tuning        → notebooks/fine_tuning_qlora_colab.ipynb
3. Evaluation         → notebooks/evaluate_model.ipynb
```

### 1️⃣ Preparar Dataset

```bash
# Preparar dados no formato correto
jupyter notebook notebooks/prepare_finetuning_data.ipynb

# Output: training_dataset/ com splits train/test
```

### 2️⃣ Fine-Tuning (Google Colab)

```bash
# No Google Colab com GPU T4 (gratuito)
1. Abra: notebooks/fine_tuning_qlora_colab.ipynb
2. Configure GPU: Runtime > Change runtime type > T4 GPU
3. Execute células para:
   - Carregar modelo com quantização 4-bit
   - Aplicar LoRA adapters
   - Treinar com seu dataset
   - Salvar modelo fine-tunado
4. Baixe modelo treinado
```

**Otimizado para T4 (15GB VRAM)**:
- Batch size: 1
- Gradient accumulation: 16
- Max sequence length: 1024
- LoRA rank: 8

### 3️⃣ Evaluation Loops

```bash
# Avaliar modelo fine-tunado vs base
jupyter notebook notebooks/evaluate_model.ipynb

# Métricas automáticas:
# - ROUGE (overlap de n-gramas)
# - BLEU (qualidade de geração)
# - BERTScore (similaridade semântica)
# - Comparação lado a lado
# - Visualizações e relatórios
```

**Classe Reutilizável**:

```python
from src.evaluation.evaluator import ModelEvaluator

# Criar evaluator
evaluator = ModelEvaluator(model, tokenizer, "meu-modelo")

# Avaliar dataset
results = evaluator.evaluate_dataset(test_data)

# Gerar relatório
report = evaluator.generate_report(results)
```

### 📚 Documentação Detalhada

- [📖 Guia Completo de Fine-Tuning](docs/FINE_TUNING_GUIDE.md)
- [📊 Guia de Evaluation Loops](docs/EVALUATION_GUIDE.md)
- [🔬 MLOps Report](docs/MLOPS_REPORT.md)
- [📐 Arquitetura do Agente](docs/AGENT_ARCHITECTURE.md)
- [📊 Monitoramento e Latência - Boas Práticas](docs/MONITORING_BEST_PRACTICES.md)
- [🧠 Memória RAG vs Cache - Como Funciona](docs/RAG_MEMORY_VS_CACHE.md)
- [🖥️ Hardware Setup e Limitações](docs/HARDWARE_SETUP.md)
- [🚀 Guia de Benchmark de Modelos com MLflow](docs/MODEL_BENCHMARK_GUIDE.md)

---

## 📊 Tecnologias

### Core RAG
- **LangChain** - Framework RAG e orquestração
- **OpenAI GPT-4o-mini** - Modelo de linguagem
- **OpenAI Embeddings** - Modelo de embeddings (text-embedding-3-small)
- **ChromaDB** - Base de dados vetorial
- **Gradio** - Interface web interativa
- **BeautifulSoup** - Web scraping
- **Pandas** - Análise de dados
- **Docker** - Containerização

### 🆕 LLMs Open Source & MLOps
- **Ollama** - Execução local de LLMs (Llama 3.1, Mistral, Phi-3)
- **HuggingFace** - Acesso a modelos open source
- **PyTorch** - Framework de deep learning
- **Transformers** - Biblioteca de modelos
- **PEFT/LoRA** - Fine-tuning eficiente
- **MLflow** - Tracking de experimentos
- **Weights & Biases** - Monitoramento de treinamento

## 📄 Licença

Apache License 2.0 - veja [LICENSE](LICENSE) para detalhes.

---

**🍉 Transformando perguntas em estratégias de sucesso no Retail Media!**
