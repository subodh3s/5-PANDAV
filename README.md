ai-chatbot/
│── server.js
│── package.json
│── .env
│── public/
│   └── index.html
{
  "name": "ai-chatbot",
  "version": "1.0.0",
  "description": "Simple AI chatbot with Node.js",
  "main": "server.js",
  "scripts": {
    "start": "node server.js"
  },
  "dependencies": {
    "express": "^4.18.2",
    "axios": "^1.6.0",
    "dotenv": "^16.4.0",
    "cors": "^2.8.5"
  }
}
OPENAI_API_KEY=your_api_key_here
PORT=3000
require("dotenv").config();
const express = require("express");
const axios = require("axios");
const cors = require("cors");

const app = express();
app.use(cors());
app.use(express.json());
app.use(express.static("public"));

const API_KEY = process.env.OPENAI_API_KEY;

app.post("/chat", async (req, res) => {
  const userMessage = req.body.message;

  try {
    const response = await axios.post(
      "https://api.openai.com/v1/chat/completions",
      {
        model: "gpt-4o-mini",
        messages: [
          { role: "system", content: "You are a helpful chatbot." },
          { role: "user", content: userMessage }
        ]
      },
      {
        headers: {
          Authorization: `Bearer ${API_KEY}`,
          "Content-Type": "application/json"
        }
      }
    );

    res.json({
      reply: response.data.choices[0].message.content
    });
  } catch (error) {
    console.error(error.response?.data || error.message);
    res.status(500).json({ error: "Error communicating with AI" });
  }
});
<!DOCTYPE html>
<html>
<head>
  <title>AI Chatbot</title>
  <style>
    body { font-family: Arial; padding: 20px; }
    #chat { max-width: 500px; margin: auto; }
    .msg { margin: 10px 0; }
    .user { color: blue; }
    .bot { color: green; }
  </style>
</head>
<body>
  <div id="chat">
    <h2>AI Chatbot</h2>
    <div id="messages"></div>
    <input id="input" placeholder="Type a message..." />
    <button onclick="send()">Send</button>
  </div>

  <script>
    async function send() {
      const input = document.getElementById("input");
      const messages = document.getElementById("messages");

      const userText = input.value;
      if (!userText) return;

      messages.innerHTML += `<div class="msg user">You: ${userText}</div>`;
      input.value = "";

      const res = await fetch("/chat", {
        method: "POST",
        headers: {
          "Content-Type": "application/json"
        },
        body: JSON.stringify({ message: userText })
      });

      const data = await res.json();
      messages.innerHTML += `<div class="msg bot">Bot: ${data.reply}</div>`;
    }
  </script>
</body>
</html>
npm install
npm start

const PORT = process.env.PORT || 3000;
app.listen(PORT, () => console.log(`Server running on port ${PORT}`));
