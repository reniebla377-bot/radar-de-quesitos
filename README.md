# radar-de-quesitos
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width,initial-scale=1">
<title>Radar de Quesitos</title>

<style>
body{
    margin:0;
    background:black;
    overflow:hidden;
}
canvas{
    display:block;
}
</style>
</head>

<body>
<canvas id="radar"></canvas>

<script>
const canvas = document.getElementById("radar");
const ctx = canvas.getContext("2d");

function ajustarCanvas(){
    canvas.width = window.innerWidth;
    canvas.height = window.innerHeight;
}
ajustarCanvas();
window.onresize = ajustarCanvas;

let barrido = 0;

// Quesitos con coordenadas relativas al centro
const quesos = [
    {nombre:"Manchego",x:100,y:-100},
    {nombre:"Brie",x:200,y:180},
    {nombre:"Cabrales",x:-150,y:-50},
    {nombre:"Gouda",x:-120,y:180}
];

function dibujar(){
    ctx.fillStyle="black";
    ctx.fillRect(0,0,canvas.width,canvas.height);

    const cx = canvas.width/2;
    const cy = canvas.height/2;

    // Dibujar círculos del radar
    ctx.strokeStyle="#00ff00";
    ctx.lineWidth = 2;
    for(let r=50; r<300; r+=50){
        ctx.beginPath();
        ctx.arc(cx, cy, r, 0, Math.PI*2);
        ctx.stroke();
    }

    // Dibujar línea de barrido
    const angRad = barrido * Math.PI / 180;
    const finX = cx + Math.cos(angRad) * 300;
    const finY = cy + Math.sin(angRad) * 300;

    ctx.strokeStyle="#00ff00";
    ctx.beginPath();
    ctx.moveTo(cx, cy);
    ctx.lineTo(finX, finY);
    ctx.stroke();

    // Dibujar quesos
    for(const q of quesos){
        const qx = cx + q.x;
        const qy = cy + q.y;

        // Calcular ángulo del quesito
        const dx = qx - cx;
        const dy = qy - cy;
        let angQ = Math.atan2(dy, dx) * 180 / Math.PI;
        if(angQ < 0) angQ += 360;

        // Detectar si el barrido está cerca
        const diferencia = Math.abs((barrido - angQ + 180) % 360 - 180);

        if(diferencia < 5){
            ctx.fillStyle="red";
            ctx.beginPath();
            ctx.arc(qx, qy, 8, 0, Math.PI*2);
            ctx.fill();
        } else {
            ctx.fillStyle="#00ff00";
            ctx.beginPath();
            ctx.arc(qx, qy, 5, 0, Math.PI*2);
            ctx.fill();
        }
    }

    barrido = (barrido + 2) % 360;
    requestAnimationFrame(dibujar);
}

dibujar();
</script>

</body>
</html>
