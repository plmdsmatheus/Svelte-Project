<script>
    import { onMount } from 'svelte';
  
    let themes = [];
  
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
  </script>
  
  <main>
    <h1>Lista de Temas</h1>
    {#if themes.length > 0}
      <ul>
        {#each themes as theme (theme.id)}
          <li>{theme.name}</li>
        {/each}
      </ul>
    {:else}
      <p>Carregando temas...</p>
    {/if}
  </main>
  
  <style>
    
  </style>