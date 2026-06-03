<!DOCTYPE html>
<html>
<head>
    <meta charset="utf-8">
    <title>Генератор чисел</title>
</head>
<body>

    <h2>Случайное число</h2>

    <div id="number">0</div>

    <br>

    <button onclick="getNumber()">
        Get Number
    </button>

    <script>

        async function getNumber()
        {
            try
            {
                const response =
                    await fetch('/api/random');

                const number =
                    await response.text();

                document.getElementById("number").innerHTML =
                    number;
            }
            catch(error)
            {
                alert(error);
            }
        }

    </script>

</body>
</html>
