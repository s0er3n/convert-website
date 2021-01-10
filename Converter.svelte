<script>
  import { Circle } from "svelte-loading-spinners";
  import {
    Button,
    Container,
    ListGroup,
    Input,
    ListGroupItem
  } from "sveltestrap";
  let url;
  $: url;
  let files = [];
  let output;
  let filetypes = ["csv", "exel", "mat"];
  let convert = false;
  let input;
  let loading;
  function handleSubmit(e) {
    loading = true;
    e.preventDefault();
    input = files[0].name.split(".").pop();
    console.log(files, input, output);
    var formdata = new FormData();
    formdata.append("file", files[0], files[0].name);

    var requestOptions = {
      method: "POST",
      body: formdata,
      redirect: "follow"
    };

    fetch("https://a51d44cffb61.ngrok.io/convert/mat/csv", requestOptions)
      .then(response => response.blob())
      .then(result => {
        url = window.URL.createObjectURL(result);
        loading = false;
      })
      .catch(error => {
        console.log("error", error);
        loading = false;
        alert(error);
      });
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
{#if loading}
<Circle size="60" color="#FFFFFF" unit="px"></Circle>
{/if}
{#if url}
<a href={url}><Button on:click={e => e.preventDefault()} color="success">Download</Button></a>
{/if}
</form>
</div>