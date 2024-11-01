Neural Networks for Photon Mapping
===============

This is our main renderer with support for photon mapping with neural networks. It uses pbrt3 as base and onnx runtime for neural network inferencing.

We added two main integrators:

cppm
For Chi-squared Progressive Photon Mapping. It has the same parameters as sppm with two modifications:
- There is no radius parameter since the algorithm decides the initial radius with K-nn search.
- We add two parameters "renderOnlyCau" and "renderOnlyIndirect" to render only caustics or indirect illumination respectively. default: false.

_______________________________________
pathcpm
This is a pipeline to render caustics separately from direct and indirect illumination. It supports rendering caustics with neural networks as well. We add the following parameters:

- radius: is interpreted differentaly compared to sppm. Here the radius purpose is to decide what is the maximum radius to render a hitpoint. This is used to spare computation in areas free of caustic photons. default: infinite.
- useNN: boolean to decide if to use the neural network or not. default: false.
- NNType: integer to decide the neural network type. We have 0: for NN predicting radii, 1: for NN predicting kernels, 2: for NN predicting radiances. default: 0.
- reducedFeatures: integer to decide the type of features we input. We have 0: for full features (positions, directions, contributions), 1: no positions, 2: no directions, 3: no contributions, 4: swap the contributions with luminance. default: 0.
- NNpath: string used to point to the neural network onnx file. default empty.
- knn: For the number of photons to search at each point. default: 50.
- glassSamples: For the number of samples used on specular surfaces in each iteration. default: 1.
- renderOnlyCau has the same effect as in cppm. default: false.

Note that using neural network with false path, or incompatible with the number of inputs or outputs will crash the program.

_______________________________________
sppm
We modified sppm to support outputing neural network dataset. We added the following parameters:

- outputNNData: integer to decide wether to output dataset and what type. We have 0: don't output anything, 1: output all indirect illumination photons, 2: output only caustic photons
- photonsToStore: integer to decide the number of photon paths to store.
- renderOnlyCau, renderOnlyIndirect have the same effect as cppm. We also add renderBoth to output image with full lighting and image with only caustics with one run.

_______________________________________

Note: don't forget to add the dlls to the exe path after building.
