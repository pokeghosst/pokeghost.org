<script lang="ts">
    // All code from here, copyright BattlefieldDuck https://github.com/BattlefieldDuck/xterm-svelte
    import {onMount} from "svelte";

    import '@xterm/xterm/css/xterm.css';
    import type {XtermProps} from "./XtermProps.ts";

    let parent = $state<HTMLElement>();

    let {
        terminal = $bindable(),
        options,
        onBell,
        onBinary,
        onCursorMove,
        onData,
        onKey,
        onLineFeed,
        onRender,
        onWriteParsed,
        onResize,
        onScroll,
        onSelectionChange,
        onTitleChange,
        onLoad,
        ...rest
    }: XtermProps = $props();

    onMount(async () => {
        const { Terminal } = await import('@xterm/xterm');

        terminal = new Terminal(options);
        terminal.onBell(() => onBell?.());
        terminal.onBinary((data) => onBinary?.(data));
        terminal.onCursorMove(() => onCursorMove?.());
        terminal.onData((data) => onData?.(data));
        terminal.onKey((data) => onKey?.(data));
        terminal.onLineFeed(() => onLineFeed?.());
        terminal.onRender((data) => onRender?.(data));
        terminal.onWriteParsed(() => onWriteParsed?.());
        terminal.onResize((data) => onResize?.(data));
        terminal.onScroll((data) => onScroll?.(data));
        terminal.onSelectionChange(() => onSelectionChange?.());
        terminal.onTitleChange((data) => onTitleChange?.(data));

        if (parent) {
            terminal.open(parent);
            onLoad?.(terminal);
        } else {
            console.error('[xterm-svelte] Parent element not found');
        }
    });
</script>

<div bind:this={parent} {...rest}></div>