Вот самый простой рабочий вариант для ASP.NET Core Web API (.NET 9).

1. Создай проект

1. Create a new project
2. Выбери ASP.NET Core Web API
3. Название проекта:

GreetingApp

4. Framework:

.NET 9.0

5. Нажми Create

⸻

2. Настрой Program.cs

Открой файл Program.cs и замени содержимое:

var builder = WebApplication.CreateBuilder(args);
builder.Services.AddControllers();
var app = builder.Build();
app.UseDefaultFiles();
app.UseStaticFiles();
app.MapControllers();
app.Run();

⸻

3. Создай контроллер

В папке Controllers создай файл:

GreetController.cs

Код:

using Microsoft.AspNetCore.Mvc;
namespace GreetingApp.Controllers
{
    [ApiController]
    [Route("api/greet")]
    public class GreetController : ControllerBase
    {
        [HttpGet("{name}")]
        public IActionResult GetGreeting(string name)
        {
            return Ok($"Hello, {name}!");
        }
    }
}

⸻

Как работает Backend

Если открыть:

/api/greet/Anastasia

сервер вернёт:

Hello, Anastasia!

⸻

4. Создай папку wwwroot

ПКМ по проекту →

Add → New Folder

Название:

wwwroot

⸻

5. Создай страницу

В папке wwwroot создай файл:

index.html

Вставь код:

<!DOCTYPE html>
<html>
<head>
    <meta charset="utf-8">
    <title>Приветствие</title>
    <style>
        body {
            font-family: Arial, sans-serif;
            background: #f4f6f9;
            display: flex;
            justify-content: center;
            align-items: center;
            height: 100vh;
        }
        .card {
            background: white;
            width: 400px;
            padding: 30px;
            text-align: center;
            border-radius: 12px;
            box-shadow: 0 0 12px rgba(0,0,0,0.15);
        }
        input {
            width: 90%;
            padding: 10px;
            font-size: 16px;
            margin-bottom: 15px;
        }
        button {
            padding: 10px 20px;
            background: #4CAF50;
            color: white;
            border: none;
            border-radius: 6px;
            cursor: pointer;
        }
        button:hover {
            transform: scale(1.05);
        }
        #result {
            margin-top: 20px;
            font-size: 24px;
            color: green;
            font-weight: bold;
        }
    </style>
</head>
<body>
<div class="card">
    <h2>Компонент "Приветствие"</h2>
    <input
        type="text"
        id="nameInput"
        placeholder="Введите имя">
    <br>
    <button onclick="getGreeting()">
        Получить приветствие
    </button>
    <div id="result"></div>
</div>
<script>
async function getGreeting()
{
    const name =
        document.getElementById("nameInput").value;
    if(name === "")
    {
        alert("Введите имя");
        return;
    }
    const response =
        await fetch("/api/greet/" + name);
    const text =
        await response.text();
    document.getElementById("result").innerHTML =
        text;
}
</script>
</body>
</html>

⸻

Проверка работы

Запусти проект:

Ctrl + F5

⸻

В поле введи:

Anastasia

Нажми кнопку:

Получить приветствие

На экране появится:

Hello, Anastasia!

⸻

Что происходит

Пользователь вводит имя

Например:

Ivan

⸻

JavaScript отправляет запрос

fetch("/api/greet/" + name)

Получается:

/api/greet/Ivan

⸻

Контроллер получает имя

public IActionResult GetGreeting(string name)

Значение:

Ivan

⸻

Сервер формирует ответ

return Ok($"Hello, {name}!");

Получается:

Hello, Ivan!

⸻

Ответ отображается на странице

document.getElementById("result").innerHTML = text;

⸻

Что сказать преподавателю

Backend:

* Реализован GET endpoint /api/greet/{name}.
* Имя передаётся через URL.
* Сервер возвращает строку приветствия в формате Hello, {name}!.

Frontend:

* Есть поле ввода имени и кнопка.
* При нажатии через fetch() отправляется GET-запрос.
* Ответ от сервера отображается на странице без перезагрузки.
