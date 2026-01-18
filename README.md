# Changelog - Versão 1.0.0.4

---

## 🆕 Novos Recursos

---

### 1. Comando `--platform`

Novo comando para gerar **apenas o Modo Plataforma** de um curso já baixado, sem precisar baixar novamente.

**Quando usar:**
- Quando você já baixou um curso e quer gerar novamente a Plataforma
- Quando atualizou o Udeffy e quer aplicar melhorias no Modo Plataforma

**O que acontece:**
1. O Udeffy localiza a pasta do curso já baixado
2. Gera novamente toda a estrutura do Modo Plataforma
3. Não faz nenhum download adicional

---

### 2. Suporte ao OpenRouter (Fallback de IA)

O Udeffy agora utiliza **dois provedores de IA**: Google Gemini (principal) e OpenRouter (fallback).

**Por que isso é útil:**
- O Google Gemini tem limite de ~40 requisições diárias na versão gratuita
- Quando esse limite é atingido, o OpenRouter assume automaticamente
- Você não perde a geração de palavras-chave e tipos de curso

**Modelos utilizados:**
| Provedor | Modelos |
|----------|---------|
| Gemini (prioridade) | gemini-2.5-flash-lite, gemini-2.5-flash |
| OpenRouter (fallback) | gemma-3-27b, llama-3.3-70b, mimo-v2-flash, devstral-2512 |

**Observação:** A OpenRouter é aberta, então os modelos gratuitos podem estourar o limite por conta do uso diário dos clientes!

**Como obter a chave OpenRouter:**
1. Acesse [openrouter.ai](https://openrouter.ai/)
2. Crie uma conta gratuita
3. Vá em "API Keys" e gere uma nova chave

---

### 3. Notificações via Telegram

Receba notificações em tempo real sobre o status dos seus downloads através de um bot do Telegram.

**Tipos de notificações:**
- ✅ **Download concluído** - Resumo com estatísticas e erros detalhados
- ⚠️ **Erro ao salvar informações** - Quando falha ao salvar info.txt do curso
- ❌ **Erro de DRM** - Quando não consegue obter a chave de descriptografia
- ⚠️ **Erro no relatório** - Quando falha ao gerar o relatório
- 🤖 **Erro de IA** - Quando todas as tentativas de IA falham

**Erros de download são agrupados e detalhados:**
- Erros iguais são consolidados com contagem (ex: `Erro ao baixar MPD (x15)`)
- Mostra no máximo 5 tipos de erros diferentes
- Evita flood de mensagens (apenas 1 notificação ao final)

**Exemplo de notificação com erros:**
```
❌ Erros: 23
   • Erro ao baixar stream MPD (x10)
   • HTTP 403 ao baixar material (x8)
   • Erro ao baixar legenda (x5)
```

**Como configurar:**
1. Crie um bot no Telegram buscando por `@BotFather` e enviando `/newbot`
2. Obtenha seu Chat ID buscando por `@userinfobot` no Telegram
3. Configure no `config.json` (veja seção de configuração no final)

**Observações:**
- Se `telegram_bot_token` estiver vazio, o Udeffy usa um bot padrão
- Se `telegram_chat_id` estiver vazio, **nenhuma notificação será enviada**
- Você só precisa configurar seu `telegram_chat_id` para ativar as notificações

---

### 4. Identificação de Tipo de Curso (via IA)

O Udeffy agora identifica automaticamente a **categoria do curso** usando IA. Isso é útil para:
- Organização automática de cursos
- Integração com o Auto Uploader (automação de uploads)

**Categorias disponíveis:**
```
55: Programação/TI    56: Idiomas           57: Design
58: Des. Pessoal      59: Arquitetura       60: Trader
62: Administração     63: Concurso          64: Copywriting
65: Culinária         66: ADS               67: Investimentos
68: Direito           69: Dropshipping      70: Saúde
71: Manutenção        72: Finanças          73: Desenho
74: ENEM/Vestibular   75: Cia. Humanas      76: Religião
77: Audiovisual       78: Eletrônica        79: Profissional
80: Engenharia        81: Apostas           82: Tráfego Pago
83: Medicina          84: Hacker            85: Diversos
86: Marketing         87: Música
```

---

### 5. Limpeza Inteligente de Nome do Autor

A IA agora analisa e limpa o nome do autor para a sugestão de título, removendo:
- Certificações (AWS, Azure, Google, etc.)
- Títulos acadêmicos (Dr., Prof., PhD, MBA, etc.)
- Nomes de empresas e textos extras irrelevantes

**Exemplos:**
| Antes | Depois |
|-------|--------|
| `Stephane Maarek \| AWS Certified Cloud Practitioner` | `Stephane Maarek` |
| `Dr. João Silva, PhD` | `João Silva` |
| `Lucas Almeida, Maria Santos, José Oliveira` | `Diversos Autores` |

---

## 🔧 Melhorias

---

### 6. Limite de Caracteres Reduzido

O limite máximo para nomes de **subpastas e arquivos** foi reduzido de 66 para **57 caracteres**.

**Por que:** Maior compatibilidade com diferentes sistemas de arquivo e caminhos longos no Windows.

---

### 7. Correção na Nota de Legendas (`--report`)

**Problema:** Ao usar o comando `--report` em cursos já baixados, a nota de legendas não aparecia.

**Solução:** O Udeffy agora detecta automaticamente as legendas existentes na pasta do curso.

---

### 8. Correções na Estrutura do Relatório

Ajustes gerais na formatação e estrutura do arquivo `relatorio.txt`.

---

## 📄 Nova Estrutura do `info.txt`

O arquivo `info.txt` gerado na pasta `_extra_NomeDoCurso-CL` agora contém:

```
https://www.udemy.com/course/nome-do-curso/
ID: 1234567

Curso: Nome Completo do Curso
Avaliação: 4.7/5
Instrutor: Nome Original do Instrutor
Instrutor (Display): Nome Exibido no Perfil
Última Atualização: 2026-01

Sugestão de título:
Udemy: Nome do Curso - Autor Limpo [01/2026] [INGLÊS] [CapyLabs]

Tipo:
55: Programação/TI

Palavras-chave:
Python, Django, API, REST, Backend
```

| Campo | Descrição |
|-------|-----------|
| **URL/ID** | Link e ID numérico do curso |
| **Curso** | Título original |
| **Avaliação** | Nota média (X/5) |
| **Instrutor** | Nome interno e display |
| **Sugestão de título** | Título formatado com autor limpo e idioma |
| **Tipo** | Categoria identificada pela IA |
| **Palavras-chave** | 5 palavras-chave geradas pela IA |

---

## 📋 Comandos Atualizados

| Comando | Descrição |
|---------|-----------|
| `--report` | Gerar apenas relatório (sem download) |
| `--platform` | 🆕 Gerar apenas Modo Plataforma (sem download) |
| `--no-report` | Pular geração de relatório |
| `--no-platform` | Pular geração do Modo Plataforma |

---

## ⚙️ Configuração do `config.json`

Todas as configurações ficam centralizadas no arquivo `config.json`:

```json
{
    "imgbb_api_key": "SUA_CHAVE_IMGBB",
    "google_gemini_api_key": "SUA_CHAVE_GEMINI",
    "openrouter_api_key": "SUA_CHAVE_OPENROUTER",
    "telegram_bot_token": "",
    "telegram_chat_id": "SEU_CHAT_ID"
}
```

| Chave | Obrigatória | Descrição |
|-------|-------------|-----------|
| `imgbb_api_key` | Sim | Upload de thumbnails no relatório |
| `google_gemini_api_key` | Sim | IA principal (palavras-chave e tipos) |
| `openrouter_api_key` | Opcional | Fallback quando Gemini excede limite |
| `telegram_bot_token` | Opcional | Token do bot (deixe vazio para usar o padrão) |
| `telegram_chat_id` | Opcional | Seu Chat ID (deixe vazio para desativar notificações) |
