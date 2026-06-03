<!DOCTYPE html>
<html>
<head>
    <meta charset="utf-8">
    <title>Генератор случайных чисел</title>

    <style>

        body {
            font-family: Arial, sans-serif;
            background: #f4f6f9;
            display: flex;
            justify-content: center;
            align-items: center;
            height: 100vh;
            margin: 0;
        }

        .container {
            background: white;
            padding: 30px;
            width: 350px;
            text-align: center;
            border-radius: 15px;
            box-shadow: 0 0 15px rgba(0,0,0,0.15);
        }

        h2 {
            color: #333;
            margin-bottom: 20px;
        }

        #number {
            font-size: 70px;
            font-weight: bold;
            color: #4CAF50;
            margin: 25px 0;
            opacity: 0;
            transition: opacity 0.5s;
        }

        .show {
            opacity: 1;
        }

        button {
            background-color: #4CAF50;
            color: white;
            border: none;
            padding: 12px 25px;
            font-size: 18px;
            border-radius: 8px;
            cursor: pointer;
            transition: 0.3s;
        }

        button:hover {
            transform: scale(1.05);
        }

    </style>
</head>
<body>

<div class="container">

    <h2>🎲 Генератор случайных чисел</h2>

    <div id="number">0</div>

    <button onclick="getNumber()">
        Get Number
    </button>

</div>

<script>

async function getNumber()
{
    try
    {
        const response = await fetch('/api/random');

        const number = await response.text();

        const div = document.getElementById("number");

        div.classList.remove("show");

        setTimeout(() =>
        {
            div.innerHTML = number;
            div.classList.add("show");
        }, 100);
    }
    catch
    {
        alert("Ошибка получения числа");
    }
}

</script>

</body>
</html>
