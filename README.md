| Nama | NRP |
| ---------------------- | ---------- |
| Faiz Haq Noviandra | 5025211132 |
|  Tugas WebGL |

## RGB Triangle WebGL
```
<!-- <!DOCTYPE html> -->
<html><head><meta http-equiv="Content-Type" content="text/html; charset=UTF-8">

<title>RGB Triangle WebGL</title>

<script>

"use strict";

const  vertexShaderSource =
       "attribute vec2 a_coords;\n" +
       "attribute vec3 a_color;\n" +
       "varying vec3 v_color;\n" +
       "void main() {\n" +
       "   gl_Position = vec4(a_coords, 0.0, 1.0);\n" +
       "   v_color = a_color;\n" +
       "}\n";

const  fragmentShaderSource =
       "precision mediump float;\n" +
       "varying vec3 v_color;\n" +
       "void main() {\n" +
       "   gl_FragColor = vec4(v_color, 1.0);\n" +
       "}\n";

let  gl;  // Konteks grafis WebGL.

let  attributeCoords;  // Lokasi atribut bernama "a_coords".
let  bufferCoords;     // Objek penyangga titik untuk menyimpan nilai koordinat.

let  attributeColor;   // Lokasi atribut bernama "a_color".
let  bufferColor;     // Objek penyangga titik untuk menyimpan nilai warna.

/**
 *  Menggambar konten kanvas, dalam hal ini segitiga warna RGB.
 */
function draw() 
{ 

    gl.clearColor(0,0,0,1);  // tentukan warna yang akan digunakan untuk pembersihan
    gl.clear(gl.COLOR_BUFFER_BIT);  // bersihkan kanvas (menjadi hitam)

    /*  Siapkan nilai untuk atribut "coords".*/

    let  coords = new Float32Array( [ -0.5,-0.5, 0.5,-0.5, -0.5,0.5, 0.5,0.5 ] );
   
    gl.bindBuffer(gl.ARRAY_BUFFER, bufferCoords);
    gl.bufferData(gl.ARRAY_BUFFER, coords, gl.STREAM_DRAW);
    gl.vertexAttribPointer(attributeCoords, 2, gl.FLOAT, false, 0, 0);
    gl.enableVertexAttribArray(attributeCoords); 
   
    /* Siapkan nilai untuk atribut "warna". */
   
    let  color = new Float32Array( [ 0,1,0, 1,1,1, 0,0,1, 1,0,0 ] );

    gl.bindBuffer(gl.ARRAY_BUFFER, bufferColor);
    gl.bufferData(gl.ARRAY_BUFFER, color, gl.STREAM_DRAW);
    gl.vertexAttribPointer(attributeColor, 3, gl.FLOAT, false, 0, 0);
    gl.enableVertexAttribArray(attributeColor); 
    
    /* Gambarlah persegi menggunakan TRIANGLE_STRIP. */
   
    gl.drawArrays(gl.TRIANGLE_STRIP, 0, 4);

}

/**
 * Membuat program untuk digunakan dalam konteks WebGL gl, dan mengembalikan
 * pengenal untuk program itu. Jika terjadi kesalahan saat kompilasi atau
 * menghubungkan program, pengecualian tipe String dilempar. Kesalahannya
 * string berisi kesalahan kompilasi atau penautan. Jika tidak terjadi kesalahan,
 * pengidentifikasi program adalah nilai kembalian fungsi.
 */
function createProgram(gl, vertexShaderSource, fragmentShaderSource) 
{
   let  vsh = gl.createShader( gl.VERTEX_SHADER );
   gl.shaderSource( vsh, vertexShaderSource );
   gl.compileShader( vsh );
   if ( ! gl.getShaderParameter(vsh, gl.COMPILE_STATUS) ) {
      throw new Error("Error in vertex shader:  " + gl.getShaderInfoLog(vsh));
   }
   let  fsh = gl.createShader( gl.FRAGMENT_SHADER );
   gl.shaderSource( fsh, fragmentShaderSource );
   gl.compileShader( fsh );
   if ( ! gl.getShaderParameter(fsh, gl.COMPILE_STATUS) ) {
      throw new Error("Error in fragment shader:  " + gl.getShaderInfoLog(fsh));
   }
   let  prog = gl.createProgram();
   gl.attachShader( prog, vsh );
   gl.attachShader( prog, fsh );
   gl.linkProgram( prog );
   if ( ! gl.getProgramParameter( prog, gl.LINK_STATUS) ) {
      throw new Error("Link error in program:  " + gl.getProgramInfoLog(prog));
   }
   return prog;
}

/**
 * Inisialisasi konteks grafis WebGL
 */
function initGL() 
{
    let  prog = createProgram( gl, vertexShaderSource, fragmentShaderSource );
    gl.useProgram(prog);    
    
    bufferCoords = gl.createBuffer();
    bufferColor = gl.createBuffer();

    attributeCoords = gl.getAttribLocation(prog, "a_coords");
    attributeColor = gl.getAttribLocation(prog, "a_color");
}

/**
 * Inisialisasi program. Fungsi ini dipanggil setelah halaman dimuat.
 */
function init() 
{
    try {
        let  canvas = document.getElementById("webglcanvas");
        gl = canvas.getContext("webgl2");
        if ( ! gl ) {
            throw "Browser does not support WebGL";
        }
    }
    catch (e) {
        document.getElementById("canvas-holder").innerHTML =
            "<p>Sorry, could not get a WebGL graphics context.</p>";
        return;
    }
    try {
        initGL();  // menginisialisasi konteks grafis WebGL
    }
    catch (e) {
        document.getElementById("canvas-holder").innerHTML =
            "<p>Sorry, could not initialize the WebGL graphics context: " + e.message + "</p>";
        return;
    }
    draw();    // menggambar gambarnya
}

window.onload = init;  //Atur agar init() dipanggil ketika halaman telah dimuat .

</script>
</head>
<body>

<h2>RGB Triangle WebGL-5025211132</h2>
<noscript><p><b>Sorry, but this page requires JavaScript.</b></p></noscript>
<div id="canvas-holder">
<canvas id="webglcanvas" width="450" height="450"></canvas>
</div>
</body></html>
```

### Dokumentasi
![RGB Triangle WebGL ](https://github.com/faiznoviandra/Tugas-WebGL-5025211132./assets/116566988/0bd12acb-fe3c-47a6-a9f2-71a01c24751f)

## Kubus Menggunakan glMatrix dan WebGL
```
<!DOCTYPE html>
<meta charset="UTF-8">
<html>
<head>
<title>Kubus Menggunakan glMatrix dan WebGL</title>
<style>
    body
    {
        background-color: #EEEEEE;
    }
</style>

<script type="x-shader/x-vertex" id="vshader-source">
    attribute vec3 a_coords;
    void main()
    {
        vec4 coords = vec4(a_coords,1.0);
        gl_Position = coords;
    }
</script>

<script type="x-shader/x-fragment" id="fshader-source">
    #ifdef GL_FRAGMENT_PRECISION_HIGH
       precision highp float;
    #else
       precision mediump float;
    #endif
    uniform vec4 color;
    void main()
    {
        gl_FragColor = color;
    }
</script>

<script src="gl-matrix-min.js"></script>
<script>

"use strict";

let gl;
let a_coords_loc;
let a_coords_buffer;
let u_color;

function drawPrimitive( primitiveType, color, vertices )
{
     gl.enableVertexAttribArray(a_coords_loc);
     gl.bindBuffer(gl.ARRAY_BUFFER,a_coords_buffer);
     gl.bufferData(gl.ARRAY_BUFFER, new Float32Array(vertices), gl.STREAM_DRAW);
     gl.uniform4fv(u_color, color);
     gl.vertexAttribPointer(a_coords_loc, 3, gl.FLOAT, false, 0, 0);
     gl.drawArrays(primitiveType, 0, vertices.length/3);
}

function draw()
{ 
    gl.clearColor(0,0,0,1);
    gl.clear(gl.COLOR_BUFFER_BIT | gl.DEPTH_BUFFER_BIT);
    //bawah
    drawPrimitive( gl.TRIANGLE_FAN, [0,1,0,1], [-0.5,-0.5,-0.5, -0.2,-0.3,0.5, 0.8,-0.3,0.5, 0.5,-0.5,-0.5]);
    //atas
    drawPrimitive( gl.TRIANGLE_FAN, [1,0,0,1], [0.5,0.5,-0.5, -0.5,0.5,-0.5, -0.2,0.7,0.5, 0.8, 0.7, 0.5]);
    //belakang
    drawPrimitive( gl.TRIANGLE_FAN, [1,1,1,1], [-0.2,-0.3,0.5, -0.2,0.7,0.5, 0.8,0.7,0.5, 0.8,-0.3,0.5]); 
    //depan
    drawPrimitive( gl.TRIANGLE_FAN, [0,1,1,1], [-0.5,-0.5,-0.5, -0.5,0.5,-0.5, 0.5,0.5,-0.5, 0.5,-0.5,-0.5]); 
    // kiri
    drawPrimitive( gl.TRIANGLE_FAN, [0,0,1,1], [-0.5,0.5,-0.5, -0.5,-0.5,-0.5, -0.2,-0.3,0.5, -0.2,0.7,0.5]);
    //kanan
    drawPrimitive( gl.TRIANGLE_FAN, [1,1,0,1], [0.5,0.5,-0.5, 0.5,-0.5,-0.5, 0.8,-0.3,0.5, 0.8,0.7,0.5]); 
    
}

function initGL()
{
    let prog = createProgram(gl,"vshader-source","fshader-source");
    gl.useProgram(prog);
    a_coords_loc =  gl.getAttribLocation(prog, "a_coords");
    u_color =  gl.getUniformLocation(prog, "color");
    a_coords_buffer = gl.createBuffer();
    gl.enable(gl.DEPTH_TEST);
}

function createProgram(gl, vertexShaderID, fragmentShaderID)
{
    function getTextContent( elementID ) 
    {
        let element = document.getElementById(elementID);
        let node = element.firstChild;
        let str = "";
        while (node)
        {
            if (node.nodeType === 3)
                str += node.textContent;
            node = node.nextSibling;
        }
        return str;
    }

    let vertexShaderSource, fragmentShaderSource;
    try
    {
        vertexShaderSource = getTextContent( vertexShaderID );
        fragmentShaderSource = getTextContent( fragmentShaderID );
    }
    catch (e)
    {
        throw new Error("Error: Could not get shader source code from script elements.");
    }
    let vsh = gl.createShader( gl.VERTEX_SHADER );
    gl.shaderSource(vsh,vertexShaderSource);
    gl.compileShader(vsh);
    if ( ! gl.getShaderParameter(vsh, gl.COMPILE_STATUS) )
    {
        throw new Error("Error in vertex shader:  " + gl.getShaderInfoLog(vsh));
    }
    let fsh = gl.createShader( gl.FRAGMENT_SHADER );
    gl.shaderSource(fsh, fragmentShaderSource);
    gl.compileShader(fsh);
    if ( ! gl.getShaderParameter(fsh, gl.COMPILE_STATUS) ) 
    {
       throw new Error("Error in fragment shader:  " + gl.getShaderInfoLog(fsh));
    }
    let prog = gl.createProgram();
    gl.attachShader(prog,vsh);
    gl.attachShader(prog, fsh);
    gl.linkProgram(prog);
    if ( ! gl.getProgramParameter( prog, gl.LINK_STATUS) )
    {
       throw new Error("Link error in program:  " + gl.getProgramInfoLog(prog));
    }
    return prog;
}

function init()
{
    try {
        let canvas = document.getElementById("webglcanvas");
        gl = canvas.getContext("webgl");
        if ( ! gl ) {
            throw "Browser does not support WebGL";
        }
    }
    catch (e) {
        document.getElementById("canvas-holder").innerHTML =
            "<p>Sorry, could not get a WebGL graphics context.</p>";
        return;
    }
    try {
        initGL();  // initialize the WebGL graphics context
    }
    catch (e) {
        document.getElementById("canvas-holder").innerHTML =
            "<p>Sorry, could not initialize the WebGL graphics context: " + e.message + "</p>";
        return;
    }

    draw();
}

window.onload = init;

</script>
</head>
<body>

<h2>Kubus dibuat dengan WebGL dan glMatrix-5025211132</h2>

<noscript><hr><h3>This page requires Javascript and a web browser that supports WebGL</h3><hr></noscript>

<div id="canvas-holder">
   <canvas width=600 height=600 id="webglcanvas"></canvas>
</div>
</body>
</html>
```

### Dokumentasi

Kubus gambar lengkap
![Kubus dengan lengkap](https://github.com/faiznoviandra/Tugas-WebGL-5025211132./assets/116566988/9b9c7168-1801-4efa-b60c-4d0365d9a7bf)

Kubus tanpa atas
![Kubus tanpa atas](https://github.com/faiznoviandra/Tugas-WebGL-5025211132./assets/116566988/552ebe2c-faa2-416f-a673-2519b3f8c1f4)

kubus tanpa bagian depan
![Kubus tanpa bagian depan](https://github.com/faiznoviandra/Tugas-WebGL-5025211132./assets/116566988/f12acc23-547f-4011-909d-f748c63e9431)

kubus tanpa bagian kanan
![Kubus tanpa bagian kanan](https://github.com/faiznoviandra/Tugas-WebGL-5025211132./assets/116566988/d9e7a780-b3fc-437e-81ec-3d3e283011e6)










