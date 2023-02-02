# Whisper-Light (Large on 8GB)

A whisper fork that contains lightweight implementations of whisper. Has one GPU branch and a CPU branch. CPU branch is heavily inspired by [TODO].
The most suprising thing to me is that the GPU branch implementation allows running the large model with Beam or Greedydecoder on an 8GB GTX 1070.


## Details on GPU implementation

The GPU implementation removes the Layers written by openai themselves and replaces them with the pytorch ones. I also force fp16 in the whole implementation.
Prior to this model loading failed on my gtx 1070, but I also forced fp16 precision there.
The observed results are two fold:
- The model is able to load on a 8gb VRAM graphics card when it wasn't with the reference implementation
- With the implementation provided by openai, on a GTX 1070 8GB the inference speed of fp16 was slower than when using fp32. Using this implementation, fp16 is much faster than the reference, and also faster than fp32-reference. Still need to investigate if there new-fp16 is also faster than new-fp32
- After first evaluation, there does not seem to be any negative effect on accuracy of transcription.

## TODO:
- The implementation seems to only use a single core of the CPU, without maxing out the GPU, maybe this is a bottleneck worth exploring


## Setup

We used Python 3.9.9 and [PyTorch](https://pytorch.org/) 1.10.1 to train and test our models, but the codebase is expected to be compatible with Python 3.8-3.10 and recent PyTorch versions. The codebase also depends on a few Python packages, most notably [HuggingFace Transformers](https://huggingface.co/docs/transformers/index) for their fast tokenizer implementation and [ffmpeg-python](https://github.com/kkroening/ffmpeg-python) for reading audio files. You can download and install (or update to) the latest release of Whisper with the following command:

    pip install -U openai-whisper

Alternatively, the following command will pull and install the latest commit from this repository, along with its Python dependencies:

    pip install git+https://github.com/Joemgu7/whisper-light.git

To update the package to the latest version of this repository, please run:

    pip install --upgrade --no-deps --force-reinstall git+https://github.com/Joemgu7/whisper-light.git


## License

The code and the model weights of Whisper are released under the MIT License. See [LICENSE](https://github.com/openai/whisper/blob/main/LICENSE) for further details.
