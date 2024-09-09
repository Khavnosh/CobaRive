<template>
  <main>
    <div class="container">
      <canvas id="canvas1" width="500" height="531"></canvas>
    </div>
    <button @click="activate">Activate (Cube)</button>
    <button @click ="activateBox"> Activate (Box)</button>
    <button @click="stop">Stop</button>
    <button @click="switchFile">Switch to Box</button>
    <button @click="switchBack">Switch to Cube</button>
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
let truckFile;
let canvas;
let cubeActive = ref(true);
const layout = new rive.Layout({
  fit: rive.Fit.Contain,
  alignment: rive.Alignment.TopCenter,
});

//Main Codes
function activate() {
  activeBoolController.value = true;
  
  
}

function activateBox() {
  boxBool.value = true;
    console.log(boxBool.value)
}

function stop() {
  activeBoolController.value = false;
}

function switchFile() {
  //If the current animation is the cube
    cubeActive.value = false;
    const riv_1 = setupRive(truckFile, canvas, "State Machine 1", "Artboard", () => {
    riv_1.resizeDrawingSurfaceToCanvas();
    const inputs = riv_1.stateMachineInputs("State Machine 1");
    boxBool = inputs.find((i) => i.name === "Bool");
    boxBool.value = false;
    console.log("Box Bool: ", boxBool)
    
    })
    console.log(cubeActive.value);

  
  
}

function switchBack() {
  cubeActive.value = true;
    console.log("cubeActive has changed")
    const cubeRive = setupRive(cubeFile, canvas, "State Machine 1", "Artboard 2", () => {
    // Prevent a blurry canvas by using the device pixel ratio
    cubeRive.resizeDrawingSurfaceToCanvas();
    const inputs = cubeRive.stateMachineInputs("State Machine 1");
    activeBoolController = inputs.find((i) => i.name === "Activate");
    console.log(activeBoolController.value);
    const stageNumInside = inputs.find((i) => i.name === "Stage");
    stageNumInside.value = stageNum.value;
    console.log(stageNum.value);
  });
  console.log(cubeActive.value);
}

let activeBoolController;
let boxBool;
let activeBoolState = ref(false);


onMounted(async () => {
  truckFile = await loadRiveFile("box.riv");
  cubeFile = await loadRiveFile("cubetransition.riv");
  canvas = document.getElementById("canvas1");
  const cubeRive=setupRive(cubeFile, canvas, "State Machine 1", "Artboard 2", () => {
    // Prevent a blurry canvas by using the device pixel ratio
    cubeRive.resizeDrawingSurfaceToCanvas();
    const inputs = cubeRive.stateMachineInputs("State Machine 1");
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
