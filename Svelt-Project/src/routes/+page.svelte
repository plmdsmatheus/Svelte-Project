<script>
  import { onMount } from 'svelte';

  let themes = [];
  let itens = []; // Lista de itens disponíveis na API
  let newTheme = { name: '', color: '', price:0, itens: []};


    const fetchItems = async () => {
    try {
      const response = await fetch('http://moss.ifrn.edu.br/api/itens/');
      if (response.ok) {
        itens = await response.json();
      } else {
        console.error('Erro ao obter a lista de itens da API');
      }
    } catch (error) {
      console.error('Erro ao realizar a requisição:', error);
    }
  };

  onMount(async () => {
    try {
      // Faz a requisição para a API
      const response = await fetch('http://moss.ifrn.edu.br/api/themes/');
      
      // Verifica se a requisição foi bem-sucedida (status code 200)
      if (response.ok) {
        // Converte a resposta para JSON
        themes = await response.json();
      } else {
        console.error('Erro ao obter os temas da API');
      }
    } catch (error) {
      console.error('Erro ao realizar a requisição:', error);
    }
  });

  const handleFormSubmit = async () => {
  try {
    const response = await fetch('http://moss.ifrn.edu.br/api/themes/', {
      method: 'POST',
      headers: {
        'Content-Type': 'application/json',
      },
      body: JSON.stringify(newTheme),
    });

    if (response.ok) {
      // Atualize a lista de temas após o POST bem-sucedido
      const updatedThemes = await response.json();
      themes = updatedThemes;
      // Limpe os campos do formulário após o POST bem-sucedido
      newTheme = { name: '', color: '', price: 0, itens: [] };
    } else {
      console.error('Erro ao criar um novo tema na API');
    }
  } catch (error) {
    console.error('Erro ao realizar a requisição:', error);
  }
};

</script>

<main class="container mt-5">
  <h1 class="mb-4">Lista de Temas</h1>
  {#if themes.length > 0}
    <ul class="list-group">
      {#each themes as theme (theme.id)}
        <li class="list-group-item">{theme.name}</li>
      {/each}
    </ul>
  {:else}
    <p>Carregando temas...</p>
  {/if}

    <!-- Formulário para criar um novo tema -->
  <form on:submit|preventDefault={handleFormSubmit} class="mt-4">
    <!-- Campos do formulário ... -->
    <div class="form-group">
    <label for="name">Nome do Tema:</label>
    <input type="text" class="form-control" id="name" bind:value={newTheme.name} required>
    </div>

    <div class="form-group">
      <label for="color">Cor do Tema:</label>
      <input type="text" class="form-control" id="color" bind:value={newTheme.color} required>
    </div>

    <div class="form-group">
      <label for="price">Preço do Tema:</label>
      <input type="number" class="form-control" id="price" bind:value={newTheme.price} required>
    </div>

    <!-- Seleção de itens disponíveis -->
    <div class="form-group">
      <label for="items">Itens do Tema:</label>
      <select class="form-control" id="items" bind:value={newTheme.itens} multiple>
        {#each itens as item (item.id)}
          <option value={item.id}>{item.name}</option>
        {/each}
      </select>
    </div>

    <button type="submit" class="btn btn-primary">Criar Tema</button>
  </form>
</main>

<style>
  /* Adicione estilos conforme necessário */
</style>