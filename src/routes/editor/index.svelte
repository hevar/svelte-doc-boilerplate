<script>
    import marked from 'marked';
    import {afterUpdate} from 'svelte';
    import Icon from "../../components/Icon.svelte";

    let textpos;
    let value = `
# H1 heading
## H2 heading
### H3 heading
---
**bold text**
*italicized text*
---
1. First item
2. Second item
3. Third item
- First item
- Second item
- Third item

> First indent
>> Second indent

[Svelte](https://svelte.dev/)
`;

    let fileName;


    function handleSave() {

        fileName = `${fileName}.md`;

        const file = new Blob([value], {type: "text/plain"});
        if (window.navigator.msSaveOrOpenBlob) // IE10+
            window.navigator.msSaveOrOpenBlob(file, fileName);
        else { // Others
            let a = document.createElement("a");
            let url = URL.createObjectURL(file);
            a.href = url;
            a.download = fileName;
            document.body.appendChild(a);
            a.click();
            setTimeout(function() {
                document.body.removeChild(a);
                window.URL.revokeObjectURL(url);
            }, 0);
        }
    }

</script>



<style>
    .container{
        background: #d0d0d0;
        padding: 20px;
    }
    .markdown-editor {
        width: 100%;
        display: flex;
        align-items:flex-start;
        justify-content: space-evenly;
    }
    .left-panel, .right-panel {
        width: 50%;
        height: 85vh;
        background: #ffffff;
    }
    .right-panel {
        overflow: auto;
    }
    .source {
        border: none;
        width: 100%;
        height: 100%;
        background: #2d2d2d;
        font-family: "Courier New", Courier, monospace;
        color: #ffd449;
        font-size: 12px;
    }
    .source:focus {
        outline: none;
    }
    .output {
        width: 100%;
        padding: 0 2em;
    }

    .float{
        left: 46%;
        position: fixed;
        width: 60px;
        height: 60px;
        bottom: 55px;
        background-color: #0C9;
        color: #FFF;
        border-radius: 50px;
        text-align: center;
        box-shadow: 2px 2px 3px #252525;
    }

    .float:hover {
        background-color: #23a786

    }

    .input-field {
        padding: 10px 20px;
        border: 1px solid #999;
        border-radius: 3px;
        display: block;
        width: 20%;
        box-sizing: border-box;
        outline: none;
        margin-left: 10px;
    }

    .input-wrapper {
        display: flex;
        align-items: center;
        margin: 10px 0;
    }

</style>

<main class="container">
    <div class="input-wrapper">
        <span>File name</span>
        <input class="input-field" bind:value={fileName} placeholder="04-file-name" />
    </div>
    <div class="markdown-editor">
        <div class="left-panel">
            <textarea class="source" bind:this={textpos} bind:value={value}></textarea>
        </div>

        <button class="float" on:click={handleSave}>
            <Icon name="save" />
        </button>

        <div class="right-panel">
            <div class="output">{@html marked(value)}</div>
        </div>


    </div>
</main>
