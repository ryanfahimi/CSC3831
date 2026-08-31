# Computer Vision

*Lecture slides in Drive under `Lectures/`, plus `CV Coursework Part 3/`
([CSC3831 Drive](https://drive.google.com/drive/folders/1ZFzIz2oyMbxYrSIvm4Wo_9jR9OrkdU-n))*

Deep learning on images, in **PyTorch**. The strand is short but builds a clear argument: fully
connected network → convolutional network → the practical machinery that makes training one actually
work.

## Practicals

**[1. Image Classification (DNN)](Practicals/1_Image_Classification_DNN.ipynb)** — a fully connected
network, and the PyTorch fundamentals: `torch.nn` for layers, `torchvision.datasets` for data,
`ToTensor` for conversion, and `DataLoader` for batching and shuffling.

`DataLoader` deserves attention beyond convenience. **Batching** is what makes training tractable —
gradients are estimated from a batch rather than the whole dataset (too slow) or one example (too
noisy) — and **shuffling** each epoch prevents the network learning anything from the order the data
happens to be stored in.

The deeper point is what this network *can't* do. Flattening an image into a vector throws away the
fact that adjacent pixels are related, and a feature learned in one corner has to be learned again
elsewhere. That limitation is the entire motivation for the next notebook.

**[2. Image Classification (ConvNet)](Practicals/2_Image_Classifcation_ConvNet.ipynb)** —
convolutional networks. Two properties fix exactly what the DNN lacked: **local connectivity** (a
filter sees a small neighbourhood, matching how images are actually structured) and **weight
sharing** (the same filter slides across the whole image, so a feature is learned once and detected
anywhere). Together they also mean drastically fewer parameters than a fully connected layer of
comparable capacity.

*(The filename's `Classifcation` typo is the instructor's; left as-is so nothing referencing the path
breaks.)*

## [Coursework Part 3](Coursework_Part_3.ipynb)

40 cells on CIFAR-10 — harder than MNIST: colour, natural images, real backgrounds — in three
sections.

**1. CNN with early stopping.** Uses `random_split` to carve out a validation set and `Subset` for
data handling. **Early stopping** is the point: training accuracy keeps improving long after
validation accuracy stops, and continuing past that is memorization, not learning. Watching
validation loss and stopping when it turns is the cheapest regularizer there is, which is why `copy`
is imported — you keep a snapshot of the best model rather than whatever the last epoch produced.

**2. With and without batch normalization.** A controlled comparison of one architectural change.
Batch norm normalizes each layer's inputs across the batch, which stabilizes the distributions later
layers see and lets you train faster with higher learning rates. Running the identical network both
ways, rather than reading the claim, is the assignment's method — and is the same
compare-don't-demonstrate framing as the other two coursework parts.

**3. Visualising convolutional features.** The interpretability section, and the most interesting
one. Rendering the learned filters and their activations typically shows early layers detecting edges
and colour gradients, with later layers responding to progressively more complex shapes — a hierarchy
nobody specified, learned from the data. It also makes a network something you can *inspect* rather
than only score, which is the appropriate closing note for the course.

## Relationship to the other strands

[Machine Learning](../Machine%20Learning/)'s coursework runs logistic regression on MNIST, which is
the linear-model-on-raw-pixels baseline. This strand is what beats it and why. Reading the two
coursework notebooks together is the intended comparison — and applying the baseline discipline
(what does the simple method get?) before reaching for a CNN is the habit worth keeping.

For the algorithms underneath — backpropagation, CNN architecture, and where they sit in the history
of the field — [CS362's module 7](https://github.com/ryanfahimi/CS362) covers the theory side.
