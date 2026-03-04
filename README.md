# 🎰 Roulette WebSocket Server

WebSocket server para o **Roulette Analyzer** - Transmite dados de roletas em tempo real com integração à API Fly.io.

## 🚀 Deploy no Railway

Este servidor está configurado para rodar no [Railway.app](https://railway.app).

### ⚙️ Variáveis de Ambiente Obrigatórias

Configure no Railway Dashboard > Variables:

```env
FLY_API_URL=https://roulette-history-api.fly.dev
PORT=3000
```

**Opcional (para cache persistente):**
```env
SUPABASE_URL=https://snrzuqjuvqkisrrgbhmg.supabase.co
SUPABASE_SERVICE_ROLE_KEY=<sua-service-role-key>
```

### Configuração

O servidor usa múltiplas fontes de dados:
- **API WebSocket Primária**: `ws://177.93.108.140:8777` (dados em tempo real)
- **API Histórico Fly.io**: `https://roulette-history-api.fly.dev` (histórico completo até 500 números)
- **Supabase**: Cache persistente (opcional)
- **Porta**: Definida pela variável `PORT` do Railway

### Recursos

- ✅ Conexão com API real de 60+ roletas (tempo real)
- ✅ Integração com API Fly.io para histórico completo (até 500 números)
- ✅ Cache persistente no Supabase
- ✅ Transmissão de dados em tempo real
- ✅ Detecção automática de novas roletas
- ✅ Broadcast para múltiplos clientes
- ✅ Fallback inteligente: WebSocket → Fly.io API → Supabase

### Endpoints

- **WebSocket**: `wss://seu-app.railway.app`
- **Health Check**: Disponível na porta configurada

## 📡 Mensagens Suportadas

### Cliente → Servidor

```json
// Obter lista de roletas
{ "type": "get_roulettes" }

// Se inscrever em uma roleta
{ "type": "subscribe", "roulette": "Speed Roulette", "limit": 500 }

// Requisitar histórico
{ "type": "request_history", "limit": 500 }

// Ping (heartbeat)
{ "type": "ping" }
```

### Servidor → Cliente

```json
// Lista de roletas disponíveis
{ "type": "roulettes", "data": ["Speed Roulette", "Lightning Roulette", ...] }

// Histórico de números
{ "type": "history", "data": [5, 13, 34, 22, ...] }

// Novo número (tempo real)
{ "type": "result", "roulette": "Speed Roulette", "number": 17, "timestamp": 1234567890 }

// Confirmação de conexão
{ "type": "connected", "timestamp": 1234567890 }

// Pong (resposta ao ping)
{ "type": "pong", "timestamp": 1234567890 }
```

## 🔧 Desenvolvimento Local

```bash
npm install
npm start
```

Servidor iniciará em `ws://localhost:3000`

## 📊 Logs

O servidor emite logs detalhados:
- `🎰` Servidor iniciado
- `✅` Conectado à API
- `🎲` Novo número detectado
- `📤` Dados enviados ao cliente
- `🔌` Cliente conectado/desconectado

## ⚠️ Importante

- Não simula dados - apenas retransmite da API real
- Mantém histórico de até 500 números por roleta
- Suporta múltiplos clientes simultâneos
- Reconexão automática à API em caso de queda
