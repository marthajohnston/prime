<svelte:options immutable tag='v-dropdown' />

<script lang='ts'>

import cx from 'classnames';
import { addStyles } from '../lib/index';
import { dispatcher } from '../lib/dispatch';
import { htmlToBoolean } from '../lib/boolean';

export let open = 'false';
export let match = 'false';

const dispatch = dispatcher();

addStyles();

let isMatch: boolean;
let isOpen: boolean;

$: isMatch = htmlToBoolean(match, 'match');
$: isOpen = htmlToBoolean(open, 'open');


const toggleDropdown = () => {
  dispatch('toggle', { open: !isOpen });
};

</script>

<div class='relative inline-block w-full'>
  <div
    class='inline-block w-full'
    on:click={toggleDropdown}
    on:keyup|stopPropagation|preventDefault={toggleDropdown}
  >
    <slot name='target'/>
  </div>
  <div 
    class={
      cx('absolute z-40', {
        'left-0': isMatch,
        'right-0': isMatch,
        'overflow-hidden': isMatch,
        invisible: !isOpen,
      }
    )}
  >
    <slot name='content' />
  </div>
</div>
