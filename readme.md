Вот самый простой вариант для Visual Studio 2022 + ASP.NET Core Web API (.NET 9).

1. Создай проект

1. Create a new project
2. Выбери ASP.NET Core Web API
3. Название:

RandomNumberApp

4. Framework:

.NET 9.0

5. Нажми Create

⸻

2. Настрой Program.cs

Открой Program.cs и вставь:

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

RandomController.cs

Код:

using Microsoft.AspNetCore.Mvc;
namespace RandomNumberApp.Controllers
{
    [ApiController]
    [Route("api/random")]
    public class RandomController : ControllerBase
    {
        [HttpGet]
        public IActionResult GetNumber()
        {
            Random random = new Random();
            int number = random.Next(1, 101);
            return Ok(number);
        }
    }
}

⸻

Объяснение

Адрес API

[Route("api/random")]

Создает адрес:

/api/random

⸻

GET-запрос

[HttpGet]

Разрешает получать данные через GET.

⸻

Генерация числа

Random random = new Random();

Создает генератор случайных чисел.

⸻

int number = random.Next(1, 101);

Генерирует число:

от 1 до 100

⸻

Ответ клиенту

return Ok(number);

Отправляет число на сайт.

⸻

4. Создай папку wwwroot

ПКМ по проекту →

Add → New Folder

Название:

wwwroot

⸻

5. Создай страницу

В папке wwwroot

Создай файл:

index.html

Вставь код:

<!DOCTYPE html>
<html>
<head>
    <meta charset="utf-8">
    <title>Генератор чисел</title>
    <style>
        body {
            font-family: Arial;
            background: #f4f4f4;
            text-align: center;
            margin-top: 100px;
        }
        .box {
            background: white;
            width: 300px;
            margin: auto;
            padding: 30px;
            border-radius: 10px;
            box-shadow: 0 0 10px gray;
        }
        #number {
            font-size: 60px;
            margin: 20px;
            opacity: 0;
            transition: opacity 0.5s;
        }
        .show {
            opacity: 1;
        }
        button {
            padding: 10px 20px;
            font-size: 18px;
            cursor: pointer;
        }
    </style>
</head>
<body>
<div class="box">
    <h2>Случайное число</h2>
    <div id="number">0</div>
    <button onclick="getNumber()">
        Get Number
    </button>
</div>
<script>
async function getNumber()
{
    const response =
        await fetch('/api/random');
    const number =
        await response.text();
    const div =
        document.getElementById("number");
    div.classList.remove("show");
    setTimeout(function ()
    {
        div.innerHTML = number;
        div.classList.add("show");
    }, 100);
}
</script>
</body>
</html>

⸻

6. Запуск

Нажми:

Ctrl + F5

или

▶ Start

⸻

Как работает сайт

При открытии

Показывается:

0

⸻

При нажатии

Get Number

вызывается функция:

getNumber()

⸻

Запрос на сервер

fetch('/api/random')

отправляет запрос:

GET /api/random

⸻

Сервер

генерирует число:

1 - 100

и отправляет его обратно.

⸻

Анимация

Сначала число исчезает:

div.classList.remove("show");

Затем появляется новое:

div.classList.add("show");

За счёт CSS:

transition: opacity 0.5s;

получается плавное появление.

⸻

Что сказать преподавателю

Backend:

* Создан API-контроллер RandomController.
* Метод GET /api/random генерирует случайное число от 1 до 100.
* Результат возвращается клиенту через Ok().

Frontend:

* Кнопка Get Number отправляет GET-запрос через fetch.
* Полученное число выводится на экран.
* Для визуального эффекта используется CSS-анимация появления через изменение прозрачности (opacity).
