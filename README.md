# Wasserstein GAN

This is a tensorflow implementation of WGAN, only in DCGAN fashion, on mnist and SVHN.

### Note

1. All data will be download automatically, the SVHN script is modified from [this](https://github.com/openai/improved-gan/blob/master/mnist_svhn_cifar10/svhn_data.py).

2. All parameters are set to the values the original paper recommends. `Diters`  represents the number of critic update in one step, in [Original PyTorch version](https://github.com/martinarjovsky/WassersteinGAN) it was set to 5 unless iterstep < 25 or iterstep % 500 == 0 , I guess since the critic is free to fully optimized, it's reasonable to make more updates to critic at the beginning and every 500 steps, so I borrowed it without tuning. The learning rates for generator and critic are both set to 5e-5 , since during the training time the gradient norms are always relatively high(around 1e3), I suggest no drastic change on learning rates.

3. In this implementation, the critic loss is `tf.reduce_mean(fake_logit - true_logit)`, and generator loss is `tf.reduce_mean(-fake_logit)` , actually, the whole system still works if you add a `-` before both of them. It doesn't matter. Recall that the critic loss in duality form is$\sup_{\|f\|_L\leq1}\mathbb{E}_{x\sim\mathbb{P}_r}[f(x)]-\mathbb{E}_{x\sim\mathbb{P}_\theta}[f(x)]$ ![](https://ww2.sinaimg.cn/large/006tKfTcly1fcewxqyfvwj307i00kglj.jpg), and the set ${\|f\|_L\leq1}$ is symmetric about the sign. Substitute $f$ with $-f$ gives us $\sup_{\|f\|_L\leq1}\mathbb{E}_{x\sim\mathbb{P}_\theta}[f(x)]-\mathbb{E}_{x\sim\mathbb{P}_r}[f(x)]$ ![](https://ww3.sinaimg.cn/large/006tKfTcly1fcewyhols5j307g00odfr.jpg), the opposite number of original form. The original PyTorch implementation takes the second form, this implementation takes the first, both will work equally. You might well want to add the `-` and try it out.

4. Please set your device you want to run on in the code, search `tf.device`. It runs on gpu:2 by default.

   ​