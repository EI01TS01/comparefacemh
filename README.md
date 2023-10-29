HTML:
```html
<!DOCTYPE html>
<html>
<head>
  <title>Comparação de Linguagens do GitHub</title>
  <link rel="stylesheet" href="style.css">
</head>
<body>
  <h1>Comparação de Linguagens do GitHub</h1>
  <div>
    <label for="language">Selecione uma linguagem:</label>
    <select id="language">
      <option value="javascript">JavaScript</option>
      <option value="python">Python</option>
      <option value="java">Java</option>
      <!-- adicione outras opções de linguagem aqui -->
    </select>
    <button onclick="buscarRepositorios()">Buscar</button>
  </div>
  <div id="resultado"></div>
  
  <script src="script.js"></script>
</body>
</html>
```

CSS (style.css):
```css
body {
  font-family: Arial, sans-serif;
  margin: 50px;
}

h1 {
  text-align: center;
}

div {
  margin-bottom: 10px;
}

select {
  margin-right: 10px;
}

#resultado {
  border: 1px solid #ccc;
  padding: 10px;
}
```

JavaScript (script.js):
```javascript
function buscarRepositorios() {
  var language = document.getElementById("language").value;
  var url = "https://api.github.com/search/repositories?q=language:" + language;

  fetch(url)
    .then(response => response.json())
    .then(data => mostrarRepositorios(data.items))
    .catch(error => console.log(error));
}

function mostrarRepositorios(repositorios) {
  var resultadoDiv = document.getElementById("resultado");
  resultadoDiv.innerHTML = "";

  if (repositorios.length === 0) {
    resultadoDiv.innerHTML = "Nenhum repositório encontrado.";
    return;
  }

  var lista = document.createElement("ul");
  resultadoDiv.appendChild(lista);

  repositorios.forEach(function (repositorio) {
    var itemLista = document.createElement("li");
    var link = document.createElement("a");
    link.href = repositorio.html_url;
    link.textContent = repositorio.full_name;
    itemLista.appendChild(link);
    lista.appendChild(itemLista);
  });
}
```

