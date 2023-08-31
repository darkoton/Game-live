<template>
  <div class="root" ref="root">
    <ThePanel
      @run="run"
      @runStep="runStep"
      @stop="pause = true"
      @clean="clean"
    />
  </div>
</template>

<script setup>
import { ref, onMounted } from "vue";
import ThePanel from "@/components/ThePanel.vue";
const root = ref(null);

const world = document.createElement("div");
let style;
let pause = true;
let step = false;

function simulate() {
  let born = [];
  let dead = [];
  let ghosts = [];
  let alive = document.querySelectorAll("span");
  alive.forEach((cell) => {
    let neighbors = checkNeigbor(cell.dataset);
    if (neighbors < 2 || neighbors > 3) {
      dead.push(cell);
    }
    neighborsPositions(cell.dataset).forEach((el) => {
      if (
        el[0] >= 1 &&
        el[0] <= (innerWidth / 10).toFixed() &&
        el[0] >= 1 &&
        el[1] <= (innerHeight / 10).toFixed() &&
        !document.querySelector(
          `span[data-col='${el[0]}'][data-row='${el[1]}']`
        ) &&
        !ghosts.filter((c) => c.dataset.col == el[0] && c.dataset.row == el[1])
          .length
      ) {
        let ghost = document.createElement("span");
        ghost.dataset.col = el[0];
        ghost.dataset.row = el[1];
        ghosts.push(ghost);
        if (checkNeigbor(ghost.dataset) == 3) {
          born.push(ghost);
        }
      }
    });
  });
  dead.forEach((cell) => {
    cell.remove();
  });
  born.forEach((cell) => {
    cell.style.gridColumn = `${cell.dataset.col} / ${+cell.dataset.col + 1}`;
    cell.style.gridRow = `${cell.dataset.row} / ${+cell.dataset.row + 1}`;
    cell.style.background = "#000";
    cell.style.border = "1px solid #ccc";
    cell.style.pointerEvents = "none";
    world.appendChild(cell);
  });
  if (!step && !pause) {
    setTimeout(simulate, 100);
  } else {
    step = false;
  }
}

function neighborsPositions({ col, row }) {
  col = +col;
  row = +row;
  return [
    [col - 1, row - 1],
    [col, row - 1],
    [col + 1, row - 1],
    [col - 1, row],
    [col + 1, row],
    [col - 1, row + 1],
    [col, row + 1],
    [col + 1, row + 1],
  ];
}
function checkNeigbor({ col, row }) {
  let neighbors = neighborsPositions({ col, row });
  return neighbors.reduce((result, position) => {
    if (
      document.querySelector(
        `span[data-col='${position[0]}'][data-row='${position[1]}']`
      )
    ) {
      return (result += 1);
    }
    return result;
  }, 0);
}
function spawnCell(col, row) {
  if (!document.querySelector(`span[data-col='${col}'][data-row='${row}']`)) {
    let cell = document.createElement("span");
    cell.dataset.col = col;
    cell.dataset.row = row;
    cell.style.gridColumn = `${col} / ${col + 1}`;
    cell.style.gridRow = `${row} / ${row + 1}`;
    cell.style.background = "#000";
    cell.style.border = "1px solid #ccc";
    cell.style.pointerEvents = "none";
    cell.classList.add("alive");
    world.appendChild(cell);
  }
}
function setStyle() {
  for (const param in style) {
    world.style[param] = style[param];
  }
  world.style.gridTemplateRows = "repeat(auto-fit, 1fr)";
}

function run() {
  if (pause) {
    pause = false;
    simulate();
  }
}

function runStep() {
  if (pause) {
    step = true;
    simulate();
    return;
  }

  pause = true;
}

function clean() {
  pause = true;

  document.querySelectorAll("span").forEach((el) => el.remove());
}

onMounted(() => {
  world.classList.add("world");
  root.value.appendChild(world);
  style = {
    display: "grid",
    gridTemplateRows: `repeat(${(innerHeight / 10).toFixed()}, 10px)`,
    gridTemplateColumns: `repeat(${(innerWidth / 10).toFixed()}, 10px)`,
    width: "100%",
    height: "100%",
  };
  setStyle();

  // document.addEventListener("keyup", (event) => {
  //   if (event.code == "Space") {
  //     if (!start) {
  //       simulate();
  //       start = true;
  //       return;
  //     }
  //     pause = !pause;
  //   }
  // });

  let mouseDown = false;
  world.addEventListener("mousedown", (event) => {
    let col = Math.ceil(event.offsetX / 10);
    let row = Math.ceil(event.offsetY / 10);
    spawnCell(col, row);
    mouseDown = true;
  });
  world.addEventListener("mousemove", (event) => {
    if (mouseDown) {
      let col = Math.ceil(event.offsetX / 10);
      let row = Math.ceil(event.offsetY / 10);
      spawnCell(col, row);
    }
  });
  world.addEventListener("mouseup", () => {
    mouseDown = false;
  });
});
</script>

<style>
html,
body,
* {
  padding: 0;
  margin: 0;
}

html,
body {
  height: 100%;
  width: 100%;
}
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin: 0;
  padding: 0;
}
</style>
