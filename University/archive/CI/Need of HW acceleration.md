![](https://i.imgur.com/u0PkaOB.png)

Introducing parallel solutions to make training faster.
First solution implemented are GPU.

![](https://i.imgur.com/Qph8ev6.png)

Deep learning models began to appear and widely adopted. Usually there are error only when images are low quality, it doesn't saturate as the common models. 
Most of the time critical operation are multiplication between matrix/vector or matrix vector multiplication. 
GPUs are up to 1000x faster than CPUs, but they need high level languages (CUDA and OpenCL). 

![](https://i.imgur.com/hL5Oamk.png)

IN 1980 the Back Propagation algorithm was released helping the training of  ML models

![](https://i.imgur.com/h2qdC3f.png)

Then these neurons are stacked and connected between them with their own weight and activation function.
In training activation functions need derivatives and the complexity depends from how much neurons are stacked. If you want to approximate a function if you consider a single layer you can approximate any function using a certain number of neurons.
Training:
- Compute an error from each computing network using the results of computation.
- Train the model with a gradient approach where you try to minimize the error.

![](https://i.imgur.com/beZg1Xv.png)

With Backpropagation I can add the result of the computation to the weight having an improvement in my results. The weight update are made for each sample category, but impossible consider all the dataset so we consider a batch of the dataset so we can have an approximation of the weight change.
Usually there are many local optimal changes.
Networks applied today consist of various layer stacked, the network expands very quickly. For each tasks there are specific models. On the inputs there could be applied filters with a weight for each input. If you decrease the size of the filter you will increase the weight of the single filter. Good architecture for image recognition. Special models for text recognition.

![](https://i.imgur.com/hY1JBCH.png)

![](https://i.imgur.com/saxOklO.png)

You can also divide the image on multiple GPUs. IN this case every GPU work on the data it received and updates its model indipendently from the others, creating multiple models. So to have a single model each GPUs shares its weight with the others and the sum of the weight is used to update the general model updating then the GPUs models.

![](https://i.imgur.com/S3aZLHV.png)

Toroidal topology, each GPU can talk with the others, the channels are bidirectional. Good having a network card that can directly talks with the GPUs to have direct communication between Network data and GPUs without the need to copy it in memory. 
Several generation of NVLink. Each one add some lines(Today we have 18 lanes with a maximum of 900 GB/s total).

GPu training DNN on multiple GPUs has become to heavy in memory(3.2 TB for GPT 3.5) so it is needed the work of multiple GPUs only to sustain the quantity of data(Each GPU can arrive to 80 GB). Memory is used mainly to store batches of the training set. Use multiple nodes to evolve the model. 
### Tensor processing unit(TPU)
Devices used only for machine learning. Data Centers could not handle the requests from models and application. Need of an architecture for ML loads. 
A Tensor is an n-dimensional matrix. This is the basic unit of operation in TensorFlow.
TPUs are used for training and inference: TPUv1 is an inference-focused accelerator connected to the host CPU through PCIe links, differently, TPUv2, TPV3, and TPV4 focus on training and inference
In TPUv2 you can access a fast memory. The memory was 10x faster than DDR4, today is 20x faster than DDR4. These modules can be put in clusters called POD with up to 512 TPU and 4  TB of total memory(Usually they have 64 units).
TPUv3 the devices could be watercooled. You don't only perform training but also AutoML(choose the best model for the task, can change it during the operations, selects the best solution for the task), new ML capabilities. 
TPUv4 was announced during 2021, initially used only to support google services(not available as a cloud service). Now there are also TPUv5 with two version v5e and v5p

![](https://i.imgur.com/N48kp2o.png)

Amazon also has created its architectures optimized for LLMs models. These architectures are called Trainium.

![](https://i.imgur.com/LIDQesv.png)

### Field Programmable Gate Array(FPGA)
You can customise the hardware to execute a single task. Set of configurable logic blocks that can implements various task.

![](https://i.imgur.com/IVFA3Tv.png)

They are not so fast but having the possibility to create new circuit for various task, they are energy efficient. 
GPU are general enough, FPGA are between TPU and GPU, they are flexible and general purpose and then there is TPU where the chipset is made to compute a single task.

![](https://i.imgur.com/blp46s1.png)

GPUs can be more energy efficient than CPUs in data center as one can substitute 40 CPUs.

![](https://i.imgur.com/EvaHE1z.png)

ML code is 10% of the code in Google ML centre. There is a lot of infrastructure and other logistic code to be implemented to use the ML code efficiently. Serving infrastructure is really important as it permit to speed up and optimise all the other operation. The Computer Infrastructure matter a lot today.  