
# example.js

``` javascript
require("./style.css");
require(["./chunk"]);
```

# style.css

``` css
body {
	background: url(image.png);
}
```

# chunk.js

``` javascript
require("./style2.css");
```

# style2.css

``` css
.xyz {
	background: url(image2.png);
}
```

# webpack.config.js

``` javascript
var ExtractTextPlugin = require("extract-text-webpack-plugin");
module.exports = {
	module: {
		loaders: [
			{
				test: /\.css$/,
				use: ExtractTextPlugin.extract({
					fallback: "style-loader",
					use: "css-loader"
				})
			},
			{ test: /\.png$/, loader: "file-loader" }
		]
	},
	plugins: [
		new ExtractTextPlugin({
			filename: "style.css"
		})
	]
};
```

# js/style.css

``` javascript
body {
	background: url(js/ce21cbdd9b894e6af794813eb3fdaf60.png);
}
```

# Info

## Uncompressed

```
Hash: b16a9606921e57d7ecd8
Version: webpack 2.2.1
                               Asset       Size  Chunks             Chunk Names
ce21cbdd9b894e6af794813eb3fdaf60.png  119 bytes          [emitted]  
                         0.output.js    2.25 kB       0  [emitted]  
                           output.js    15.3 kB       1  [emitted]  main
                           style.css   68 bytes       1  [emitted]  main
Entrypoint main = output.js style.css
chunk    {0} 0.output.js 1.25 kB {1} [rendered]
    > [2] ./example.js 2:0-20
    [1] ./chunk.js 25 bytes {0} [built]
        amd require ./chunk [2] ./example.js 2:0-20
    [5] (webpack)/~/css-loader!./style2.css 223 bytes {0} [built]
        cjs require !!./../../node_modules/css-loader/index.js!./style2.css [6] ./style2.css 4:14-80
    [6] ./style2.css 922 bytes {0} [built]
        cjs require ./style2.css [1] ./chunk.js 1:0-23
    [7] ./image2.png 82 bytes {0} [built]
        cjs require ./image2.png [5] (webpack)/~/css-loader!./style2.css 6:56-79
chunk    {1} output.js, style.css (main) 8.75 kB [entry] [rendered]
    > main [2] ./example.js 
    [0] ./style.css 41 bytes {1} [built]
        cjs require ./style.css [2] ./example.js 1:0-22
    [2] ./example.js 46 bytes {1} [built]
    [3] (webpack)/~/css-loader/lib/css-base.js 1.51 kB {1} [built]
        cjs require ./../../node_modules/css-loader/lib/css-base.js [5] (webpack)/~/css-loader!./style2.css 1:27-85
        cjs require ./../../node_modules/css-loader/lib/css-base.js [8] (webpack)/~/css-loader!./style.css 1:27-85
    [4] (webpack)/~/style-loader/addStyles.js 7.15 kB {1} [built]
        cjs require !./../../node_modules/style-loader/addStyles.js [6] ./style2.css 7:13-71
Child extract-text-webpack-plugin:
    Entrypoint undefined = extract-text-webpack-plugin-output-filename
    chunk    {0} extract-text-webpack-plugin-output-filename 1.81 kB [entry] [rendered]
        > [2] (webpack)/~/css-loader!./style.css 
        [0] (webpack)/~/css-loader/lib/css-base.js 1.51 kB {0} [built]
            cjs require ./../../node_modules/css-loader/lib/css-base.js [2] (webpack)/~/css-loader!./style.css 1:27-85
        [1] ./image.png 82 bytes {0} [built]
            cjs require ./image.png [2] (webpack)/~/css-loader!./style.css 6:56-78
        [2] (webpack)/~/css-loader!./style.css 222 bytes {0} [built]
```

## Minimized (uglify-js, no zip)

```
Hash: 74b8d1cfd74bb5650705
Version: webpack 2.2.1
                               Asset       Size  Chunks             Chunk Names
ce21cbdd9b894e6af794813eb3fdaf60.png  119 bytes          [emitted]  
                         0.output.js  309 bytes       0  [emitted]  
                           output.js    5.07 kB       1  [emitted]  main
                           style.css   61 bytes       1  [emitted]  main
Entrypoint main = output.js style.css
chunk    {0} 0.output.js 1.24 kB {1} [rendered]
    > [2] ./example.js 2:0-20
    [1] ./chunk.js 25 bytes {0} [built]
        amd require ./chunk [2] ./example.js 2:0-20
    [5] (webpack)/~/css-loader!./style2.css 212 bytes {0} [built]
        cjs require !!./../../node_modules/css-loader/index.js!./style2.css [6] ./style2.css 4:14-80
    [6] ./style2.css 922 bytes {0} [built]
        cjs require ./style2.css [1] ./chunk.js 1:0-23
    [7] ./image2.png 82 bytes {0} [built]
        cjs require ./image2.png [5] (webpack)/~/css-loader!./style2.css 6:50-73
chunk    {1} output.js, style.css (main) 8.75 kB [entry] [rendered]
    > main [2] ./example.js 
    [0] ./style.css 41 bytes {1} [built]
        cjs require ./style.css [2] ./example.js 1:0-22
    [2] ./example.js 46 bytes {1} [built]
    [3] (webpack)/~/css-loader/lib/css-base.js 1.51 kB {1} [built]
        cjs require ./../../node_modules/css-loader/lib/css-base.js [5] (webpack)/~/css-loader!./style2.css 1:27-85
        cjs require ./../../node_modules/css-loader/lib/css-base.js [8] (webpack)/~/css-loader!./style.css 1:27-85
    [4] (webpack)/~/style-loader/addStyles.js 7.15 kB {1} [built]
        cjs require !./../../node_modules/style-loader/addStyles.js [6] ./style2.css 7:13-71
Child extract-text-webpack-plugin:
    Entrypoint undefined = extract-text-webpack-plugin-output-filename
    chunk    {0} extract-text-webpack-plugin-output-filename 1.8 kB [entry] [rendered]
        > [2] (webpack)/~/css-loader!./style.css 
        [0] (webpack)/~/css-loader/lib/css-base.js 1.51 kB {0} [built]
            cjs require ./../../node_modules/css-loader/lib/css-base.js [2] (webpack)/~/css-loader!./style.css 1:27-85
        [1] ./image.png 82 bytes {0} [built]
            cjs require ./image.png [2] (webpack)/~/css-loader!./style.css 6:50-72
        [2] (webpack)/~/css-loader!./style.css 211 bytes {0} [built]
```
