<template>
  <main>
    <div class="container">
      <canvas id="canvas1" width="1366" height="531"></canvas>
    </div>
    <button> Test Buttons </button>
  </main>
</template>

<script setup>
//Imports
import * as rive from "@rive-app/canvas";
import {ref, onMounted, useTemplateRef} from 'vue'



//Defining Variables and Functions
//Variables
  //Layouts
let layout = new rive.Layout({
        fit: rive.Fit.Cover,
        alignment: rive.Alignment.Center,
        });
  
let file = new rive.RiveFile({ 
  src: 'cubetransition (1).riv',
  onLoad: () => {} })
let activeBool = ref(false);
const stageNum = ref(1);
const canvas = document.getElementById('canvas1');

//Functions
function loadRiveFile(src) {
  file = new rive.RiveFile({
    src: src,
    onLoad: () => {}
  });
  file.init();
  
}

function playCube(){
  activeBool.value = true
}



//Main Codes
onMounted(() => {
console.log("hi")
console.log("halo ini evan yang tambah")
  const cubeRive = new rive.Rive({
    riveFile : file,
    canvas : canvas,
    layout : layout,
    stateMachine: 'State Machine 1',
    autoplay : true,
    artboard: 'Artboard 2',
    onLoad: () => {
      // Prevent a blurry canvas by using the device pixel ratio
      cubeRive.resizeDrawingSurfaceToCanvas();

      const inputs = cubeRive.stateMachineInputs('State Machine 1');
      const activeBoolInside = inputs.find(i => i.name === 'Activate');
      activeBoolInside.value = activeBool.value
      console.log(activeBool)
      const stageNumInside = inputs.find(i => i.name === 'Stage');
      stageNumInside.value = stageNum.value
      console.log(stageNum)
      
    },
  })


});




</script>
