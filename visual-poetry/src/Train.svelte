<script>
  import {useGltf, HTML} from '@threlte/extras';
  import {
    RigidBody,
    AutoColliders,
    Collider,
    CollisionGroups,
  } from '@threlte/rapier';
  import {createEventDispatcher} from 'svelte';
  import {Mesh} from '@threlte/core';
  import {derived} from 'svelte/store';
  const dispatch = createEventDispatcher();
  let grounded = false;
  let objectsInSensor = false;
  $: objectsInSensor && console.log(objectsInSensor);
  $: grounded ? dispatch('groundenter') : dispatch('groundexit');

  const {gltf} = useGltf('/mav_m41/scene.gltf');
  const train = derived(gltf, (gltf) => {
    if (!gltf || !gltf.nodes['m413_(2)objcleanermaterialmergergles']) return;
    console.log(gltf);
    return gltf.nodes['m413_(2)objcleanermaterialmergergles'].children;
  });
</script>

{#if $train}
  <RigidBody
    type={'fixed'}
    position={{x: 10, y: 0, z: 12}}
    rotation={{
      x: 5,
    }}
  >
    <CollisionGroups groups={[0]}>
      <AutoColliders
        shape={'convexHull'}
        on:collisionenter={() => (objectsInSensor = true)}
        on:collisionexit={() => (objectsInSensor = false)}
      >
        {#each $train as mesh}
          <Mesh castShadow geometry={mesh.geometry} material={mesh.material} />
        {/each}
      </AutoColliders>
    </CollisionGroups>
  </RigidBody>
{/if}
{#if objectsInSensor}
  <HTML position={{x: 10, y: 0, z: 12}} transform>
    <button
      on:click={() => console.log('html')}
      class="bg-brand rounded-full px-3 text-white hover:opacity-90 active:opacity-70"
    >
      Este elemento representa el viaje, el desapego y el destierro
    </button>
  </HTML>
{/if}
