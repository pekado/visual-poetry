<script>
  import {useTweakpane} from './useTweakpane';
  import {Canvas} from '@threlte/core';
  import {Debug, World} from '@threlte/rapier';
  import Scene from './Scene.svelte';
  const {pane, action} = useTweakpane();
  pane.addBlade({
    view: 'text',
    text: "Use the 'wasd' keys to move around",
    lineCount: 3,
  });
</script>

<div use:action />

<Canvas>
  <World>
    <Debug />
    <Scene />
  </World>
</Canvas>
