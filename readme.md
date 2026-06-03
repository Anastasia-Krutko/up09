<!DOCTYPE html>
<html>
<head>
    <meta charset="utf-8">
    <title>Загрузка изображения</title>

    <style>
        body {
            font-family: Arial, sans-serif;
            background-color: #f4f6f9;
            display: flex;
            justify-content: center;
            margin-top: 50px;
        }

        .container {
            background: white;
            padding: 25px;
            border-radius: 12px;
            box-shadow: 0 0 15px rgba(0,0,0,0.15);
            width: 400px;
            text-align: center;
        }

        h2 {
            color: #333;
        }

        #preview {
            margin-top: 15px;
            max-width: 100%;
            max-height: 250px;
            border-radius: 10px;
            display: none;
            border: 2px solid #ddd;
        }

        button {
            background-color: #4CAF50;
            color: white;
            border: none;
            padding: 10px 20px;
            border-radius: 6px;
            cursor: pointer;
            font-size: 16px;
        }

        button:hover {
            background-color: #45a049;
        }

        progress {
            width: 100%;
            height: 20px;
            margin-top: 15px;
        }

        input[type="file"] {
            margin-top: 10px;
        }

        #status {
            margin-top: 10px;
            color: green;
            font-weight: bold;
        }
    </style>
</head>
<body>

<div class="container">

    <h2>Загрузка изображения</h2>

    <input type="file"
           id="fileInput"
           accept=".jpg,.png,.jpeg">

    <img id="preview">

    <progress id="progressBar"
              value="0"
              max="100">
    </progress>

    <br><br>

    <button onclick="uploadFile()">
        Загрузить
    </button>

    <div id="status"></div>

</div>

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

        if (!file) {
            alert("Выберите файл!");
            return;
        }

        const formData = new FormData();
        formData.append("file", file);

        const xhr = new XMLHttpRequest();

        xhr.upload.addEventListener("progress", function (e) {

            if (e.lengthComputable) {

                let percent =
                    (e.loaded / e.total) * 100;

                document.getElementById("progressBar").value = percent;
            }
        });

        xhr.onload = function () {

            if (xhr.status == 200) {
                document.getElementById("status").innerHTML =
                    "✓ Файл успешно загружен";
            }
        };

        xhr.open("POST", "/api/upload");
        xhr.send(formData);
    }

</script>

</body>
</html>
