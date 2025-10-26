# Dude - Documentação Técnica

## Visão Geral da Arquitetura

O Dude é uma aplicação web full-stack construída com as seguintes tecnologias:

- **Frontend**: React 19 + Tailwind CSS 4 + TypeScript
- **Backend**: Express 4 + tRPC 11 + Node.js
- **Banco de Dados**: MySQL/TiDB com Drizzle ORM
- **Autenticação**: Manus OAuth
- **IA**: Google Gemini 2.5 Flash API

## Estrutura do Projeto

```
dude-chatbot/
├── client/                    # Frontend React
│   ├── src/
│   │   ├── pages/            # Páginas (Home, Chat)
│   │   ├── components/       # Componentes reutilizáveis
│   │   ├── lib/              # Utilitários e configurações
│   │   ├── App.tsx           # Router principal
│   │   └── index.css         # Estilos globais
│   └── index.html            # HTML principal
├── server/                    # Backend Node.js
│   ├── routers.ts            # Endpoints tRPC
│   ├── db.ts                 # Helpers de banco de dados
│   ├── gemini.ts             # Integração com API Gemini
│   └── _core/                # Código framework
├── drizzle/                   # Schema e migrações do banco
│   └── schema.ts             # Definição de tabelas
├── shared/                    # Código compartilhado
└── package.json              # Dependências do projeto
```

## Banco de Dados

### Schema

O projeto utiliza três tabelas principais:

#### `users`
Armazena informações de usuários autenticados via Manus OAuth.

```sql
- id (INT, PK, AUTO_INCREMENT)
- openId (VARCHAR, UNIQUE) - ID do OAuth
- name (TEXT)
- email (VARCHAR)
- loginMethod (VARCHAR)
- role (ENUM: 'user', 'admin')
- createdAt (TIMESTAMP)
- updatedAt (TIMESTAMP)
- lastSignedIn (TIMESTAMP)
```

#### `conversations`
Armazena sessões de chat para cada usuário.

```sql
- id (INT, PK, AUTO_INCREMENT)
- userId (INT, FK)
- title (VARCHAR)
- createdAt (TIMESTAMP)
- updatedAt (TIMESTAMP)
```

#### `messages`
Armazena mensagens individuais dentro de conversas.

```sql
- id (INT, PK, AUTO_INCREMENT)
- conversationId (INT, FK)
- role (ENUM: 'user', 'assistant')
- content (TEXT)
- mediaUrls (TEXT) - JSON array
- mediaTypes (TEXT) - JSON array
- createdAt (TIMESTAMP)
```

### Migrações

As migrações são gerenciadas com Drizzle Kit:

```bash
pnpm db:push  # Gera e aplica migrações
```

## Backend - tRPC Routers

### `auth` Router

Gerencia autenticação e sessões:

- `auth.me`: Retorna informações do usuário atual
- `auth.logout`: Faz logout do usuário

### `chat` Router

Gerencia conversas e mensagens:

- `chat.listConversations`: Lista todas as conversas do usuário
- `chat.getConversation`: Obtém uma conversa específica com todas as mensagens
- `chat.createConversation`: Cria uma nova conversa
- `chat.updateConversationTitle`: Atualiza o título de uma conversa
- `chat.deleteConversation`: Deleta uma conversa
- `chat.sendMessage`: Envia uma mensagem e obtém resposta do Gemini

## Integração com Gemini API

### Configuração

A API Gemini é configurada via variável de ambiente `GEMINI_API_KEY`.

### Funcionalidades

O arquivo `server/gemini.ts` fornece:

- `generateResponse()`: Gera resposta síncrona
- `generateResponseStream()`: Gera resposta com streaming

Ambas as funções:

- Aceitam histórico de mensagens para contexto
- Suportam system instructions personalizadas
- Processam múltiplas modalidades (texto, imagem, áudio, vídeo)

### Modelo Utilizado

- **Gemini 2.5 Flash**: Modelo rápido, eficiente e versátil
- **System Instruction**: Personalizada para apresentar o assistente como "Dude"

## Frontend - Componentes Principais

### ChatLayout

Componente wrapper que fornece:

- Sidebar com lista de conversas
- Botão "Novo chat"
- Menu de usuário e logout
- Layout responsivo com menu mobile

### Chat Page

Página principal de chat com:

- Área de mensagens com scroll automático
- Input com suporte a múltiplos arquivos
- Indicador de carregamento
- Animações de entrada de mensagens

### Home Page

Landing page com:

- Informações sobre o Dude
- Grid de funcionalidades
- CTA para começar
- Redirecionamento automático para chat se autenticado

## Estilos e Tema

### Paleta de Cores

Definida em `client/src/index.css`:

- **Primary (Verde)**: `#10b981`
- **Sidebar (Preto)**: `#1f2937` (light), `#0f172a` (dark)
- **Background**: Branco (light), `#0f172a` (dark)
- **Foreground**: Cinza escuro (light), Cinza claro (dark)

### Tema Escuro

O projeto usa tema escuro por padrão (`defaultTheme="dark"` em App.tsx).

### Componentes UI

Utiliza shadcn/ui para componentes consistentes:

- Button
- Input
- Card
- Dialog
- Tooltip
- Sonner (Toast notifications)

## Fluxo de Autenticação

1. Usuário acessa a aplicação
2. Se não autenticado, é redirecionado para página de login
3. Login via Manus OAuth
4. Token JWT é armazenado em cookie seguro
5. Cada requisição tRPC inclui contexto do usuário
6. Procedures protegidas verificam autenticação

## Fluxo de Chat

1. Usuário clica "Novo chat" ou seleciona conversa existente
2. Frontend carrega histórico de mensagens
3. Usuário digita mensagem e clica enviar
4. Frontend envia para `chat.sendMessage`
5. Backend salva mensagem do usuário no DB
6. Backend chama API Gemini com histórico
7. Backend salva resposta do Gemini no DB
8. Frontend atualiza UI com resposta

## Deployment

### Variáveis de Ambiente Necessárias

- `DATABASE_URL`: Conexão MySQL/TiDB
- `GEMINI_API_KEY`: Chave da API Google Gemini
- `JWT_SECRET`: Segredo para assinatura de cookies
- `VITE_APP_ID`: ID da aplicação Manus
- `OAUTH_SERVER_URL`: URL do servidor OAuth Manus
- `VITE_OAUTH_PORTAL_URL`: URL do portal de login Manus

### Build

```bash
pnpm build  # Compila frontend e backend
pnpm start  # Inicia servidor de produção
```

### Performance

- Frontend: ~200KB gzipped (incluindo dependências)
- Tempo de resposta: <100ms para mensagens de texto
- Processamento de mídia: 2-10s dependendo do tamanho

## Melhorias Futuras

- [ ] Implementar upload de arquivos para S3
- [ ] Adicionar transcrição de áudio com Whisper
- [ ] Implementar geração de imagens
- [ ] Adicionar busca em histórico de conversas
- [ ] Implementar compartilhamento de conversas
- [ ] Adicionar temas customizáveis
- [ ] Implementar rate limiting por usuário
- [ ] Adicionar analytics de uso
- [ ] Implementar cache de respostas
- [ ] Adicionar suporte a múltiplos idiomas

## Troubleshooting

### Erro de Conexão ao Banco de Dados

Verifique se `DATABASE_URL` está configurada corretamente e o banco está acessível.

### Erro de API Gemini

Verifique se `GEMINI_API_KEY` é válida e não expirou. Consulte https://ai.google.dev para gerar uma nova chave.

### Erro de Autenticação

Verifique se as variáveis de OAuth (`VITE_APP_ID`, `OAUTH_SERVER_URL`) estão configuradas corretamente.

### Problema de Responsividade

Limpe o cache do navegador e recarregue. O Tailwind CSS deve compilar automaticamente.

## Referências

- [Documentação Gemini API](https://ai.google.dev/gemini-api)
- [Documentação tRPC](https://trpc.io)
- [Documentação Drizzle ORM](https://orm.drizzle.team)
- [Documentação Tailwind CSS](https://tailwindcss.com)
- [Documentação shadcn/ui](https://ui.shadcn.com)

