# Pesquisa: API Gemini Gratuita

## Resumo das Funcionalidades

A API Gemini gratuita oferece suporte completo a múltiplas modalidades de entrada e saída, permitindo criar um chatbot versátil.

### Modelos Disponíveis
- **Gemini 2.5 Flash**: Modelo rápido e eficiente, ideal para aplicações em tempo real
- **Gemini 2.5 Pro**: Modelo mais poderoso, com limite de 100 requisições/dia na versão gratuita
- Limite geral: 1.500 requisições/dia com Gemini 2.5 Flash

### Funcionalidades Suportadas

#### 1. Geração de Texto
- Entrada: texto puro
- Saída: texto gerado
- Suporta system instructions para guiar o comportamento
- Streaming de respostas disponível
- Conversas multi-turno (chat)

#### 2. Processamento de Imagens
- **Entrada**: JPEG, PNG, WebP, GIF
- **Métodos de upload**:
  - Inline (até 20MB total da requisição)
  - File API (para arquivos maiores ou reuso)
- **Capacidades**:
  - Descrição e análise de imagens
  - Detecção de objetos com bounding boxes
  - Segmentação de imagens
  - Resposta a perguntas sobre imagens
  - Suporte a múltiplas imagens por requisição (até 10)

#### 3. Processamento de Vídeos
- **Entrada**: MP4, WebM, MOV, AVI, etc.
- **Métodos de upload**:
  - File API (recomendado para vídeos > 20MB ou > 1 minuto)
  - Inline (para vídeos pequenos)
  - YouTube URLs
- **Capacidades**:
  - Descrição e análise de vídeos
  - Resposta a perguntas sobre conteúdo
  - Referência a timestamps específicos (formato MM:SS)
  - Transcrição de áudio do vídeo
  - Descrições visuais (1 frame por segundo)
- **Limites**: Máximo 15MB por arquivo

#### 4. Processamento de Áudio
- **Formatos suportados**: WAV, MP3, AIFF, AAC, OGG Vorbis, FLAC
- **Métodos de upload**:
  - File API (recomendado)
  - Inline (até 20MB)
- **Capacidades**:
  - Transcrição de áudio
  - Análise e descrição do conteúdo
  - Referência a timestamps
  - Suporte a múltiplos canais (combinados em um)
- **Limites**: Máximo 9.5 horas de áudio por requisição

### Recursos Avançados

#### Streaming
- Respostas incrementais em tempo real
- Ideal para UX fluida com animação de digitação

#### Conversas Multi-turno
- Histórico automático de mensagens
- Contexto preservado entre turnos

#### Structured Output
- Respostas em formato JSON Schema
- Útil para dados estruturados

#### System Instructions
- Guiar comportamento do modelo
- Definir personalidade (ex: "Você é um assistente chamado Dude")

### Limitações Importantes

1. **Taxa de requisições**: 5-15 requisições/minuto na versão gratuita
2. **Tamanho de arquivo**: Máximo 15MB por arquivo de mídia
3. **Tamanho de requisição**: Máximo 20MB total (incluindo texto e arquivos inline)
4. **Duração de áudio**: Máximo 9.5 horas por requisição
5. **Duração de vídeo**: Aproximadamente 1 minuto para inline, sem limite para File API
6. **Imagens por requisição**: Até 10 imagens

### Implementação Recomendada

Para o chatbot Dude:

1. **Backend**: Usar SDK Python ou Node.js do Gemini
2. **Armazenamento**: Usar File API para arquivos reutilizáveis
3. **Streaming**: Implementar streaming para respostas de texto
4. **System Instruction**: Configurar para apresentar-se como "Dude"
5. **Histórico**: Manter histórico local de conversas para contexto
6. **Rate Limiting**: Implementar fila de requisições para respeitar limites

### Modelos de Preço

- **Gratuito**: Acesso limitado, sem cartão de crédito necessário
- **Pago**: Maior número de requisições, acesso a modelos mais avançados
- Recomendação: Começar com Gemini 2.5 Flash na versão gratuita

## Referências

- [Documentação oficial: Text Generation](https://ai.google.dev/gemini-api/docs/text-generation)
- [Documentação oficial: Image Understanding](https://ai.google.dev/gemini-api/docs/image-understanding)
- [Documentação oficial: Video Understanding](https://ai.google.dev/gemini-api/docs/video-understanding)
- [Documentação oficial: Audio Understanding](https://ai.google.dev/gemini-api/docs/audio)
- [Rate Limits](https://ai.google.dev/gemini-api/docs/rate-limits)
- [Pricing](https://ai.google.dev/gemini-api/docs/pricing)

