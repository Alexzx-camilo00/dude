# Dude Chatbot - TODO

## Core Features
- [x] Integração com API Gemini gratuita
- [x] Sistema de autenticação com email/senha
- [x] Salvamento de histórico de conversas no banco de dados
- [x] Visualização de chats anteriores (apenas após login)
- [x] Funcionalidade "Novo chat"

## Backend
- [x] Criar tabelas de banco de dados (users, conversations, messages)
- [x] Implementar endpoints tRPC para gerenciar conversas
- [x] Implementar endpoint tRPC para enviar mensagens e chamar API Gemini
- [x] Integração com API Gemini para processamento de texto
- [ ] Suporte a upload de imagens, vídeos e áudios (estrutura pronta, precisa implementar upload)
- [ ] Transcrição de áudio usando Whisper API
- [ ] Geração de imagens usando Image Generation API

## Frontend
- [x] Design do layout principal (sidebar + área de chat)
- [x] Componente de barra lateral com "Novo chat" e lista de conversas
- [x] Componente de área de mensagens
- [x] Componente de input com suporte a múltiplos tipos de arquivo
- [x] Animação de digitação para respostas do Dude
- [x] Responsividade para mobile e desktop
- [x] Tema verde e preto com detalhes em branco/cinza
- [x] Ícones visuais para tipos de arquivo
- [x] Página de login/registro (automática via OAuth)
- [x] Página de chat principal

## Design
- [x] Paleta de cores: verde principal, preto, branco, cinza
- [x] Tipografia e espaçamento consistentes
- [x] Componentes reutilizáveis
- [x] Transições e animações suaves
- [x] Estados de carregamento e erro

## Bugs Encontrados e Corrigidos
- [x] Erro na integração com API Gemini (falha ao chamar generateContent) - CORRIGIDO: Adicionado ENV.geminiApiKey ao arquivo de configuração
- [x] Erro de nesting: button dentro de button no ChatLayout - CORRIGIDO: Substituído button por div com evento onClick

## Testes
- [ ] Testar fluxo de autenticação
- [ ] Testar envio de mensagens de texto
- [ ] Testar upload e processamento de imagens
- [ ] Testar upload e processamento de áudios
- [ ] Testar upload e processamento de vídeos
- [ ] Testar responsividade em mobile
- [ ] Testar responsividade em desktop



## Bugs Novos Encontrados
- [x] Dude respondendo em inglês em vez de português - CORRIGIDO: Atualizado system prompt para português




## Novas Funcionalidades Solicitadas
- [x] Implementar geração de imagens com comando /imagine - CONCLUÍDO: Integrado com API de geração de imagens, suporte a /imagine e variações em português




## Bugs Encontrados em Testes
- [x] Erro na API de geração de imagens: "Not Found" - endpoint incorreto - CORRIGIDO: Atualizado para usar endpoint correto do Manus (images.v1.ImageService/GenerateImage)




## Preparação para Lançamento
- [x] Limpar todo o histórico de conversas e mensagens do banco de dados - CONCLUÍDO




## Bugs Encontrados Pós-Lançamento
- [x] Erro "Conversation not found" quando tenta acessar conversa deletada - redirecionar para nova conversa - CORRIGIDO: Adicionado tratamento de erro que cria nova conversa automaticamente



- [x] Tratamento de erro "Conversation not found" não está funcionando corretamente - melhorar lógica de detecção e redirecionamento - CORRIGIDO: Adicionado estado isRedirecting e melhorada detecção de erro

