<!DOCTYPE html>
<html lang="tr">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Kaju Chatbot</title>
    <style>
        body {
            font-family: Arial, sans-serif;
            text-align: center;
            background-color: #f4f4f4;
        }
        #chatContainer {
            width: 350px;
            height: 500px;
            border: 2px solid #333;
            border-radius: 10px;
            background: white;
            overflow-y: scroll;
            padding: 15px;
            margin: auto;
            box-shadow: 2px 2px 10px rgba(0, 0, 0, 0.2);
            position: relative;
        }
        #typingIndicator {
            display: none;
            font-style: italic;
            color: gray;
        }
        #userInput {
            width: 75%;
            padding: 8px;
            border-radius: 5px;
            border: 1px solid #ccc;
            margin-top: 10px;
        }
        button {
            padding: 8px 15px;
            border: none;
            border-radius: 5px;
            background-color: #007bff;
            color: white;
            cursor: pointer;
            margin-top: 5px;
        }
        button:hover {
            background-color: #0056b3;
        }
        .message {
            text-align: left;
            padding: 5px;
        }
        .user-message {
            text-align: right;
            color: blue;
        }
        .bot-message {
            color: green;
        }
        .logo {
            width: 100px;
            margin-bottom: 10px;
        }
        .avatar {
            width: 30px;
            height: 30px;
            border-radius: 50%;
            margin-right: 5px;
        }
        .user-container, .bot-container {
            display: flex;
            align-items: center;
            margin: 5px 0;
        }
        .dark-mode {
            background-color: #222;
            color: white;
        }
    </style>
</head>
<body>
    <h2>Kaju Chatbot 🤖</h2>
    <img src="https://lh6.googleusercontent.com/9oMxiETAXGa1TCIyNXmmuG5P1gN8XUo4P5WFKd8WNBrEzEG4t9SHmkBluLm9xKwWn3duqww_O_IJKITBZ8Eb4eM=w16383" alt="Kaju Logo" class="logo">
    <button onclick="toggleDarkMode()">🌙/☀️ Tema Değiştir</button>
    <div id="chatContainer">
        <p class="bot-message"><strong>Kaju:</strong> Merhaba! Ben Kaju. Adını öğrenebilir miyim? 😊</p>
        <p id="typingIndicator">Kaju yazıyor...</p>
    </div>
    <input type="text" id="userInput" placeholder="Mesajınızı yazın..." onkeypress="if(event.key === 'Enter') sendMessage()">
    <button onclick="sendMessage()">Gönder</button>
    <button onclick="startVoiceRecognition()">🎤 Sesle Yaz</button>

    <script>
        let userName = "";
        function sendMessage() {
            let userInput = document.getElementById("userInput").value;
            let chatContainer = document.getElementById("chatContainer");
            let typingIndicator = document.getElementById("typingIndicator");
            if (userInput.trim() === "") return;

            let response = "Bunu anlayamadım 🤔";
            if (userName === "") {
                userName = userInput;
                response = `Memnun oldum, ${userName}! Sana nasıl yardımcı olabilirim?`;
            } else {
                let lowerInput = userInput.toLowerCase();
                if (lowerInput.includes("merhaba")) {
                    response = `Merhaba ${userName}! 😊`;
                } else if (lowerInput.includes("nasılsın")) {
                    response = "Harikayım, sen nasılsın? 😎";
                } else if (lowerInput.includes("fenerbahçe")) {
                    response = "Yaşa Fenerbahçe! 💛💙";
                } else if (lowerInput.includes("teşekkür")) {
                    response = "Rica ederim! 😊";
                } else if (lowerInput.includes("kaju")) {
                    response = "Uslu durmazsan tırmalarım! 🐾";
                } else if (lowerInput.includes("sahibin kim")) {
                    response = "Benim sahibim Luwia7 yani Aslı! 👑";
                } else if (lowerInput.includes("ne yapıyorsun")) {
                    response = "Kedilerle dövüş antrenmanı yapıyorum! 🥋🐱";
                }
            }

            chatContainer.innerHTML += `<div class='user-container'><img src='https://cdn-icons-png.flaticon.com/512/847/847969.png' class='avatar'><p class='user-message'><strong>Sen:</strong> ${userInput}</p></div>`;
            document.getElementById("userInput").value = "";
            chatContainer.scrollTop = chatContainer.scrollHeight;
            
            typingIndicator.style.display = "block";
            setTimeout(() => {
                typingIndicator.style.display = "none";
                chatContainer.innerHTML += `<div class='bot-container'><img src='https://cdn-icons-png.flaticon.com/512/4712/4712037.png' class='avatar'><p class='bot-message'><strong>Kaju:</strong> ${response}</p></div>`;
                chatContainer.scrollTop = chatContainer.scrollHeight;
                speak(response);
            }, 1000);
        }

        function speak(text) {
            let speech = new SpeechSynthesisUtterance(text);
            speech.lang = "tr-TR";
            window.speechSynthesis.speak(speech);
        }

        function startVoiceRecognition() {
            let recognition = new (window.SpeechRecognition || window.webkitSpeechRecognition)();
            recognition.lang = "tr-TR";
            recognition.start();
            recognition.onresult = function(event) {
                document.getElementById("userInput").value = event.results[0][0].transcript;
                sendMessage();
            };
        }
        
        function toggleDarkMode() {
            document.body.classList.toggle("dark-mode");
        }
    </script>
</body>
</html>
