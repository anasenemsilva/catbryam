<!DOCTYPE html>
<html lang="pt-BR">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>CATAAS API Pro</title>
    <style>
        body { font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif; text-align: center; background-color: #f4f4f9; color: #333; }
        .container { max-width: 600px; margin: 30px auto; background: white; padding: 20px; border-radius: 15px; box-shadow: 0 4px 15px rgba(0,0,0,0.1); }
        img { max-width: 100%; height: 350px; object-fit: cover; border-radius: 10px; border: 3px solid #4CAF50; margin-top: 15px; }
        .controls { margin-top: 20px; display: flex; flex-direction: column; gap: 10px; }
        input, select, button { padding: 10px; border-radius: 5px; border: 1px solid #ddd; }
        button { cursor: pointer; background-color: #4CAF50; color: white; border: none; font-weight: bold; }
        button:hover { background-color: #45a049; }
        .row { display: flex; gap: 10px; justify-content: center; }
    </style>
</head>
<body>

<div class="container">
    <h1>CATAAS PRO</h1>
    
    <div id="imageWrapper">
        <img id="catImage" src="" alt="Clique no botão para carregar um gato">
    </div>

    <div class="controls">
        <div class="row">
            <input type="text" id="textInput" placeholder="Texto no gato (opcional)">
            <input type="text" id="tagInput" placeholder="Tag (ex: cute, gif)">
        </div>
        <div class="row">
            <select id="typeSelect">
                <option value="cat">Imagem</option>
                <option value="cat?type=gif">GIF</option>
                <option value="cat?type=small">Pequeno</option>
            </select>
            <button id="btnFetch">Carregar Gato</button>
        </div>
    </div>
</div>

<script>
    const catImage = document.getElementById('catImage');
    const btnFetch = document.getElementById('btnFetch');
    const textInput = document.getElementById('textInput');
    const tagInput = document.getElementById('tagInput');
    const typeSelect = document.getElementById('typeSelect');

    const BASE_URL = 'https://cataas.com';

    async function fetchCat() {
        catImage.alt = "Carregando...";
        
        let url = `${BASE_URL}/${typeSelect.value}`;
        
        // Adiciona tag se houver
        if (tagInput.value) {
            url = `${BASE_URL}/cat/${tagInput.value}`;
        }
        
        // Adiciona texto se houver (o cataas faz texto via URL)
        if (textInput.value) {
            url += `/says/${encodeURIComponent(textInput.value)}`;
        }

        // Evitar cache (forçar nova imagem)
        url += `?t=${new Date().getTime()}`;

        // Verifica se a resposta é uma imagem real antes de aplicar
        try {
            const response = await fetch(url);
            if (response.ok) {
                catImage.src = url;
            } else {
                throw new Error("Erro na API");
            }
        } catch (error) {
            console.error("Erro ao buscar gato:", error);
            catImage.alt = "Falha ao carregar.";
        }
    }

    // Busca um novo gato ao clicar no botão
    btnFetch.addEventListener('click', fetchCat);

    // Busca o primeiro gato ao carregar a página
    fetchCat();
</script>

</body>
</html>
