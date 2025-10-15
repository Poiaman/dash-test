// index.js — Nerdola News (WhatsApp + endpoint HTTP para o Node-RED)
// Execução: node index.js  → vai mostrar o QR no terminal na 1ª vez

import 'dotenv/config';
import express from 'express';
import qrcode from 'qrcode-terminal'; // opcional (printa QR manualmente)
import makeWASocket, {
  useMultiFileAuthState,
  fetchLatestBaileysVersion,
  DisconnectReason,
  Browsers
} from '@whiskeysockets/baileys';

const PORT = process.env.PORT || 3005;
const API_KEY = process.env.API_KEY || 'troque-este-valor';

// ---------- WhatsApp (Baileys) ----------
let sock; // socket global

async function startWhatsApp() {
  // autenticação persistente em pasta separada
  const { state, saveCreds } = await useMultiFileAuthState('./auth_nerdola_news');

  // versão mais recente suportada
  const { version } = await fetchLatestBaileysVersion();

  sock = makeWASocket({
    version,
    auth: state,
    printQRInTerminal: true,              // Baileys já imprime o QR no terminal
    browser: Browsers.ubuntu('Nerdola-News')
  });

  // salva credenciais quando mudarem
  sock.ev.on('creds.update', saveCreds);

  // eventos de conexão (reconecta quando possível)
  sock.ev.on('connection.update', (update) => {
    const { connection, lastDisconnect, qr } = update;

    // redundante (opcional): imprime QR também via qrcode-terminal
    if (qr) {
      qrcode.generate(qr, { small: true });
    }

    if (connection === 'open') {
      console.log('✅ WhatsApp conectado!');
    }

    if (connection === 'close') {
      const statusCode = lastDisconnect?.error?.output?.statusCode;
      const shouldReconnect = statusCode !== DisconnectReason.loggedOut;
      console.log('⚠️ Conexão fechada. statusCode:', statusCode, '→ Reconnect:', shouldReconnect);
      if (shouldReconnect) startWhatsApp().catch(console.error);
      else console.log('❌ Deslogado (loggedOut). Escaneie o QR novamente após reiniciar.');
    }
  });

  // (opcional) log básico de msgs recebidas
  sock.ev.on('messages.upsert', (m) => {
    const msg = m.messages?.[0];
    if (!msg || !msg.message) return;
    const from = msg.key.remoteJid;
    const txt = msg.message?.conversation || msg.message?.extendedTextMessage?.text;
    if (txt) console.log(`📩 de ${from}: ${txt}`);
  });
}

// helper: enviar texto
async function sendWhatsApp(to, text) {
  if (!sock) throw new Error('Socket WhatsApp ainda não inicializado');
  const digits = String(to || '').replace(/\D/g, '');
  if (!digits) throw new Error('Destino inválido. Use algo como 55DDDNÚMERO');
  const jid = `${digits}@s.whatsapp.net`;
  return await sock.sendMessage(jid, { text });
}

// ---------- API HTTP (Express) ----------
const app = express();
app.use(express.json());

// healthcheck
app.get('/', (_req, res) => {
  res.send('Nerdola-News up ✅');
});

// endpoint para o Node-RED chamar
app.post('/api/send', async (req, res) => {
  try {
    const key = req.headers['x-api-key'] || req.body.key;
    if (key !== API_KEY) {
      return res.status(401).json({ ok: false, error: 'unauthorized' });
    }

    const { to, text } = req.body || {};
    if (!to || !text) {
      return res.status(400).json({ ok: false, error: 'missing to/text' });
    }

    await sendWhatsApp(to, text);
    res.json({ ok: true });
  } catch (err) {
    console.error('send error:', err);
    res.status(500).json({ ok: false, error: String(err) });
  }
});

// sobe servidor e conecta no WhatsApp
app.listen(PORT, () => {
  console.log(`🌐 API ouvindo em http://localhost:${PORT}`);
  startWhatsApp().catch((e) => {
    console.error('Falha ao iniciar WhatsApp:', e);
  });
});
