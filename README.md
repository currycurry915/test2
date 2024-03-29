<div align="center">

<h1>Motion-to-Attention: Attention Motion Composer using Optical Flow for Text-to-Video Editing</h1>


<br>

<image src="fig/attention-1.png"  />
<br>

</div>

## Abstract
Text-to-image diffusion, which has been trained with a large amount of text-image pair dataset, shows remarkable performance in generating high-quality images. Recent research using diffusion model has been expanded for text-guided video editing tasks by using text-guided image diffusion models as baseline. Existing video editing studies have devised an implicit method of adding cross-frame attention to estimate frame-frame attention to attention maps, resulting in temporal consistent editing. However, because these methods use generative models trained on text-image pair data, they do not take into account one of the most important characteristics of video: motion. When editing a video with prompts, the attention map of the prompt implying the motion of the video, such as 'running' or 'moving', is not clearly estimated and accurate editing cannot be performed. In this paper, we propose the `Motion Map Injection' (AMC) module to perform accurate video editing by considering movement information explicitly. The AMC module provides a simple but effective way to convey video motion information to T2V models by performing three steps: 1) extracting motion map, 2) calculating the similarity between the motion map and the attention map of each prompt, and 3) injecting motion map into the attention maps.  Considering experimental results, input video can be edited accurately and effectively with AMC module. To the best of our knowledge, our study is the first method that utilizes the motion in video for text-to-video editing.


## Examples
### You can find more experimental results [on our project page](https://currycurry915.github.io/Motion-to-Attention/).

<table class="center">
<tr>
  <td align="center" ><b>Input Video</b></td>
  <td align="center" ><b>Video-P2P</b></td>
  <td align="center" ><b>Ours</b></td>
</tr>

 <tr>
  <td align="center" width=25% style="text-align:center;color:gray;">"Clouds {flowing} under a skyscraper"</td>
  <td align="center" width=25% style="text-align:center;">"Waves {flowing} under a skyscraper"</td>
  <td align="center" width=25% style="text-align:center;color:gray;">"Waves {flowing} under a skyscraper"</td>
</tr>

<tr>
  <td align="center" style colspan="1"><img src="results/clouds_waves_input.gif" loop=infinite></td>
  <td align="center" style colspan="1"><img src="results/clouds_waves_ori.gif"></td>
  <td align="center" style colspan="1"><img src="results/clouds_waves_AMC.gif"></td>
</tr>


<tr>
  <td align="center" width=25% style="text-align:center;color:gray;">"Clouds {flowing} on the mountain"</td>
  <td align="center" width=25% style="text-align:center;">"Lava {flowing} on the mountain"</td>
  <td align="center" width=25% style="text-align:center;color:gray;">"Lava {flowing} on the mountain"</td>
</tr>

<tr>
  <td align="center" style colspan="1"><img src="results/clouds_lava_input.gif"></td>
  <td align="center" style colspan="1"><img src="results/clouds_lava_ori.gif"></td>
  <td align="center" style colspan="1"><img src="results/clouds_lava_AMC.gif"></td>       
</tr>

<tr>
  <td align="center" width=25% style="text-align:center;color:gray;">"{Spinning} wings of windmill are beside the river"</td>
  <td align="center" width=25% style="text-align:center;">"Yellow {spinning} wings of windmill are beside the river"</td>
  <td align="center" width=25% style="text-align:center;color:gray;">"Yellow {spinning} wings of windmill are beside the river"</td>
</tr>

<tr>
  <td align="center" style colspan="1"><img src="results/yellow_windmill_input.gif"></td>
  <td align="center" style colspan="1"><img src="results/yellow_windmill_ori.gif"></td>
  <td align="center" style colspan="1"><img src="results/yellow_windmill_AMC.gif"></td>       
</tr>
</table>



## Setup

The environment is very similar to [Video-P2P](https://github.com/ShaoTengLiu/Video-P2P).

The versions of the packages we installed are:

torch: 1.12.1 \
xformers: 0.0.15.dev0+0bad001.d20230712

In the case of xformers, I installed it through the [link](https://github.com/bryandlee/Tune-A-Video/issues/4) introduced by Video-P2P.

```shell
pip install -r requirements.txt
```


## Weights

We use the pre-trained stable diffusion model. You can download it [here](https://huggingface.co/runwayml/stable-diffusion-v1-5). 


## Quickstart

Since we developed our codes based on Video-P2P codes, you could refer to their [github](https://github.com/ShaoTengLiu/Video-P2P), if you need.

Please replace **pretrained_model_path** with the path to your stable-diffusion.

To download the pre-trained model, please refer to [diffusers](https://github.com/huggingface/diffusers).


``` bash
# Stage 1: Tuning to do model initialization.

# You can minimize the tuning epochs to speed up.
python run_tuning.py  --config="configs/cloud-1-tune.yaml"
```

``` bash
# Stage 2: Attention Control

python run_attention_flow.py --config="configs/cloud-1-p2p.yaml" --motion_prompt "Please enter motion prompt"

# If the prompt is "clouds flowing under a skyscraper", the motion prompt is "flowing".
# You can input the motion prompt as below.

python run_attention_flow.py --config="configs/cloud-1-p2p.yaml" --motion_prompt "flowing"
```

Find your results in **Video-P2P/outputs/xxx/results**.
