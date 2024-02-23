<script lang="ts">
    import {
        Button, Dropdown,
        TextInput,
    } from "carbon-components-svelte";
    import OpenAI from "openai";
    import { token } from "$lib/store";

    let selectedModel = "dall-e-3";

    let openai: OpenAI;

    let imageURL: string|null;

    token.subscribe(value => {
        openai = new OpenAI({apiKey: value, dangerouslyAllowBrowser: true});
    });

    function generateImage() {
        openai.images.generate({
            model: selectedModel,
            prompt: (document.getElementById('promptInput') as HTMLInputElement).value,
        })
            .then(image => {
            imageURL = image.data[0].url!;
            console.log(imageURL);
        })
            .catch(error => {
                document.querySelector('p')!.textContent = error.message;
            });
    }
</script>

<style lang="scss">
    div#inputRow {
        display: flex;
        flex-direction: row;
        justify-content: space-between;
        align-items: center;
        padding: 1rem;
        flex-grow: 2;
    }
</style>

<div id="inputRow">
    <TextInput placeholder="Your image prompt" size="xl" id="promptInput"></TextInput>
    <Button on:click={() => { generateImage(); }}>Send</Button>
    <Dropdown size="xl" selectedId="dall-e-3" id="modelDropdown" items={[
                { id: "dall-e-3", text: "Dall-E 3" },
                { id: "dall-e-2", text: "Dall-E 2" }
            ]} on:select={event => {
                selectedModel = event.detail.selectedId;
            }} />
</div>

<img alt="The AI-Generated image" src={imageURL} />
<p></p>