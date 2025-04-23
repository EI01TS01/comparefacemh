// Estrutura do Frontend (React + Tailwind + JavaScript) // Arquivo: src/App.jsx

import React, { useState } from 'react'; import './App.css';

const App = () => { const [message, setMessage] = useState(''); const [chatResponse, setChatResponse] = useState('');

const handleChat = async () => { const response = await fetch('http://localhost:3001/chat', { method: 'POST', headers: { 'Content-Type': 'application/json' }, body: JSON.stringify({ message }) }); const data = await response.json(); setChatResponse(data.response); };

return ( <div className='App'> <header className='App-header'> <h1>BasketHub</h1> </header> <main className='chat-box'> <input type='text' value={message} onChange={(e) => setMessage(e.target.value)} placeholder='Digite sua pergunta sobre basquete...' /> <button onClick={handleChat}>Enviar</button> <p>Resposta da IA: {chatResponse}</p> </main> </div> ); };

export default App;

// Arquivo: src/App.css

.App { text-align: center; font-family: 'Arial', sans-serif; }

.App-header { background-color: #ff6f00; padding: 20px; color: white; }

.chat-box { margin: 20px auto; padding: 20px; width: 300px; background-color: #f7f7f7; border-radius: 10px; }

.chat-box input { width: 80%; padding: 10px; border-radius: 5px; }

.chat-box button { padding: 10px 20px; background-color: #ff6f00; border: none; color: white; cursor: pointer; }

.chat-box button:hover { background-color: #e65c00; }

// Estrutura do Backend (Node.js + Express + Supabase + OpenAI) // Arquivo: server/index.js

import express from 'express'; import cors from 'cors'; import { createClient } from '@supabase/supabase-js'; import { config } from 'dotenv'; import OpenAI from 'openai';

config();

const app = express(); app.use(cors()); app.use(express.json());

const supabase = createClient( process.env.SUPABASE_URL, process.env.SUPABASE_ANON_KEY );

const openai = new OpenAI({ apiKey: process.env.OPENAI_KEY });

// Rota de exemplo: autenticação com Supabase app.post('/signup', async (req, res) => { const { email, password } = req.body; const { data, error } = await supabase.auth.signUp({ email, password }); if (error) return res.status(400).json({ error }); res.json({ data }); });

app.post('/login', async (req, res) => { const { email, password } = req.body; const { data, error } = await supabase.auth.signInWithPassword({ email, password }); if (error) return res.status(400).json({ error }); res.json({ data }); });

// Chat IA app.post('/chat', async (req, res) => { const { message } = req.body; try { const completion = await openai.chat.completions.create({ model: 'gpt-3.5-turbo', messages: [ { role: 'system', content: 'Você é um especialista em basquete. Responda de forma clara, precisa e envolvente.' }, { role: 'user', content: message } ] }); res.json({ response: completion.choices[0].message.content }); } catch (err) { res.status(500).json({ error: 'Erro ao se comunicar com a IA.' }); } });

const PORT = process.env.PORT || 3001; app.listen(PORT, () => { console.log(Servidor rodando na porta ${PORT}); });

// Exemplo de .env // SUPABASE_URL=https://pmrwituyadinexqcpzbo.supabase.co // SUPABASE_ANON_KEY=eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9... // OPENAI_KEY=sk-proj-Tp3ulHgJmfzTh8m4aoGv__vWNOyjgiykvZmnX...

