# Bag Features

### Preprocessing

Bag features are treated in the same way of set features, with the only difference being that the matrix had float values \(frequencies\).

### Input Features and Encoders

Bag features have one encoder, the raw float values coming from the input placeholders are first transformed in sparse integer lists, then they are mapped to either dense or sparse embeddings \(one-hot encodings\), they are aggregated as a weighted sum, where the weights are the original float values, and finally returned as outputs. Inputs are of size `b` while outputs are fo size `b x h` where `b` is the batch size and `h` is the dimensionality of the embeddings.

The parameters are the same used for set input features with the exception of `reduce_output` that does not apply in this case because the weighted sum already acts as a reducer.

### Output Features and Decoders

There is no bag decoder available yet.

### Measures

As there is no decoder there is also no measure available yet for bag feature.

