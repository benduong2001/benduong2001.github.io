# Deep Learning Image Auto-Captioner with PyTorch


![](/images/images_cse151b_pa3/image_autocaptioner)

* This is a class project for CSE151B at UC San Diego, where I worked with Takuro Kitasawa, Rye Gleason, and Jeremy Nurding.
* In this project, we developed and trained an AI generative model that could take in input images, and auto-generate English captions properly describing the image's content with correct grammar.
* Having to figure out the proper custom PyTorch Architecture, we eventually developed several versions of this model:
    * The model architecture connects either a custom Convolutional Neural Network or a pre-trained ResNet to an LSTM Recurrent Neural Network
    * The Convolutional Neural Network serves as the Encoder, and the LSTM serves as the Decoder
* Using UCSD's Datahub server, this trained for up to 6 hours, several times for different configurations of parameters.

[See our report!](/images/images_cse151b_pa3/cse151b_report.pdf)
