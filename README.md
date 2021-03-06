# LeNet5-Fashion

<details><summary>Table of Contents</summary><p>

* [Why we made Fashion-MNIST](#why-we-made-fashion-mnist)
* [Get the Data](#get-the-data)
* [Usage](#usage)
* 
</p></details><p></p>


`Fashion-MNIST` is a dataset of [Zalando](https://jobs.zalando.com/tech/)'s article images—consisting of a training set of 60,000 examples and a test set of 10,000 examples. Each example is a 28x28 grayscale image, associated with a label from 10 classes. We intend `Fashion-MNIST` to serve as a direct **drop-in replacement** for the original [MNIST dataset](http://yann.lecun.com/exdb/mnist/) for benchmarking machine learning algorithms. It shares the same image size and structure of training and testing splits.

Here's an example how the data looks (*each class takes three-rows*):

![](https://s3-eu-central-1.amazonaws.com/zalando-wp-zalando-research-production/2017/08/fashion-mnist-sprite.png)


## Why we made Fashion-MNIST

The original [MNIST dataset](http://yann.lecun.com/exdb/mnist/) contains a lot of handwritten digits. Members of the AI/ML/Data Science community love this dataset and use it as a benchmark to validate their algorithms. In fact, MNIST is often the first dataset researchers try. *"If it doesn't work on MNIST, it **won't work** at all"*, they said. *"Well, if it does work on MNIST, it may still fail on others."* 

### To Serious Machine Learning Researchers

Seriously, we are talking about replacing MNIST. Here are some good reasons:

- **MNIST is too easy.** Convolutional nets can achieve 99.7% on MNIST. Classic machine learning algorithms can also achieve 97% easily. Check out [our side-by-side benchmark for Fashion-MNIST vs. MNIST](http://fashion-mnist.s3-website.eu-central-1.amazonaws.com/), and read "[Most pairs of MNIST digits can be distinguished pretty well by just one pixel](https://gist.github.com/dgrtwo/aaef94ecc6a60cd50322c0054cc04478)."
- **MNIST is overused.** In [this April 2017 Twitter thread](https://twitter.com/goodfellow_ian/status/852591106655043584), Google Brain research scientist and deep learning expert Ian Goodfellow calls for people to move away from MNIST.
- **MNIST can not represent modern CV tasks**, as noted in [this April 2017 Twitter thread](https://twitter.com/fchollet/status/852594987527045120), deep learning expert/Keras author François Chollet.

## Get the Data

Many ML libraries  already include Fashion-MNIST data/API, give it a try!

You can use direct links to download the dataset. The data is stored in the **same** format as the original [MNIST data](http://yann.lecun.com/exdb/mnist/).


### Labels
Each training and test example is assigned to one of the following labels:

| Label | Description |
| --- | --- |
| 0 | T-shirt/top |
| 1 | Trouser |
| 2 | Pullover |
| 3 | Dress |
| 4 | Coat |
| 5 | Sandal |
| 6 | Shirt |
| 7 | Sneaker |
| 8 | Bag |
| 9 | Ankle boot |

## Usage

### Loading data with Python (requires [NumPy](http://www.numpy.org/))

Use `utils/mnist_reader` in this repo:
```python
import mnist_reader
X_train, y_train = mnist_reader.load_mnist('data/fashion', kind='train')
X_test, y_test = mnist_reader.load_mnist('data/fashion', kind='t10k')
```

### Loading data with Tensorflow
Make sure you have [downloaded the data](#get-the-data) and placed it in `data/fashion`. Otherwise, *Tensorflow will download and use the original MNIST.*

```python
from tensorflow.examples.tutorials.mnist import input_data
data = input_data.read_data_sets('data/fashion')

data.train.next_batch(BATCH_SIZE)
```

Note, Tensorflow supports passing in a source url to the `read_data_sets`. You may use: 
```python
data = input_data.read_data_sets('data/fashion', source_url='http://fashion-mnist.s3-website.eu-central-1.amazonaws.com/')
```

Also, an official Tensorflow tutorial of using `tf.keras`, a high-level API to train Fashion-MNIST [can be found here](https://www.tensorflow.org/tutorials/keras/basic_classification).

## Architecture
I decided to start with a slightly modified [LeNet-5](http://yann.lecun.com/exdb/lenet/) architecture. It is a very simple and well known convolutional neural network architecture that is easy to implement and usually gives good results out of the box to start with. 
![image](https://miro.medium.com/max/1200/1*y68ztClLF6ae7P53ayyFzQ.png)

## Conclusions
It was relativelly easy to train a model that at least could learn the train dataset well (avoidable bias reduction). The resulting model was clearly overfitting the train dataset and not generalizing well enough. However, reducing the overfitting was a much more challenging problem. Dropout regularization and data augmentation helped a bit, but probably LeNet-5 was not the ideal architecture for this concrete dataset. Some of the dataset classes were very similar to each other (e.g. ankle boots and sneakers, dresses and coats), so a slightly more sophisticated and deeper model like [AlexNet](http://vision.stanford.edu/teaching/cs231b_spring1415/slides/alexnet_tugce_kyunghee.pdf) would probably perform better.

Training the model on a Macbook with no GPU acceleration was far from ideal too, and in the future I will definitely invest some time to setup an easy way to run these experiments on AWS spot instances or in Google Cloud. Either way, if you are using a Macbook and still want to run these experiments or similar ones, I definitely recommend that you compile Tensorflow from source, as it will enable some CPU optimizations that are not enabled in the binary package and which makes a big difference.
