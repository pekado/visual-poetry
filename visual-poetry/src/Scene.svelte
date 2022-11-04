<script>
  import {AmbientLight, DirectionalLight, Mesh} from '@threlte/core';
  import {AutoColliders, CollisionGroups} from '@threlte/rapier';
  import {BoxBufferGeometry, MeshStandardMaterial} from 'three';
  import Door from './Door.svelte';
  import Player from './Character.svelte';
  import Train from './Train.svelte';
  import Ground from './Ground.svelte';
  import {ContactShadows, Environment} from '@threlte/extras';
</script>

<DirectionalLight shadow position={{y: 20, x: 8, z: -3}} />
<AmbientLight intensity={0.8} />

<CollisionGroups groups={[0, 15]}>
  <AutoColliders shape={'cuboid'} position={{y: -0.5}}>
    <Mesh
      receiveShadow
      geometry={new BoxBufferGeometry(100, 1, 100)}
      material={new MeshStandardMaterial()}
    />
  </AutoColliders>
</CollisionGroups>

<CollisionGroups groups={[0]}>
  <!-- position={{ x: 2 }} -->
  <Player />
  <Door />
  <Train />
  <!-- <Environment /> -->
  <ContactShadows scale={10} blur={2} far={2.5} opacity={0.9} frames={1} />
  <!-- WALLS -->
  <AutoColliders shape={'cuboid'}>
    <Mesh
      receiveShadow
      castShadow
      position={{y: 1.275, x: 30 + 0.7 + 0.15}}
      geometry={new BoxBufferGeometry(60, 2.55, 0.15)}
      material={new MeshStandardMaterial({
        transparent: true,
        opacity: 0.5,
        color: 0x333333,
      })}
    />
    <Mesh
      receiveShadow
      castShadow
      position={{y: 1.275, x: -30 - 0.7 - 0.15}}
      geometry={new BoxBufferGeometry(60, 2.55, 0.15)}
      material={new MeshStandardMaterial({
        transparent: true,
        opacity: 0.5,
        color: 0x333333,
      })}
    />
  </AutoColliders>
</CollisionGroups>
