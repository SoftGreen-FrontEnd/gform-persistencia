<script>
    let past;
        async function fetchResponse() {
		    const response = await fetch("http://localhost:8080/form/inputs.jsp");
		    const data = await response.json();
            console.log(data.datasets);
            return data.datasets;
	    }
        let getData = fetchResponse();

    const postValue = (async (term) => {
		const response = await fetch(`http://localhost:8080/form/retorno.jsp?term=${term}`)
		const retorno = await response.json();
		console.log(retorno);
		document.getElementById(retorno.id).value = retorno.value;
	})
</script>

<form>
{#await getData}
	Loading...
{:then data}
    {#each data as { label, type, value, id, field, key }}
    <label for={label} class="form-label">{label}</label>
    <div class="input-group mb-3">
        <input type="text" class="form-control" id={id} bind:value={value} on:focus={() => past = value} on:blur={() => {
            if(past.localeCompare(value) != 0){
                postValue(1);
                console.log([{"key": key}, {"value": value},{"field": field}, {"id": id}]);
            }
        }}>
      </div>
    {/each}
{:catch someError}
	System error: {someError.message}.
{/await}

</form>
