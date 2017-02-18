This example demonstrates the AggressiveSplittingPlugin for splitting the bundle into multiple smaller chunks to improve caching. This works best with a HTTP2 web server elsewise there is an overhead for the increased number of requests.

The AggressiveSplittingPlugin split every chunk until it reaches the specified `maxSize`. In this example it tries to create chunks with <50kB code (after minimizing this reduces to ~10kB). It groups modules together by folder structure. We assume modules in the same folder as similar likely to change and minimize and gzip good together.

The AggressiveSplittingPlugin records it's splitting in the webpack records and try to restore splitting from records. This ensures that after changes to the application old splittings (and chunks) are reused. They are probably already in the clients cache. Therefore it's heavily recommended to use records!

Only chunks which are bigger than the specified `minSize` are stored into the records. This ensures that these chunks fill up as your application grows, instead of creating too many chunks for every change.

Chunks can get invalid if a module changes. Modules from invalid chunks go back into the module pool and new chunks are created from all modules in the pool.

There is a tradeoff here:

The caching improves with smaller `maxSize`, as chunks change less often and can be reused more often after an update.

The compression improves with bigger `maxSize`, as gzip works better for bigger files. It's more likely to find duplicate strings, etc.

The backward compatibility (non HTTP2 client) improves with bigger `maxSize`, as the number of requests decreases.

``` js
var path = require("path");
var webpack = require("../../");
module.exports = {
	entry: "./example",
	output: {
		path: path.join(__dirname, "js"),
		filename: "[chunkhash].js",
		chunkFilename: "[chunkhash].js"
	},
	plugins: [
		new webpack.optimize.AggressiveSplittingPlugin({
			minSize: 30000,
			maxSize: 50000
		}),
		new webpack.DefinePlugin({
			"process.env.NODE_ENV": JSON.stringify("production")
		})
	],
	recordsOutputPath: path.join(__dirname, "js", "records.json")
};
```

# Info

## Uncompressed

```
Hash: 00f5d308d70e00fe7713
Version: webpack 2.2.1
                  Asset     Size  Chunks             Chunk Names
4f84f005b0c46df43fd2.js  52.3 kB       7  [emitted]  
6b21c67c0964251394af.js  58.2 kB       0  [emitted]  
d360e468c99903c2d129.js    56 kB       2  [emitted]  
76860534ab8ba5e484a2.js  54.4 kB       3  [emitted]  
b75a1bd78f5b92799a11.js  53.8 kB       4  [emitted]  
8b27bb3161668f698375.js  53.9 kB       5  [emitted]  
1a42bbc60adaf6991bb7.js  53.4 kB       6  [emitted]  
6df160cf3935656c5665.js  43.3 kB       1  [emitted]  
3d93b56502b22b64f4bc.js  52.1 kB       8  [emitted]  
00d4d2a2ad4d2da7b940.js  32.8 kB       9  [emitted]  
3b0e2394af1c44c15c75.js    52 kB      10  [emitted]  
24047db0d33992324c0e.js  51.5 kB      11  [emitted]  
34d435e6fa54d2244802.js  58.5 kB      12  [emitted]  
8781e3116c98876a6b44.js    26 kB      13  [emitted]  
0baa068ca7eedead78e0.js    33 kB      14  [emitted]  
Entrypoint main = 34d435e6fa54d2244802.js 8781e3116c98876a6b44.js 0baa068ca7eedead78e0.js
chunk    {0} 6b21c67c0964251394af.js 49.8 kB {12} {13} {14} [rendered] [recorded]
    > aggressive-splitted [16] ./example.js 2:0-22
   [30] (webpack)/~/react-dom/lib/reactProdInvariant.js 1.24 kB {0} [built]
   [49] (webpack)/~/react-dom/lib/Transaction.js 9.45 kB {0} [built]
   [50] (webpack)/~/react-dom/lib/escapeTextContentForBrowser.js 3.43 kB {0} [built]
   [62] (webpack)/~/react-dom/lib/getEventCharCode.js 1.5 kB {0} [built]
   [63] (webpack)/~/react-dom/lib/getEventModifierState.js 1.23 kB {0} [built]
   [64] (webpack)/~/react-dom/lib/getEventTarget.js 1.01 kB {0} [built]
   [65] (webpack)/~/react-dom/lib/isEventSupported.js 1.94 kB {0} [built]
   [83] (webpack)/~/react-dom/lib/forEachAccumulated.js 855 bytes {0} [built]
   [84] (webpack)/~/react-dom/lib/getHostComponentFromComposite.js 740 bytes {0} [built]
   [85] (webpack)/~/react-dom/lib/getTextContentAccessor.js 955 bytes {0} [built]
   [86] (webpack)/~/react-dom/lib/instantiateReactComponent.js 5.05 kB {0} [built]
   [87] (webpack)/~/react-dom/lib/isTextInputElement.js 1.04 kB {0} [built]
   [88] (webpack)/~/react-dom/lib/setTextContent.js 1.45 kB {0} [built]
   [91] (webpack)/~/react-dom/~/fbjs/lib/emptyObject.js 458 bytes {0} [built]
  [148] (webpack)/~/react-dom/lib/adler32.js 1.19 kB {0} [built]
  [149] (webpack)/~/react-dom/lib/dangerousStyleValue.js 3.02 kB {0} [built]
  [150] (webpack)/~/react-dom/lib/findDOMNode.js 2.46 kB {0} [built]
  [151] (webpack)/~/react-dom/lib/flattenChildren.js 2.77 kB {0} [built]
  [152] (webpack)/~/react-dom/lib/getEventKey.js 2.87 kB {0} [built]
  [153] (webpack)/~/react-dom/lib/getIteratorFn.js 1.12 kB {0} [built]
  [154] (webpack)/~/react-dom/lib/getNextDebugID.js 437 bytes {0} [built]
  [155] (webpack)/~/react-dom/lib/getNodeForCharacterOffset.js 1.62 kB {0} [built]
  [156] (webpack)/~/react-dom/lib/getVendorPrefixedEventName.js 2.87 kB {0} [built]
  [157] (webpack)/~/react-dom/lib/quoteAttributeValueForBrowser.js 700 bytes {0} [built]
  [158] (webpack)/~/react-dom/lib/renderSubtreeIntoContainer.js 422 bytes {0} [built]
chunk    {1} 6df160cf3935656c5665.js 37.3 kB {12} {13} {14} [rendered] [recorded]
    > aggressive-splitted [16] ./example.js 2:0-22
   [29] (webpack)/~/react-dom/~/fbjs/lib/invariant.js 1.63 kB {1} [built]
   [31] (webpack)/~/react-dom/~/fbjs/lib/warning.js 2.1 kB {1} [built]
   [33] (webpack)/~/react-dom/~/object-assign/index.js 2.11 kB {1} [built]
   [38] (webpack)/~/react-dom/~/fbjs/lib/emptyFunction.js 1.08 kB {1} [built]
   [68] (webpack)/~/react-dom/~/fbjs/lib/shallowEqual.js 1.74 kB {1} [built]
   [92] (webpack)/~/react-dom/~/fbjs/lib/focusNode.js 704 bytes {1} [built]
   [93] (webpack)/~/react-dom/~/fbjs/lib/getActiveElement.js 895 bytes {1} [built]
   [94] (webpack)/~/react/lib/ReactComponentTreeHook.js 10.4 kB {1} [built]
  [160] (webpack)/~/react-dom/~/fbjs/lib/camelizeStyleName.js 1 kB {1} [built]
  [161] (webpack)/~/react-dom/~/fbjs/lib/containsNode.js 1.05 kB {1} [built]
  [162] (webpack)/~/react-dom/~/fbjs/lib/createArrayFromMixed.js 4.11 kB {1} [built]
  [163] (webpack)/~/react-dom/~/fbjs/lib/createNodesFromMarkup.js 2.66 kB {1} [built]
  [164] (webpack)/~/react-dom/~/fbjs/lib/getMarkupWrap.js 3.04 kB {1} [built]
  [165] (webpack)/~/react-dom/~/fbjs/lib/getUnboundedScrollPosition.js 1.05 kB {1} [built]
  [166] (webpack)/~/react-dom/~/fbjs/lib/hyphenate.js 800 bytes {1} [built]
  [167] (webpack)/~/react-dom/~/fbjs/lib/hyphenateStyleName.js 974 bytes {1} [built]
  [168] (webpack)/~/react-dom/~/fbjs/lib/isNode.js 693 bytes {1} [built]
  [169] (webpack)/~/react-dom/~/fbjs/lib/isTextNode.js 605 bytes {1} [built]
  [170] (webpack)/~/react-dom/~/fbjs/lib/memoizeStringOnly.js 698 bytes {1} [built]
chunk    {2} d360e468c99903c2d129.js 49.8 kB {12} {13} {14} [rendered] [recorded]
    > aggressive-splitted [16] ./example.js 2:0-22
   [37] (webpack)/~/react-dom/lib/SyntheticEvent.js 9.18 kB {2} [built]
   [46] (webpack)/~/react-dom/lib/SyntheticUIEvent.js 1.57 kB {2} [built]
   [48] (webpack)/~/react-dom/lib/SyntheticMouseEvent.js 2.14 kB {2} [built]
   [82] (webpack)/~/react-dom/lib/accumulateInto.js 1.69 kB {2} [built]
  [135] (webpack)/~/react-dom/lib/SVGDOMPropertyConfig.js 7.32 kB {2} [built]
  [136] (webpack)/~/react-dom/lib/SelectEventPlugin.js 6.06 kB {2} [built]
  [137] (webpack)/~/react-dom/lib/SimpleEventPlugin.js 7.97 kB {2} [built]
  [138] (webpack)/~/react-dom/lib/SyntheticAnimationEvent.js 1.21 kB {2} [built]
  [139] (webpack)/~/react-dom/lib/SyntheticClipboardEvent.js 1.17 kB {2} [built]
  [140] (webpack)/~/react-dom/lib/SyntheticCompositionEvent.js 1.1 kB {2} [built]
  [141] (webpack)/~/react-dom/lib/SyntheticDragEvent.js 1.07 kB {2} [built]
  [142] (webpack)/~/react-dom/lib/SyntheticFocusEvent.js 1.07 kB {2} [built]
  [143] (webpack)/~/react-dom/lib/SyntheticInputEvent.js 1.09 kB {2} [built]
  [144] (webpack)/~/react-dom/lib/SyntheticKeyboardEvent.js 2.71 kB {2} [built]
  [145] (webpack)/~/react-dom/lib/SyntheticTouchEvent.js 1.28 kB {2} [built]
  [146] (webpack)/~/react-dom/lib/SyntheticTransitionEvent.js 1.23 kB {2} [built]
  [147] (webpack)/~/react-dom/lib/SyntheticWheelEvent.js 1.94 kB {2} [built]
chunk    {3} 76860534ab8ba5e484a2.js 50 kB {12} {13} {14} [rendered] [recorded]
    > aggressive-splitted [16] ./example.js 2:0-22
   [59] (webpack)/~/react-dom/lib/ReactErrorUtils.js 2.25 kB {3} [built]
   [74] (webpack)/~/react-dom/lib/ReactDOMSelect.js 6.81 kB {3} [built]
   [76] (webpack)/~/react-dom/lib/ReactFeatureFlags.js 628 bytes {3} [built]
   [77] (webpack)/~/react-dom/lib/ReactHostComponent.js 1.98 kB {3} [built]
  [114] (webpack)/~/react-dom/lib/ReactDOMInput.js 12.6 kB {3} [built]
  [116] (webpack)/~/react-dom/lib/ReactDOMSelection.js 6.78 kB {3} [built]
  [117] (webpack)/~/react-dom/lib/ReactDOMTextComponent.js 5.82 kB {3} [built]
  [118] (webpack)/~/react-dom/lib/ReactDOMTextarea.js 6.46 kB {3} [built]
  [120] (webpack)/~/react-dom/lib/ReactDefaultBatchingStrategy.js 1.88 kB {3} [built]
  [121] (webpack)/~/react-dom/lib/ReactDefaultInjection.js 3.5 kB {3} [built]
  [123] (webpack)/~/react-dom/lib/ReactEventEmitterMixin.js 959 bytes {3} [built]
  [134] (webpack)/~/react-dom/lib/ReactVersion.js 350 bytes {3} [built]
chunk    {4} b75a1bd78f5b92799a11.js 50 kB {12} {13} {14} [rendered] [recorded]
    > aggressive-splitted [16] ./example.js 2:0-22
   [35] (webpack)/~/react-dom/lib/ReactInstrumentation.js 601 bytes {4} [built]
   [45] (webpack)/~/react-dom/lib/ReactInstanceMap.js 1.22 kB {4} [built]
   [78] (webpack)/~/react-dom/lib/ReactInputSelection.js 4.27 kB {4} [built]
   [79] (webpack)/~/react-dom/lib/ReactMount.js 25.5 kB {4} [built]
   [80] (webpack)/~/react-dom/lib/ReactNodeTypes.js 1.02 kB {4} [built]
   [81] (webpack)/~/react-dom/lib/ViewportMetrics.js 606 bytes {4} [built]
  [124] (webpack)/~/react-dom/lib/ReactEventListener.js 5.3 kB {4} [built]
  [125] (webpack)/~/react-dom/lib/ReactInjection.js 1.2 kB {4} [built]
  [126] (webpack)/~/react-dom/lib/ReactMarkupChecksum.js 1.47 kB {4} [built]
  [128] (webpack)/~/react-dom/lib/ReactOwner.js 3.53 kB {4} [built]
  [130] (webpack)/~/react-dom/lib/ReactReconcileTransaction.js 5.26 kB {4} [built]
chunk    {5} 8b27bb3161668f698375.js 50 kB {12} {13} {14} [rendered] [recorded]
    > aggressive-splitted [16] ./example.js 2:0-22
   [17] (webpack)/~/react-dom/index.js 59 bytes {5} [built]
   [40] (webpack)/~/react-dom/lib/DOMLazyTree.js 3.71 kB {5} [built]
   [53] (webpack)/~/react-dom/lib/DOMNamespaces.js 505 bytes {5} [built]
   [69] (webpack)/~/node-libs-browser/~/process/browser.js 5.3 kB {5} [built]
   [70] (webpack)/~/react-dom/lib/CSSProperty.js 3.66 kB {5} [built]
   [71] (webpack)/~/react-dom/lib/CallbackQueue.js 3.16 kB {5} [built]
   [95] (webpack)/~/react-dom/lib/ARIADOMPropertyConfig.js 1.82 kB {5} [built]
   [96] (webpack)/~/react-dom/lib/AutoFocusUtils.js 599 bytes {5} [built]
   [97] (webpack)/~/react-dom/lib/BeforeInputEventPlugin.js 13.3 kB {5} [built]
   [98] (webpack)/~/react-dom/lib/CSSPropertyOperations.js 6.87 kB {5} [built]
   [99] (webpack)/~/react-dom/lib/ChangeEventPlugin.js 11.1 kB {5} [built]
chunk    {6} 1a42bbc60adaf6991bb7.js 49.8 kB {12} {13} {14} [rendered] [recorded]
    > aggressive-splitted [16] ./example.js 2:0-22
   [39] (webpack)/~/react-dom/lib/PooledClass.js 3.36 kB {6} [built]
   [44] (webpack)/~/react-dom/lib/EventPropagators.js 5.09 kB {6} [built]
   [47] (webpack)/~/react-dom/lib/ReactBrowserEventEmitter.js 12.6 kB {6} [built]
   [55] (webpack)/~/react-dom/lib/EventPluginUtils.js 7.95 kB {6} [built]
   [56] (webpack)/~/react-dom/lib/KeyEscapeUtils.js 1.29 kB {6} [built]
   [57] (webpack)/~/react-dom/lib/LinkedValueUtils.js 5.15 kB {6} [built]
   [73] (webpack)/~/react-dom/lib/ReactDOMComponentFlags.js 429 bytes {6} [built]
  [103] (webpack)/~/react-dom/lib/FallbackCompositionState.js 2.43 kB {6} [built]
  [104] (webpack)/~/react-dom/lib/HTMLDOMPropertyConfig.js 5.44 kB {6} [built]
  [105] (webpack)/~/react-dom/lib/ReactChildReconciler.js 6.11 kB {6} [built]
chunk    {7} 4f84f005b0c46df43fd2.js 49.8 kB {12} {13} {14} [rendered] [recorded]
    > aggressive-splitted [16] ./example.js 2:0-22
   [41] (webpack)/~/react-dom/lib/DOMProperty.js 8.24 kB {7} [built]
   [43] (webpack)/~/react-dom/lib/EventPluginHub.js 9.11 kB {7} [built]
   [52] (webpack)/~/react-dom/lib/DOMChildrenOperations.js 7.67 kB {7} [built]
   [54] (webpack)/~/react-dom/lib/EventPluginRegistry.js 9.75 kB {7} [built]
   [72] (webpack)/~/react-dom/lib/DOMPropertyOperations.js 7.61 kB {7} [built]
  [100] (webpack)/~/react-dom/lib/Danger.js 2.24 kB {7} [built]
  [101] (webpack)/~/react-dom/lib/DefaultEventPluginOrder.js 1.08 kB {7} [built]
  [102] (webpack)/~/react-dom/lib/EnterLeaveEventPlugin.js 3.16 kB {7} [built]
  [106] (webpack)/~/react-dom/lib/ReactComponentBrowserEnvironment.js 906 bytes {7} [built]
chunk    {8} 3d93b56502b22b64f4bc.js 49.8 kB {12} {13} {14} [rendered] [recorded]
    > aggressive-splitted [16] ./example.js 2:0-22
   [36] (webpack)/~/react-dom/lib/ReactUpdates.js 9.53 kB {8} [built]
   [42] (webpack)/~/react-dom/lib/ReactReconciler.js 6.21 kB {8} [built]
   [60] (webpack)/~/react-dom/lib/ReactUpdateQueue.js 9.01 kB {8} [built]
   [61] (webpack)/~/react-dom/lib/createMicrosoftUnsafeLocalFunction.js 810 bytes {8} [built]
  [127] (webpack)/~/react-dom/lib/ReactMultiChild.js 14.6 kB {8} [built]
  [131] (webpack)/~/react-dom/lib/ReactRef.js 2.56 kB {8} [built]
  [132] (webpack)/~/react-dom/lib/ReactServerRenderingTransaction.js 2.29 kB {8} [built]
  [133] (webpack)/~/react-dom/lib/ReactServerUpdateQueue.js 4.83 kB {8} [built]
chunk    {9} 00d4d2a2ad4d2da7b940.js 30.4 kB {12} {13} {14} [rendered] [recorded]
    > aggressive-splitted [16] ./example.js 2:0-22
   [34] (webpack)/~/react-dom/~/fbjs/lib/ExecutionEnvironment.js 1.06 kB {9} [built]
   [51] (webpack)/~/react-dom/lib/setInnerHTML.js 3.86 kB {9} [built]
   [66] (webpack)/~/react-dom/lib/shouldUpdateReactComponent.js 1.4 kB {9} [built]
   [67] (webpack)/~/react-dom/lib/validateDOMNesting.js 13.7 kB {9} [built]
   [89] (webpack)/~/react-dom/lib/traverseAllChildren.js 7.04 kB {9} [built]
   [90] (webpack)/~/react-dom/~/fbjs/lib/EventListener.js 2.67 kB {9} [built]
  [159] (webpack)/~/react-dom/~/fbjs/lib/camelize.js 708 bytes {9} [built]
chunk   {10} 3b0e2394af1c44c15c75.js 49.9 kB {12} {13} {14} [rendered] [recorded]
    > aggressive-splitted [16] ./example.js 2:0-22
   [75] (webpack)/~/react-dom/lib/ReactEmptyComponent.js 704 bytes {10} [built]
  [109] (webpack)/~/react-dom/lib/ReactDOMComponent.js 38.5 kB {10} [built]
  [111] (webpack)/~/react-dom/lib/ReactDOMEmptyComponent.js 1.9 kB {10} [built]
  [113] (webpack)/~/react-dom/lib/ReactDOMIDOperations.js 956 bytes {10} [built]
  [115] (webpack)/~/react-dom/lib/ReactDOMOption.js 3.69 kB {10} [built]
  [119] (webpack)/~/react-dom/lib/ReactDOMTreeTraversal.js 3.72 kB {10} [built]
  [129] (webpack)/~/react-dom/lib/ReactPropTypesSecret.js 442 bytes {10} [built]
chunk   {11} 24047db0d33992324c0e.js 49.9 kB {12} {13} {14} [rendered] [recorded]
    > aggressive-splitted [16] ./example.js 2:0-22
   [32] (webpack)/~/react-dom/lib/ReactDOMComponentTree.js 6.27 kB {11} [built]
   [58] (webpack)/~/react-dom/lib/ReactComponentEnvironment.js 1.3 kB {11} [built]
  [107] (webpack)/~/react-dom/lib/ReactCompositeComponent.js 35.2 kB {11} [built]
  [108] (webpack)/~/react-dom/lib/ReactDOM.js 5.14 kB {11} [built]
  [110] (webpack)/~/react-dom/lib/ReactDOMContainerInfo.js 967 bytes {11} [built]
  [112] (webpack)/~/react-dom/lib/ReactDOMFeatureFlags.js 439 bytes {11} [built]
  [122] (webpack)/~/react-dom/lib/ReactElementSymbol.js 622 bytes {11} [built]
chunk   {12} 34d435e6fa54d2244802.js 49.8 kB [entry] [rendered] [recorded]
    > aggressive-splitted main [16] ./example.js 
    [5] (webpack)/~/react/lib/ReactComponent.js 4.61 kB {12} [built]
    [6] (webpack)/~/react/lib/ReactNoopUpdateQueue.js 3.36 kB {12} [built]
    [9] (webpack)/~/react/lib/ReactCurrentOwner.js 623 bytes {12} [built]
   [10] (webpack)/~/react/lib/ReactElementSymbol.js 622 bytes {12} [built]
   [11] (webpack)/~/react/lib/ReactPropTypeLocationNames.js 572 bytes {12} [built]
   [14] (webpack)/~/react/react.js 56 bytes {12} [built]
   [15] (webpack)/~/react/lib/React.js 2.69 kB {12} [built]
   [18] (webpack)/~/react/lib/KeyEscapeUtils.js 1.29 kB {12} [built]
   [19] (webpack)/~/react/lib/PooledClass.js 3.36 kB {12} [built]
   [20] (webpack)/~/react/lib/ReactChildren.js 6.19 kB {12} [built]
   [21] (webpack)/~/react/lib/ReactClass.js 26.5 kB {12} [built]
chunk   {13} 8781e3116c98876a6b44.js 23.2 kB [initial] [rendered]
    > aggressive-splitted main [16] ./example.js 
    [1] (webpack)/~/react/lib/ReactElement.js 11.2 kB {13} [built]
    [2] (webpack)/~/react/lib/reactProdInvariant.js 1.24 kB {13} [built]
   [12] (webpack)/~/react/lib/canDefineProperty.js 661 bytes {13} [built]
   [13] (webpack)/~/react/lib/getIteratorFn.js 1.12 kB {13} [built]
   [22] (webpack)/~/react/lib/ReactDOMFactories.js 5.53 kB {13} [built]
   [24] (webpack)/~/react/lib/ReactPropTypesSecret.js 442 bytes {13} [built]
   [25] (webpack)/~/react/lib/ReactPureComponent.js 1.32 kB {13} [built]
   [26] (webpack)/~/react/lib/ReactVersion.js 350 bytes {13} [built]
   [27] (webpack)/~/react/lib/onlyChild.js 1.34 kB {13} [built]
chunk   {14} 0baa068ca7eedead78e0.js 30.3 kB [initial] [rendered]
    > aggressive-splitted main [16] ./example.js 
    [0] (webpack)/~/react/~/fbjs/lib/warning.js 2.1 kB {14} [built]
    [3] (webpack)/~/react/~/fbjs/lib/invariant.js 1.63 kB {14} [built]
    [4] (webpack)/~/react/~/object-assign/index.js 2.11 kB {14} [built]
    [7] (webpack)/~/react/~/fbjs/lib/emptyFunction.js 1.08 kB {14} [built]
    [8] (webpack)/~/react/~/fbjs/lib/emptyObject.js 458 bytes {14} [built]
   [16] ./example.js 42 bytes {14} [built]
   [23] (webpack)/~/react/lib/ReactPropTypes.js 15.8 kB {14} [built]
   [28] (webpack)/~/react/lib/traverseAllChildren.js 7.03 kB {14} [built]
```

## Minimized (uglify-js, no zip)

```
Hash: 00f5d308d70e00fe7713
Version: webpack 2.2.1
                  Asset     Size  Chunks             Chunk Names
4f84f005b0c46df43fd2.js  10.2 kB       7  [emitted]  
6b21c67c0964251394af.js  9.46 kB       0  [emitted]  
d360e468c99903c2d129.js  15.1 kB       2  [emitted]  
76860534ab8ba5e484a2.js  10.2 kB       3  [emitted]  
b75a1bd78f5b92799a11.js   9.3 kB       4  [emitted]  
8b27bb3161668f698375.js  12.3 kB       5  [emitted]  
1a42bbc60adaf6991bb7.js  13.2 kB       6  [emitted]  
6df160cf3935656c5665.js  9.45 kB       1  [emitted]  
3d93b56502b22b64f4bc.js   9.2 kB       8  [emitted]  
00d4d2a2ad4d2da7b940.js  2.84 kB       9  [emitted]  
3b0e2394af1c44c15c75.js    13 kB      10  [emitted]  
24047db0d33992324c0e.js  10.7 kB      11  [emitted]  
34d435e6fa54d2244802.js  9.28 kB      12  [emitted]  
8781e3116c98876a6b44.js  5.02 kB      13  [emitted]  
0baa068ca7eedead78e0.js  7.09 kB      14  [emitted]  
Entrypoint main = 34d435e6fa54d2244802.js 8781e3116c98876a6b44.js 0baa068ca7eedead78e0.js
chunk    {0} 6b21c67c0964251394af.js 49.8 kB {12} {13} {14} [rendered] [recorded]
    > aggressive-splitted [16] ./example.js 2:0-22
   [30] (webpack)/~/react-dom/lib/reactProdInvariant.js 1.24 kB {0} [built]
   [49] (webpack)/~/react-dom/lib/Transaction.js 9.45 kB {0} [built]
   [50] (webpack)/~/react-dom/lib/escapeTextContentForBrowser.js 3.43 kB {0} [built]
   [62] (webpack)/~/react-dom/lib/getEventCharCode.js 1.5 kB {0} [built]
   [63] (webpack)/~/react-dom/lib/getEventModifierState.js 1.23 kB {0} [built]
   [64] (webpack)/~/react-dom/lib/getEventTarget.js 1.01 kB {0} [built]
   [65] (webpack)/~/react-dom/lib/isEventSupported.js 1.94 kB {0} [built]
   [83] (webpack)/~/react-dom/lib/forEachAccumulated.js 855 bytes {0} [built]
   [84] (webpack)/~/react-dom/lib/getHostComponentFromComposite.js 740 bytes {0} [built]
   [85] (webpack)/~/react-dom/lib/getTextContentAccessor.js 955 bytes {0} [built]
   [86] (webpack)/~/react-dom/lib/instantiateReactComponent.js 5.05 kB {0} [built]
   [87] (webpack)/~/react-dom/lib/isTextInputElement.js 1.04 kB {0} [built]
   [88] (webpack)/~/react-dom/lib/setTextContent.js 1.45 kB {0} [built]
   [91] (webpack)/~/react-dom/~/fbjs/lib/emptyObject.js 458 bytes {0} [built]
  [148] (webpack)/~/react-dom/lib/adler32.js 1.19 kB {0} [built]
  [149] (webpack)/~/react-dom/lib/dangerousStyleValue.js 3.02 kB {0} [built]
  [150] (webpack)/~/react-dom/lib/findDOMNode.js 2.46 kB {0} [built]
  [151] (webpack)/~/react-dom/lib/flattenChildren.js 2.77 kB {0} [built]
  [152] (webpack)/~/react-dom/lib/getEventKey.js 2.87 kB {0} [built]
  [153] (webpack)/~/react-dom/lib/getIteratorFn.js 1.12 kB {0} [built]
  [154] (webpack)/~/react-dom/lib/getNextDebugID.js 437 bytes {0} [built]
  [155] (webpack)/~/react-dom/lib/getNodeForCharacterOffset.js 1.62 kB {0} [built]
  [156] (webpack)/~/react-dom/lib/getVendorPrefixedEventName.js 2.87 kB {0} [built]
  [157] (webpack)/~/react-dom/lib/quoteAttributeValueForBrowser.js 700 bytes {0} [built]
  [158] (webpack)/~/react-dom/lib/renderSubtreeIntoContainer.js 422 bytes {0} [built]
chunk    {1} 6df160cf3935656c5665.js 37.3 kB {12} {13} {14} [rendered] [recorded]
    > aggressive-splitted [16] ./example.js 2:0-22
   [29] (webpack)/~/react-dom/~/fbjs/lib/invariant.js 1.63 kB {1} [built]
   [31] (webpack)/~/react-dom/~/fbjs/lib/warning.js 2.1 kB {1} [built]
   [33] (webpack)/~/react-dom/~/object-assign/index.js 2.11 kB {1} [built]
   [38] (webpack)/~/react-dom/~/fbjs/lib/emptyFunction.js 1.08 kB {1} [built]
   [68] (webpack)/~/react-dom/~/fbjs/lib/shallowEqual.js 1.74 kB {1} [built]
   [92] (webpack)/~/react-dom/~/fbjs/lib/focusNode.js 704 bytes {1} [built]
   [93] (webpack)/~/react-dom/~/fbjs/lib/getActiveElement.js 895 bytes {1} [built]
   [94] (webpack)/~/react/lib/ReactComponentTreeHook.js 10.4 kB {1} [built]
  [160] (webpack)/~/react-dom/~/fbjs/lib/camelizeStyleName.js 1 kB {1} [built]
  [161] (webpack)/~/react-dom/~/fbjs/lib/containsNode.js 1.05 kB {1} [built]
  [162] (webpack)/~/react-dom/~/fbjs/lib/createArrayFromMixed.js 4.11 kB {1} [built]
  [163] (webpack)/~/react-dom/~/fbjs/lib/createNodesFromMarkup.js 2.66 kB {1} [built]
  [164] (webpack)/~/react-dom/~/fbjs/lib/getMarkupWrap.js 3.04 kB {1} [built]
  [165] (webpack)/~/react-dom/~/fbjs/lib/getUnboundedScrollPosition.js 1.05 kB {1} [built]
  [166] (webpack)/~/react-dom/~/fbjs/lib/hyphenate.js 800 bytes {1} [built]
  [167] (webpack)/~/react-dom/~/fbjs/lib/hyphenateStyleName.js 974 bytes {1} [built]
  [168] (webpack)/~/react-dom/~/fbjs/lib/isNode.js 693 bytes {1} [built]
  [169] (webpack)/~/react-dom/~/fbjs/lib/isTextNode.js 605 bytes {1} [built]
  [170] (webpack)/~/react-dom/~/fbjs/lib/memoizeStringOnly.js 698 bytes {1} [built]
chunk    {2} d360e468c99903c2d129.js 49.8 kB {12} {13} {14} [rendered] [recorded]
    > aggressive-splitted [16] ./example.js 2:0-22
   [37] (webpack)/~/react-dom/lib/SyntheticEvent.js 9.18 kB {2} [built]
   [46] (webpack)/~/react-dom/lib/SyntheticUIEvent.js 1.57 kB {2} [built]
   [48] (webpack)/~/react-dom/lib/SyntheticMouseEvent.js 2.14 kB {2} [built]
   [82] (webpack)/~/react-dom/lib/accumulateInto.js 1.69 kB {2} [built]
  [135] (webpack)/~/react-dom/lib/SVGDOMPropertyConfig.js 7.32 kB {2} [built]
  [136] (webpack)/~/react-dom/lib/SelectEventPlugin.js 6.06 kB {2} [built]
  [137] (webpack)/~/react-dom/lib/SimpleEventPlugin.js 7.97 kB {2} [built]
  [138] (webpack)/~/react-dom/lib/SyntheticAnimationEvent.js 1.21 kB {2} [built]
  [139] (webpack)/~/react-dom/lib/SyntheticClipboardEvent.js 1.17 kB {2} [built]
  [140] (webpack)/~/react-dom/lib/SyntheticCompositionEvent.js 1.1 kB {2} [built]
  [141] (webpack)/~/react-dom/lib/SyntheticDragEvent.js 1.07 kB {2} [built]
  [142] (webpack)/~/react-dom/lib/SyntheticFocusEvent.js 1.07 kB {2} [built]
  [143] (webpack)/~/react-dom/lib/SyntheticInputEvent.js 1.09 kB {2} [built]
  [144] (webpack)/~/react-dom/lib/SyntheticKeyboardEvent.js 2.71 kB {2} [built]
  [145] (webpack)/~/react-dom/lib/SyntheticTouchEvent.js 1.28 kB {2} [built]
  [146] (webpack)/~/react-dom/lib/SyntheticTransitionEvent.js 1.23 kB {2} [built]
  [147] (webpack)/~/react-dom/lib/SyntheticWheelEvent.js 1.94 kB {2} [built]
chunk    {3} 76860534ab8ba5e484a2.js 50 kB {12} {13} {14} [rendered] [recorded]
    > aggressive-splitted [16] ./example.js 2:0-22
   [59] (webpack)/~/react-dom/lib/ReactErrorUtils.js 2.25 kB {3} [built]
   [74] (webpack)/~/react-dom/lib/ReactDOMSelect.js 6.81 kB {3} [built]
   [76] (webpack)/~/react-dom/lib/ReactFeatureFlags.js 628 bytes {3} [built]
   [77] (webpack)/~/react-dom/lib/ReactHostComponent.js 1.98 kB {3} [built]
  [114] (webpack)/~/react-dom/lib/ReactDOMInput.js 12.6 kB {3} [built]
  [116] (webpack)/~/react-dom/lib/ReactDOMSelection.js 6.78 kB {3} [built]
  [117] (webpack)/~/react-dom/lib/ReactDOMTextComponent.js 5.82 kB {3} [built]
  [118] (webpack)/~/react-dom/lib/ReactDOMTextarea.js 6.46 kB {3} [built]
  [120] (webpack)/~/react-dom/lib/ReactDefaultBatchingStrategy.js 1.88 kB {3} [built]
  [121] (webpack)/~/react-dom/lib/ReactDefaultInjection.js 3.5 kB {3} [built]
  [123] (webpack)/~/react-dom/lib/ReactEventEmitterMixin.js 959 bytes {3} [built]
  [134] (webpack)/~/react-dom/lib/ReactVersion.js 350 bytes {3} [built]
chunk    {4} b75a1bd78f5b92799a11.js 50 kB {12} {13} {14} [rendered] [recorded]
    > aggressive-splitted [16] ./example.js 2:0-22
   [35] (webpack)/~/react-dom/lib/ReactInstrumentation.js 601 bytes {4} [built]
   [45] (webpack)/~/react-dom/lib/ReactInstanceMap.js 1.22 kB {4} [built]
   [78] (webpack)/~/react-dom/lib/ReactInputSelection.js 4.27 kB {4} [built]
   [79] (webpack)/~/react-dom/lib/ReactMount.js 25.5 kB {4} [built]
   [80] (webpack)/~/react-dom/lib/ReactNodeTypes.js 1.02 kB {4} [built]
   [81] (webpack)/~/react-dom/lib/ViewportMetrics.js 606 bytes {4} [built]
  [124] (webpack)/~/react-dom/lib/ReactEventListener.js 5.3 kB {4} [built]
  [125] (webpack)/~/react-dom/lib/ReactInjection.js 1.2 kB {4} [built]
  [126] (webpack)/~/react-dom/lib/ReactMarkupChecksum.js 1.47 kB {4} [built]
  [128] (webpack)/~/react-dom/lib/ReactOwner.js 3.53 kB {4} [built]
  [130] (webpack)/~/react-dom/lib/ReactReconcileTransaction.js 5.26 kB {4} [built]
chunk    {5} 8b27bb3161668f698375.js 50 kB {12} {13} {14} [rendered] [recorded]
    > aggressive-splitted [16] ./example.js 2:0-22
   [17] (webpack)/~/react-dom/index.js 59 bytes {5} [built]
   [40] (webpack)/~/react-dom/lib/DOMLazyTree.js 3.71 kB {5} [built]
   [53] (webpack)/~/react-dom/lib/DOMNamespaces.js 505 bytes {5} [built]
   [69] (webpack)/~/node-libs-browser/~/process/browser.js 5.3 kB {5} [built]
   [70] (webpack)/~/react-dom/lib/CSSProperty.js 3.66 kB {5} [built]
   [71] (webpack)/~/react-dom/lib/CallbackQueue.js 3.16 kB {5} [built]
   [95] (webpack)/~/react-dom/lib/ARIADOMPropertyConfig.js 1.82 kB {5} [built]
   [96] (webpack)/~/react-dom/lib/AutoFocusUtils.js 599 bytes {5} [built]
   [97] (webpack)/~/react-dom/lib/BeforeInputEventPlugin.js 13.3 kB {5} [built]
   [98] (webpack)/~/react-dom/lib/CSSPropertyOperations.js 6.87 kB {5} [built]
   [99] (webpack)/~/react-dom/lib/ChangeEventPlugin.js 11.1 kB {5} [built]
chunk    {6} 1a42bbc60adaf6991bb7.js 49.8 kB {12} {13} {14} [rendered] [recorded]
    > aggressive-splitted [16] ./example.js 2:0-22
   [39] (webpack)/~/react-dom/lib/PooledClass.js 3.36 kB {6} [built]
   [44] (webpack)/~/react-dom/lib/EventPropagators.js 5.09 kB {6} [built]
   [47] (webpack)/~/react-dom/lib/ReactBrowserEventEmitter.js 12.6 kB {6} [built]
   [55] (webpack)/~/react-dom/lib/EventPluginUtils.js 7.95 kB {6} [built]
   [56] (webpack)/~/react-dom/lib/KeyEscapeUtils.js 1.29 kB {6} [built]
   [57] (webpack)/~/react-dom/lib/LinkedValueUtils.js 5.15 kB {6} [built]
   [73] (webpack)/~/react-dom/lib/ReactDOMComponentFlags.js 429 bytes {6} [built]
  [103] (webpack)/~/react-dom/lib/FallbackCompositionState.js 2.43 kB {6} [built]
  [104] (webpack)/~/react-dom/lib/HTMLDOMPropertyConfig.js 5.44 kB {6} [built]
  [105] (webpack)/~/react-dom/lib/ReactChildReconciler.js 6.11 kB {6} [built]
chunk    {7} 4f84f005b0c46df43fd2.js 49.8 kB {12} {13} {14} [rendered] [recorded]
    > aggressive-splitted [16] ./example.js 2:0-22
   [41] (webpack)/~/react-dom/lib/DOMProperty.js 8.24 kB {7} [built]
   [43] (webpack)/~/react-dom/lib/EventPluginHub.js 9.11 kB {7} [built]
   [52] (webpack)/~/react-dom/lib/DOMChildrenOperations.js 7.67 kB {7} [built]
   [54] (webpack)/~/react-dom/lib/EventPluginRegistry.js 9.75 kB {7} [built]
   [72] (webpack)/~/react-dom/lib/DOMPropertyOperations.js 7.61 kB {7} [built]
  [100] (webpack)/~/react-dom/lib/Danger.js 2.24 kB {7} [built]
  [101] (webpack)/~/react-dom/lib/DefaultEventPluginOrder.js 1.08 kB {7} [built]
  [102] (webpack)/~/react-dom/lib/EnterLeaveEventPlugin.js 3.16 kB {7} [built]
  [106] (webpack)/~/react-dom/lib/ReactComponentBrowserEnvironment.js 906 bytes {7} [built]
chunk    {8} 3d93b56502b22b64f4bc.js 49.8 kB {12} {13} {14} [rendered] [recorded]
    > aggressive-splitted [16] ./example.js 2:0-22
   [36] (webpack)/~/react-dom/lib/ReactUpdates.js 9.53 kB {8} [built]
   [42] (webpack)/~/react-dom/lib/ReactReconciler.js 6.21 kB {8} [built]
   [60] (webpack)/~/react-dom/lib/ReactUpdateQueue.js 9.01 kB {8} [built]
   [61] (webpack)/~/react-dom/lib/createMicrosoftUnsafeLocalFunction.js 810 bytes {8} [built]
  [127] (webpack)/~/react-dom/lib/ReactMultiChild.js 14.6 kB {8} [built]
  [131] (webpack)/~/react-dom/lib/ReactRef.js 2.56 kB {8} [built]
  [132] (webpack)/~/react-dom/lib/ReactServerRenderingTransaction.js 2.29 kB {8} [built]
  [133] (webpack)/~/react-dom/lib/ReactServerUpdateQueue.js 4.83 kB {8} [built]
chunk    {9} 00d4d2a2ad4d2da7b940.js 30.4 kB {12} {13} {14} [rendered] [recorded]
    > aggressive-splitted [16] ./example.js 2:0-22
   [34] (webpack)/~/react-dom/~/fbjs/lib/ExecutionEnvironment.js 1.06 kB {9} [built]
   [51] (webpack)/~/react-dom/lib/setInnerHTML.js 3.86 kB {9} [built]
   [66] (webpack)/~/react-dom/lib/shouldUpdateReactComponent.js 1.4 kB {9} [built]
   [67] (webpack)/~/react-dom/lib/validateDOMNesting.js 13.7 kB {9} [built]
   [89] (webpack)/~/react-dom/lib/traverseAllChildren.js 7.04 kB {9} [built]
   [90] (webpack)/~/react-dom/~/fbjs/lib/EventListener.js 2.67 kB {9} [built]
  [159] (webpack)/~/react-dom/~/fbjs/lib/camelize.js 708 bytes {9} [built]
chunk   {10} 3b0e2394af1c44c15c75.js 49.9 kB {12} {13} {14} [rendered] [recorded]
    > aggressive-splitted [16] ./example.js 2:0-22
   [75] (webpack)/~/react-dom/lib/ReactEmptyComponent.js 704 bytes {10} [built]
  [109] (webpack)/~/react-dom/lib/ReactDOMComponent.js 38.5 kB {10} [built]
  [111] (webpack)/~/react-dom/lib/ReactDOMEmptyComponent.js 1.9 kB {10} [built]
  [113] (webpack)/~/react-dom/lib/ReactDOMIDOperations.js 956 bytes {10} [built]
  [115] (webpack)/~/react-dom/lib/ReactDOMOption.js 3.69 kB {10} [built]
  [119] (webpack)/~/react-dom/lib/ReactDOMTreeTraversal.js 3.72 kB {10} [built]
  [129] (webpack)/~/react-dom/lib/ReactPropTypesSecret.js 442 bytes {10} [built]
chunk   {11} 24047db0d33992324c0e.js 49.9 kB {12} {13} {14} [rendered] [recorded]
    > aggressive-splitted [16] ./example.js 2:0-22
   [32] (webpack)/~/react-dom/lib/ReactDOMComponentTree.js 6.27 kB {11} [built]
   [58] (webpack)/~/react-dom/lib/ReactComponentEnvironment.js 1.3 kB {11} [built]
  [107] (webpack)/~/react-dom/lib/ReactCompositeComponent.js 35.2 kB {11} [built]
  [108] (webpack)/~/react-dom/lib/ReactDOM.js 5.14 kB {11} [built]
  [110] (webpack)/~/react-dom/lib/ReactDOMContainerInfo.js 967 bytes {11} [built]
  [112] (webpack)/~/react-dom/lib/ReactDOMFeatureFlags.js 439 bytes {11} [built]
  [122] (webpack)/~/react-dom/lib/ReactElementSymbol.js 622 bytes {11} [built]
chunk   {12} 34d435e6fa54d2244802.js 49.8 kB [entry] [rendered] [recorded]
    > aggressive-splitted main [16] ./example.js 
    [5] (webpack)/~/react/lib/ReactComponent.js 4.61 kB {12} [built]
    [6] (webpack)/~/react/lib/ReactNoopUpdateQueue.js 3.36 kB {12} [built]
    [9] (webpack)/~/react/lib/ReactCurrentOwner.js 623 bytes {12} [built]
   [10] (webpack)/~/react/lib/ReactElementSymbol.js 622 bytes {12} [built]
   [11] (webpack)/~/react/lib/ReactPropTypeLocationNames.js 572 bytes {12} [built]
   [14] (webpack)/~/react/react.js 56 bytes {12} [built]
   [15] (webpack)/~/react/lib/React.js 2.69 kB {12} [built]
   [18] (webpack)/~/react/lib/KeyEscapeUtils.js 1.29 kB {12} [built]
   [19] (webpack)/~/react/lib/PooledClass.js 3.36 kB {12} [built]
   [20] (webpack)/~/react/lib/ReactChildren.js 6.19 kB {12} [built]
   [21] (webpack)/~/react/lib/ReactClass.js 26.5 kB {12} [built]
chunk   {13} 8781e3116c98876a6b44.js 23.2 kB [initial] [rendered]
    > aggressive-splitted main [16] ./example.js 
    [1] (webpack)/~/react/lib/ReactElement.js 11.2 kB {13} [built]
    [2] (webpack)/~/react/lib/reactProdInvariant.js 1.24 kB {13} [built]
   [12] (webpack)/~/react/lib/canDefineProperty.js 661 bytes {13} [built]
   [13] (webpack)/~/react/lib/getIteratorFn.js 1.12 kB {13} [built]
   [22] (webpack)/~/react/lib/ReactDOMFactories.js 5.53 kB {13} [built]
   [24] (webpack)/~/react/lib/ReactPropTypesSecret.js 442 bytes {13} [built]
   [25] (webpack)/~/react/lib/ReactPureComponent.js 1.32 kB {13} [built]
   [26] (webpack)/~/react/lib/ReactVersion.js 350 bytes {13} [built]
   [27] (webpack)/~/react/lib/onlyChild.js 1.34 kB {13} [built]
chunk   {14} 0baa068ca7eedead78e0.js 30.3 kB [initial] [rendered]
    > aggressive-splitted main [16] ./example.js 
    [0] (webpack)/~/react/~/fbjs/lib/warning.js 2.1 kB {14} [built]
    [3] (webpack)/~/react/~/fbjs/lib/invariant.js 1.63 kB {14} [built]
    [4] (webpack)/~/react/~/object-assign/index.js 2.11 kB {14} [built]
    [7] (webpack)/~/react/~/fbjs/lib/emptyFunction.js 1.08 kB {14} [built]
    [8] (webpack)/~/react/~/fbjs/lib/emptyObject.js 458 bytes {14} [built]
   [16] ./example.js 42 bytes {14} [built]
   [23] (webpack)/~/react/lib/ReactPropTypes.js 15.8 kB {14} [built]
   [28] (webpack)/~/react/lib/traverseAllChildren.js 7.03 kB {14} [built]
```

## Records

```
{
  "modules": {
    "byIdentifier": {
      "../../node_modules/react/node_modules/fbjs/lib/warning.js": 0,
      "../../node_modules/react/lib/ReactElement.js": 1,
      "../../node_modules/react/lib/reactProdInvariant.js": 2,
      "../../node_modules/react/node_modules/fbjs/lib/invariant.js": 3,
      "../../node_modules/react/node_modules/object-assign/index.js": 4,
      "../../node_modules/react/lib/ReactComponent.js": 5,
      "../../node_modules/react/lib/ReactNoopUpdateQueue.js": 6,
      "../../node_modules/react/node_modules/fbjs/lib/emptyFunction.js": 7,
      "../../node_modules/react/node_modules/fbjs/lib/emptyObject.js": 8,
      "../../node_modules/react/lib/ReactCurrentOwner.js": 9,
      "../../node_modules/react/lib/ReactElementSymbol.js": 10,
      "../../node_modules/react/lib/ReactPropTypeLocationNames.js": 11,
      "../../node_modules/react/lib/canDefineProperty.js": 12,
      "../../node_modules/react/lib/getIteratorFn.js": 13,
      "../../node_modules/react/react.js": 14,
      "../../node_modules/react/lib/React.js": 15,
      "example.js": 16,
      "../../node_modules/react-dom/index.js": 17,
      "../../node_modules/react/lib/KeyEscapeUtils.js": 18,
      "../../node_modules/react/lib/PooledClass.js": 19,
      "../../node_modules/react/lib/ReactChildren.js": 20,
      "../../node_modules/react/lib/ReactClass.js": 21,
      "../../node_modules/react/lib/ReactDOMFactories.js": 22,
      "../../node_modules/react/lib/ReactPropTypes.js": 23,
      "../../node_modules/react/lib/ReactPropTypesSecret.js": 24,
      "../../node_modules/react/lib/ReactPureComponent.js": 25,
      "../../node_modules/react/lib/ReactVersion.js": 26,
      "../../node_modules/react/lib/onlyChild.js": 27,
      "../../node_modules/react/lib/traverseAllChildren.js": 28,
      "../../node_modules/react-dom/node_modules/fbjs/lib/invariant.js": 29,
      "../../node_modules/react-dom/lib/reactProdInvariant.js": 30,
      "../../node_modules/react-dom/node_modules/fbjs/lib/warning.js": 31,
      "../../node_modules/react-dom/lib/ReactDOMComponentTree.js": 32,
      "../../node_modules/react-dom/node_modules/object-assign/index.js": 33,
      "../../node_modules/react-dom/node_modules/fbjs/lib/ExecutionEnvironment.js": 34,
      "../../node_modules/react-dom/lib/ReactInstrumentation.js": 35,
      "../../node_modules/react-dom/lib/ReactUpdates.js": 36,
      "../../node_modules/react-dom/lib/SyntheticEvent.js": 37,
      "../../node_modules/react-dom/node_modules/fbjs/lib/emptyFunction.js": 38,
      "../../node_modules/react-dom/lib/PooledClass.js": 39,
      "../../node_modules/react-dom/lib/DOMLazyTree.js": 40,
      "../../node_modules/react-dom/lib/DOMProperty.js": 41,
      "../../node_modules/react-dom/lib/ReactReconciler.js": 42,
      "../../node_modules/react-dom/lib/EventPluginHub.js": 43,
      "../../node_modules/react-dom/lib/EventPropagators.js": 44,
      "../../node_modules/react-dom/lib/ReactInstanceMap.js": 45,
      "../../node_modules/react-dom/lib/SyntheticUIEvent.js": 46,
      "../../node_modules/react-dom/lib/ReactBrowserEventEmitter.js": 47,
      "../../node_modules/react-dom/lib/SyntheticMouseEvent.js": 48,
      "../../node_modules/react-dom/lib/Transaction.js": 49,
      "../../node_modules/react-dom/lib/escapeTextContentForBrowser.js": 50,
      "../../node_modules/react-dom/lib/setInnerHTML.js": 51,
      "../../node_modules/react-dom/lib/DOMChildrenOperations.js": 52,
      "../../node_modules/react-dom/lib/DOMNamespaces.js": 53,
      "../../node_modules/react-dom/lib/EventPluginRegistry.js": 54,
      "../../node_modules/react-dom/lib/EventPluginUtils.js": 55,
      "../../node_modules/react-dom/lib/KeyEscapeUtils.js": 56,
      "../../node_modules/react-dom/lib/LinkedValueUtils.js": 57,
      "../../node_modules/react-dom/lib/ReactComponentEnvironment.js": 58,
      "../../node_modules/react-dom/lib/ReactErrorUtils.js": 59,
      "../../node_modules/react-dom/lib/ReactUpdateQueue.js": 60,
      "../../node_modules/react-dom/lib/createMicrosoftUnsafeLocalFunction.js": 61,
      "../../node_modules/react-dom/lib/getEventCharCode.js": 62,
      "../../node_modules/react-dom/lib/getEventModifierState.js": 63,
      "../../node_modules/react-dom/lib/getEventTarget.js": 64,
      "../../node_modules/react-dom/lib/isEventSupported.js": 65,
      "../../node_modules/react-dom/lib/shouldUpdateReactComponent.js": 66,
      "../../node_modules/react-dom/lib/validateDOMNesting.js": 67,
      "../../node_modules/react-dom/node_modules/fbjs/lib/shallowEqual.js": 68,
      "../../node_modules/node-libs-browser/node_modules/process/browser.js": 69,
      "../../node_modules/react-dom/lib/CSSProperty.js": 70,
      "../../node_modules/react-dom/lib/CallbackQueue.js": 71,
      "../../node_modules/react-dom/lib/DOMPropertyOperations.js": 72,
      "../../node_modules/react-dom/lib/ReactDOMComponentFlags.js": 73,
      "../../node_modules/react-dom/lib/ReactDOMSelect.js": 74,
      "../../node_modules/react-dom/lib/ReactEmptyComponent.js": 75,
      "../../node_modules/react-dom/lib/ReactFeatureFlags.js": 76,
      "../../node_modules/react-dom/lib/ReactHostComponent.js": 77,
      "../../node_modules/react-dom/lib/ReactInputSelection.js": 78,
      "../../node_modules/react-dom/lib/ReactMount.js": 79,
      "../../node_modules/react-dom/lib/ReactNodeTypes.js": 80,
      "../../node_modules/react-dom/lib/ViewportMetrics.js": 81,
      "../../node_modules/react-dom/lib/accumulateInto.js": 82,
      "../../node_modules/react-dom/lib/forEachAccumulated.js": 83,
      "../../node_modules/react-dom/lib/getHostComponentFromComposite.js": 84,
      "../../node_modules/react-dom/lib/getTextContentAccessor.js": 85,
      "../../node_modules/react-dom/lib/instantiateReactComponent.js": 86,
      "../../node_modules/react-dom/lib/isTextInputElement.js": 87,
      "../../node_modules/react-dom/lib/setTextContent.js": 88,
      "../../node_modules/react-dom/lib/traverseAllChildren.js": 89,
      "../../node_modules/react-dom/node_modules/fbjs/lib/EventListener.js": 90,
      "../../node_modules/react-dom/node_modules/fbjs/lib/emptyObject.js": 91,
      "../../node_modules/react-dom/node_modules/fbjs/lib/focusNode.js": 92,
      "../../node_modules/react-dom/node_modules/fbjs/lib/getActiveElement.js": 93,
      "../../node_modules/react/lib/ReactComponentTreeHook.js": 94,
      "../../node_modules/react-dom/lib/ARIADOMPropertyConfig.js": 95,
      "../../node_modules/react-dom/lib/AutoFocusUtils.js": 96,
      "../../node_modules/react-dom/lib/BeforeInputEventPlugin.js": 97,
      "../../node_modules/react-dom/lib/CSSPropertyOperations.js": 98,
      "../../node_modules/react-dom/lib/ChangeEventPlugin.js": 99,
      "../../node_modules/react-dom/lib/Danger.js": 100,
      "../../node_modules/react-dom/lib/DefaultEventPluginOrder.js": 101,
      "../../node_modules/react-dom/lib/EnterLeaveEventPlugin.js": 102,
      "../../node_modules/react-dom/lib/FallbackCompositionState.js": 103,
      "../../node_modules/react-dom/lib/HTMLDOMPropertyConfig.js": 104,
      "../../node_modules/react-dom/lib/ReactChildReconciler.js": 105,
      "../../node_modules/react-dom/lib/ReactComponentBrowserEnvironment.js": 106,
      "../../node_modules/react-dom/lib/ReactCompositeComponent.js": 107,
      "../../node_modules/react-dom/lib/ReactDOM.js": 108,
      "../../node_modules/react-dom/lib/ReactDOMComponent.js": 109,
      "../../node_modules/react-dom/lib/ReactDOMContainerInfo.js": 110,
      "../../node_modules/react-dom/lib/ReactDOMEmptyComponent.js": 111,
      "../../node_modules/react-dom/lib/ReactDOMFeatureFlags.js": 112,
      "../../node_modules/react-dom/lib/ReactDOMIDOperations.js": 113,
      "../../node_modules/react-dom/lib/ReactDOMInput.js": 114,
      "../../node_modules/react-dom/lib/ReactDOMOption.js": 115,
      "../../node_modules/react-dom/lib/ReactDOMSelection.js": 116,
      "../../node_modules/react-dom/lib/ReactDOMTextComponent.js": 117,
      "../../node_modules/react-dom/lib/ReactDOMTextarea.js": 118,
      "../../node_modules/react-dom/lib/ReactDOMTreeTraversal.js": 119,
      "../../node_modules/react-dom/lib/ReactDefaultBatchingStrategy.js": 120,
      "../../node_modules/react-dom/lib/ReactDefaultInjection.js": 121,
      "../../node_modules/react-dom/lib/ReactElementSymbol.js": 122,
      "../../node_modules/react-dom/lib/ReactEventEmitterMixin.js": 123,
      "../../node_modules/react-dom/lib/ReactEventListener.js": 124,
      "../../node_modules/react-dom/lib/ReactInjection.js": 125,
      "../../node_modules/react-dom/lib/ReactMarkupChecksum.js": 126,
      "../../node_modules/react-dom/lib/ReactMultiChild.js": 127,
      "../../node_modules/react-dom/lib/ReactOwner.js": 128,
      "../../node_modules/react-dom/lib/ReactPropTypesSecret.js": 129,
      "../../node_modules/react-dom/lib/ReactReconcileTransaction.js": 130,
      "../../node_modules/react-dom/lib/ReactRef.js": 131,
      "../../node_modules/react-dom/lib/ReactServerRenderingTransaction.js": 132,
      "../../node_modules/react-dom/lib/ReactServerUpdateQueue.js": 133,
      "../../node_modules/react-dom/lib/ReactVersion.js": 134,
      "../../node_modules/react-dom/lib/SVGDOMPropertyConfig.js": 135,
      "../../node_modules/react-dom/lib/SelectEventPlugin.js": 136,
      "../../node_modules/react-dom/lib/SimpleEventPlugin.js": 137,
      "../../node_modules/react-dom/lib/SyntheticAnimationEvent.js": 138,
      "../../node_modules/react-dom/lib/SyntheticClipboardEvent.js": 139,
      "../../node_modules/react-dom/lib/SyntheticCompositionEvent.js": 140,
      "../../node_modules/react-dom/lib/SyntheticDragEvent.js": 141,
      "../../node_modules/react-dom/lib/SyntheticFocusEvent.js": 142,
      "../../node_modules/react-dom/lib/SyntheticInputEvent.js": 143,
      "../../node_modules/react-dom/lib/SyntheticKeyboardEvent.js": 144,
      "../../node_modules/react-dom/lib/SyntheticTouchEvent.js": 145,
      "../../node_modules/react-dom/lib/SyntheticTransitionEvent.js": 146,
      "../../node_modules/react-dom/lib/SyntheticWheelEvent.js": 147,
      "../../node_modules/react-dom/lib/adler32.js": 148,
      "../../node_modules/react-dom/lib/dangerousStyleValue.js": 149,
      "../../node_modules/react-dom/lib/findDOMNode.js": 150,
      "../../node_modules/react-dom/lib/flattenChildren.js": 151,
      "../../node_modules/react-dom/lib/getEventKey.js": 152,
      "../../node_modules/react-dom/lib/getIteratorFn.js": 153,
      "../../node_modules/react-dom/lib/getNextDebugID.js": 154,
      "../../node_modules/react-dom/lib/getNodeForCharacterOffset.js": 155,
      "../../node_modules/react-dom/lib/getVendorPrefixedEventName.js": 156,
      "../../node_modules/react-dom/lib/quoteAttributeValueForBrowser.js": 157,
      "../../node_modules/react-dom/lib/renderSubtreeIntoContainer.js": 158,
      "../../node_modules/react-dom/node_modules/fbjs/lib/camelize.js": 159,
      "../../node_modules/react-dom/node_modules/fbjs/lib/camelizeStyleName.js": 160,
      "../../node_modules/react-dom/node_modules/fbjs/lib/containsNode.js": 161,
      "../../node_modules/react-dom/node_modules/fbjs/lib/createArrayFromMixed.js": 162,
      "../../node_modules/react-dom/node_modules/fbjs/lib/createNodesFromMarkup.js": 163,
      "../../node_modules/react-dom/node_modules/fbjs/lib/getMarkupWrap.js": 164,
      "../../node_modules/react-dom/node_modules/fbjs/lib/getUnboundedScrollPosition.js": 165,
      "../../node_modules/react-dom/node_modules/fbjs/lib/hyphenate.js": 166,
      "../../node_modules/react-dom/node_modules/fbjs/lib/hyphenateStyleName.js": 167,
      "../../node_modules/react-dom/node_modules/fbjs/lib/isNode.js": 168,
      "../../node_modules/react-dom/node_modules/fbjs/lib/isTextNode.js": 169,
      "../../node_modules/react-dom/node_modules/fbjs/lib/memoizeStringOnly.js": 170
    },
    "usedIds": {
      "0": 0,
      "1": 1,
      "2": 2,
      "3": 3,
      "4": 4,
      "5": 5,
      "6": 6,
      "7": 7,
      "8": 8,
      "9": 9,
      "10": 10,
      "11": 11,
      "12": 12,
      "13": 13,
      "14": 14,
      "15": 15,
      "16": 16,
      "17": 17,
      "18": 18,
      "19": 19,
      "20": 20,
      "21": 21,
      "22": 22,
      "23": 23,
      "24": 24,
      "25": 25,
      "26": 26,
      "27": 27,
      "28": 28,
      "29": 29,
      "30": 30,
      "31": 31,
      "32": 32,
      "33": 33,
      "34": 34,
      "35": 35,
      "36": 36,
      "37": 37,
      "38": 38,
      "39": 39,
      "40": 40,
      "41": 41,
      "42": 42,
      "43": 43,
      "44": 44,
      "45": 45,
      "46": 46,
      "47": 47,
      "48": 48,
      "49": 49,
      "50": 50,
      "51": 51,
      "52": 52,
      "53": 53,
      "54": 54,
      "55": 55,
      "56": 56,
      "57": 57,
      "58": 58,
      "59": 59,
      "60": 60,
      "61": 61,
      "62": 62,
      "63": 63,
      "64": 64,
      "65": 65,
      "66": 66,
      "67": 67,
      "68": 68,
      "69": 69,
      "70": 70,
      "71": 71,
      "72": 72,
      "73": 73,
      "74": 74,
      "75": 75,
      "76": 76,
      "77": 77,
      "78": 78,
      "79": 79,
      "80": 80,
      "81": 81,
      "82": 82,
      "83": 83,
      "84": 84,
      "85": 85,
      "86": 86,
      "87": 87,
      "88": 88,
      "89": 89,
      "90": 90,
      "91": 91,
      "92": 92,
      "93": 93,
      "94": 94,
      "95": 95,
      "96": 96,
      "97": 97,
      "98": 98,
      "99": 99,
      "100": 100,
      "101": 101,
      "102": 102,
      "103": 103,
      "104": 104,
      "105": 105,
      "106": 106,
      "107": 107,
      "108": 108,
      "109": 109,
      "110": 110,
      "111": 111,
      "112": 112,
      "113": 113,
      "114": 114,
      "115": 115,
      "116": 116,
      "117": 117,
      "118": 118,
      "119": 119,
      "120": 120,
      "121": 121,
      "122": 122,
      "123": 123,
      "124": 124,
      "125": 125,
      "126": 126,
      "127": 127,
      "128": 128,
      "129": 129,
      "130": 130,
      "131": 131,
      "132": 132,
      "133": 133,
      "134": 134,
      "135": 135,
      "136": 136,
      "137": 137,
      "138": 138,
      "139": 139,
      "140": 140,
      "141": 141,
      "142": 142,
      "143": 143,
      "144": 144,
      "145": 145,
      "146": 146,
      "147": 147,
      "148": 148,
      "149": 149,
      "150": 150,
      "151": 151,
      "152": 152,
      "153": 153,
      "154": 154,
      "155": 155,
      "156": 156,
      "157": 157,
      "158": 158,
      "159": 159,
      "160": 160,
      "161": 161,
      "162": 162,
      "163": 163,
      "164": 164,
      "165": 165,
      "166": 166,
      "167": 167,
      "168": 168,
      "169": 169,
      "170": 170
    }
  },
  "chunks": {
    "byName": {},
    "byBlocks": {
      "example.js:0/0:0": 0,
      "example.js:0/0:11": 1,
      "example.js:0/0:10": 2,
      "example.js:0/0:7": 3,
      "example.js:0/0:8": 4,
      "example.js:0/0:5": 5,
      "example.js:0/0:3": 6,
      "example.js:0/0:2": 7,
      "example.js:0/0:9": 8,
      "example.js:0/0:1": 9,
      "example.js:0/0:6": 10,
      "example.js:0/0:4": 11
    },
    "usedIds": {
      "0": 0,
      "1": 1,
      "2": 2,
      "3": 3,
      "4": 4,
      "5": 5,
      "6": 6,
      "7": 7,
      "8": 8,
      "9": 9,
      "10": 10,
      "11": 11,
      "12": 12,
      "13": 13,
      "14": 14
    }
  },
  "aggressiveSplits": [
    {
      "modules": [
        "../../node_modules/react-dom/lib/reactProdInvariant.js",
        "../../node_modules/react-dom/lib/Transaction.js",
        "../../node_modules/react-dom/lib/escapeTextContentForBrowser.js",
        "../../node_modules/react-dom/lib/getEventCharCode.js",
        "../../node_modules/react-dom/lib/getEventModifierState.js",
        "../../node_modules/react-dom/lib/getEventTarget.js",
        "../../node_modules/react-dom/lib/isEventSupported.js",
        "../../node_modules/react-dom/lib/forEachAccumulated.js",
        "../../node_modules/react-dom/lib/getHostComponentFromComposite.js",
        "../../node_modules/react-dom/lib/getTextContentAccessor.js",
        "../../node_modules/react-dom/lib/instantiateReactComponent.js",
        "../../node_modules/react-dom/lib/isTextInputElement.js",
        "../../node_modules/react-dom/lib/setTextContent.js",
        "../../node_modules/react-dom/node_modules/fbjs/lib/emptyObject.js",
        "../../node_modules/react-dom/lib/adler32.js",
        "../../node_modules/react-dom/lib/dangerousStyleValue.js",
        "../../node_modules/react-dom/lib/findDOMNode.js",
        "../../node_modules/react-dom/lib/flattenChildren.js",
        "../../node_modules/react-dom/lib/getEventKey.js",
        "../../node_modules/react-dom/lib/getIteratorFn.js",
        "../../node_modules/react-dom/lib/getNextDebugID.js",
        "../../node_modules/react-dom/lib/getNodeForCharacterOffset.js",
        "../../node_modules/react-dom/lib/getVendorPrefixedEventName.js",
        "../../node_modules/react-dom/lib/quoteAttributeValueForBrowser.js",
        "../../node_modules/react-dom/lib/renderSubtreeIntoContainer.js"
      ],
      "hash": "6b21c67c0964251394af593f74f95239",
      "id": 0
    },
    {
      "modules": [
        "../../node_modules/react-dom/node_modules/fbjs/lib/invariant.js",
        "../../node_modules/react-dom/node_modules/fbjs/lib/warning.js",
        "../../node_modules/react-dom/node_modules/object-assign/index.js",
        "../../node_modules/react-dom/node_modules/fbjs/lib/emptyFunction.js",
        "../../node_modules/react-dom/node_modules/fbjs/lib/shallowEqual.js",
        "../../node_modules/react-dom/node_modules/fbjs/lib/focusNode.js",
        "../../node_modules/react-dom/node_modules/fbjs/lib/getActiveElement.js",
        "../../node_modules/react/lib/ReactComponentTreeHook.js",
        "../../node_modules/react-dom/node_modules/fbjs/lib/camelizeStyleName.js",
        "../../node_modules/react-dom/node_modules/fbjs/lib/containsNode.js",
        "../../node_modules/react-dom/node_modules/fbjs/lib/createArrayFromMixed.js",
        "../../node_modules/react-dom/node_modules/fbjs/lib/createNodesFromMarkup.js",
        "../../node_modules/react-dom/node_modules/fbjs/lib/getMarkupWrap.js",
        "../../node_modules/react-dom/node_modules/fbjs/lib/getUnboundedScrollPosition.js",
        "../../node_modules/react-dom/node_modules/fbjs/lib/hyphenate.js",
        "../../node_modules/react-dom/node_modules/fbjs/lib/hyphenateStyleName.js",
        "../../node_modules/react-dom/node_modules/fbjs/lib/isNode.js",
        "../../node_modules/react-dom/node_modules/fbjs/lib/isTextNode.js",
        "../../node_modules/react-dom/node_modules/fbjs/lib/memoizeStringOnly.js"
      ],
      "hash": "6df160cf3935656c5665a22e9af2ed0a",
      "id": 1
    },
    {
      "modules": [
        "../../node_modules/react-dom/lib/SyntheticEvent.js",
        "../../node_modules/react-dom/lib/SyntheticUIEvent.js",
        "../../node_modules/react-dom/lib/SyntheticMouseEvent.js",
        "../../node_modules/react-dom/lib/accumulateInto.js",
        "../../node_modules/react-dom/lib/SVGDOMPropertyConfig.js",
        "../../node_modules/react-dom/lib/SelectEventPlugin.js",
        "../../node_modules/react-dom/lib/SimpleEventPlugin.js",
        "../../node_modules/react-dom/lib/SyntheticAnimationEvent.js",
        "../../node_modules/react-dom/lib/SyntheticClipboardEvent.js",
        "../../node_modules/react-dom/lib/SyntheticCompositionEvent.js",
        "../../node_modules/react-dom/lib/SyntheticDragEvent.js",
        "../../node_modules/react-dom/lib/SyntheticFocusEvent.js",
        "../../node_modules/react-dom/lib/SyntheticInputEvent.js",
        "../../node_modules/react-dom/lib/SyntheticKeyboardEvent.js",
        "../../node_modules/react-dom/lib/SyntheticTouchEvent.js",
        "../../node_modules/react-dom/lib/SyntheticTransitionEvent.js",
        "../../node_modules/react-dom/lib/SyntheticWheelEvent.js"
      ],
      "hash": "d360e468c99903c2d1299ebef056f16a",
      "id": 2
    },
    {
      "modules": [
        "../../node_modules/react-dom/lib/ReactErrorUtils.js",
        "../../node_modules/react-dom/lib/ReactDOMSelect.js",
        "../../node_modules/react-dom/lib/ReactFeatureFlags.js",
        "../../node_modules/react-dom/lib/ReactHostComponent.js",
        "../../node_modules/react-dom/lib/ReactDOMInput.js",
        "../../node_modules/react-dom/lib/ReactDOMSelection.js",
        "../../node_modules/react-dom/lib/ReactDOMTextComponent.js",
        "../../node_modules/react-dom/lib/ReactDOMTextarea.js",
        "../../node_modules/react-dom/lib/ReactDefaultBatchingStrategy.js",
        "../../node_modules/react-dom/lib/ReactDefaultInjection.js",
        "../../node_modules/react-dom/lib/ReactEventEmitterMixin.js",
        "../../node_modules/react-dom/lib/ReactVersion.js"
      ],
      "hash": "76860534ab8ba5e484a2a977ba187fa6",
      "id": 3
    },
    {
      "modules": [
        "../../node_modules/react-dom/lib/ReactInstrumentation.js",
        "../../node_modules/react-dom/lib/ReactInstanceMap.js",
        "../../node_modules/react-dom/lib/ReactInputSelection.js",
        "../../node_modules/react-dom/lib/ReactMount.js",
        "../../node_modules/react-dom/lib/ReactNodeTypes.js",
        "../../node_modules/react-dom/lib/ViewportMetrics.js",
        "../../node_modules/react-dom/lib/ReactEventListener.js",
        "../../node_modules/react-dom/lib/ReactInjection.js",
        "../../node_modules/react-dom/lib/ReactMarkupChecksum.js",
        "../../node_modules/react-dom/lib/ReactOwner.js",
        "../../node_modules/react-dom/lib/ReactReconcileTransaction.js"
      ],
      "hash": "b75a1bd78f5b92799a11b8975d4c0c94",
      "id": 4
    },
    {
      "modules": [
        "../../node_modules/react-dom/index.js",
        "../../node_modules/react-dom/lib/DOMLazyTree.js",
        "../../node_modules/react-dom/lib/DOMNamespaces.js",
        "../../node_modules/node-libs-browser/node_modules/process/browser.js",
        "../../node_modules/react-dom/lib/CSSProperty.js",
        "../../node_modules/react-dom/lib/CallbackQueue.js",
        "../../node_modules/react-dom/lib/ARIADOMPropertyConfig.js",
        "../../node_modules/react-dom/lib/AutoFocusUtils.js",
        "../../node_modules/react-dom/lib/BeforeInputEventPlugin.js",
        "../../node_modules/react-dom/lib/CSSPropertyOperations.js",
        "../../node_modules/react-dom/lib/ChangeEventPlugin.js"
      ],
      "hash": "8b27bb3161668f698375f7be84b6a7c6",
      "id": 5
    },
    {
      "modules": [
        "../../node_modules/react-dom/lib/PooledClass.js",
        "../../node_modules/react-dom/lib/EventPropagators.js",
        "../../node_modules/react-dom/lib/ReactBrowserEventEmitter.js",
        "../../node_modules/react-dom/lib/EventPluginUtils.js",
        "../../node_modules/react-dom/lib/KeyEscapeUtils.js",
        "../../node_modules/react-dom/lib/LinkedValueUtils.js",
        "../../node_modules/react-dom/lib/ReactDOMComponentFlags.js",
        "../../node_modules/react-dom/lib/FallbackCompositionState.js",
        "../../node_modules/react-dom/lib/HTMLDOMPropertyConfig.js",
        "../../node_modules/react-dom/lib/ReactChildReconciler.js"
      ],
      "hash": "1a42bbc60adaf6991bb7b50249a175a5",
      "id": 6
    },
    {
      "modules": [
        "../../node_modules/react-dom/lib/DOMProperty.js",
        "../../node_modules/react-dom/lib/EventPluginHub.js",
        "../../node_modules/react-dom/lib/DOMChildrenOperations.js",
        "../../node_modules/react-dom/lib/EventPluginRegistry.js",
        "../../node_modules/react-dom/lib/DOMPropertyOperations.js",
        "../../node_modules/react-dom/lib/Danger.js",
        "../../node_modules/react-dom/lib/DefaultEventPluginOrder.js",
        "../../node_modules/react-dom/lib/EnterLeaveEventPlugin.js",
        "../../node_modules/react-dom/lib/ReactComponentBrowserEnvironment.js"
      ],
      "hash": "4f84f005b0c46df43fd299291eb05438",
      "id": 7
    },
    {
      "modules": [
        "../../node_modules/react-dom/lib/ReactUpdates.js",
        "../../node_modules/react-dom/lib/ReactReconciler.js",
        "../../node_modules/react-dom/lib/ReactUpdateQueue.js",
        "../../node_modules/react-dom/lib/createMicrosoftUnsafeLocalFunction.js",
        "../../node_modules/react-dom/lib/ReactMultiChild.js",
        "../../node_modules/react-dom/lib/ReactRef.js",
        "../../node_modules/react-dom/lib/ReactServerRenderingTransaction.js",
        "../../node_modules/react-dom/lib/ReactServerUpdateQueue.js"
      ],
      "hash": "3d93b56502b22b64f4bc0cb3c7d5d5a2",
      "id": 8
    },
    {
      "modules": [
        "../../node_modules/react-dom/node_modules/fbjs/lib/ExecutionEnvironment.js",
        "../../node_modules/react-dom/lib/setInnerHTML.js",
        "../../node_modules/react-dom/lib/shouldUpdateReactComponent.js",
        "../../node_modules/react-dom/lib/validateDOMNesting.js",
        "../../node_modules/react-dom/lib/traverseAllChildren.js",
        "../../node_modules/react-dom/node_modules/fbjs/lib/EventListener.js",
        "../../node_modules/react-dom/node_modules/fbjs/lib/camelize.js"
      ],
      "hash": "00d4d2a2ad4d2da7b940b81417ae434c",
      "id": 9
    },
    {
      "modules": [
        "../../node_modules/react-dom/lib/ReactEmptyComponent.js",
        "../../node_modules/react-dom/lib/ReactDOMComponent.js",
        "../../node_modules/react-dom/lib/ReactDOMEmptyComponent.js",
        "../../node_modules/react-dom/lib/ReactDOMIDOperations.js",
        "../../node_modules/react-dom/lib/ReactDOMOption.js",
        "../../node_modules/react-dom/lib/ReactDOMTreeTraversal.js",
        "../../node_modules/react-dom/lib/ReactPropTypesSecret.js"
      ],
      "hash": "3b0e2394af1c44c15c75d8055f5ffd22",
      "id": 10
    },
    {
      "modules": [
        "../../node_modules/react-dom/lib/ReactDOMComponentTree.js",
        "../../node_modules/react-dom/lib/ReactComponentEnvironment.js",
        "../../node_modules/react-dom/lib/ReactCompositeComponent.js",
        "../../node_modules/react-dom/lib/ReactDOM.js",
        "../../node_modules/react-dom/lib/ReactDOMContainerInfo.js",
        "../../node_modules/react-dom/lib/ReactDOMFeatureFlags.js",
        "../../node_modules/react-dom/lib/ReactElementSymbol.js"
      ],
      "hash": "24047db0d33992324c0e0f60c8d34efb",
      "id": 11
    },
    {
      "modules": [
        "../../node_modules/react/lib/ReactComponent.js",
        "../../node_modules/react/lib/ReactNoopUpdateQueue.js",
        "../../node_modules/react/lib/ReactCurrentOwner.js",
        "../../node_modules/react/lib/ReactElementSymbol.js",
        "../../node_modules/react/lib/ReactPropTypeLocationNames.js",
        "../../node_modules/react/react.js",
        "../../node_modules/react/lib/React.js",
        "../../node_modules/react/lib/KeyEscapeUtils.js",
        "../../node_modules/react/lib/PooledClass.js",
        "../../node_modules/react/lib/ReactChildren.js",
        "../../node_modules/react/lib/ReactClass.js"
      ],
      "hash": "34d435e6fa54d2244802662444183767",
      "id": 12
    }
  ]
}
```
