<script lang="ts">
    import {
        TextInput,
        Button,
        Dropdown,
    } from "carbon-components-svelte";
    import Message from "$lib/Message.svelte";
    import OpenAI from "openai";
    import { token } from "$lib/store";

    let openai: OpenAI;

    let selectedModel = "gpt-3.5-turbo";

    let messageHistory = [{
        role: "system",
        content: "You are a helpful assistant."
    }];

    token.subscribe(value => {
        openai = new OpenAI({apiKey: value, dangerouslyAllowBrowser: true});
    });



    async function sendMessage() {
        const messageInput = document.getElementById("messageInput") as HTMLInputElement;
        const message = messageInput.value;

        if (!(message.length > 0)) return;

        messageInput.value = "";

        new Message({
            target: document.getElementById("messages") as HTMLElement,
            props: {
                author: "You",
                message: message,
            }
        });

        openai.chat.completions.create({
            model: selectedModel,
            messages: [
                ...messageHistory as OpenAI.Chat.Completions.ChatCompletionMessageParam,
                {
                    role: "user",
                    content: message
                }
            ]
        })
            .then(completion => {
                completion = completion.choices[0].message.content;

                new Message({
                    target: document.getElementById("messages") as HTMLElement,
                    props: {
                        author: "Jibbidy",
                        message: completion,
                    }
                });

                messageHistory.push({
                    role: "user",
                    content: message,
                });

                messageHistory.push({
                    role: "assistant",
                    content: completion,
                });
            })
            .catch((error) => {
                console.error(error);
                new Message({
                    target: document.getElementById("messages") as HTMLElement,
                    props: {
                        author: "Jibbidy",
                        message: error.message,
                    }
                });

                return;
            });
    }
</script>

<style lang="scss">
    div#chatWindow {
        height: 100%;
        width: 100%;
        background-color: #f4f4f4;
        padding: 1rem;
        display: flex;
        flex-direction: column;
        justify-content: flex-end;

        div#inputRow {
            display: flex;
            flex-direction: row;
            justify-content: space-between;
            align-items: center;
            padding: 1rem;
            flex-grow: 2;
        }

        div#messages {
            display: flex;
            flex-direction: column;
            justify-content: flex-end;
            align-items: flex-start;
            padding: 1rem;
            //overflow-y: auto;
            height: 100%;
            flex-grow: 1;
            overflow-y: scroll;
        }
    }
</style>

<div id="chatWindow">
    <div id="inputRow">
        <TextInput placeholder="Your message" size="xl" id="messageInput"></TextInput>
        <Button on:click={() => { sendMessage(); }}>Send</Button>
            <Dropdown size="xl" selectedId="gpt-3.5-turbo" id="modelDropdown" items={[
                { id: "gpt-3.5-turbo", text: "GPT 3.5 turbo" },
                { id: "gpt-4", text: "GPT 4" }
            ]} on:select={event => {
                selectedModel = event.detail.selectedId;
            }} />
    </div>
    <div id="messages">
    </div>
</div>