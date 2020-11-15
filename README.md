# bAIthoven: An AI Music Generator 

Integrating Music21, Keras and Tensorflow, we were able to create bAIthoven, an AI Music Generator. Using a Long Short-Term Memory Neural Network, bAIthoven accepts piano MIDI files as an input, and can generate a completely original song as an output. 

## Inspiration

A fascinating discipline which people would never suspect could be revolutionized through Artificial Intelligence is the music industry. The future of music production is very compelling, as start-ups such as OpenAI have created their own music generators. This way, people with no knowledge on music theory can start a successful career as an artist through generating their own original music simply by experimenting with the software. Not only this, but even artists from the past are being reborn. OpenAI in particular has successfully created new music titles of deceased artists by running MIDI files of their old songs through a neural network. This is simply crazy, and shows the potential this sort of technology will have. 

Having said that, our interest was peaked, and we wanted to delve into this niche ourselves. This was our first indeavour into what we believe will be a highly profitable industry in the near future: AI Generation Music Services/Platforms.


## What it does

bAIthoven generates songs from MIDI files that a user inputs. For this project we decided to use classical Beethoven piano pieces, hense the inspiration for the project's name. 

## How we built it

The first step we needed to take prior to training our model was to pre-process the data so it could be accepted by our Rerecurrent (LSTM) Neural Network. As mentioned earlier, the raw data were piano tracks in the form of MIDI files. Using the Music21 API we split the data into two types: Notes and Chords. Notes consisted of pitch, octave, and offset, while chords were containers for sets of notes that were played at the same time. Now, on a basic level, the way that music generation functions is by predicting which note or chord will be played next. So, our prediction model had to contain a sample of every note or chord from all of our training data. To do this we put all the notes and chords into a sequential list so we could create the sequences that served as the input of our network. Lastly, we used a mapping function to map from string-based categorical data to integer-based numerical data. This is done because neural networks perform much better with integer-based numerical data than string-based categorical data.

Next, we created the actual training model itself. This consisted of four different types of layers:
- LSTM layers, which took a sequence as an input and returned either sequences or a matrix.
- Dropout layers which are regularisation techniques that consist of setting a fraction of input units to 0 at each update during the training to prevent overfitting (causes the model to misrepresent the data from which it learned i.e. inaccurate model). 
- Dense layers which are fully connected neural network layers where each input node is connected to each output node.
- Activation layer which determined what activation function our neural network uses to calculate the output of a node.

Our training model was a network consisting of three LSTM layers, three Dropout layers, two Dense layers and one activation layer. As is the case with any neural network, we had to calculate the loss for each iteration of the training. We used categorical cross entropy, and to optimise our network we used a RMSprop optimizer as it is usually a very good choice for recurrent neural networks.

The model.fit() function in Keras was used to train the network. The first parameter was the list of input sequences that we prepared and the second was a list of their respective outputs. We trained the network for 200 epochs (iterations), with each batch propagated through the network containing 64 samples. This took a total time of 6 hours. 

We chose to generate 500 notes using the network since which is roughly two minutes of music and gives the network plenty of space to create a melody. For each note generated we had to submit a sequence to the network. The first sequence we submitted was the sequence of notes at the starting index. For every subsequent sequence that we used as input, we removed the first note of the sequence and inserted the output of the previous iteration at the end of the sequence. To determine the most likely prediction from the output from the network, we extracted the index of the highest value. Afterwards, we collected all the outputs from the network into a single array, creating an array of Note and Chord objects.

Pausing the model at random points in its progress (during the 6 hour interval), we were able to document the progression it made as it got progressively more sophisticated and complex. After the 200th epoch we were finished. 

## Challenges we ran into

Other than time constraints, our main issue was simply learning what the best model to use was. While companies such as OpenAI, as well as some other open source projects on the internet, use Markov regression as their model for AI generated music platforms, we ended up choosing LSTM for its beginner friendly integration system. Doing research prior to starting to actually code was a challenge for sure. 

## Accomplishments that we're proud of

This was our first endeavour into prediction based algorithms. While we have worked with feed forward neural networks used in Deep Learning, this is the first time we experienced a model that predicts and creates an output as it is being trained, not just a model which is intially trained and does not expand its knowledge past that point. 

## What we learned

We learned a lot about Neural Networks, and how AI is trained for music in general. This was a great experience and immersive introduction to this genre of Machine Learning. 

## What's next for bAIthoven: An AI Music Generator 

In the future we plan on having more epochs for more sophisticated music. Also, potentially turning this into either a service or application for anyone to use, regardless of their knowledge of programming. 

