Вот самый простой вариант для ASP.NET Core Web API (.NET 9).

1. Создай проект

Create a new project → ASP.NET Core Web API

Название:

StringLengthApp

⸻

2. Program.cs

Полностью замени содержимое на:

var builder = WebApplication.CreateBuilder(args);
builder.Services.AddControllers();
var app = builder.Build();
app.UseDefaultFiles();
app.UseStaticFiles();
app.MapControllers();
app.Run();

⸻

3. Создай модель

В проекте создай папку:

Models

В ней файл:

TextRequest.cs

Код:

namespace StringLengthApp.Models
{
    public class TextRequest
    {
        public string Text { get; set; }
    }
}

⸻

4. Создай контроллер

В папке Controllers создай файл:

StringLengthController.cs

Код:

using Microsoft.AspNetCore.Mvc;
using StringLengthApp.Models;
namespace StringLengthApp.Controllers
{
    [ApiController]
    [Route("api/string-length")]
    public class StringLengthController : ControllerBase
    {
        [HttpPost]
        public IActionResult GetLength([FromBody] TextRequest request)
        {
            int length = request.Text.Length;
            return Ok(new
            {
                length = length
            });
        }
    }
}

⸻

Как работает Backend

Получает:

{
  "text": "hello"
}

⸻

Считает:

hello = 5 символов

⸻

Возвращает:

{
  "length": 5
}

⸻

5. Создай папку wwwroot

ПКМ по проекту →

Add → New Folder

Название:

wwwroot

⸻

6. Создай страницу

В папке wwwroot

Создай файл:

index.html

Вставь код:

<!DOCTYPE html>
<html>
<head>
    <meta charset="utf-8">
    <title>Калькулятор длины строки</title>
    <style>
        body {
            font-family: Arial;
            background: #f4f6f9;
            display: flex;
            justify-content: center;
            align-items: center;
            height: 100vh;
        }
        .card {
            background: white;
            padding: 30px;
            width: 400px;
            border-radius: 10px;
            box-shadow: 0 0 10px gray;
            text-align: center;
        }
        input {
            width: 90%;
            padding: 10px;
            font-size: 18px;
        }
        #result {
            margin-top: 20px;
            font-size: 30px;
            color: green;
            font-weight: bold;
        }
    </style>
</head>
<body>
<div class="card">
    <h2>Калькулятор длины строки</h2>
    <input
        type="text"
        id="textInput"
        placeholder="Введите текст">
    <div id="result">
        0
    </div>
</div>
<script>
const input =
    document.getElementById("textInput");
input.addEventListener("input", getLength);
async function getLength()
{
    const text = input.value;
    const response =
        await fetch("/api/string-length",
        {
            method: "POST",
            headers:
            {
                "Content-Type": "application/json"
            },
            body: JSON.stringify(
            {
                text: text
            })
        });
    const data =
        await response.json();
    document.getElementById("result")
        .innerHTML =
        data.length;
}
</script>
</body>
</html>

⸻

Проверка работы

Запусти проект:

Ctrl + F5

⸻

Вводишь:

hello

Сайт автоматически отправляет:

{
  "text": "hello"
}

⸻

Сервер отвечает:

{
  "length": 5
}

⸻

На экране появится:

5

⸻

Если введёшь:

Привет

то покажет:

6

⸻

Что сказать преподавателю

Backend:

* Создан API POST /api/string-length.
* Текст принимается в формате JSON.
* Сервер вычисляет количество символов и возвращает результат.

Frontend:

* Используется текстовое поле.
* Событие input срабатывает при каждом вводе символа.
* Через fetch отправляется POST-запрос на сервер.
* Полученная длина строки автоматически отображается на странице без перезагрузки.
