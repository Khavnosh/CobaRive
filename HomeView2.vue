<template>
<container>
    <canvas ref="canvas1" width="500" height="500"></canvas>
</container>
<button @click="activate"> Activate </button>

</template>

<script>
import * as rive from "@rive-app/canvas";
import { ref, onMounted, useTemplateRef } from "vue";

//Variables
const layout = new rive.Layout({
  fit: rive.Fit.Contain,
  alignment: rive.Alignment.TopCenter,
});

let activeBoolController;
let activeBoxController;
let cubeFile;
let cube = ref(true);

//Functions
async function loadRiveFile(src){
    const file = new rive.RiveFile({
        src: src,
        onLoad: () => {
            console.log("File Loaded")
        }
    })
    await file.init();
    return file;

}

function activate(){
    activeBoolController.value = true
}

function switchFile(){
    if(cube.value = true){
        cube.value = false;
        const riveSet = setupRive(cubeFile, "Artboard 1", "State Machine 1", () => {
        riveSet.resizeDrawingSurfaceToCanvas;
        const inputs = riveSet.stateMachineInputs("State Machine 1");
        activeBoolController = inputs.find((i) => i.name === "Activate");
    } )
    }
    else{
        cube.value = true;
        const riveSet1 = setupRive(cubeFile, "Artboard 1", "State Machine 1", () => {
            riveSet1.resizeDrawingSurfaceToCanvas;
            const input = riveSet1.stateMachineInputs("State Machine 1");
            activeBoxController = inputs.find((i) => i.name === "Bool");
            activeBoxController.value = false;

    } )
    }
}

//Main Code
onMounted( async () => {
    const canvas1 = document.getElementById('canvas1');
    cubeFile = await loadRiveFile("cubetransition (1).riv");
    boxFile = await loadRiveFile("box.riv");
    const riveSet = setupRive(cubeFile, "Artboard 1", "State Machine 1", () => {
        riveSet.resizeDrawingSurfaceToCanvas;
        const inputs = cubeRive.stateMachineInputs("State Machine 1");
        activeBoolController.value = inputs.find((i) => i.name === "Activate");
    } )
})

function setupRive(file, artboard, stateMachineName, onLoad){
    const rive_ = new rive.Rive({
        src: file,
        layout: layout,
        canvas: canvas1s,
        artboard: artboard,
        stateMachines: stateMachineName,
        onLoad: onLoad
    })
    return rive_;
}
</script>