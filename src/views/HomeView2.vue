<template>
<main>
    <div class="container">
        <canvas id="canvas1" width="500" height="500"></canvas>
    </div>
    <button @click="activate"> Activate </button>
    <button @click="switchFile"> Switch File </button>
</main>

</template>

<script setup>
import * as rive from "@rive-app/canvas";
import { ref, onMounted, useTemplateRef } from "vue";

//Variables
const layout = new rive.Layout({
  fit: rive.Fit.Contain,
  alignment: rive.Alignment.TopCenter,
});


let cubeFile;
let boxFile;
let cube = ref(true);
let canvas1;

//Functions
async function loadRiveFile(src){
    const file = new rive.RiveFile({
        src: src,
        onLoad: () => {
            console.log("File Loaded")
        },
        onLoadError: (error) => {
            console.error("Error loading file: " + error);
        },
    })
    await file.init();
    return file;

};

function activate(){
    if (cube.value == true){
        activeBoolController.value = true;
    }
    else{
        activeBoxController.value = true;
    }
};

function switchFile(){
    if(cube.value == false){
        cube.value = true;
        const riveSet = setupRive(cubeFile, "Artboard 2","State Machine 1", canvas1, () => {
        riveSet.resizeDrawingSurfaceToCanvas();
        const inputs = riveSet.stateMachineInputs("State Machine 1");
        activeBoolController = inputs.find((i) => i.name === "Activate");
    })
    }
    else{
        cube.value = false;
        const riveSet1 = setupRive(boxFile, "Artboard", "State Machine 1", canvas1, () => {
            riveSet1.resizeDrawingSurfaceToCanvas();
            const inputs = riveSet1.stateMachineInputs("State Machine 1");
            activeBoxController = inputs.find((i) => i.name === "Bool");
        
    })
    }
};

let activeBoolController;
let activeBoxController;
//Main Code
onMounted(async () => {
    console.log("Mounted")
    canvas1 = document.getElementById('canvas1');
    cubeFile = await loadRiveFile("cubetransition (1).riv");
    boxFile = await loadRiveFile("box.riv");
    const riveSet = setupRive(cubeFile, "Artboard 2", "State Machine 1", canvas1 , () => {
        riveSet.resizeDrawingSurfaceToCanvas();
        const inputs = riveSet.stateMachineInputs("State Machine 1");
        activeBoolController = inputs.find((i) => i.name === "Activate");
    })
});

function setupRive(file, artboard, stateMachineName, canvas, onLoad){
    const rive_ = new rive.Rive({
        riveFile: file,
        layout: layout,
        canvas: canvas,
        artboard: artboard,
        stateMachines: stateMachineName,
        autoplay: true,
        onLoad: onLoad
    })
    return rive_;
}
</script>