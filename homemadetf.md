# Re-inventing Convolutional Neural Networks on Python From Scratch Without Packages.

* During my first year of college as a data science major, I was very intrigued about how neural networks worked, particularly convolutional neural networks - which could algorithmically tell you if a picture is a cat or a dog for example. 
* There were many youtube tutorial and Medium articles that covered it. But I felt very unsatisfied by them, and didn't want to mindlessly copy and paste Tensorflow code, without seeing how or why the math worked under the hood.
* I had just finished my multi-variable vector calculus and linear algebra classes, which gave me foundation in finally understanding the underlying math of these models
* And so, I felt that seeing was believing, and spent time deciding to recreate neural networks on Python from scratch without packages. 
    * Not only did I implement Neural networks by its own on this project, but I also integrated the support for convolutional ones too. Meaning this project was capable of being trained to do basic image recognition.
    * This project was also personal practice to apply more computer science-related topics and principles, such as Objected Oriented Programming, Class Inheritance, extensibility, and the recursion needed for the multi-variable calculus in the neural network's backpropagation.
    * Having concluded my matrix math classes, I wanted to test-apply what I just learned, meaning I also coded the Matrix operations in this project from scratch too. ... No NumPy at all!
    * Thus, the classes that I've implemented in this project would be:
         * Different Neural Network Layers:
               - The "Dense" Layer for Matrix Multiplication with a Bias Addition (MX+b)
               - The Activation Layer for Non-linearities- namely Sigmoid and ReLU
               - The final Loss Function Layer
               - The "convolutional filter" layer that behaves like Conv2D.
               - The MaxPooling Layer.
         * Matrices, with Matrix math operations. Having concluded my matrix math classes, I wanted to test-apply what I just learned, meaning I also coded the Matrix operations in this project from scratch too. ... **No NumPy at all**!

* With a graphic drawing-canvas interface done with Python Tkinter, I trained and tested the final project's iteration with the basic task of differentiating 1's and 0's, and showed the python implementation worked as intended.

![](images/images_HomemadeTF/bootlegCNN_demo_gif.gif)

### What did I gain from this?
* Even though this project, in hindsight was a reinvention of the wheel -with no business usefulness, it was a highly foundational introduction that pushed me into the cold swimming pool of machine learning. 
