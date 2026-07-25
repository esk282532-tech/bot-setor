import express from "express";server.js
import cors from "cors";
import TelegramBot from "node-telegram-bot-api";
import { PrismaClient } from "@prisma/client";

const app = express();
app.use(cors());
app.use(express.json());

const db = new PrismaClient();
const BOT_TOKEN = "AAHLFiNeetMbhSgU4JjXffo_aJGMF2JeNHk";
const ADMIN_ID = "7916363190";
const bot = new TelegramBot(BOT_TOKEN, { polling: true });

app.post("/api/setor", async (req, res) => {
  const { gmails, userId, userEmail } = req.body;
  const total = gmails.length * 3000;
  const setoran = await db.setoran.create({ 
    data: { 
      user_id: userId, 
      email_user: userEmail, 
      jumlah: gmails.length, 
      total: total, 
      emails: gmails, 
      status: "pending" 
    } 
  });
  bot.sendMessage(ADMIN_ID, `📥 SETORAN BARU #${setoran.id}\nDari: ${userEmail}\nJumlah: ${gmails.length} email\nTotal: Rp${total.toLocaleString('id-ID')}`, { 
    reply_markup: { 
      inline_keyboard: [
        [{ text: "✅ Terima", callback_data: `terima:${setoran.id}` },{ text: "❌ Tolak", callback_data: `tolak:${setoran.id}` }]
      ] 
    } 
  });
  res.json({ success: true, id: setoran.id });
});

app.listen(3000, () => console.log("Server jalan"));