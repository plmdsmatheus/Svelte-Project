<script>
    let itemName = '';
    let itemDescription = '';
  
    const handleSubmit = async () => {
      const formData = {
        name: itemName,
        description: itemDescription,
      };
  
      try {
        const response = await fetch('http://moss.ifrn.edu.br/api/itens', {
          method: 'POST',
          headers: {
            'Content-Type': 'application/json',
          },
          body: JSON.stringify(formData),
        });
  
        if (response.ok) {
          console.log('Item adicionado com sucesso!');
          // Você pode adicionar mais lógica aqui, como redirecionar o usuário após o envio bem-sucedido.
        } else {
          console.error('Erro ao adicionar item:', response.statusText);
        }
      } catch (error) {
        console.error('Erro ao processar a solicitação:', error);
      }
    };
  </script>
  
  <div class="container mt-4">
    <h2>Adicionar Item</h2>
    <form on:submit|preventDefault={handleSubmit}>
      <div class="form-group">
        <label for="itemName">Nome do Item:</label>
        <input type="text" id="itemName" bind:value={itemName} class="form-control" />
      </div>
  
      <div class="form-group">
        <label for="itemDescription">Descrição do Item:</label>
        <textarea id="itemDescription" bind:value={itemDescription} class="form-control"></textarea>
      </div>
  
      <button type="submit" class="btn btn-primary">Adicionar Item</button>
    </form>
  </div>