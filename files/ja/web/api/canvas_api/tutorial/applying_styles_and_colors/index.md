---
title: スタイルと色を適用する
slug: Web/API/Canvas_API/Tutorial/Applying_styles_and_colors
translation_of: Web/API/Canvas_API/Tutorial/Applying_styles_and_colors
original_slug: Web/Guide/HTML/Canvas_tutorial/Applying_styles_and_colors
---
<div>{{CanvasSidebar}} {{PreviousNext("Web/API/Canvas_API/Tutorial/Drawing_shapes", "Web/API/Canvas_API/Tutorial/Drawing_text")}}</div>

<div class="summary">
<p>「<a href="/ja/docs/Web/Guide/HTML/Canvas_tutorial/Drawing_shapes">canvas に図形を描く</a>」の章ではデフォルトの線と塗りのスタイルのみを使いました。ここではより魅力的に描くために使うことのできるcanvasのオプションについて見ていきます。具体的には、色、線のスタイル、グラデーション、パターンや影を追加する方法について学びます。</p>
</div>

<h2 id="Colors" name="Colors">色</h2>

<p>これまでは<strong>描画コンテキスト</strong>の方法についてのみ見てきました。色を図形に適用するために、"<code>fillStyle"と<font face="Open Sans, Arial, sans-serif">"</font></code><code>strokeStyle"という</code>2つの重要なプロパティを利用することができます。</p>

<dl>
 <dt>{{domxref("CanvasRenderingContext2D.fillStyle", "fillStyle = color")}}</dt>
 <dd>図形の塗りつぶしのスタイルを記述する</dd>
 <dt>{{domxref("CanvasRenderingContext2D.strokeStyle", "strokeStyle = color")}}</dt>
 <dd>図形のアウトラインのスタイルを記述する。</dd>
</dl>

<p><code>color</code>の部分にはCSSでの{{cssxref("&lt;color&gt;")}}表現やグラデーションオブジェクトまたはパターンオブジェクトが入ります。グラデーションオブジェクトとパターンオブジェクトについては後ほど学ぶことにします。 デフォルトでは、輪郭線・塗りつぶしの色は黒に設定されています。 (CSS色では<code>#000000</code>)</p>

<div class="note">
<p><strong>注記:</strong> <code>strokeStyle</code>および<code>fillStyle</code>プロパティを設定すると、その設定した値がデフォルトとなって、それ以降に描かれる図形の線や塗りはその色で行なわれるようになります。それぞれの図形をそれぞれ別の色で描きたい場合は、シェイプを描くごとに<code>strokeStyle</code>および<code>fillStyle</code>プロパティを設定する必要があります。</p>
</div>

<p>入力できる有効な文字列は、CSS {{cssxref("&lt;color&gt;")}}表現の値である必要があります。 下記の例では同じ色について説明しています。</p>

<pre class="brush: js notranslate">// これらは全てfillStyleにオレンジ色を代入します

ctx.fillStyle = "orange";
ctx.fillStyle = "#FFA500";
ctx.fillStyle = "rgb(255,165,0)";
ctx.fillStyle = "rgba(255,165,0,1)";
</pre>

<h3 id="A_fillStyle_example" name="A_fillStyle_example">プロパティ <code>fillStyle</code> の例</h3>

<p>この例では二重のforループを使って正方形からなるグリッドを作ってみたい。そしてその正方形の一つひとつは違った色になるようにしたい。結果は下のスクリーンショットのようになるだろう。かなり面白い画像ができているだろう。それぞれのブロックで別々な色を表現するために、２つの変数<code>i</code>,<code>j</code>を用いている。変数<code>i</code>は赤成分を、変数<code>j</code>は緑成分を変化させている。青成分は固定されている。By modifying the channels, you can generate all kinds of palettes. By increasing the steps, you can achieve something that looks like the color palettes Photoshop uses.</p>

<pre class="brush: js;highlight[5,6] notranslate">function draw() {
  var ctx = document.getElementById('canvas').getContext('2d');
  for (var i = 0; i &lt; 6; i++) {
    for (var j = 0; j &lt; 6; j++) {
      ctx.fillStyle = `rgb(${Math.floor(255-42.5*i)}, ${Math.floor(255-42.5*j)}, 0)`;
      ctx.fillRect(j*25, i*25, 25, 25);
    }
  }
}</pre>

<div class="hidden">
<pre class="brush: html notranslate">&lt;canvas id="canvas" width="150" height="150"&gt;&lt;/canvas&gt;</pre>

<pre class="brush: js notranslate">draw();</pre>
</div>

<p>結果は以下のようになる:</p>

<p>{{EmbedLiveSample("A_fillStyle_example", 160, 160, "https://mdn.mozillademos.org/files/5417/Canvas_fillstyle.png")}}</p>

<h3 id="A_strokeStyle_example" name="A_strokeStyle_example">プロパティ <code>strokeStyle</code> の例</h3>

<p>This example is similar to the one above, but uses the <code>strokeStyle</code> property to change the colors of the shapes' outlines. We use the <code>arc()</code> method to draw circles instead of squares.</p>

<pre class="brush: js;highlight[5,6] notranslate">  function draw() {
    var ctx = document.getElementById('canvas').getContext('2d');
    for (var i=0;i&lt;6;i++){
      for (var j=0;j&lt;6;j++){
        ctx.strokeStyle = 'rgb(0,' + Math.floor(255-42.5*i) + ',' +
                         Math.floor(255-42.5*j) + ')';
        ctx.beginPath();
        ctx.arc(12.5+j*25,12.5+i*25,10,0,Math.PI*2,true);
        ctx.stroke();
      }
    }
  }
</pre>

<div class="hidden">
<pre class="brush: html notranslate">&lt;canvas id="canvas" width="150" height="150"&gt;&lt;/canvas&gt;</pre>

<pre class="brush: js notranslate">draw();</pre>
</div>

<p>The result looks like this:</p>

<p>{{EmbedLiveSample("A_strokeStyle_example", "180", "180", "https://mdn.mozillademos.org/files/253/Canvas_strokestyle.png")}}</p>

<h2 id="Transparency" name="Transparency">透明度のコントロール</h2>

<p>canvasに不透明な形状を描画するだけでなく、半透明の形状を描画することもできます。 これは、<code>globalAlpha</code>プロパティを設定するか、輪郭線や塗りつぶしのスタイルに半透明の色を割り当てることによって行われます。</p>

<dl>
 <dt>{{domxref("CanvasRenderingContext2D.globalAlpha", "globalAlpha = transparencyValue")}}</dt>
 <dd>代入された透明度の値を、代入後にcanvasに描画されるすべての図形に適用します。値は0.0（完全に透明）から1.0（完全に不透明）の間でなければなりません。デフォルトでは1.0（完全に不透明）が設定されています。</dd>
</dl>

<p><code>globalAlpha</code>プロパティは、同様の透明度でcanvasにいくつもの図形を描画する場合に役に立ちますが、それ以外の場合は、色を設定するときにそれぞれの図形に透明度を設定する方が一般的に便利です。</p>

<p><code>strokeStyle</code>プロパティと<code>fillStyle</code>プロパティはCSSのrgba表現を利用できるため、次のような表記を使用して透明な色を割り当てることもできます。</p>

<pre class="brush: js notranslate">// 輪郭線と塗りつぶしの色に透明色を割り当てる

ctx.strokeStyle = "rgba(255,0,0,0.5)";
ctx.fillStyle = "rgba(255,0,0,0.5)";
</pre>

<p><code>rgba()</code>関数は<code>rgb()</code>関数によく似ていますが、1つ引数が増加します。最後の引数には、この色の透明度の値を設定します。有効な値の範囲は、0.0（完全に透明）から1.0（完全に不透明）です。</p>

<h3 id="A_globalAlpha_example" name="A_globalAlpha_example">プロパティ <code>globalAlpha</code> の例</h3>

<p>In this example, we'll draw a background of four different colored squares. On top of these, we'll draw a set of semi-transparent circles. The <code>globalAlpha</code> property is set at <code>0.2</code> which will be used for all shapes from that point on. Every step in the <code>for</code> loop draws a set of circles with an increasing radius. The final result is a radial gradient. By overlaying ever more circles on top of each other, we effectively reduce the transparency of the circles that have already been drawn. By increasing the step count and in effect drawing more circles, the background would completely disappear from the center of the image.</p>

<pre class="brush: js;highlight[15] notranslate">function draw() {
  var ctx = document.getElementById('canvas').getContext('2d');
  // draw background
  ctx.fillStyle = '#FD0';
  ctx.fillRect(0,0,75,75);
  ctx.fillStyle = '#6C0';
  ctx.fillRect(75,0,75,75);
  ctx.fillStyle = '#09F';
  ctx.fillRect(0,75,75,75);
  ctx.fillStyle = '#F30';
  ctx.fillRect(75,75,75,75);
  ctx.fillStyle = '#FFF';

  // set transparency value
  ctx.globalAlpha = 0.2;

  // Draw semi transparent circles
  for (i=0;i&lt;7;i++){
    ctx.beginPath();
    ctx.arc(75,75,10+10*i,0,Math.PI*2,true);
    ctx.fill();
  }
}</pre>

<div class="hidden">
<pre class="brush: html notranslate">&lt;canvas id="canvas" width="150" height="150"&gt;&lt;/canvas&gt;</pre>

<pre class="brush: js notranslate">draw();</pre>
</div>

<p>{{EmbedLiveSample("A_globalAlpha_example", "180", "180", "https://mdn.mozillademos.org/files/232/Canvas_globalalpha.png")}}</p>

<h3 id="An_example_using_rgba" name="An_example_using_rgba()">An example using <code>rgba()</code></h3>

<p>In this second example, we do something similar to the one above, but instead of drawing circles on top of each other, I've drawn small rectangles with increasing opacity. Using <code>rgba()</code> gives you a little more control and flexibility because we can set the fill and stroke style individually.</p>

<pre class="brush: js;highlight[16] notranslate">function draw() {
  var ctx = document.getElementById('canvas').getContext('2d');

  // Draw background
  ctx.fillStyle = 'rgb(255,221,0)';
  ctx.fillRect(0,0,150,37.5);
  ctx.fillStyle = 'rgb(102,204,0)';
  ctx.fillRect(0,37.5,150,37.5);
  ctx.fillStyle = 'rgb(0,153,255)';
  ctx.fillRect(0,75,150,37.5);
  ctx.fillStyle = 'rgb(255,51,0)';
  ctx.fillRect(0,112.5,150,37.5);

  // Draw semi transparent rectangles
  for (var i=0;i&lt;10;i++){
    ctx.fillStyle = 'rgba(255,255,255,'+(i+1)/10+')';
    for (var j=0;j&lt;4;j++){
      ctx.fillRect(5+i*14,5+j*37.5,14,27.5);
    }
  }
}</pre>

<div class="hidden">
<pre class="brush: html notranslate">&lt;canvas id="canvas" width="150" height="150"&gt;&lt;/canvas&gt;</pre>

<pre class="brush: js notranslate">draw();</pre>
</div>

<p>{{EmbedLiveSample("An_example_using_rgba()", "180", "180", "https://mdn.mozillademos.org/files/246/Canvas_rgba.png")}}</p>

<h2 id="Line_styles" name="Line_styles">Line styles</h2>

<p>There are several properties which allow us to style lines.</p>

<dl>
 <dt>{{domxref("CanvasRenderingContext2D.lineWidth", "lineWidth = value")}}</dt>
 <dd>Sets the width of lines drawn in the future.</dd>
 <dt>{{domxref("CanvasRenderingContext2D.lineCap", "lineCap = type")}}</dt>
 <dd>Sets the appearance of the ends of lines.</dd>
 <dt>{{domxref("CanvasRenderingContext2D.lineJoin", "lineJoin = type")}}</dt>
 <dd>Sets the appearance of the "corners" where lines meet.</dd>
 <dt>{{domxref("CanvasRenderingContext2D.miterLimit", "miterLimit = value")}}</dt>
 <dd>Establishes a limit on the miter when two lines join at a sharp angle, to let you control how thick the junction becomes.</dd>
 <dt>{{domxref("CanvasRenderingContext2D.getLineDash", "getLineDash()")}}</dt>
 <dd>Returns the current line dash pattern array containing an even number of non-negative numbers.</dd>
 <dt>{{domxref("CanvasRenderingContext2D.setLineDash", "setLineDash(segments)")}}</dt>
 <dd>Sets the current line dash pattern.</dd>
 <dt>{{domxref("CanvasRenderingContext2D.lineDashOffset", "lineDashOffset = value")}}</dt>
 <dd>Specifies where to start a dash array on a line.</dd>
</dl>

<p>You'll get a better understanding of what these do by looking at the examples below.</p>

<h3 id="A_lineWidth_example" name="A_lineWidth_example">A <code>lineWidth</code> example</h3>

<p>This property sets the current line thickness. Values must be positive numbers. By default this value is set to 1.0 units.</p>

<p>The line width is the thickness of the stroke centered on the given path. In other words, the area that's drawn extends to half the line width on either side of the path. Because canvas coordinates do not directly reference pixels, special care must be taken to obtain crisp horizontal and vertical lines.</p>

<p>In the example below, 10 straight lines are drawn with increasing line widths. The line on the far left is 1.0 units wide. However, the leftmost and all other odd-integer-width thickness lines do not appear crisp, because of the path's positioning.</p>

<pre class="brush: js;highlight[4] notranslate">function draw() {
  var ctx = document.getElementById('canvas').getContext('2d');
  for (var i = 0; i &lt; 10; i++){
    ctx.lineWidth = 1+i;
    ctx.beginPath();
    ctx.moveTo(5+i*14,5);
    ctx.lineTo(5+i*14,140);
    ctx.stroke();
  }
}
</pre>

<div class="hidden">
<pre class="brush: html notranslate">&lt;canvas id="canvas" width="150" height="150"&gt;&lt;/canvas&gt;</pre>

<pre class="brush: js notranslate">draw();</pre>
</div>

<p>{{EmbedLiveSample("A_lineWidth_example", "180", "180", "https://mdn.mozillademos.org/files/239/Canvas_linewidth.png")}}</p>

<p>Obtaining crisp lines requires understanding how paths are stroked. In the images below, the grid represents the canvas coordinate grid. The squares between gridlines are actual on-screen pixels. In the first grid image below, a rectangle from (2,1) to (5,5) is filled. The entire area between them (light red) falls on pixel boundaries, so the resulting filled rectangle will have crisp edges.</p>

<p><img alt="" class="internal" src="https://mdn.mozillademos.org/files/201/Canvas-grid.png"></p>

<p>If you consider a path from (3,1) to (3,5) with a line thickness of <code>1.0</code>, you end up with the situation in the second image. The actual area to be filled (dark blue) only extends halfway into the pixels on either side of the path. An approximation of this has to be rendered, which means that those pixels being only partially shaded, and results in the entire area (the light blue and dark blue) being filled in with a color only half as dark as the actual stroke color. This is what happens with the <code>1.0</code> width line in the previous example code.</p>

<p>To fix this, you have to be very precise in your path creation. Knowing that a <code>1.0</code> width line will extend half a unit to either side of the path, creating the path from (3.5,1) to (3.5,5) results in the situation in the third image—the <code>1.0</code> line width ends up completely and precisely filling a single pixel vertical line.</p>

<div class="note">
<p><strong>Note:</strong> Be aware that in our vertical line example, the Y position still referenced an integer gridline position—if it hadn't, we would see pixels with half coverage at the endpoints (but note also that this behavior depends on the current <code>lineCap</code> style whose default value is <code>butt</code>; you may want to compute consistent strokes with half-pixel coordinates for odd-width lines, by setting the <code>lineCap</code> style to <code>square</code>, so that the outer border of the stroke around the endpoint will be automatically extended to cover the whole pixel exactly).</p>

<p>Note also that only start and final endpoints of a path are affected: if a path is closed with <code>closePath()</code>, there's no start and final endpoint; instead, all endpoints in the path are connected to their attached previous and next segment using the current setting of the <code>lineJoin</code> style, whose default value is <code>miter</code>, with the effect of automatically extending the outer borders of the connected segments to their intersection point, so that the rendered stroke will exactly cover full pixels centered at each endpoint if those connected segments are horizontal and/or vertical). See the next two sections for demonstrations of these additional line styles.</p>
</div>

<p>For even-width lines, each half ends up being an integer amount of pixels, so you want a path that is between pixels (that is, (3,1) to (3,5)), instead of down the middle of pixels.</p>

<p>While slightly painful when initially working with scalable 2D graphics, paying attention to the pixel grid and the position of paths ensures that your drawings will look correct regardless of scaling or any other transformations involved. A 1.0-width vertical line drawn at the correct position will become a crisp 2-pixel line when scaled up by 2, and will appear at the correct position.</p>

<h3 id="A_lineCap_example" name="A_lineCap_example">A <code>lineCap</code> example</h3>

<p>The <code>lineCap</code> property determines how the end points of every line are drawn. There are three possible values for this property and those are: <code>butt</code>, <code>round</code> and <code>square</code>. By default this property is set to <code>butt</code>.</p>

<p><img alt="" src="https://mdn.mozillademos.org/files/236/Canvas_linecap.png" style="float: right; height: 190px; width: 190px;"></p>

<dl>
 <dt><code>butt</code></dt>
 <dd>The ends of lines are squared off at the endpoints.</dd>
 <dt><code>round</code></dt>
 <dd>The ends of lines are rounded.</dd>
 <dt><code>square</code></dt>
 <dd>The ends of lines are squared off by adding a box with an equal width and half the height of the line's thickness.</dd>
</dl>

<p>In this example, we'll draw three lines, each with a different value for the <code>lineCap</code> property. I also added two guides to see the exact differences between the three. Each of these lines starts and ends exactly on these guides.</p>

<p>The line on the left uses the default <code>butt</code> option. You'll notice that it's drawn completely flush with the guides. The second is set to use the <code>round</code> option. This adds a semicircle to the end that has a radius half the width of the line. The line on the right uses the <code>square</code> option. This adds a box with an equal width and half the height of the line thickness.</p>

<pre class="brush: js;highlight[18] notranslate">function draw() {
  var ctx = document.getElementById('canvas').getContext('2d');
  var lineCap = ['butt','round','square'];

  // Draw guides
  ctx.strokeStyle = '#09f';
  ctx.beginPath();
  ctx.moveTo(10,10);
  ctx.lineTo(140,10);
  ctx.moveTo(10,140);
  ctx.lineTo(140,140);
  ctx.stroke();

  // Draw lines
  ctx.strokeStyle = 'black';
  for (var i=0;i&lt;lineCap.length;i++){
    ctx.lineWidth = 15;
    ctx.lineCap = lineCap[i];
    ctx.beginPath();
    ctx.moveTo(25+i*50,10);
    ctx.lineTo(25+i*50,140);
    ctx.stroke();
  }
}
</pre>

<div class="hidden">
<pre class="brush: html notranslate">&lt;canvas id="canvas" width="150" height="150"&gt;&lt;/canvas&gt;</pre>

<pre class="brush: js notranslate">draw();</pre>
</div>

<p>{{EmbedLiveSample("A_lineCap_example", "180", "180", "https://mdn.mozillademos.org/files/236/Canvas_linecap.png")}}</p>

<h3 id="A_lineJoin_example" name="A_lineJoin_example">A <code>lineJoin</code> example</h3>

<p>The <code>lineJoin</code> property determines how two connecting segments (of lines, arcs or curves) with non-zero lengths in a shape are joined together (degenerate segments with zero lengths, whose specified endpoints and control points are exactly at the same position, are skipped).</p>

<p>There are three possible values for this property: <code>round</code>, <code>bevel</code> and <code>miter</code>. By default this property is set to <code>miter</code>. Note that the <code>lineJoin</code> setting has no effect if the two connected segments have the same direction, because no joining area will be added in this case.</p>

<p><img alt="" src="https://mdn.mozillademos.org/files/237/Canvas_linejoin.png" style="float: right; height: 190px; width: 190px;"></p>

<dl>
 <dt><code>round</code></dt>
 <dd>Rounds off the corners of a shape by filling an additional sector of disc centered at the common endpoint of connected segments. The radius for these rounded corners is equal to half the line width.</dd>
 <dt><code>bevel</code></dt>
 <dd>Fills an additional triangular area between the common endpoint of connected segments, and the separate outside rectangular corners of each segment.</dd>
 <dt><code>miter</code></dt>
 <dd>Connected segments are joined by extending their outside edges to connect at a single point, with the effect of filling an additional lozenge-shaped area. This setting is effected by the <code>miterLimit</code> property which is explained below.</dd>
</dl>

<p>The example below draws three different paths, demonstrating each of these three <code>lineJoin</code> property settings; the output is shown above.</p>

<pre class="brush: js;highlight[6] notranslate">function draw() {
  var ctx = document.getElementById('canvas').getContext('2d');
  var lineJoin = ['round','bevel','miter'];
  ctx.lineWidth = 10;
  for (var i=0;i&lt;lineJoin.length;i++){
    ctx.lineJoin = lineJoin[i];
    ctx.beginPath();
    ctx.moveTo(-5,5+i*40);
    ctx.lineTo(35,45+i*40);
    ctx.lineTo(75,5+i*40);
    ctx.lineTo(115,45+i*40);
    ctx.lineTo(155,5+i*40);
    ctx.stroke();
  }
}
</pre>

<div class="hidden">
<pre class="brush: html notranslate">&lt;canvas id="canvas" width="150" height="150"&gt;&lt;/canvas&gt;</pre>

<pre class="brush: js notranslate">draw();</pre>
</div>

<p>{{EmbedLiveSample("A_lineJoin_example", "180", "180", "https://mdn.mozillademos.org/files/237/Canvas_linejoin.png")}}</p>

<h3 id="A_demo_of_the_miterLimit_property" name="A_demo_of_the_miterLimit_property">A demo of the <code>miterLimit</code> property</h3>

<p>As you've seen in the previous example, when joining two lines with the <code>miter</code> option, the outside edges of the two joining lines are extended up to the point where they meet. For lines which are at large angles with each other, this point is not far from the inside connection point. However, as the angles between each line decreases, the distance (miter length) between these points increases exponentially.</p>

<p>The <code>miterLimit</code> property determines how far the outside connection point can be placed from the inside connection point. If two lines exceed this value, a bevel join gets drawn instead. Note that the maximum miter length is the product of the line width measured in the current coordinate system, by the value of this <code>miterLimit</code> property (whose default value is 10.0 in the HTML {{HTMLElement("canvas")}}), so the <code>miterLimit</code> can be set independently from the current display scale or any affine transforms of paths: it only influences the effectively rendered shape of line edges.</p>

<p>More exactly, the miter limit is the maximum allowed ratio of the extension length (in the HTML canvas, it is measured between the outside corner of the joined edges of the line and the common endpoint of connecting segments specified in the path) to half the line width. It can equivalently be defined as the maximum allowed ratio of the distance between the inside and outside points of jonction of edges, to the total line width. It is then equal to the cosecant of half the minimum inner angle of connecting segments below which no miter join will be rendered, but only a bevel join:</p>

<ul>
 <li><code>miterLimit</code> = <strong>max</strong> <code>miterLength</code> / <code>lineWidth</code> = 1 / <strong>sin</strong> ( <strong>min</strong> <em>θ</em> / 2 )</li>
 <li>The default miter limit of 10.0 will strip all miters for sharp angles below about 11 degrees.</li>
 <li>A miter limit equal to √2 ≈ 1.4142136 (rounded up) will strip miters for all acute angles, keeping miter joins only for obtuse or right angles.</li>
 <li>A miter limit equal to 1.0 is valid but will disable all miters.</li>
 <li>Values below 1.0 are invalid for the miter limit.</li>
</ul>

<p>Here's a little demo in which you can set <code>miterLimit</code> dynamically and see how this effects the shapes on the canvas. The blue lines show where the start and endpoints for each of the lines in the zig-zag pattern are.</p>

<p>If you specify a <code>miterLimit</code> value below 4.2 in this demo, none of the visible corners will join with a miter extension, but only with a small bevel near the blue lines; with a <code>miterLimit</code> above 10, most corners in this demo should join with a miter far away from the blue lines, and whose height is decreasing between corners from left to right because they connect with growing angles; with intermediate values, the corners on the left side will only join with a bevel near the blue lines, and the corners on the right side with a miter extension (also with a decreasing height).</p>

<pre class="brush: js;highlight[18] notranslate">function draw() {
  var ctx = document.getElementById('canvas').getContext('2d');

  // Clear canvas
  ctx.clearRect(0,0,150,150);

  // Draw guides
  ctx.strokeStyle = '#09f';
  ctx.lineWidth   = 2;
  ctx.strokeRect(-5,50,160,50);

  // Set line styles
  ctx.strokeStyle = '#000';
  ctx.lineWidth = 10;

  // check input
  if (document.getElementById('miterLimit').value.match(/\d+(\.\d+)?/)) {
    ctx.miterLimit = parseFloat(document.getElementById('miterLimit').value);
  } else {
    alert('Value must be a positive number');
  }

  // Draw lines
  ctx.beginPath();
  ctx.moveTo(0,100);
  for (i=0;i&lt;24;i++){
    var dy = i%2==0 ? 25 : -25 ;
    ctx.lineTo(Math.pow(i,1.5)*2,75+dy);
  }
  ctx.stroke();
  return false;
}
</pre>

<div class="hidden">
<pre class="brush: html notranslate">&lt;table&gt;
  &lt;tr&gt;
    &lt;td&gt;&lt;canvas id="canvas" width="150" height="150"&gt;&lt;/canvas&gt;&lt;/td&gt;
    &lt;td&gt;Change the &lt;code&gt;miterLimit&lt;/code&gt; by entering a new value below and clicking the redraw button.&lt;br&gt;&lt;br&gt;
      &lt;form onsubmit="return draw();"&gt;
        &lt;label&gt;Miter limit&lt;/label&gt;
        &lt;input type="text" size="3" id="miterLimit"/&gt;
        &lt;input type="submit" value="Redraw"/&gt;
      &lt;/form&gt;
    &lt;/td&gt;
  &lt;/tr&gt;
&lt;/table&gt;</pre>

<pre class="brush: js notranslate">document.getElementById('miterLimit').value = document.getElementById('canvas').getContext('2d').miterLimit;
draw();</pre>
</div>

<p>{{EmbedLiveSample("A_demo_of_the_miterLimit_property", "400", "180", "https://mdn.mozillademos.org/files/240/Canvas_miterlimit.png")}}</p>

<h3 id="Using_line_dashes">Using line dashes</h3>

<p>The <code>setLineDash</code> method and the <code>lineDashOffset</code> property specify the dash pattern for lines. The <code>setLineDash</code> method accepts a list of numbers that specifies distances to alternately draw a line and a gap and the <code>lineDashOffset</code> property sets an offset where to start the pattern.</p>

<p>In this example we are creating a marching ants effect. It is an animation technique often found in <span class="new">selection</span> tools of computer graphics programs. It helps the user to distinguish the selection border from the image background by animating the border. In a later part of this tutorial, you can learn how to do this and other <a href="/ja/docs/Web/API/Canvas_API/Tutorial/Basic_animations">basic animations</a>.</p>

<div class="hidden">
<pre class="brush: html notranslate">&lt;canvas id="canvas" width="110" height="110"&gt;&lt;/canvas&gt;</pre>
</div>

<pre class="brush: js;highlight[6] notranslate">var ctx = document.getElementById('canvas').getContext('2d');
var offset = 0;

function draw() {
  ctx.clearRect(0,0, canvas.width, canvas.height);
  ctx.setLineDash([4, 2]);
  ctx.lineDashOffset = -offset;
  ctx.strokeRect(10,10, 100, 100);
}

function march() {
  offset++;
  if (offset &gt; 16) {
    offset = 0;
  }
  draw();
  setTimeout(march, 20);
}

march();</pre>

<p>{{EmbedLiveSample("Using_line_dashes", "120", "120", "https://mdn.mozillademos.org/files/9853/marching-ants.png")}}</p>

<h2 id="Gradients" name="Gradients">Gradients</h2>

<p>Just like any normal drawing program, we can fill and stroke shapes using linear and radial gradients. We create a {{domxref("CanvasGradient")}} object by using one of the following methods. We can then assign this object to the <code>fillStyle</code> or <code>strokeStyle</code> properties.</p>

<dl>
 <dt>{{domxref("CanvasRenderingContext2D.createLinearGradient", "createLinearGradient(x1, y1, x2, y2)")}}</dt>
 <dd>Creates a linear gradient object with a starting point of (<code>x1</code>, <code>y1</code>) and an end point of (<code>x2</code>, <code>y2</code>).</dd>
 <dt>{{domxref("CanvasRenderingContext2D.createRadialGradient", "createRadialGradient(x1, y1, r1, x2, y2, r2)")}}</dt>
 <dd>Creates a radial gradient. The parameters represent two circles, one with its center at (<code>x1</code>, <code>y1</code>) and a radius of <code>r1</code>, and the other with its center at (<code>x2</code>, <code>y2</code>) with a radius of <code>r2</code>.</dd>
</dl>

<p>For example:</p>

<pre class="brush: js notranslate">var lineargradient = ctx.createLinearGradient(0, 0, 150, 150);
var radialgradient = ctx.createRadialGradient(75, 75, 0, 75, 75, 100);
</pre>

<p>Once we've created a <code>CanvasGradient</code> object we can assign colors to it by using the <code>addColorStop()</code> method.</p>

<dl>
 <dt>{{domxref("CanvasGradient.addColorStop", "gradient.addColorStop(position, color)")}}</dt>
 <dd>Creates a new color stop on the <code>gradient</code> object. The <code>position</code> is a number between 0.0 and 1.0 and defines the relative position of the color in the gradient, and the <code>color</code> argument must be a string representing a CSS {{cssxref("&lt;color&gt;")}}, indicating the color the gradient should reach at that offset into the transition.</dd>
</dl>

<p>You can add as many color stops to a gradient as you need. Below is a very simple linear gradient from white to black.</p>

<pre class="brush: js notranslate">var lineargradient = ctx.createLinearGradient(0,0,150,150);
lineargradient.addColorStop(0, 'white');
lineargradient.addColorStop(1, 'black');
</pre>

<h3 id="A_createLinearGradient_example" name="A_createLinearGradient_example">A <code>createLinearGradient</code> example</h3>

<p>In this example, we'll create two different gradients. As you can see here, both the <code>strokeStyle</code> and <code>fillStyle</code> properties can accept a <code>canvasGradient</code> object as valid input.</p>

<pre class="brush: js;highlight[5,11] notranslate">function draw() {
  var ctx = document.getElementById('canvas').getContext('2d');

  // Create gradients
  var lingrad = ctx.createLinearGradient(0,0,0,150);
  lingrad.addColorStop(0, '#00ABEB');
  lingrad.addColorStop(0.5, '#fff');
  lingrad.addColorStop(0.5, '#26C000');
  lingrad.addColorStop(1, '#fff');

  var lingrad2 = ctx.createLinearGradient(0,50,0,95);
  lingrad2.addColorStop(0.5, '#000');
  lingrad2.addColorStop(1, 'rgba(0,0,0,0)');

  // assign gradients to fill and stroke styles
  ctx.fillStyle = lingrad;
  ctx.strokeStyle = lingrad2;

  // draw shapes
  ctx.fillRect(10,10,130,130);
  ctx.strokeRect(50,50,50,50);

}
</pre>

<div class="hidden">
<pre class="brush: html notranslate">&lt;canvas id="canvas" width="150" height="150"&gt;&lt;/canvas&gt;</pre>

<pre class="brush: js notranslate">draw();</pre>
</div>

<p>The first is a background gradient. As you can see, we assigned two colors at the same position. You do this to make very sharp color transitions—in this case from white to green. Normally, it doesn't matter in what order you define the color stops, but in this special case, it does significantly. If you keep the assignments in the order you want them to appear, this won't be a problem.</p>

<p>In the second gradient, we didn't assign the starting color (at position 0.0) since it wasn't strictly necessary, because it will automatically assume the color of the next color stop. Therefore, assigning the black color at position 0.5 automatically makes the gradient, from the start to this stop, black.</p>

<p>{{EmbedLiveSample("A_createLinearGradient_example", "180", "180", "https://mdn.mozillademos.org/files/235/Canvas_lineargradient.png")}}</p>

<h3 id="A_createRadialGradient_example" name="A_createRadialGradient_example">A <code>createRadialGradient</code> example</h3>

<p>In this example, we'll define four different radial gradients. Because we have control over the start and closing points of the gradient, we can achieve more complex effects than we would normally have in the "classic" radial gradients we see in, for instance, Photoshop (that is, a gradient with a single center point where the gradient expands outward in a circular shape).</p>

<pre class="brush: js;highlight[5,10,15,20] notranslate">function draw() {
  var ctx = document.getElementById('canvas').getContext('2d');

  // Create gradients
  var radgrad = ctx.createRadialGradient(45,45,10,52,50,30);
  radgrad.addColorStop(0, '#A7D30C');
  radgrad.addColorStop(0.9, '#019F62');
  radgrad.addColorStop(1, 'rgba(1,159,98,0)');

  var radgrad2 = ctx.createRadialGradient(105,105,20,112,120,50);
  radgrad2.addColorStop(0, '#FF5F98');
  radgrad2.addColorStop(0.75, '#FF0188');
  radgrad2.addColorStop(1, 'rgba(255,1,136,0)');

  var radgrad3 = ctx.createRadialGradient(95,15,15,102,20,40);
  radgrad3.addColorStop(0, '#00C9FF');
  radgrad3.addColorStop(0.8, '#00B5E2');
  radgrad3.addColorStop(1, 'rgba(0,201,255,0)');

  var radgrad4 = ctx.createRadialGradient(0,150,50,0,140,90);
  radgrad4.addColorStop(0, '#F4F201');
  radgrad4.addColorStop(0.8, '#E4C700');
  radgrad4.addColorStop(1, 'rgba(228,199,0,0)');

  // draw shapes
  ctx.fillStyle = radgrad4;
  ctx.fillRect(0,0,150,150);
  ctx.fillStyle = radgrad3;
  ctx.fillRect(0,0,150,150);
  ctx.fillStyle = radgrad2;
  ctx.fillRect(0,0,150,150);
  ctx.fillStyle = radgrad;
  ctx.fillRect(0,0,150,150);
}
</pre>

<div class="hidden">
<pre class="brush: html notranslate">&lt;canvas id="canvas" width="150" height="150"&gt;&lt;/canvas&gt;</pre>

<pre class="brush: js notranslate">draw();</pre>
</div>

<p>In this case, we've offset the starting point slightly from the end point to achieve a spherical 3D effect. It's best to try to avoid letting the inside and outside circles overlap because this results in strange effects which are hard to predict.</p>

<p>The last color stop in each of the four gradients uses a fully transparent color. If you want to have a nice transition from this to the previous color stop, both colors should be equal. This isn't very obvious from the code because it uses two different CSS color methods as a demonstration, but in the first gradient <code>#019F62 = rgba(1,159,98,1)</code>.</p>

<p>{{EmbedLiveSample("A_createRadialGradient_example", "180", "180", "https://mdn.mozillademos.org/files/244/Canvas_radialgradient.png")}}</p>

<h2 id="Patterns" name="Patterns">Patterns</h2>

<p>In one of the examples on the previous page, we used a series of loops to create a pattern of images. There is, however, a much simpler method: the <code>createPattern()</code> method.</p>

<dl>
 <dt>{{domxref("CanvasRenderingContext2D.createPattern", "createPattern(image, type)")}}</dt>
 <dd>Creates and returns a new canvas pattern object. <code>image</code> is a {{domxref("CanvasImageSource")}} (that is, an {{domxref("HTMLImageElement")}}, another canvas, a {{HTMLElement("video")}} element, or the like. <code>type</code> is a string indicating how to use the image.</dd>
</dl>

<p>The type specifies how to use the image in order to create the pattern, and must be one of the following string values:</p>

<dl>
 <dt><code>repeat</code></dt>
 <dd>Tiles the image in both vertical and horizontal directions.</dd>
 <dt><code>repeat-x</code></dt>
 <dd>Tiles the image horizontally but not vertically.</dd>
 <dt><code>repeat-y</code></dt>
 <dd>Tiles the image vertically but not horizontally.</dd>
 <dt><code>no-repeat</code></dt>
 <dd>Doesn't tile the image. It's used only once.</dd>
</dl>

<p>We use this method to create a {{domxref("CanvasPattern")}} object which is very similar to the gradient methods we've seen above. Once we've created a pattern, we can assign it to the <code>fillStyle</code> or <code>strokeStyle</code> properties. For example:</p>

<pre class="brush: js notranslate">var img = new Image();
img.src = 'someimage.png';
var ptrn = ctx.createPattern(img,'repeat');
</pre>

<div class="note">
<p><strong>Note:</strong> Like with the <code>drawImage()</code> method, you must make sure the image you use is loaded before calling this method or the pattern may be drawn incorrectly.</p>
</div>

<h3 id="A_createPattern_example" name="A_createPattern_example">A <code>createPattern</code> example</h3>

<p>In this last example, we'll create a pattern to assign to the <code>fillStyle</code> property. The only thing worth noting is the use of the image's <code>onload</code> handler. This is to make sure the image is loaded before it is assigned to the pattern.</p>

<pre class="brush: js;highlight[10] notranslate">function draw() {
  var ctx = document.getElementById('canvas').getContext('2d');

  // create new image object to use as pattern
  var img = new Image();
  img.src = 'https://mdn.mozillademos.org/files/222/Canvas_createpattern.png';
  img.onload = function(){

    // create pattern
    var ptrn = ctx.createPattern(img,'repeat');
    ctx.fillStyle = ptrn;
    ctx.fillRect(0,0,150,150);

  }
}
</pre>

<div class="hidden">
<pre class="brush: html notranslate">&lt;canvas id="canvas" width="150" height="150"&gt;&lt;/canvas&gt;</pre>

<pre class="brush: js notranslate">draw();</pre>

<p>The result looks like this:</p>
</div>

<p>{{EmbedLiveSample("A_createPattern_example", "180", "180", "https://mdn.mozillademos.org/files/222/Canvas_createpattern.png")}}</p>

<h2 id="Shadows">Shadows</h2>

<p>Using shadows involves just four properties:</p>

<dl>
 <dt>{{domxref("CanvasRenderingContext2D.shadowOffsetX", "shadowOffsetX = float")}}</dt>
 <dd>Indicates the horizontal distance the shadow should extend from the object. This value isn't affected by the transformation matrix. The default is 0.</dd>
 <dt>{{domxref("CanvasRenderingContext2D.shadowOffsetY", "shadowOffsetY = float")}}</dt>
 <dd>Indicates the vertical distance the shadow should extend from the object. This value isn't affected by the transformation matrix. The default is 0.</dd>
 <dt>{{domxref("CanvasRenderingContext2D.shadowBlur", "shadowBlur = float")}}</dt>
 <dd>Indicates the size of the blurring effect; this value doesn't correspond to a number of pixels and is not affected by the current transformation matrix. The default value is 0.</dd>
 <dt>{{domxref("CanvasRenderingContext2D.shadowColor", "shadowColor = color")}}</dt>
 <dd>A standard CSS color value indicating the color of the shadow effect; by default, it is fully-transparent black.</dd>
</dl>

<p>The properties <code>shadowOffsetX</code> and <code>shadowOffsetY</code> indicate how far the shadow should extend from the object in the X and Y directions; these values aren't affected by the current transformation matrix. Use negative values to cause the shadow to extend up or to the left, and positive values to cause the shadow to extend down or to the right. These are both 0 by default.</p>

<p>The <code>shadowBlur</code> property indicates the size of the blurring effect; this value doesn't correspond to a number of pixels and is not affected by the current transformation matrix. The default value is 0.</p>

<p>The <code>shadowColor</code> property is a standard CSS color value indicating the color of the shadow effect; by default, it is fully-transparent black.</p>

<div class="note">
<p><strong>Note:</strong> Shadows are only drawn for <code>source-over</code> <a href="/ja/docs/Web/API/Canvas_API/Tutorial/Compositing" title="Web/Guide/HTML/Canvas_tutorial/Compositing">compositing operations</a>.</p>
</div>

<h3 id="A_shadowed_text_example">A shadowed text example</h3>

<p>This example draws a text string with a shadowing effect.</p>

<pre class="brush: js;highlight[4,5,6,7] notranslate">function draw() {
  var ctx = document.getElementById('canvas').getContext('2d');

  ctx.shadowOffsetX = 2;
  ctx.shadowOffsetY = 2;
  ctx.shadowBlur = 2;
  ctx.shadowColor = "rgba(0, 0, 0, 0.5)";

  ctx.font = "20px Times New Roman";
  ctx.fillStyle = "Black";
  ctx.fillText("Sample String", 5, 30);
}
</pre>

<div class="hidden">
<pre class="brush: html notranslate">&lt;canvas id="canvas" width="150" height="80"&gt;&lt;/canvas&gt;</pre>

<pre class="brush: js notranslate">draw();</pre>
</div>

<p>{{EmbedLiveSample("A_shadowed_text_example", "180", "100", "https://mdn.mozillademos.org/files/2505/shadowed-string.png")}}</p>

<p>We will look at the <code>font</code> property and <code>fillText</code> method in the next chapter about <a href="/ja/docs/Web/API/Canvas_API/Tutorial/Drawing_text">drawing text</a>.</p>

<h2 id="Canvas_fill_rules">Canvas fill rules</h2>

<p>When using <code>fill</code> (or {{domxref("CanvasRenderingContext2D.clip", "clip")}} and {{domxref("CanvasRenderingContext2D.isPointInPath", "isPointinPath")}}) you can optionally provide a fill rule algorithm by which to determine if a point is inside or outside a path and thus if it gets filled or not. This is useful when a path intersects itself or is nested.<br>
 <br>
 Two values are possible:</p>

<ul>
 <li><code><strong>"nonzero</strong></code>": The <a class="external external-icon" href="http://en.wikipedia.org/wiki/Nonzero-rule">non-zero winding rule</a>, which is the default rule.</li>
 <li><code><strong>"evenodd"</strong></code>: The <a class="external external-icon" href="http://en.wikipedia.org/wiki/Even%E2%80%93odd_rule">even-odd winding rule</a>.</li>
</ul>

<p>In this example we are using the <code>evenodd</code> rule.</p>

<pre class="brush: js;highlight[6] notranslate">function draw() {
  var ctx = document.getElementById('canvas').getContext('2d');
  ctx.beginPath();
  ctx.arc(50, 50, 30, 0, Math.PI*2, true);
  ctx.arc(50, 50, 15, 0, Math.PI*2, true);
  ctx.fill("evenodd");
}</pre>

<div class="hidden">
<pre class="brush: html notranslate">&lt;canvas id="canvas" width="100" height="100"&gt;&lt;/canvas&gt;</pre>

<pre class="brush: js notranslate">draw();</pre>
</div>

<p>{{EmbedLiveSample("Canvas_fill_rules", "110", "110", "https://mdn.mozillademos.org/files/9855/fill-rule.png")}}</p>

<p>{{PreviousNext("Web/API/Canvas_API/Tutorial/Drawing_shapes", "Web/API/Canvas_API/Tutorial/Drawing_text")}}</p>