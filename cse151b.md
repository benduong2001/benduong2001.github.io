# Deep Learning Image Auto-Captioner with PyTorch

* For this class specifically, we are prohibited from publicly sharing our project's github repo and its code. However, we can show our report.

[See our report!](/images/images_cse151b/cse151b_report.pdf)

![](/images/images_cse151b/image_autocaptioner.png)

* This is a class project for CSE151B at UC San Diego. CSE151B is a class about Deep Learning in PyTorch taught by Professor Garrison Cottrell
(see https://jacobsschool.ucsd.edu/node/3288).

*  Our team included Takuro Kitasawa, Rye Gleason, Jeremy Nurding, and myself.
* In this project, we developed and trained an AI generative model that could take in input images, and auto-generate English captions properly describing the image's content with correct grammar.
* Having to figure out the proper custom PyTorch Architecture, we eventually developed several versions of this model:
    * The model architecture connects either a custom Convolutional Neural Network or a pre-trained ResNet to an LSTM Recurrent Neural Network.
         * This custom Convolutional Neural Network Architecture was called AlexNet (Not shown in the flowchart above). I was the teammate responsible for implementing it's architecture on PyTorch. Since it was not pre-trained, the model variant that used it trained much slower.
         * Regardless, each image needs to undergo another Dense Layer to a shape compatible with the convolutional neural network's input.
    * The Convolutional Neural Network serves as the Encoder, and the LSTM serves as the Decoder
    * The LSTM is originally trained with Teacher-forcing. Each auto-generated sentence is capped at 20 words.
* Using UCSD's Datahub server, this trained for up to 6 hours, several times for different configurations of parameters.


![](/images/images_cse151b/image3.png)

![](/images/images_cse151b/image2.png)

![](/images/images_cse151b/image1.png)



