import express from "express";
import cors from "cors";
import { PrismaClient } from "@prisma/client";
import TelegramBot from "node-telegram-bot-api";

const app = express();
app.use(cors());
app.use(express.json());
const db = new PrismaClient();

const BOT_TOKEN = "TOKEN_KAMU_DISINI";
const ADMIN_ID = "7916363190";
const bot = new TelegramBot(BOT_TOKEN, { polling: true });

app.post("/api/setor", async (req,