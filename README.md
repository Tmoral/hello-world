# hello-world
This is my first repository.
hhhhhh

<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
    <script src="jquery-1.11.3.js"></script>
    <style>
        ul{
            list-style:none;
            float:left;
            /*height:300px;*/
            /*width:150px;*/
        }
        ul li{
            height:50px;
            width:150px;
        }
        canvas{
            float:left;
        }
        #cricle1{
            background:#FFF2F2;
            border-radius:50%;
            position:absolute;
            width:15px;
            height:15px;
            box-shadow: 0px 0px 3px 0px #787B83;
        }
        #cricle2{
             background:#FFF2F2;
             border-radius:50%;
             position:absolute;
             width:20px;
             height:20px;
             box-shadow: 0px 0px 3px 0px #787B83;
         }
    </style>
</head>
<body>
<div id="container">
    <div id="cricle1"></div>
    <div id="cricle2"></div>
    <canvas id="canvas" width="300px" height="300px"></canvas>
    <!--<div id="bar"></div>-->
    <ul>
        <li>R:<input type="number" min="0" max="255"></li>
        <li>G:<input type="number" min="0" max="255"></li>
        <li>B:<input type="number" min="0" max="255"></li>
        <li>H:<input type="number" min="0" max="1"></li>
        <li>S:<input type="number" min="0" max="1"></li>
        <li>L:<input type="number" min="0" max="1"></li>
    </ul>
</div>
<script>
    window.onload=function(){
        var canvas=$('#canvas')[0];
        var ctx=canvas.getContext('2d');
        var height=Math.round(canvas.height*0.9);
        var width=canvas.width;
        var input=$("ul li input");
        var canvasOffset=$(canvas).offset;
        function drawBox(color) {
            var gradientBase1=ctx.createLinearGradient(0,0,width,0);
            gradientBase1.addColorStop(0,'rgba(255,255,255,1)');
            gradientBase1.addColorStop(1,color ? color :'rgba(255,0,0,1)');
            ctx.fillStyle=gradientBase1;
            ctx.fillRect(0,0,width,height);
            var gradientBase2=ctx.createLinearGradient(0,0,0,height);
            gradientBase2.addColorStop(0,'rgba(0,0,0,0)');
            gradientBase2.addColorStop(1,'rgba(0,0,0,1)');
            ctx.fillStyle=gradientBase2;
            ctx.fillRect(0,0,width,height);
        }
        var sidebar=ctx.createLinearGradient(0,height,width,height);
        sidebar.addColorStop(0, '#f00');
        sidebar.addColorStop(1 / 6, '#f0f');
        sidebar.addColorStop(2 / 6, '#00f');
        sidebar.addColorStop(3 / 6, '#0ff');
        sidebar.addColorStop(4 / 6, '#0f0');
        sidebar.addColorStop(5 / 6, '#ff0');
        sidebar.addColorStop(1, '#f00');
        ctx.fillStyle=sidebar;
        ctx.fillRect(0,height,width,height);
        var c1=$("#cricle1");
        var c2=$("#cricle2");
        var cw1=c1.width()/2;
        var ch1=c1.height()/2;
        var cw2=c2.width()/2;
        function canvasBind(){
            //初始化圆圈位置
            c1.css({"left": width - cw1,"top":ch1});
            c2.css({"left":8,"top": height+8});
            /*******鼠标点击事件*******/
            $("#canvas").bind("click",function(e){
                var canvasX, canvasY, //保存鼠标相对于canvas的位置
                        boxArea, //canvas 色块区域
                        sidebarArea; //canvas 底栏区域
//                canvasX = Math.floor(e.pageX - canvasOffset.left);
//                canvasY = Math.floor(e.pageY - canvasOffset.top);
//                console.log(e.offsetX);
//                console.log(e.offsetY);
//                boxArea = canvasX >= 0 && canvasX < width && canvasY >= 0 && canvasY < height;
//                sidebarArea = canvasX >= 0 && canvasX < width && canvasY >= height && canvasY < width;
                if(0< e.offsetX<width && 0< e.offsetY<height) {
                    c1.css({ "left": e.pageX - cw1, "top": e.pageY - ch1 });
                    colorConfig(getSelectColor(e.pageX, e.pageY), "rgb"); //显示颜色数据
                } else if(0< e.offsetX<width&&height< e.offsetY<width) {
                    c2.css({ "left": e.pageX - cw2 });
                    var rgb = getSelectColor(e.pageX, e.pageY); //保存返回的颜色
                    var rgbaStr = "rgba(" + rgb.r + "," + rgb.g + "," + rgb.b + ",255)"; //拼接颜色
                    drawBox(rgbaStr); //更新颜色区块
                    colorConfig(getSelectColor(e.pageX, e.pageY), "rgb");
                }
            });
        }
        function getSelectColor(x, y) {
            var imgData = ctx.getImageData(x, y, 1, 1);
            var data = jQuery.makeArray(imgData.data); //类数组转换成数组
            return {
                r: data[0],
                g: data[1],
                b: data[2]
            }
        }
        function colorConfig(color, type) {
            var RGB;
            switch(type) {
                case "rgb":
                    RGB = color;
//                    HSL = rgbToHsl(color);
//                    HEX = rgbToHex(color);
                    break;
//                case "hsl":
//                    RGB = hslToRgb(color);
//                    HSL = color;
//                    HEX = rgbToHex(RGB);
//                    break;
//                case "hek":
//                    RGB = hexToRgb(color);
//                    HSL = rgbToHsl(RGB);
//                    HEX = color;
//                    break;
            }
            showColorData(RGB);
        }
        function showColorData(rgb) {
//            $(".u-box").css("background", "#" + hex.h + hex.e + hex.x);
//            input[0].value = "#" + hex.h + hex.e + hex.x;
            input[0].value = rgb.r;
            input[1].value = rgb.g;
            input[2].value = rgb.b;
//            input[4].value = Math.round(hsl.h * 100) / 100;
//            input[5].value = Math.round(hsl.s * 100) / 100;
//            input[6].value = Math.round(hsl.l * 100) / 100;
        }
        function init(){
            drawBox();
            canvasBind();
        }
        init();
//        canvasBind();
    }
</script>
</body>
</html>