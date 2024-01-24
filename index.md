---
layout: default
---



## Why do we try to do it?

Imagine that you have a decent model that could make using your application/website more interesting, but you:
- don't want to pay for hosting the model for inference and associated infrastructure
- make use of the end user hardware
- make use (if possible) of the end user GPU
- make it distributable to every architecture
- make the inference efficient (by using WASM)

WASM itself already partly answers the problem.

## WASM

WebAssembly (abbreviated _Wasm_) is a binary instruction format for a stack-based virtual machine. Wasm is designed as a portable compilation target for programming languages, enabling deployment on the web for client and server applications.

> *WebAssembly (Wasm) is generally faster than JavaScript for certain types of tasks. WebAssembly is a binary instruction format that allows developers to run code at near-native speed in modern web browsers. JavaScript, on the other hand, is a high-level scripting language that is often used for web development.*

https://graffersid.com/webassembly-vs-javascript/

So, it would be nice to have the inference part of your code written in WASM, to make it more optimized.

## How to run models with WASM in the browser

First, I would like to clarify that using WASM in the following examples practically means installing packages which contain WASM binaries. 
Operations and functions which would run slower using vanilla JS can be rewritten in RUST and compiled to WASM, but JavaScript still interacts with WASM.

- ### Transformers.js

Transformers.js is designed to be functionally equivalent to Hugging Face’s transformers python library, meaning you can run the same pretrained models using a very similar API. The models support common tasks in different modalities, such as:
- 📝 Natural Language Processing: text classification, named entity recognition, question answering, language modeling, summarization, translation, multiple choice, and text generation.
- 🖼️ Computer Vision: image classification, object detection, and segmentation.
- 🗣️ Audio: automatic speech recognition and audio classification.
- 🐙 Multimodal: zero-shot image classification.

Transformers.js uses ONNX Runtime to run models in the browser. 
If you don't manage to find one of the numerous models from Hugging Face tailored to your needs (or you have finetuned your own model and want to use with Transformers.js), you can follow the path of converting your model, from popular formats (PyTorch, Tensorflow) to ONNX. 
https://huggingface.co/docs/transformers.js

Advantages:
- this library is very well documented
- extremely easy to use (syntax similar to HF transformers)
- surprisingly fast

Disadvantages:

As of now WebGPU support is not included. This means, that running larger models will be constrained to CPU architecture and may not be efficient.










### Header 3

```js
// Javascript code with syntax highlighting.
var fun = function lang(l) {
  dateformat.i18n = require('./lang/' + l)
  return true;
}
```

```ruby
# Ruby code with syntax highlighting
GitHubPages::Dependencies.gems.each do |gem, version|
  s.add_dependency(gem, "= #{version}")
end
```

#### Header 4

*   This is an unordered list following a header.
*   This is an unordered list following a header.
*   This is an unordered list following a header.

##### Header 5

1.  This is an ordered list following a header.
2.  This is an ordered list following a header.
3.  This is an ordered list following a header.

###### Header 6

| head1        | head two          | three |
|:-------------|:------------------|:------|
| ok           | good swedish fish | nice  |
| out of stock | good and plenty   | nice  |
| ok           | good `oreos`      | hmm   |
| ok           | good `zoute` drop | yumm  |

### There's a horizontal rule below this.

* * *

### Here is an unordered list:

*   Item foo
*   Item bar
*   Item baz
*   Item zip

### And an ordered list:

1.  Item one
1.  Item two
1.  Item three
1.  Item four

### And a nested list:

- level 1 item
  - level 2 item
  - level 2 item
    - level 3 item
    - level 3 item
- level 1 item
  - level 2 item
  - level 2 item
  - level 2 item
- level 1 item
  - level 2 item
  - level 2 item
- level 1 item

### Small image

![Octocat](https://github.githubassets.com/images/icons/emoji/octocat.png)

### Large image

![Branching](https://guides.github.com/activities/hello-world/branching.png)


### Definition lists can be used with HTML syntax.

<dl>
<dt>Name</dt>
<dd>Godzilla</dd>
<dt>Born</dt>
<dd>1952</dd>
<dt>Birthplace</dt>
<dd>Japan</dd>
<dt>Color</dt>
<dd>Green</dd>
</dl>

```
Long, single-line code blocks should not wrap. They should horizontally scroll if they are too long. This line should be long enough to demonstrate this.
```

```
The final element.
```
