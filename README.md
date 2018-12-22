# TF Multi Classifier

<img src="https://github.com/edodm85/TF_Multi_Classifier/blob/master/Resources/img1.png" width="150">

## What is TF Multi Classifier?

TF Multi Classifier App uses Tensorflow lite to classify camera frames in real time.

You can download from Google Play here:
* [TF Multi Classifier FREE](https://play.google.com/store/apps/details?id=com.edodm85.tfmulticlassifier.free)


## Features

- Classify with Tensorflow lite
- Custom dataset


## Transfer Learning

<B>Step 1</B>: Install tensorflow (following the official tutorial [HERE](https://www.tensorflow.org/install/pip))



<B>Step 2</B>: Clone this Git repository:

<pre>git clone https://github.com/googlecodelabs/tensorflow-for-poets-2</pre>

and "cd" into the following directory:

<pre>cd tensorflow-for-poets-2</pre>



<B>Step 3</B>: Before you start any training, you'll need a set of images to teach the model about the new classes you want to recognize. Download the photos:


Flower photos by google:
<pre>curl http://download.tensorflow.org/example_images/flower_photos.tgz \
    | tar xz -C tf_files</pre>





<B>Step 4</B>: Run the training with this command:

<pre>python -m scripts.retrain \
  --bottleneck_dir=tf_files/bottlenecks \
  --model_dir=tf_files/models/ \
  --summaries_dir=tf_files/training_summaries/mobilenet_1.0_224 \
  --output_graph=tf_files/retrained_graph_mobilenet_1.0_224.pb \
  --output_labels=tf_files/retrained_labels.txt \
  --architecture=mobilenet_1.0_224 \
  --image_dir=tf_files/flower_photos</pre>

This script downloads the pre-trained model and trains that layer on the flower photos you've downloaded. 
In this script I used "mobilenet_1.0_224" a MobileNet (small efficient convolutional neural network) with image resolution 224px and size of model 1.0.

You can use other configurations, for example:
  - mobilenet_A_B with A: 128, 160, 192, or 224px and B: 1.0, 0.75, 0.50, or 0.25.
  - inception_v3



<B>Step 5</B>: Now we have these two file:
  - retrained_labels.txt: text file containing labels.
  - retrained_graph_mobilenet_1.0_224.pb: contains a version of the selected network with a final layer retrained on your categories.


<B>Step 6</B>: Convert the model ".pb" in ".tflite" for tensorflow lite.

I tryed to use tflite_convert in windows without positive results. So I installed ubuntu in a virtual machine, installed tensorflow and now the conversion works. 

<pre>tflite_convert \
  --output_file=tf_files/retrained_graph_lite.tflite \
  --graph_def_file=tf_files/retrained_graph.pb \
  --input_arrays=Mul \
  --output_arrays=final_result</pre>


<B>Step 7</B>: Now you can load this two file in my Android App:
  - retrained_graph_lite.tflite
  - retrained_labels.txt



## License

> Copyright (C) 2018 edodm85.  
> Licensed under the MIT license.  
> (See the [LICENSE](https://github.com/edodm85/TF_Multi_Classifier/blob/master/LICENSE) file for the whole license text.)
