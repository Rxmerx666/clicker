# clicker

<!DOCTYPE html>
<html lang="ru">
<head>
    <meta charset="UTF-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no"/>
    <title>Telegram Кликер</title>
    <style>
        body {
            margin: 0;
            padding: 0;
            font-family: Arial, sans-serif;
            background-color: #f5f6fa;
            display: flex;
            flex-direction: column;
            align-items: center;
            justify-content: center;
            height: 100vh;
            box-sizing: border-box;
        }

        h1 {
            font-size: 20px;
            margin-bottom: 10px;
            text-align: center;
        }

        button {
            padding: 20px 40px;
            font-size: 24px;
            border: none;
            border-radius: 12px;
            background-color: #007AFF;
            color: white;
            cursor: pointer;
            transition: background-color 0.3s ease;
        }

        button:hover {
            background-color: #005ECF;
        }

        p {
            margin-top: 20px;
            font-size: 18px;
        }

        .container {
            text-align: center;
            padding: 20px;
        }
    </style>
</head>
<body>
    <div class="container">
        <h1>Привет, <span id="username">пользователь</span>!</h1>
        <button onclick="clickButton()">Клик!</button>
        <p>Всего кликов: <span id="count">0</span></p>
    </div>

    <script src="https://unpkg.com/ @tma.js/sdk@beta"></script>
    <script src="script.js"></script>
</body>
</html>

let count = 0;

window.onload = () => {
    const savedCount = localStorage.getItem("clickCount");
    if (savedCount !== null) {
        count = parseInt(savedCount);
        document.getElementById("count").innerText = count;
    }

    if (window.Telegram && Telegram.WebApp) {
        Telegram.WebApp.ready();
        Telegram.WebApp.setHeaderTitle("Кликер");
        Telegram.WebApp.expand();

        Telegram.WebApp.BackButton.show();
        Telegram.WebApp.BackButton.onClick(() => {
            Telegram.WebApp.close();
        });
    }

    const { init } = window['@tma.js/sdk'];
    try {
        const user = init();
        if (user && user.username) {
            document.getElementById("username").innerText = user.username;
        }
    } catch (e) {
        console.error("Ошибка инициализации TMA SDK", e);
    }
};

function clickButton() {
    count++;
    document.getElementById("count").innerText = count;
    localStorage.setItem("clickCount", count);
    if (navigator.vibrate) {
        navigator.vibrate(20);
    }
}
