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
(Transformers.js](https://huggingface.co/docs/transformers.js)
The [conversion to ONNX format](https://github.com/huggingface/optimum#onnx--onnx-runtime) (in case of Transformers.js for ONNX runtime) is rather easy with these commands:
```
pip install --upgrade-strategy eager optimum[onnxruntime]
optimum-cli inc quantize --model distilbert-base-cased-distilled-squad --output ./quantized_distilbert
```
As you can see optimum-cli makes it easy to perform quantization of your model, which allows to make a smaller file from the model weights. 
It is a good practice to load your model to verify that its accuracy after quantization is still acceptable for your use.

Advantages:
- Transformers.js library is very well documented
- extremely easy to use (syntax similar to HF transformers)
- surprisingly fast

Disadvantages:

As of now WebGPU support is not included. This means, that running larger models will be constrained to CPU architecture and may not be efficient.
However I recommend checking out their [projects repo](https://xenova.github.io/transformers.js/) for some impressive examples, they may be fast enough and not need GPU support.

### Simple Transformers.js app
I experimented with one of the examples on Transformers.js Github page: [react-translator](https://github.com/xenova/transformers.js/tree/main/examples/react-translator) to convert it to a [simple question answering app](https://github.com/justyna3773/text_manipulation_transformers_js.git).
I was impressed with how adaptable this example is, the only thing you have to change the pipeline from *translation* to *text2text-generation* to make the model generate answer instead of translating is MyTranslationPipeline class in worker.js file:

```js
import { pipeline } from '@xenova/transformers';

class MyTranslationPipeline {
    static task = 'text2text-generation';
    static model = 'Xenova/flan-t5-base';
    static instance = null;

    static async getInstance(progress_callback = null) {
        if (this.instance === null) {
            this.instance = pipeline(this.task, this.model, { progress_callback });
        }

        return this.instance;
    }
}

```
Essentially you change the name of the task and model ID from HuggingFace.
After that you run ```npm run dev``` and voila, you can access your App under local address: [http://localhost:5173/]()

* * *

## WebGPU

![implementation](implementation_status.png)
WebGPU exposes an API for performing operations, such as rendering and computation, on a Graphics Processing Unit.
It is still in the early stages, but Chrome enables it by default since April 2023 (version 113).

### Project which use WebGPU
- ### Web-LLM
>*WebLLM is a modular, customizable javascript package that directly brings language model chats directly onto web browsers with hardware acceleration. **Everything runs inside the browser with no server support and is accelerated with WebGPU.** We can bring a lot of fun opportunities to build AI assistants for everyone and enable privacy while enjoying GPU acceleration.*
[Web-LLM demo](https://webllm.mlc.ai/)

Advantages:
If you are fine with your user downloading large model files then it works well. It also has a pretty extensive documentation.

Problems:
- initial downloading of the model is slow and the models supported are heavy (several GB). TinyLLama for example, takes 2 GB of your memory.
- I experimented with disabling GPU support in the browser - then the chat does not work at all. This undermines the idea that we can have one app to run on all hardwares.
- customization capabilities - there is a limited number of model architectures it supports (Mistral, LLama, Red Pyjamas and a few others). You can add model weights from your finetuned model in easy fashion, however adding your own architecture is problematic.


Anyway, this project seems to have a lot of potential and well documented workflow on how to add your own model library. 
I wanted to add a small BERT model, to make it decently good for conversations or question but at the same time keep it as small as possible.  
I followed their worflow for adding new model library: [https://llm.mlc.ai/docs/deploy/javascript.html#bring-your-own-model-library](https://llm.mlc.ai/docs/deploy/javascript.html#bring-your-own-model-library)













From there it is really easy to deploy the app to Github Pages:

1. Install gh-pages extension ```npm install gh-pages```
2. Add following lines to your package.json file:
```
"homepage": "https://justyna3773.github.io/chat_wasm/",
```












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
