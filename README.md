# style-transfer-pytorch

An implementation of neural style transfer ([A Neural Algorithm of Artistic Style](https://arxiv.org/abs/1508.06576)) in PyTorch, supporting CPUs and Nvidia GPUs. It does automatic multi-scale (coarse-to-fine) stylization to produce high-quality high resolution stylizations, even up to print resolution if the GPUs have sufficient memory. If two GPUs are available, they can both be used to increase the maximum resolution. (Using two GPUs is not faster than using one.)

The algorithm has been modified from that in the literature by:

- Using the PyTorch pre-trained VGG-19 weights instead of the original VGG-19 weights

- Changing the padding mode of the first layer of VGG-19 to 'replicate', to reduce edge artifacts

- When using average or L2 pooling, scaling the result by an empirically derived factor to ensure that the magnitude of the result stays the same on average (Gatys et al. (2015) did not do this)

- Using [Wasserstein-2 style loss](https://wandb.ai/johnowhitaker/style_loss_showdown/reports/An-Explanation-of-Style-Transfer-with-a-Showdown-of-Different-Techniques--VmlldzozMDIzNjg0#style-loss-#3:-%22vincent's-loss%22)

- Taking an exponential moving average over the iterates to reduce iterate noise (each new scale is initialized with the previous scale's averaged iterate)

- Warm-starting the Adam optimizer with scaled-up versions of its first and second moment buffers at the beginning of each new scale, to prevent noise from being added to the iterates at the beginning of each scale

- Using non-equal weights for the style layers to improve visual quality

- Stylizing the image at progressively larger scales, each greater by a factor of sqrt(2) (this is improved from the multi-scale scheme given in Gatys et al. (2016))

## Example outputs (click for the full-sized version)

<a href="https://9285c52c-d9b1-40d1-8ac1-e75634aad92d.s3-us-west-2.amazonaws.com/mst195.jpg"><img src="https://9285c52c-d9b1-40d1-8ac1-e75634aad92d.s3-us-west-2.amazonaws.com/mst195_small.jpg" width="512" height="401"></a>

<a href="https://9285c52c-d9b1-40d1-8ac1-e75634aad92d.s3-us-west-2.amazonaws.com/mst235.jpg"><img src="https://9285c52c-d9b1-40d1-8ac1-e75634aad92d.s3-us-west-2.amazonaws.com/mst235_small.jpg" width="512" height="401"></a>

<a href="https://9285c52c-d9b1-40d1-8ac1-e75634aad92d.s3-us-west-2.amazonaws.com/mst256.jpg"><img src="https://9285c52c-d9b1-40d1-8ac1-e75634aad92d.s3-us-west-2.amazonaws.com/mst256_small.jpg" width="512" height="401"></a>

<a href="https://9285c52c-d9b1-40d1-8ac1-e75634aad92d.s3-us-west-2.amazonaws.com/mst201.jpg"><img src="https://9285c52c-d9b1-40d1-8ac1-e75634aad92d.s3-us-west-2.amazonaws.com/mst201_small.jpg" width="512" height="401"></a>

<a href="https://9285c52c-d9b1-40d1-8ac1-e75634aad92d.s3-us-west-2.amazonaws.com/mst310.jpg"><img src="https://9285c52c-d9b1-40d1-8ac1-e75634aad92d.s3-us-west-2.amazonaws.com/mst310_small.jpg" width="512" height="401"></a>


