!\[title\](README\_resources/images/title.png)

ABOUT THE PROJECT.

Dependency
need to install
# pytorch
# numpy
# mido
# music21
# cuda : if you want run it gpu

Before we talk about music composition AI

!\[title\](README\_resources/images/about/rnn-lstm-cell.png)

Currently existing music composition AI are all comprised using RNN or LSTM network. But can we really call it a "composition", if what it does is merely predicting the next data from the data tha came before? I don't think those music that were composed using RNN or LSTM networks are legitimate music. Composers do not make the next note by looking at the previous note. They don't write note 'Sol' because 'Do' 'Re' 'Mi' 'Fa' comes before it. Let's consider an example from the artist's perspective. Van Gogh doesn't paint blue at the point (0,2) becaue there is red at (0,0) and yellow at (0,1). Every artist expresses art based on their inspiration. So I think, in order to compose "real music", we need to free ourselves from RNN or LSTM networks.

!\[title\](README\_resources/images/about/composer.png) 
!\[title\](README\_resources/images/about/vangogh.png)

\### Step 1\\. Process The following steps are algorithmic representations of artists' creative processes. 
1\\. Getting inspiration from some objects or events. 
2\\. Draw the entire outline. 
3\\. Add and draw details. 
I thought the generative models I'll be using here have highly similar work flow with the above steps.

!\[title\](README\_resources/images/about/process.png) Similarity between artist's creation process and generative model 
!\[title\](README\_resources/images/about/generator.png) DCGAN, one of the famous generative models

\### Step 2\\. Midi data to network I created an AI model using the GAN model, which is the most mentioned since the NIPS started. I collected 1024 midi data. Midi is one of the formats for expressing music. It uses the note and duration in time sequence. Raw midi data is not suitable for editing. So I refined it to be more easily editable. Also I made training data into fixed-length data. To improve data loading speed in network virtual environment I assembled data to chunks. You can see the results of my project in the work section.

!\[title\](README\_resources/images/about/midiData\_0.png) 
!\[title\](README\_resources/images/about/midiData\_1.png) 
!\[title\](README\_resources/images/about/midiData\_2.png) 
!\[title\](README\_resources/images/about/midiData\_3.png) 
Real data (target data)

\### Step 3\\. Model & Training For this project I've gone through a lot of trials and errors. If you are curious about them, you can read them in the history section. I decided to use the DCGAN model which is known as the most proven GAN model. The data shape used in the training, However, did not match the input shape of the DCGAN model itself. So I changed parameters in convolution network and modified the last layer of discriminator to go through the fully connected layer twice. And to generate \[64, 256\] shape data, I also modified the generator's noise vector to extend from \[2, 8\].

!\[title\](README\_resources/images/about/model\_D\_arch.png) Architecture of Discriminator

!\[title\](README\_resources/images/about/model\_G\_arch.png) Architecture of Generator

!\[title\](README\_resources/images/about/training.png) Training...

!\[title\](README\_resources/images/about/training-weight.png) Training : weight of each sequence note

\### Reference

\[1) LSTM Architecture image : http://colah.github.io/posts/2015-08-Understanding-LSTMs/ (2018.11.01) \](#img\_lstm)
\[2) Tchaikovsky faces : http://witsens.tistory.com/74 (2018.11.01) \](#img\_composer)
\[3) Bedroom in Arles : https://pt.wikipedia.org/wiki/Ficheiro:Vincent\_Willem\_van\_Gogh\_135.jpg (2018.11.01) \](#img\_vangogh)
\[4) DCGAN Architecture image : https://arxiv.org/abs/1607.07539 \](#img\_dcganModel)

!\[title\](README\_resources/images/title.png)

HISTORY OF THE PROJECT.

I've gone through a lot of trials and errors to complete the project. Following logs are the history of the project.

\### Motivation Currently existing music composition AI are all comprised using RNN or LSTM network. But I don't think those music that were composed using RNN or LSTM networks are legitimate music. Meanwhile, the Generative models, especially GAN and VAE, drew my attention. And I found that most of them are used for mostly processing images. I thought it would make more interesting result, if I apply this model to compose music. Moreover I, recently, read interesting paper that the author's team successfully generated 3MB image of people's face (High resolution image). Then the idea popped up to me, 'If AI model can make 3MB image, then it would be also possible to make 3MB music'. So it was the basic idea that started my project.

\### First try - VAE

!\[title\](README\_resources/images/history/vae-gaussian.png)

The first AI-generated model I've used is VAE. Unfortunately, however, VAE was not able to achieve the desired result. And it lead me to postulate the following explanation for the failure of VAE model. 1\\. The VAE model itself has a blurring problem. It appears in the form of noise in music. To check the degree of noise caused by the VAE model, I tried overfitting for one isolated data. Then when I reconstructed the train data, this resulted in emergence of unwanted noise. 2\\. VAE is essentially based on Monte Carlo estimation. Unlike image files, mini batch of the wave file that I use failed to represent a large batch. These lead me to the conclusion that VAE model is not suitable for my project.

 Your browser does not support the audio element.

1 data overfitting - reconstruction Turn down the sound before listening to the music. It can be very loud.

\### Second try - FFT After failure of VAE trial, I contemplated how to use the train data for my model efficiently. And following my professor's advice, I thought the Fourier transform has a potential to solve my problem. The Fourier transform was effective at representing wave and had the ability to compress the wave files. So I decided to use Fourier transform for training data. And while trying to implement the Fourier transformations, another possible application of this model occurred to me. The value of converting the wave file using Fourier's method is represented by the substitution of three values with a real value, a false value, and a frequency value. So if I changed the one-dimensional wave sequence into two dimension, then the data can be expressed in three-dimension. It means that Fourier transformation makes it much easier for computer to deal with the wave file as an three dimensional image. (real value, false value, frequency value in Fourier transformation -> RGB values in image) And if I could represent music file as image file, I thought it would be possible to use the GAN model which has been extensively researched for image generation. Thus, I decided to apply DCGAN model to this AI project.

!\[title\](README\_resources/images/history/fft\_01wave.png) 
!\[title\](README\_resources/images/history/fft\_02fft.png) 1) Wave file to Fourier transformation 
!\[title\](README\_resources/images/history/fft\_03RGB.png) 2) Transformation to 3 dimensional data 
!\[title\](README\_resources/images/history/fft\_04model.png) 3) Normalization and applying to model 
!\[title\](README\_resources/images/history/mytarget\_0.png) 
!\[title\](README\_resources/images/history/mytarget\_1.png) 
!\[title\](README\_resources/images/history/mytarget\_2.png) 
!\[title\](README\_resources/images/history/mytarget\_3.png) 4) Above is the target data in AI model

\### Third try - GAN

!\[title\](README\_resources/images/history/gan-model.png) Idea of the GAN 
!\[title\](README\_resources/images/history/dcgan\_architecture.png) DCGAN architecture - Generator

Although the GAN model is famously known for being difficult to train, I expected the result to be successful. This is because the DCGAN model is clearly validated in the image processing field. Although there was no guarantee that this attempt would work in music composition, It seemed very feasible. I collected 10,240 data and moved its environment to a cloud service that supports graphics card operations. To reduce wave file complexity, the sample rate was lowered from 2048 to 1024\\. There has been numerous attempts. I tried the DCGAN itself. I created and tested 1D DCGAN using 1D convolution network. And I also tried making custom network rather than DCGAN architecture. Unfortunately, however, all of the attempts didn't work. Following images are the result of the model when I borrowed the GAN architecture. As you can see, the significant difference from the target data can be shown from the above images.

!\[title\](README\_resources/images/history/myResult\_0.png) 
!\[title\](README\_resources/images/history/myResult\_1.png) 
!\[title\](README\_resources/images/history/myResult\_2.png) 
!\[title\](README\_resources/images/history/myResult\_3.png) Generated data of DCGAN (Image)

 Your browser does not support the audio element.

Generated data of DCGAN (Audio) Turn down the sound before listening to the music. It can be very loud.

\### Current project While I tested the model, I suddenly started to question the validity of the music. "If AI model can make similar image then is it really worth for music?". So I tried its feasibility by using a reassembling technique. I collected 50 real data and sliced them into 50 pieces and reassembled them. The output of this technique is very similar to the target image. But it's sound is not good as I expected. I also tried Overfitting to reduce complexity of the problem. But generated data, however, was close to null. I suspected the reason for the problem was activation function ReLU in AI model. So I decided to change it to leakyReLU. It didn't work out either. I've tried various approaches, but, as you can see, All the outputs has failed to produce meaningful results. So I decided to change training data format to midi. That is why I decided to use midi format for music composition AI model.

!\[title\](README\_resources/images/history/fft\_reassemble.png) 1) Result : Reassemble 
!\[title\](README\_resources/images/history/gan\_overfitting.png) 2) Result : Overfitting

\### Reference

\[1) VAE model image : https://lilianweng.github.io/lil-log/2018/08/12/from-autoencoder-to-beta-vae.html (2018.11.01) \](#img\_vaeModel)
\[2) GAN model image : https://towardsdatascience.com/understanding-generative-adversarial-networks-4dafc963f2ef (2018.11.01) \](#img\_ganModel)
\[3) DCGAN Architecture image : https://arxiv.org/abs/1607.07539 \](#img\_dcganModel)

\### Good result

DCGAN type A - 1260th training model

!\[note\](README\_resources/images/bestWork/Piano-DCGAN-trialA1-1260.png)

DCGAN type B - 30th training model

!\[note\](README\_resources/images/bestWork/Piano-DCGAN-trialB-30.png)

\### Another selected result

DCGAN type A - 700th training model

!\[note\](README\_resources/images/bestWork/Piano-DCGAN-trialA1-700.png)

DCGAN type A - 80th training model

!\[note\](README\_resources/images/bestWork/Piano-DCGAN-trialA2-80.png)

DCGAN type B - 45th training model

!\[note\](README\_resources/images/bestWork/Piano-DCGAN-trialB-45.png)

DCGAN type B - 60th training model

!\[note\](README\_resources/images/bestWork/Piano-DCGAN-trialB-60.png)

DCGAN type B - 65th training model

!\[note\](README\_resources/images/bestWork/Piano-DCGAN-trialB-65.png)

BEDCGAN - 50th training model

!\[note\](README\_resources/images/bestWork/Piano-BEDCGAN.png)

DCGAN with another generator - Mini Batch

!\[note\](README\_resources/images/bestWork/Piano-MiniBatch.png)

\### Special thanks to

Beck sun woo for translation.
