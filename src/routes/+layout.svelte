<script lang="ts">
    import {
        Header,
        HeaderNav,
        Content,
        PasswordInput,
        SideNav,
        SkipToContent,
        SideNavItems,
        SideNavLink,
    } from "carbon-components-svelte";
    import "carbon-components-svelte/css/all.css";
    import { onMount } from "svelte";
    import "../global.scss";
    import { token } from "$lib/store";
    import { get } from "svelte/store";
    import { Chat, Image } from "carbon-icons-svelte";

    let theme = "g10"; // "white" | "g10" | "g80" | "g90" | "g100"
    onMount(() => {
        document.documentElement.setAttribute("theme", theme);
    });

    function setToken() {
        let tokenInput = document.getElementById("tokenInput") as HTMLInputElement;

        token.set(tokenInput.value);
    }

    let isSideNavOpen = false;
</script>

<Header platformName="BetterJibbidy" bind:isSideNavOpen>
    <svelte:fragment slot="skip-to-content">
        <SkipToContent />
    </svelte:fragment>
    <HeaderNav>
        <PasswordInput placeholder="Enter OpenAI API token..." id="tokenInput" value={get(token)} on:change={() => setToken()} />
    </HeaderNav>
</Header>

<SideNav bind:isOpen={isSideNavOpen} rail>
    <SideNavItems>
        <SideNavLink icon={Chat} href="/" text="Chat" />
        <SideNavLink icon={Image} href="/image" text="Image" />
    </SideNavItems>
</SideNav>


<Content style="padding: 0">
    <slot></slot>
</Content>