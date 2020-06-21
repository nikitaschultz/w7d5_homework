<template lang="html">
  <div id="win-screen">
    <canvas id="canvas" width="800" height="500"></canvas>
  </div>
</template>

<script>
export default {
  name: "win-screen",
  mounted(){
    const canvas = document.getElementById("canvas");
    const ctx = canvas.getContext("2d");
    const winScreen = document.getElementById("win-screen");

    canvas.width = winScreen.clientWidth;
    canvas.height = winScreen.clientHeight;

    const cardImage = new Image();
    cardImage.src = this.card.image;

    const maxFireworks = 3;
    const maxSparks = 15;

    let card = {
      w: 70,
      h: 96,
      x: 0,
      y: 0,
      vx: 5,
      vy: 2,
      draw: function(){
        ctx.drawImage(cardImage, this.x, this.y, this.w, this.h);
      },
      update: function(){
        if(card.x + card.vx > canvas.width - card.w || card.x + card.vx < 0){
          card.vx = -card.vx
        }
        card.x += card.vx;
        if(card.y + card.vy > canvas.height - card.h || card.y + card.vy < 0){
          card.vy = -card.vy
        }
        card.y += card.vy;
      }
    }

    let congratulations = {
      x: canvas.width / 2,
      y: canvas.height / 2,
      draw: function(){
        ctx.font = "48px MuseoModerno";
        ctx.fillStyle = "#034f84";
        ctx.textAlign = "center";
        ctx.fillText("Congratulations!", this.x, this.y);
      }
    }

    let fireworks = [];

    for (let i = 0; i < maxFireworks; i++){
      let firework = {
        sparks: []
      };
      for (let n = 0; n < maxSparks; n++){
        let spark = {
          vx: Math.random() * 5 + 0.5,
          vy: Math.random() * 5 + 0.5,
          red: Math.random() * 255,
          green: Math.random() * 255,
          blue: Math.random() * 255
        };
        if (Math.random() > 0.5){
          spark.vx = -spark.vx
        };
        if (Math.random() > 0.5){
          spark.vy = -spark.vy
        };
        firework.sparks.push(spark);
      }
      fireworks.push(firework);
      resetFirework(firework);
    }

    console.log(this.fireworks);

    function resetFirework(firework){
      firework.x = Math.floor(Math.random() * canvas.width);
      firework.y = canvas.height;
      firework.age = 0;
      firework.phase = 'fly';
      firework.colour = `rgb(${Math.random() * 255}, ${Math.random() * 255}, ${Math.random() * 255})`;
    }

    function drawFireworks(){
      fireworks.forEach((firework, index) => {
        if (firework.phase == 'explode'){
          firework.sparks.forEach((spark) => {
            for (let i = 0; i < 10; i++){
              let trailAge = firework.age + i;
              let x = firework.x + spark.vx * trailAge;
              let y = firework.y + spark.vy * trailAge;
              ctx.beginPath();
              ctx.arc(x, y, 2, 0, 2 * Math.PI);
              ctx.fillStyle = `rgb(${spark.red}, ${spark.green}, ${spark.blue}, ${1 - trailAge / 100})`
              ctx.fill();
            }
          });
          firework.age += 1;
          if (firework.age > 100 && Math.random() <0.5){
            resetFirework(firework);
          }
        }else{
          firework.y = firework.y - 7;
          for (let spark = 0; spark < 15; spark++){
            ctx.beginPath();
            ctx.arc(firework.x, firework.y, 2, 0, 2 * Math.PI);
            ctx.fillStyle = firework.colour;
            ctx.fill()
          }
          if (Math.random() < 0.001 || firework.y < 100){
            firework.phase = "explode";
          }
        }
      })
    }


    function draw(){
      ctx.fillStyle = 'rgba(146, 168, 209, 0.3)';
      ctx.fillRect(0, 0, canvas.width, canvas.height);

      ctx.clearRect(card.x - card.vx, card.y - card.vy, card.w, card.h);

      card.draw();
      card.update();

      drawFireworks();

      congratulations.draw();

      window.requestAnimationFrame(draw)
    }

    window.requestAnimationFrame(draw);


  },
  props: ["card"]
}
</script>

<style lang="css" scoped>

  #win-screen {
    display: flex;
    align-items: center;
    justify-content: center;
    width: 68vw;
    height: 100%;
    padding: 1%;
    background-color: #92a8d1;
  }

</style>
