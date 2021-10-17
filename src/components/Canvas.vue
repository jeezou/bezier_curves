<template>
  <canvas id="bezier" width="601" height="602"></canvas>
</template>

<script>
export default {
  name: "Canvas",
  data() {
    return { coords: Array() };
  },
  props: {
    data: Array,
  },
  watch: {
    data: function(val) {
      console.log(val);
      this.coords = val;
      this.redraw();
    },
  },
  methods: {
    // i - номер вершины, n - количество вершин, t - положение кривой (от 0 до 1)
    getBezierBasis(i, n, t) {
      // Факториал
      function f(n) {
        return n <= 1 ? 1 : n * f(n - 1);
      }

      // считаем i-й элемент полинома Берштейна
      return (
        (f(n) / (f(i) * f(n - i))) * Math.pow(t, i) * Math.pow(1 - t, n - i)
      );
    },

    // arr - массив опорных точек. Точка - двухэлементный массив, (x = arr[0], y = arr[1])
    // step - шаг при расчете кривой (0 < step < 1), по умолчанию 0.01
    getBezierCurve(arr, step) {
      if (step == undefined) {
        step = 0.01;
      }

      var res = new Array();

      for (var t = 0; t < 1 + step; t += step) {
        if (t > 1) {
          t = 1;
        }

        var ind = res.length;

        res[ind] = new Array(0, 0);

        for (var i = 0; i < arr.length; i++) {
          var b = this.getBezierBasis(i, arr.length - 1, t);

          res[ind][0] += arr[i][0] * b;
          res[ind][1] += arr[i][1] * b;
        }
      }

      return res;
    },

    // ctx - rendering context холста, arr - массив точек по которым строим кривую
    // delay - задержка перед отрисовкой следующей точки, pause - пауза перед началом  рисования,
    drawLines(ctx, arr, delay, pause) {
      if (delay == undefined) {
        delay = 10;
      }

      if (pause == undefined) {
        pause = delay;
      }
      var i = 0;

      function delayDraw() {
        if (i >= arr.length - 1) {
          return;
        }

        ctx.moveTo(arr[i][0], arr[i][1]);
        ctx.lineTo(arr[i + 1][0], arr[i + 1][1]);
        ctx.stroke();

        ++i;

        setTimeout(delayDraw, delay);
      }

      setTimeout(delayDraw, pause);
    },

    redraw() {
      const drawC = document.querySelector("canvas");
      console.log(this.coords);
      if (drawC && drawC.getContext) {
        let ctx = drawC.getContext("2d");
        ctx.clearRect(0, 0, 602, 602);
        ctx.strokeStyle = "#dedede";
        ctx.beginPath();
        ctx.lineWidth = 1;

        for (var x = 0; x < 602; x += 25) {
          ctx.moveTo(x, 0);
          ctx.lineTo(x, 600);
        }

        for (var y = 0; y < 602; y += 25) {
          ctx.moveTo(0, y);
          ctx.lineTo(6000, y);
        }

        ctx.stroke();
        ctx.closePath();

        ctx.beginPath();
        ctx.moveTo(this.coords[0][0], [0][1]);
        this.coords.forEach((el) => {
          ctx.lineTo(el[0], el[1]);
          ctx.fillText(`[${el[0]}, ${el[1]}]`, el[0] - 25, el[1] - 10);
          ctx.stroke();
          ctx.closePath();

          ctx.beginPath();
          ctx.lineCap = "round";
          ctx.strokeStyle = "#07afda";
          ctx.arc(el[0], el[1], 2, 0, 360);
          ctx.stroke();
          ctx.closePath();

          ctx.beginPath();
          ctx.strokeStyle = "#cdcdfd";
          ctx.moveTo(el[0], el[1]);
        });
        ctx.beginPath();

        ctx.imageSmoothingEnabled = true;
        ctx.strokeStyle = "#454545";
        ctx.fillStyle = "#454545";
        ctx.lineWidth = 1;

        var flow; // Массив координат кривой
        var arr = this.coords;

        flow = this.getBezierCurve(arr, 0.01);
        this.drawLines(ctx, flow, 10);
        ctx.closePath();
      }
    },
  },
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
canvas {
  align-self: center;
  background: #fafafa;
  margin: 0 auto;
  border-radius: 12px;
}
</style>
