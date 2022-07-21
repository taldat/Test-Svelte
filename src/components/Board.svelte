<script lang="ts">
  import { onMount } from "svelte";
  import _ from "lodash";
  export let size = 5;
  $: totalCells = Math.pow(size, 2);
  $: playground = Array(totalCells).fill(0);
  $: target = Array(totalCells).fill(0);

  const generateBoard = () => {
    let positions = Array(totalCells).fill(0);
    let maxLength = Math.floor(totalCells / 3);
    let picks = [];

    while (picks.length < maxLength) {
      let randomIndex = Math.round(Math.random() * (totalCells - 1));

      if (!picks.includes(randomIndex)) {
        picks.push(randomIndex);
      }
    }

    picks.forEach((num, index) => {
      if (index === 0) {
        positions[num] = -1;
      } else if (index === maxLength - 1) {
        positions[num] = 1;
      } else {
        positions[num] = 2;
      }
    });

    return positions;
  };

  const move = (index) => {
    let checkPositions = [index + 1, index - 1, index + size, index - size];

    switch (index % size) {
      case 0:
        checkPositions = [index + 1, index + size, index - size];
        break;
      case size - 1:
        checkPositions = [index - 1, index + size, index - size];
        break;
      default:
        break;
    }

    checkPositions.forEach((pos) => {
      if (playground[pos] === -1) {
        playground[pos] = _.cloneDeep(playground[index]);
        playground[index] = -1;
      }
    });
  };

  const compare = () => {
    let isCorrect = true;

    playground.forEach((value, index) => {
      if (value !== target[index]) {
        isCorrect = false;
      }
    });

    if (isCorrect) {
      alert("Correct!");
    } else {
      alert("Wrong!");
    }
  };

  onMount(() => {
    playground = generateBoard();
    target = generateBoard();
  });
</script>

<main>
  <div class="flex">
    <button on:click={compare}>Check</button>
  </div>
  <div class="flex">
    <div>
      <table class="main">
        {#each Array(size) as _, row}
          <tr>
            {#each Array(size) as _, col}
              <td class="main">
                <div
                  class={`box main type-${playground[row * size + col]}`}
                  on:click={() => move(row * size + col)}
                />
              </td>
            {/each}
          </tr>
        {/each}
      </table>
    </div>
    <div>
      <table>
        {#each Array(size) as _, row}
          <tr>
            {#each Array(size) as _, col}
              <td>
                <div
                  class={`box no-box-shadow type-${target[row * size + col]}`}
                />
              </td>
            {/each}
          </tr>
        {/each}
      </table>
    </div>
  </div>
</main>

<style>
  .flex {
    display: flex;
    justify-content: center;
    gap: 50px;
  }

  td {
    border: 1px solid rgb(63, 63, 63);
    padding: 2px;
  }

  .box {
    width: 20px;
    height: 20px;
    border-radius: 5px;
  }

  td.main {
    border: 1px solid rgb(63, 63, 63);
    padding: 12px;
  }

  .box.main {
    width: 100px;
    height: 100px;
    border-radius: 10px;
    cursor: pointer;
  }

  .box.type--1 {
    background-color: transparent;
  }

  .box.type-0 {
    background-color: #404040;
    box-shadow: -3px 3px 6px 0px black;
  }

  .box.type-1 {
    background-color: #ff8080;
    box-shadow: -3px 3px 6px #ffb3b3;
  }

  .box.type-2 {
    background-color: #80b3ff;
    box-shadow: -3px 3px 6px #b3b3ff;
  }

  .box.no-box-shadow {
    box-shadow: unset;
  }
</style>
