<!doctype html>
<html lang="de">
<head>
  <meta charset="utf-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1" />
  <title>Chatbot Test</title>
  <style>
    body { font-family: system-ui, Arial; background:#f6f7fb; margin:0; }
    .wrap { max-width: 760px; margin: 40px auto; padding: 0 16px; }
    .card { background:#fff; border:1px solid #e7e8ee; border-radius:14px; overflow:hidden; }
    .header { padding:14px 16px; border-bottom:1px solid #eee; font-weight:600; }
    .chat { height: 520px; overflow:auto; padding: 16px; display:flex; flex-direction:column; gap:10px; }
    .row { display:flex; }
    .msg { max-width: 78%; padding: 10px 12px; border-radius: 14px; line-height:1.35; white-space:pre-wrap; }
    .bot { justify-content:flex-start; }
    .bot .msg { background:#f0f2f7; border:1px solid #e5e7f0; border-top-left-radius:6px; }
    .me { justify-content:flex-end; }
    .me .msg { background:#2563eb; color:#fff; border-top-right-radius:6px; }
    .footer { display:flex; gap:10px; padding: 12px; border-top:1px solid #eee; }
    input { flex:1; padding:12px 12px; border-radius: 12px; border:1px solid #d8dbe6; }
    button { padding:12px 14px; border-radius:12px; border:0; background:#111827; color:#fff; cursor:pointer; }
    button:disabled { opacity:.6; cursor:not-allowed; }
    .hint { font-size:12px; color:#6b7280; padding: 10px 16px 14px; }
  </style>
</head>
<body>
  <div class="wrap">
    <div class="card">
      <div class="header">Chatbot – Test</div>
      <div id="chat" class="chat"></div>
      <div class="footer">
        <input id="input" placeholder="Nachricht eingeben…" />
        <button id="send">Senden</button>
      </div>
      <div class="hint">
        Hinweis: Dies ist eine Testumgebung. Inhalte können protokolliert werden.
      </div>
    </div>
  </div>

<script>
  // TODO: eintragen:
  const WEBHOOK_URL = "https://DEIN-N8N-CLOUD/webhook/chat";
  // Optional:
  const API_KEY = ""; // z.B. "test-123"

  const chatEl = document.getElementById("chat");
  const inputEl = document.getElementById("input");
  const sendBtn = document.getElementById("send");

  const state = { sessionId: crypto.randomUUID(), messages: [] };

  function addMsg(role, text) {
    const row = document.createElement("div");
    row.className = "row " + (role === "user" ? "me" : "bot");
    const bubble = document.createElement("div");
    bubble.className = "msg";
    bubble.textContent = text;
    row.appendChild(bubble);
    chatEl.appendChild(row);
    chatEl.scrollTop = chatEl.scrollHeight;
  }

  async function sendMessage() {
    const text = inputEl.value.trim();
    if (!text) return;
    inputEl.value = "";
    addMsg("user", text);
    sendBtn.disabled = true;

    try {
      const res = await fetch(WEBHOOK_URL, {
        method: "POST",
        headers: {
          "Content-Type": "application/json",
          ...(API_KEY ? { "X-API-Key": API_KEY } : {})
        },
        body: JSON.stringify({
          sessionId: state.sessionId,
          message: text
        })
      });

      if (!res.ok) {
        const errText = await res.text();
        addMsg("bot", `Fehler (${res.status}): ${errText}`);
        return;
      }

      const data = await res.json();
      const reply = data.reply ?? data.text ?? JSON.stringify(data);
      addMsg("bot", reply);
    } catch (e) {
      addMsg("bot", "Netzwerkfehler: " + e.message);
    } finally {
      sendBtn.disabled = false;
      inputEl.focus();
    }
  }

  sendBtn.addEventListener("click", sendMessage);
  inputEl.addEventListener("keydown", (e) => {
    if (e.key === "Enter") sendMessage();
  });

  addMsg("bot", "Hallo! Stell mir eine Frage.");
</script>
</body>
</html>
