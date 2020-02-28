# Image Features

## Preprocessing

Ludwig supports both grayscale and color images. The number of channels is inferred, but make sure all your images have the same number of channels. During preprocessing, raw image files are transformed into numpy ndarrays and saved in the hdf5 format. All images in the dataset should have the same size. If they have different sizes, a `resize_method`, together with a target `width` and `height`, must be specified in the feature preprocessing parameters.

* `in_memory` \(default `true`\): defines whether image dataset will reside in memory during the training process or will be dynamically fetched from disk \(useful for large datasets\). In the latter case a training batch of input images will be fetched from disk each training iteration.
* `resize_method` \(default `crop_or_pad`\): available options: `crop_or_pad` - crops images larger than the specified `width` and `height` to the desired size or pads smalled images using edge padding; `interpolate` - uses interpolation to resize images to the specified `width` and `height`.
* `height` \(default `null`\): image height in pixels, must be set if resizing is required
* `width` \(default `null`\): image width in pixels, must be set if resizing is required
* `num_channels` \(default `null`\): number of channels in the images. By default, if the value is `null`, the number of channels of the first image of the dataset will be used and if there is an image in the dataset with a different number of channels, an error will be reported. If the value specified is not `null`, images in the dataset will be adapted to the specified size. If the value is `1`, all images with more then one channel will be greyscaled and reduced to one channel \(trasparecy will be lost\). If the value is `3` all images with 1 channel will be repeated 3 times to obtain 3 channels, while images with 4 channels will lose the transparecy channel. If the value is `4`, all the images with less than 4 channels will have the remaining channels filled with zeros.
* `scaling` \(default `pixel_normalization`\): what scaling to perform on images. By default `pixel_normalization` is performed, which consists in dividing each pixel values by 255, but `pixel_standardization` is also available, whic uses [TensorFlow's per image standardization](https://www.tensorflow.org/api_docs/python/tf/image/per_image_standardization).

Depending on the application, it is preferrable not to exceed a size of `256 x 256`, as bigger sizes will, in most cases, not provide much advantage in terms of performance, while they will considerably slow down training and inference and also make both forward and backward passes consume considerably more memory, leading to memory overflows on machines with limited amounts of RAM or on GPUs with limited amounts of VRAM.

Example of a preprocessing specification:

```yaml
name: image_feature_name
type: image
preprocessing:
  height: 128
  width: 128
  resize_method: interpolate
  scaling: pixel_normalization
```

## Input Features and Encoders

Input image features are transformed into a float valued tensors of size `N x H x W x C` \(where `N` is the size of the dataset and `H x W` is a specific resizing of the image that can be set, and `C` is the number of channels\) and added to HDF5 with a key that reflects the name of column in the CSV. The column name is added to the JSON file, with an associated dictionary containing preprocessing information about the sizes of the resizing.

Currently there are two encoders supported for images: Convolutional Stack Encoder and ResNet encoder which can be set by setting `encoder` parameter to `stacked_cnn` or `resnet` in the input feature dictionary in the model definition \(`stacked_cnn` is the default one\).

**CONVOLUTIONAL STACK ENCODER¶**

Convolutional Stack Encoder takes the following optional parameters:

* `conv_layers` \(default `null`\): it is a list of dictionaries containing the parameters of all the convolutional layers. The length of the list determines the number of stacked convolutional layers and the content of each dictionary determines the parameters for a specific layer. The available parameters for each layer are: `filter_size`, `num_filters`, `pool_size`, `norm`, `activation` and `regularize`. If any of those values is missing from the dictionary, the default one specified as a parameter of the encoder will be used instead. If both `conv_layers` and `num_conv_layers` are `null`, a default list will be assigned to `conv_layers` with the value `[{filter_size: 7, pool_size: 3, regularize: false}, {filter_size: 7, pool_size: 3, regularize: false}, {filter_size: 3, pool_size: null, regularize: false}, {filter_size: 3, pool_size: null, regularize: false}, {filter_size: 3, pool_size: null, regularize: true}, {filter_size: 3, pool_size: 3, regularize: true}]`.
* `num_conv_layers` \(default `null`\): if `conv_layers` is `null`, this is the number of stacked convolutional layers.
* `filter_size` \(default `3`\): if a `filter_size` is not already specified in `conv_layers` this is the default `filter_size` that will be used for each layer. It indicates how wide is the 1d convolutional filter.
* `num_filters` \(default `256`\): if a `num_filters` is not already specified in `conv_layers` this is the default `num_filters` that will be used for each layer. It indicates the number of filters, and by consequence the output channels of the 2d convolution.
* `pool_stride` \(default `1`\): if a `pool_stride` is not already specified in `conv_layers` this is the default `pool_stride` that will be used for each layer.
* `fc_layers` \(default `null`\): it is a list of dictionaries containing the parameters of all the fully connected layers. The length of the list determines the number of stacked fully connected layers and the content of each dictionary determines the parameters for a specific layer. The available parameters for each layer are: `fc_size`, `norm`, `activation` and `regularize`. If any of those values is missing from the dictionary, the default one specified as a parameter of the encoder will be used instead. If both `fc_layers` and `num_fc_layers` are `null`, a default list will be assigned to `fc_layers` with the value `[{fc_size: 512}, {fc_size: 256}]`. \(only applies if `reduce_output` is not `null`\).
* `num_fc_layers` \(default `1`\): This is the number of stacked fully connected layers.
* `fc_size` \(default `256`\): if a `fc_size` is not already specified in `fc_layers` this is the default `fc_size` that will be used for each layer. It indicates the size of the output of a fully connected layer.
* `norm` \(default `null`\): if a `norm` is not already specified in `fc_layers` or `conv_layers` this is the default `norm` that will be used for each layer. It indicates the norm of the output and it can be `null`, `batch` or `layer`.
* `activation` \(default `relu`\): if an `activation` is not already specified in `fc_layers` or `conv_layers` this is the default `activation` that will be used for each layer. It indicates the activation function applied to the output.
* `dropout` \(default `false`\): determines if there should be a dropout layer after each layer.
* `initializer` \(default `null`\): the initializer to use. If `null`, the default initialized of each variable is used \(`glorot_uniform` in most cases\). Options are: `constant`, `identity`, `zeros`, `ones`, `orthogonal`, `normal`, `uniform`, `truncated_normal`, `variance_scaling`, `glorot_normal`, `glorot_uniform`, `xavier_normal`, `xavier_uniform`, `he_normal`, `he_uniform`, `lecun_normal`, `lecun_uniform`. Alternatively it is possible to specify a dictionary with a key `type` that identifies the type of initializer and other keys for its parameters, e.g. `{type: normal, mean: 0, stddev: 0}`. To know the parameters of each initializer, please refer to [TensorFlow's documentation](https://www.tensorflow.org/api_docs/python/tf/keras/initializers).
* `regularize` \(default `true`\): if `true` the weights of the layers are added to the set of weights that get regularized by a regularization loss \(if the `regularization_lambda` in `training` is greater than 0\).

Example image feature entry using a convolutional stack encoder \(with default parameters\) in the input features list:

```yaml
name: image_csv_column_name
type: image
encoder: stacked_cnn
tied_weights: null
conv_layers: null
num_conv_layers: null
filter_size: 3
num_filters: 256
pool_size: null
fc_layers: null
num_fc_layers: 1
fc_size: 256
activation: relu
norm: null
dropout: false
initializer: null
regularize: true
preprocessing:  # example pre-processing
  -
    height: 28
    width: 28
    num_channels: 1
```

### RESNET ENCODER

[ResNet](https://arxiv.org/abs/1603.05027) Encoder takes the following optional parameters:

* `resnet_size` \(default `50`\): A single integer for the size of the ResNet model. If has to be one of the following values: `8`, `14`, `18`, `34`, `50`, `101`, `152`, `200`.
* `num_filters` \(default `16`\): It indicates the number of filters, and by consequence the output channels of the 2d convolution.
* `kernel_size` \(default `3`\): The kernel size to use for convolution.
* `conv_stride` \(default `1`\): Stride size for the initial convolutional layer.
* `first_pool_size` \(default `null`\): Pool size to be used for the first pooling layer. If none, the first pooling layer is skipped.
* `batch_norm_momentum` \(default `0.9`\): Momentum of the batch norm running statistics. The suggested parameter in [TensorFlow's implementation](https://github.com/tensorflow/models/blob/master/official/resnet/resnet_model.py#L36) is `0.997`, but that leads to a big discrepancy between the normalization at training time and test time, so the default value is a more conservative `0.9`.
* `batch_norm_epsilon` \(default `0.001`\): Epsilon of the batch norm. The suggested parameter in [TensorFlow's implementation](https://github.com/tensorflow/models/blob/master/official/resnet/resnet_model.py#L37) is `1e-5`, but that leads to a big discrepancy between the normalization at training time and test time, so the default value is a more conservative `0.001`.
* `fc_layers` \(default `null`\): it is a list of dictionaries containing the parameters of all the fully connected layers. The length of the list determines the number of stacked fully connected layers and the content of each dictionary determines the parameters for a specific layer. The available parameters for each layer are: `fc_size`, `norm`, `activation` and `regularize`. If any of those values is missing from the dictionary, the default one specified as a parameter of the encoder will be used instead. If both `fc_layers` and `num_fc_layers` are `null`, a default list will be assigned to `fc_layers` with the value `[{fc_size: 512}, {fc_size: 256}]`. \(only applies if `reduce_output` is not `null`\).
* `num_fc_layers` \(default `1`\): This is the number of stacked fully connected layers.
* `fc_size` \(default `256`\): if a `fc_size` is not already specified in `fc_layers` this is the default `fc_size` that will be used for each layer. It indicates the size of the output of a fully connected layer.
* `norm` \(default `null`\): if a `norm` is not already specified in `fc_layers` this is the default `norm` that will be used for each fully connected layer. It indicates the norm of the output and it can be `null`, `batch` or `layer`.
* `activation` \(default `relu`\): if an `activation` is not already specified in `fc_layers` this is the default `activation` that will be used for each fully connected layer. It indicates the activation function applied to the output.
* `dropout` \(default `false`\): determines if there should be a dropout layer after each fully connected layer.
* `initializer` \(default `null`\): the initializer to use in the fully connected layers. If `null`, the default initialized of each variable is used \(`glorot_uniform` in most cases\). Options are: `constant`, `identity`, `zeros`, `ones`, `orthogonal`, `normal`, `uniform`, `truncated_normal`, `variance_scaling`, `glorot_normal`, `glorot_uniform`, `xavier_normal`, `xavier_uniform`, `he_normal`, `he_uniform`, `lecun_normal`, `lecun_uniform`. Alternatively it is possible to specify a dictionary with a key `type` that identifies the type of initializer and other keys for its parameters, e.g. `{type: normal, mean: 0, stddev: 0}`. To know the parameters of each initializer, please refer to [TensorFlow's documentation](https://www.tensorflow.org/api_docs/python/tf/keras/initializers).
* `regularize` \(default `true`\): if `true` the weights of the layers are added to the set of weights that get regularized by a regularization loss \(if the `regularization_lambda` in `training` is greater than 0\).

Example image feature entry using a ResNet encoder \(with default parameters\) in the input features list:

```yaml
name: image_csv_column_name
type: image
encoder: resnet
tied_weights: null
resnet_size: 50
num_filters: 16
kernel_size: 3
conv_stride: 1
first_pool_size: null
batch_norm_momentum: 0.9
batch_norm_epsilon: 0.001
fc_layers: null
num_fc_layers: 1
fc_size: 256
activation: relu
norm: null
dropout: false
initializer: null
regularize: true
preprocessing:  # example pre-processing
  -
    height: 224
    width: 224
    num_channels: 3
```

## Output Features and Decoders

There are no image decoders at the moment \(WIP\), so image cannot be used as output features.

## Measures

As no image decoders are available at the moment, there are also no image measures.

