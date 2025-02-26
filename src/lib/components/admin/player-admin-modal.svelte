<script type="ts">
   import PlayerLink from '$lib/components/player/player-link.svelte';
   import TextInput from '$lib/components/common/text-input.svelte';
   const { close } = getContext('simple-modal');
   import type { Player, Role } from '$lib/models/PlayerData';
   import { getContext } from 'svelte';
   import { getPermissionList } from '$lib/utils/helpers';
   export let onBanClick: (player: Player, reason: string) => void;
   export let onUnbanClick: (player: Player) => void;
   export let onGiveRoleClick: (player: Player, role: string) => void;
   export let onRemoveRoleClick: (player: Player, role: string) => void;
   export let player: Player;

   let currentRoles: Role[] = getPermissionList(player.permissions);

   $: banReason = '';
   $: givenRole = 'rtr';
   $: roleToRemove = '';
</script>

<div class="media">
   <div class="media-content is-clipped">
      <div class="player-info mb-2">
         <span class="text-muted">Administrating</span>
         <img src={player.profilePicture} alt={player.name} title={player.name} class="image rounded is-24x24" />
         <b>
            <PlayerLink {player} destination={`/u/${player.id}`} />
         </b>
      </div>
      <div class="tools">
         {#if player.banned}
            <div class="window tool mt-3">
               <div class="title is-6 mb-3">Unban User</div>
               <button
                  on:click={() => {
                     onUnbanClick(player);
                     close();
                  }}
                  class="button is-success is-small "
               >
                  <span>Unban</span>
               </button>
            </div>
         {:else}
            <div class="window tool mt-3">
               <div class="title is-6 mb-3">Ban User</div>
               <TextInput bind:value={banReason} isSmall={true} placeholder="Ban Reason" />
               <button
                  on:click={() => {
                     onBanClick(player, banReason);
                     close();
                  }}
                  class="button is-danger is-small mt-1"
               >
                  <span>Ban</span>
               </button>
            </div>
         {/if}

         <div class="window tool mt-3">
            <div class="title is-6 mb-3">Give Role</div>
            <div class="role-container">
               <div class="select is-small">
                  <select bind:value={givenRole} id="roles">
                     <option value="rtr">RT Recruit</option>
                     <option value="rt">RT</option>
                     <option value="qat">QAT</option>
                     <option value="cat">CAT</option>
                     <option value="nat">NAT</option>
                     <option value="qathead">QAT Head</option>
                  </select>
               </div>
               <button
                  on:click={() => {
                     onGiveRoleClick(player, givenRole);
                     close();
                  }}
                  class="button is-small is-success mt-1"
               >
                  <span>Give Role</span>
               </button>
            </div>
         </div>
         <div class="window tool mt-3">
            <div class="title is-6 mb-3">Remove Role</div>
            <div class="role-container">
               <div class="select is-small">
                  <select bind:value={roleToRemove} id="roles">
                     {#each currentRoles as role}
                        <option value={role}>{role}</option>
                     {/each}
                  </select>
               </div>
               <button
                  on:click={() => {
                     onRemoveRoleClick(player, roleToRemove);
                     close();
                  }}
                  class="button is-small is-danger mt-1"
               >
                  <span>Remove Role</span>
               </button>
            </div>
         </div>
         <div class="window mt-3 fill">
            <div class="title is-6 mb-3">More Soon...</div>
            <span>Sorry Pug</span>
         </div>
      </div>
   </div>
</div>

<style>
   .window {
      background-color: var(--gray) !important;
   }
   .window.tool {
      margin-left: 5px;
      margin-right: 5px;
   }
   .media-content {
      color: white;
   }

   .tools {
      display: flex;
   }

   .role-container {
      display: flex;
      flex-direction: column;
   }

   .media {
      padding: 1rem;
      position: relative;
      z-index: 1;
   }
   .player-info {
      display: flex;
      align-items: center;
      gap: 0.4rem;
   }
   .text-muted {
      color: var(--muted);
   }

   @media only screen and (max-width: 769px) {
      .tools {
         flex-direction: column;
      }
   }
</style>
