var gl = canvas.getContext("webgl2") canvas.getContext("experimental-webgl2");
var compileShader = function(prog, src, type){
    var sh = gl.createShader(type);
    gl.shaderSource(sh, src.replace(/^\n/, ""));
    gl.compileShader(sh);
    if (!gl.getShaderParameter(sh, gl.COMPILE_STATUS)) {
      console.log(gl.getShaderInfoLog(sh));
    }    
    gl.attachShader(prog, sh);
    gl.deleteShader(sh);
};
        
var p = gl.createProgram();
compileShader(p, vs.text, gl.VERTEX_SHADER);
compileShader(p, fs.text, gl.FRAGMENT_SHADER);
gl.linkProgram(p);
gl.useProgram(p);
gl.uniform2f(gl.getUniformLocation(p, "resolution"), canvas.width, canvas.height);

var zero = Date.now();
(function () { 
    gl.clear(gl.COLOR_BUFFER_BIT | gl.DEPTH_BUFFER_BIT);
    gl.uniform1f(gl.getUniformLocation(p, "time"), (Date.now() - zero) * 0.001);
    gl.drawArrays(gl.TRIANGLE_STRIP, 0, 4);
    requestAnimationFrame(arguments.callee);
})();
