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
