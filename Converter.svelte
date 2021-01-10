<script>
  import {
    Button,
    Container,
    ListGroup,
    Input,
    ListGroupItem
  } from "sveltestrap";
  let files = [];
  let output;
  let filetypes = ["csv", "exel", "mat"];
  let convert = false;
  let input;
  function handleSubmit(e) {
    e.preventDefault();
    input = files[0].name.split(".").pop();
    console.log(files, input, output);
    var myHeaders = new Headers();
    myHeaders.append("accept", "application/json");
    myHeaders.append("Content-Type", "multipart/form-data");

    var formdata = new FormData();
    formdata.append("file", files[0], files[0].name);

    var requestOptions = {
      method: "POST",
      headers: myHeaders,
      body: formdata,
      redirect: "follow"
    };

    fetch("http://localhost:8000/convert/mat/csv", requestOptions)
      .then(response => response.text())
      .then(result => console.log(result))
      .catch(error => console.log("error", error));
  }
  function handleChoice(e, filetype) {
    e.preventDefault();
    output = filetype;
    convert = true;
  }
</script>

<style>
  @import "https://stackpath.bootstrapcdn.com/bootstrap/4.5.2/css/bootstrap.min.css";
  .Zentrum {
    display: flex;
    justify-content: center;
    align-items: center; /* for single line flex container */
    margin: 50px;
  }
</style>
<div class="Zentrum">
<form on:submit={e => handleSubmit(e)}>
<div class="custom-file">
  <input type="file" class="custom-file-input" id="customFile" bind:files>
  <label class="custom-file-label" for="customFile">Choose file</label>
</div>
<div class="test">
<ListGroup class="m-2">
{#each files as file}
<ListGroupItem>{file.name}</ListGroupItem>
{/each}
</ListGroup>
<div>
{#if files.length > 0}
{#each filetypes as filetype}
<Button class="m-2" on:click={e =>handleChoice(e, filetype)}>{filetype}</Button>
{/each}
{/if}
{#if convert}
<Button color="primary"type="submit">Convert</Button>
{/if}
</form>
</div>