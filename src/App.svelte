<script lang="ts">
  import { AceEditor } from "svelte-ace";
  // import "brace/mode/javascript";
  // import "brace/mode/html";
  import "brace/mode/css";
  import "brace/theme/dracula";
  // const test:string = "brace/mode/json"
  //import `${test}`;
  import codeEditorLangList from './assets/AceEditorLanguages.json'
  let text = ""
  const codeLangList = codeEditorLangList
  const codeLangDefault = "css"
  let codeLangSelected = codeLangDefault

  function asigneObjToText (obj:any) {
    text = obj
  }
  async function onChangeSelect (e:any) {
    const name = e.target.value
    await importDynamic(name)
    console.log("importDynamic returned")
    codeLangSelected = name
  }
  async function importDynamic (name: string) {
    const path = '../node_modules/brace/mode/'+ name + '.js'
    console.log("--path: ",path)
    await import(`${path}`)
    // const test = await import(`${path}`)
    // console.log(test)
    console.log("dynamicaly imported")
    return 
  }

</script>

<main>
  <select name="codeEditorLangList" id="codeEditorLangList" on:change={onChangeSelect}>
    {#each codeLangList as name, i}
      {#if name == codeLangDefault}
        <option value="{name}" selected>{name}</option>
      {:else}
        <option value="{name}">{name}</option>
      {/if}
	  {/each}
  </select>
  {#key codeLangSelected}
    <p>{codeLangSelected}</p>
    <AceEditor
    on:selectionChange={(obj) => console.log(obj.detail)}
    on:paste={(obj) => console.log(obj.detail)}
    on:input={(obj) => asigneObjToText(obj.detail)}
    
    on:focus={() => console.log('focus')}
    on:documentChange={(obj) => console.log(`document change : ${obj.detail}`)}
    on:cut={() => console.log('cut')}
    on:cursorChange={() => console.log('cursor change')}
    on:copy={() => console.log('copy')}
    on:init={(editor) => console.log(editor.detail)}
    on:commandKey={(obj) => console.log(obj.detail)}
    on:changeMode={(obj) => console.log(`change mode : ${obj.detail}`)}
    on:blur={(obj) => console.log(`document change : ${obj.detail}`)}
    width='100%'
    height='300px'
    lang={codeLangSelected}
    theme="dracula"
    value={text} />
  {/key}
</main>

<style>
 
</style>
