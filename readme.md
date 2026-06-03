Если ты делаешь это в Visual Studio 2022 (ASP.NET Core Web API), вот самый простой способ, который точно должен работать.

1. Создай проект

1. Открой Visual Studio.
2. Create a new project.
3. Выбери ASP.NET Core Web API.
4. Название проекта:

FileUploadApp

5. Нажми Create.

⸻

2. Создай папку Uploads

В обозревателе решений:

ПКМ по проекту → Add → New Folder

Название:

Uploads

⸻

3. Создай контроллер

В папке Controllers создай файл:

UploadController.cs

Вставь код:

using Microsoft.AspNetCore.Mvc;
namespace FileUploadApp.Controllers
{
    [ApiController]
    [Route("api/upload")]
    public class UploadController : ControllerBase
    {
        [HttpPost]
        public async Task<IActionResult> Upload(IFormFile file)
        {
            if (file == null)
                return BadRequest("Файл не выбран");
            string uploadsFolder =
                Path.Combine(
                    Directory.GetCurrentDirectory(),
                    "Uploads");
            string filePath =
                Path.Combine(
                    uploadsFolder,
                    file.FileName);
            using (var stream =
                new FileStream(filePath, FileMode.Create))
            {
                await file.CopyToAsync(stream);
            }
            return Ok("Файл загружен");
        }
    }
}

⸻

4. Настрой Program.cs

Открой файл:

Program.cs

Замени содержимое на:

var builder = WebApplication.CreateBuilder(args);
builder.Services.AddControllers();
var app = builder.Build();
app.UseDefaultFiles();
app.UseStaticFiles();
app.MapControllers();
app.Run();

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

Вставь:

<!DOCTYPE html>
<html>
<head>
    <meta charset="utf-8">
    <title>Загрузка файла</title>
</head>
<body>
    <h2>Загрузка изображения</h2>
    <input type="file"
           id="fileInput"
           accept=".jpg,.png">
    <br><br>
    <img id="preview"
         width="250"
         style="display:none;">
    <br><br>
    <progress id="progressBar"
              value="0"
              max="100">
    </progress>
    <br><br>
    <button onclick="uploadFile()">
        Загрузить
    </button>
    <script>
        const fileInput =
            document.getElementById("fileInput");
        const preview =
            document.getElementById("preview");
        fileInput.addEventListener("change", function ()
        {
            const file = this.files[0];
            if (file)
            {
                preview.src =
                    URL.createObjectURL(file);
                preview.style.display =
                    "block";
            }
        });
        function uploadFile()
        {
            const file =
                fileInput.files[0];
            const formData =
                new FormData();
            formData.append("file", file);
            const xhr =
                new XMLHttpRequest();
            xhr.upload.addEventListener(
                "progress",
                function (e)
                {
                    if (e.lengthComputable)
                    {
                        let percent =
                            (e.loaded / e.total) * 100;
                        document.getElementById(
                            "progressBar"
                        ).value = percent;
                    }
                });
            xhr.open(
                "POST",
                "/api/upload"
            );
            xhr.send(formData);
        }
    </script>
</body>
</html>

⸻

7. Запусти проект

Нажми:

Ctrl + F5

или кнопку:

▶ Start

Откроется страница:

https://localhost:xxxx

⸻

8. Проверка работы

1. Нажми Выбрать файл.
2. Выбери JPG или PNG.
3. Появится превью изображения.
4. Нажми Загрузить.
5. Индикатор загрузки начнет заполняться.
6. После завершения файл появится в папке:

FileUploadApp
│
├─ Uploads
│   └─ фото.jpg
│
├─ Controllers
├─ wwwroot
└─ Program.cs

⸻

Что сказать преподавателю

Backend:

* Реализирован метод POST /api/upload.
* Файл принимается через IFormFile.
* Изображение сохраняется в папку Uploads.

Frontend:

* Выбор изображения через <input type="file">.
* Предварительный просмотр через <img>.
* Загрузка через FormData.
* Индикатор прогресса реализован элементом <progress>.
