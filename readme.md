using Microsoft.AspNetCore.Mvc;

namespace UploadProject.Controllers
{
    [Route("api/upload")]
    [ApiController]
    public class UploadController : ControllerBase
    {
        [HttpPost]
        public async Task<IActionResult> Upload(IFormFile file)
        {
            if (file == null || file.Length == 0)
                return BadRequest("Файл не выбран");

            string folder = Path.Combine(
                Directory.GetCurrentDirectory(),
                "Uploads");

            if (!Directory.Exists(folder))
                Directory.CreateDirectory(folder);

            string filePath = Path.Combine(folder, file.FileName);

            using (var stream = new FileStream(filePath, FileMode.Create))
            {
                await file.CopyToAsync(stream);
            }

            return Ok("Файл загружен");
        }
    }
}
INDEX 
<!DOCTYPE html>
<html>
<head>
    <meta charset="utf-8">
    <title>Загрузка файла</title>
</head>
<body>

    <h2>Загрузка изображения</h2>

    <input type="file" id="fileInput" accept=".jpg,.png">

    <br><br>

    <img id="preview"
         width="200"
         style="display:none;">

    <br><br>

    <progress id="progressBar"
              value="0"
              max="100"
              style="width:300px;">
    </progress>

    <br><br>

    <button onclick="uploadFile()">
        Загрузить
    </button>

    <script>

        const fileInput = document.getElementById("fileInput");
        const preview = document.getElementById("preview");

        fileInput.addEventListener("change", function () {

            const file = this.files[0];

            if (file) {
                preview.src = URL.createObjectURL(file);
                preview.style.display = "block";
            }
        });

        function uploadFile() {

            const file = fileInput.files[0];

            const formData = new FormData();
            formData.append("file", file);

            const xhr = new XMLHttpRequest();

            xhr.upload.addEventListener("progress", function (e) {

                if (e.lengthComputable) {

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
