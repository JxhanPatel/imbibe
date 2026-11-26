# Week 8. NLP & Visual Recognition

#### Module 1: Text & Language Processing

- 8.1: **Natural Language Processing**

#### Module 2: Vision & Recognition

- 8.2: **Image Classification**

---

# Module 1: Text & Language Processing

## 8.1 Natural Language Processing

### Converting Text Data to Numeric Form

This data is different than what we have seen so far. Here the training data contains document in text form. As we have mentioned this in the first week of machine learning techniques course, we need to convert the text data to numeric form. When we are presented with any non-numeric dataset, we need to perform some kind of pre-processing to get that data into numeric form. `TfidfVectorizer` is one such API that converts text input into a vector of numerical values.

> [!NOTE] `sklearn.feature_extraction.text.*` has useful APIs to extract features from text data.
>
>

<img width="664" height="646" alt="image" src="https://github.com/user-attachments/assets/e6a90219-ec1e-490a-a49b-378fdcf1562a" />

### Bag-of-Words and Term-Frequency × Inverse-Document-Frequency (TF-IDF)

For text, one option is to use a bag-of-words representation: a sentence is represented by a vector counting the counts of each possible word. Since common words are usually not very important, you’ll want to use TF-IDF to reduce their weight. Instead of counting words, it is also common to count n-grams, which are sequences of n consecutive words—nice and simple. Alternatively, you can encode each word using word embeddings, possibly pretrained. Rather than encoding words, it is also possible to encode each letter, or subword tokens (e.g., splitting “smartest” into “smart” and “est”).

The `TextVectorization` layer will also have an option to output word-count vectors instead of word indices. For example, if the vocabulary contains three words, say `["and", "basketball", "more"]`, then the text `"more and more"` will be mapped to the vector `[1, 0, 2]`: the word `"and"` appears once, the word `"basketball"` does not appear at all, and the word `"more"` appears twice. This text representation is called a *bag of words, since it completely loses the order of the words.* Common words like `"and"` will have a large value in most texts, even though they are usually the least interesting (e.g., in the text `"more and more basketball"` the word `"basketball"` is clearly the most important, precisely because it is not a very frequent word). So, the word counts should be normalized in a way that reduces the importance of frequent words. A common way to do this is to divide each word count by the log of the total number of training instances in which the word appears. This technique is called *Term-Frequency × Inverse-Document-Frequency (TF-IDF).* For example, let’s imagine that the words `"and"`, `"basketball"`, and `"more"` appear respectively in 200, 10, and 100 text instances in the training set: in this case, the final vector will be `[1/ log(200), 0/log(10), 2/log(100)]`, which is approximately equal to `[0.19, 0., 0.43]`.

### CountVectorizer and HashingVectorizer

Vectorizers are used to convert a collection or text documents to a vector representation, thus helping in preprocessing them before applying any model on these text documents. `CountVectorizer` and `HashingVectorizer` both perform the task of vectorizing the text documents. However, there are some differences among them:

- One difference is that `HashingVectorizer` does not store the resulting vocabulary (i.e. the unique tokens). Hence, it can be used to learn from data that does not fit into the computer's main memory. Each mini-batch is vectorized using `HashingVectorizer` so as to guarantee that the input space of the estimator has always the same dimensionality .

When using `CountVectorizer` (part of `sklearn.feature_extraction.text`), we instantiate a `CountVectorizer` object and call `fit_transform` on the list of text documents to obtain a transformed vector representation where the shape of the feature matrix has rows equal to the number of text documents and columns equal to the size of the vocabulary (unique tokens), with vocabulary accessible via the `vocabulary_` attribute  .

<img width="703" height="596" alt="image" src="https://github.com/user-attachments/assets/0b1592f4-26d7-48da-be82-9a06fa48941a" />

### Word Embeddings, Pretrained Embeddings, and Sequence Models for NLP

The idea of using vectors to represent words dates back to the 1960s, and many sophisticated techniques have been used to generate useful vectors, including using neural networks. In 2013, Tomáš Mikolov and other Google researchers introduced an efficient technique to learn word embeddings using neural networks trained on a large corpus of text to predict the words near any given word, producing embeddings where synonyms are close and semantically related words cluster together, as well as organizing concepts along meaningful axes in the embedding space (such as $\text{King} - \text{Man} + \text{Woman}$ being close to $\text{Queen}$, illustrated in Figure 13-5)  .

The TensorFlow Hub project makes it easy to reuse pretrained model components (modules) along with their pretrained weights in your own models  . For NLP applications—such as automatically classifying news articles, automatically flagging offensive comments on discussion forums, summarizing long documents automatically, creating a chatbot or personal assistant, sentiment analysis, or neural machine translation—models include recurrent neural networks (RNNs), CNNs, or Transformers, working over sequences of arbitrary lengths (sentences, documents, or audio samples) as characters or words  .



---






# Module 2: Vision & Recognition

## 8.2 Image Classification

### Loading Image Datasets and Preprocessing

The first step is to create a dataset that contains a collection of digit images (also called examples, samples) written by humans, and then each image should be labelled properly. We have a standard benchmark dataset called MNIST, which has an image of a digit along with the label of the digit present in that image.

When loading MNIST using `keras.dataset` utility `mnist.load_data()`, it returns the training feature matrix and training label vector along with test feature matrix and test label vector. Training data contains 60,000 images and each image is $28 \times 28$; in the test data we have 10,000 images again with the same dimension $28 \times 28$, and the label vectors have 60,000 examples in the training and 10,000 examples in the test.

When loading MNIST or Fashion MNIST using Keras rather than Scikit-Learn, one important difference is that every image is represented as a $28 \times 28$ array rather than a 1D array of size 784, and the pixel intensities are represented as integers (from 0 to 255) rather than floats (from 0.0 to 255.0). When fetching MNIST via `fetch_openml('mnist_784', version=1, return_X_y=True)`, the data matrix $X$ and the respective label vector $y$ are returned as pandas objects and need to be converted to a NumPy array by calling a `to_numpy` method.

In the preprocessing step for image classification:

- Each input image can be flattened into a vector of length 784, reshaping the training feature matrix or training feature tensor with the shape 60,000 by 784 and the test tensor to 10,000 by 784.
- Then we normalize the training feature matrix and test feature matrix by dividing each value in it by 255.

> [!NOTE] Fashion MNIST is a drop-in replacement of MNIST with the exact same format (70,000 grayscale images of $28 \times 28$ pixels each, with 10 classes: `["T-shirt/top", "Trouser", "Pullover", "Dress", "Coat", "Sandal", "Shirt", "Sneaker", "Bag", "Ankle boot"]`), but the images represent fashion items rather than handwritten digits, so each class is more diverse and the problem is significantly more challenging than MNIST (e.g., a simple linear model reaches about 92% accuracy on MNIST, but only about 83% on Fashion MNIST).
>
>

<img width="872" height="404" alt="image" src="https://github.com/user-attachments/assets/fc462d7c-7935-46b2-832c-1d052792d47f" />

### Image Feature Extraction and Tensor Representations

`sklearn.feature_extraction.image.*` has useful APIs to extract features from image data.

In TensorFlow:

- Each input image is typically represented as a 3D tensor of shape `[height, width, channels]`.
- A mini-batch is represented as a 4D tensor of shape `[minibatch size, height, width, channels]`.
- The weights of a convolutional layer are represented as a 4D tensor of shape `[fh, fw, fn', fn]`.
- The bias terms of a convolutional layer are simply represented as a 1D tensor of shape `[fn]`.

Input images are also composed of multiple sublayers: one per color channel (typically three: red, green, and blue / RGB), whereas grayscale images have just one.

<img width="870" height="436" alt="image" src="https://github.com/user-attachments/assets/0cc595d6-d777-4ff5-93a2-f44093c699d6" />

### Convolutional Neural Networks (CNNs) for Image Classification

Convolutional neural networks (CNNs) emerged from the study of the brain’s visual cortex, and they have been used in image recognition since the 1980s. Studies of the visual cortex inspired the neocognitron (introduced in 1980), which evolved into convolutional neural networks, including the 1998 LeNet-5 architecture by Yann LeCun et al. (widely used by banks to recognize handwritten check numbers), which introduced convolutional layers and pooling layers alongside fully connected layers and sigmoid activation functions.

In a convolutional layer:

- Neurons in the first convolutional layer are connected only to pixels in their receptive fields (see Figure 14-2), and each neuron in the second convolutional layer is connected only to neurons located within a small rectangle in the first layer, allowing the network to concentrate on small low-level features in the first hidden layer and assemble them into larger higher-level features in the next hidden layer.
- All neurons in a feature map share the same parameters, which dramatically reduces the number of parameters in the model and allows a pattern learned in one location to be recognized in any other location.

For a simple CNN implementing Fashion MNIST in `tf.keras`:

- It uses `Conv2D` layers (with `relu` activation and `padding="same"`, starting with `input_shape=[28, 28, 1]`), `MaxPooling2D(2)` layers, `Flatten()`, `Dense` layers with `Dropout(0.5)`, and a final `Dense(10, activation="softmax")` output layer.
- For semantic segmentation or object detection, dense layers on top of a CNN can be converted into convolutional layers to form a Fully Convolutional Network (FCN).

<img width="897" height="395" alt="image" src="https://github.com/user-attachments/assets/d107e243-03cc-4564-a18d-b7ac1c35059c" />
