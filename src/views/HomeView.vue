<template>
  <main>
    <div class="container">
      <canvas id="canvas1" width="500" height="531"></canvas>
    </div>
    <button @click="activate">Activate</button>
    <button @click="stop">Stop</button>
  </main>
</template>

<script setup>
//Imports
import * as rive from "@rive-app/canvas";
import { ref, onMounted, useTemplateRef } from "vue";

const stageNum = ref(1);


async function loadRiveFile(src) {
  const file = new rive.RiveFile({
    src: src,
    onLoad: () => {
      console.log("File Loaded");
    },
    onLoadError: (error) => {
      console.error("Error loading file: " + error);
    },
  });
  await file.init();
  return file;
}

function playCube() {
  activeBool.value = true;
}

let cubeFile;
let canvas;
const layout = new rive.Layout({
  fit: rive.Fit.Contain,
  alignment: rive.Alignment.TopCenter,
});

//Main Codes
function activate() {
  activeBoolController.value = true;
}

function stop() {
  activeBoolController.value = false;
}

let activeBoolController;
let activeBoolState = ref(false);

onMounted(async () => {
  cubeFile = await loadRiveFile("cubetransition.riv");
  canvas = document.getElementById("canvas1");
  const cubeRive=setupRive(cubeFile, canvas, "State Machine 1", "Artboard 2", () => {
    // Prevent a blurry canvas by using the device pixel ratio
    cubeRive.resizeDrawingSurfaceToCanvas();
    const inputs = cubeRive.stateMachineInputs(stateMachineName);
    activeBoolController = inputs.find((i) => i.name === "Activate");
    console.log(activeBoolController.value);
    const stageNumInside = inputs.find((i) => i.name === "Stage");
    stageNumInside.value = stageNum.value;
    console.log(stageNum.value);
  });

  console.log("hi");
  console.log("halo ini evan yang tambah");
});

function setupRive(file, canvas, stateMachineName, artboardName, onLoad) {
  const rive_ = new rive.Rive({
    riveFile: file,
    canvas: canvas,
    layout: layout,
    stateMachines: stateMachineName,
    artboard: artboardName,
    autoplay: true,
    onLoad: onLoad,
  });
  return rive_;
}
</script>
