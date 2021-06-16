
# Data Augmentation for Deep Learning


We describe various techniques to perform data augmentation used in training a TensorFlow-Keras based Deep Learning model. Data augmentation may or may not be a part of data preprocessing pipeline. For example, when training is done by a single device (CPU or GPU), data augmentation is typically done during preprocessing. However, in distributed training (using multiple GPUs), we can expedite the training time by integrating data augmentation into the learning model so that it is done by GPUs.

Thus, as we present various techniques for data augmentation, we keep in mind the following two broad implementation scenarios. 
- Data Augmentation during Data Preprocessing
- Data Augmentation during Training



## Data Augmentation: Various Techniques

The standard choice of a data augmentation technique is **Keras ImageDataGenerator** class. In general, Keras image data preprocessing API is very convenient to build a data preprocessing pipeline and is a go-to option for **small projects**. 

https://keras.io/api/preprocessing/image/#image-data-preprocessing

In the first notebook, we briefly introduce the ImageDataGenerator class. Second notebook provides description of various augmentation methods of this class.

- The main limitation of the ImageDataGenerator class is that it is **not efficient**. It adds significant delay in data preprocessing. Thus, it is not suitable for large projects.

An efficient alternative to build the data pipeline for large projects is to use the **tf.data** API. In this approach, we represent the dataset as a tf Dataset object, then apply various transformations including data augmentation. For performance benefit, data augmentation should be done using TensorFlow operations or ops. An op is a node in the TensorFlow graph, which is used to represent computations in the TensorFlow ecosystem. Various augmentation ops are defined in the **tf.image** module.
https://www.tensorflow.org/api_docs/python/tf/image

- Although the tf.image module is very efficient, it is **not always flexible**. For example, its rotation function is limited to a fixed 90 degree and is applied deterministically.

For flexibility, a great choice is **Keras image preproessing layer** API. It uses the Sequential API to build data augmentation pipeline. The best part of it is that we can conveniently incorporate this data augmentation layer inside a model and perform augmentation during training, which is a suitable approach in distributed training (for **large projects**).

https://www.tensorflow.org/api_docs/python/tf/keras/layers/experimental/preprocessing

We emphasize two aspects of the data augmentation pipeline.
- Efficiency (required in large projects)
- Flexibility (required in large projects and complex problems)

Based on these two considerations, we recommed:
- Small projects: Keras ImageGenerator 
- Large projects (distributed training): Keras image preprocessing layer (it can be combined with TensorFlow image ops as well as arbitrary python functions)


## Notebook Index

- Notebook 1: We present the following data augmentation techniques.
    
      - Keras ImageGenerator
      - TensorFlow Image Ops (tf.image module)
      - TensorFlow Addons Image Ops (tfa.image module)
      - Keras Image Preprocessing Layer
      - Flexible Data Augmentation Layer
          -- Using Python Library Functions as TensorFlow Ops 
          -- Combining TensorFlow Image Ops with Keras Preprocessing Layer
          
- Notebook 2: Data augmentation using Keras ImageGenerator
