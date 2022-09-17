# TuSimple Lane Detection Challenge

![](./assets/examples/lane_example.jpg)

Objects on the road can be divided into two main groups: static objects and dynamic objects. Lane markings are the main static component on the highway. They instruct the vehicles to interactively and safely drive on the highway. To encourage people to solve the lane detection problem on highways, we are releasing about 7,000 one-second-long video clips of 20 frames each.   
 
Lane detection is a critical task in autonomous driving, which provides localization information to the control of the car. We provide video clips for this task, and the last frame of each clip contains labelled lanes. The video clip can help algorithms to infer better lane detection results. With clips, we expect competitors to come up with more efficient algorithms. For an autonomous driving vehicle, a time/memory-efficient algorithm means more resources for other algorithms and engineering pipelines. 
 
At the same time, we expect competitors to think about the semantic meaning of lanes for autonomous driving, rather than detecting every single lane marking on the road. Therefore, the annotations and testing are focused on the current and left/right lanes.
 
We will have a leaderboard showing the evaluation results for the submissions. We have prizes for the top-three competitors, and they will also be mentioned at the CVPR 2017 Workshop on Autonomous Driving Challenge.

## 数据集特点
Complexity:
- Good and medium weather conditions
- Different daytime
- 2-lane/3-lane/4-lane/ or more highway roads.
- Different traffic conditions

Dataset size:
- Training: 3626 video clips, 3626 annotated frames
- Testing: 2782 video clips

Camera and video clip:
- 1s clip of 20 frames
- The view direction of the camera is very close to the driving direction

Type of annotations:
- polylines for lane markings

## 数据集细节

### 1. 目录结构
The directory structure for the training/testing dataset is following. We have a JSON file to instruct you how to use the data in `clips` directory.

    dataset
      |
      |----clips/                   # video clips
      |------|
      |------|----some_clip/        # Sequential images for the clip, 20 frames
      |------|----...
      |
      |----tasks.json               # Label data in training set, and a submission template for testing set. 

### 2. Demo
The [demo code](../../example/lane_demo.ipynb) shows the data
format of the lane dataset and the usage of the evaluation tool.

### 3. Label Data Format
Each json line in 'label_data.json' is the label data for **the last (20th) frame** of this clip.

`label_data.json`中的每个 json 行都是此剪辑片段的**最后（第 20）帧**的标签数据。

**Format**

```
    {
      'raw_file': 字符串. 片段文件的路径.
      'lanes': 列表. A list of lanes. For each list of one lane, the elements are width values on image.
      'h_samples': 列表. A list of height values corresponding to the 'lanes', which means len(h_samples) == len(lanes[i])
    }
```

Actually there will be at most 5 lane markings in `lanes`. We expect at most 4 lane markings (current lane and left/right lanes). The extra lane is used when changing lane because it is confused to tell which lane is the current lane.
实际上，`lanes` 中最多会有 5 个车道标记。 我们预计最多有 4 个车道标记（当前车道和左/右车道）。 换车道时会使用额外的车道，因为很难分辨哪条车道是当前车道。

The polylines are orgnized by the same distance gap ('h_sample' in each label data) from the recording car. It means you can pair each element in one lane and h_samples to get position of lane marking on images.
您可以将一个 `lane` 中的每个元素和 h_samples 配对以获得图像上车道标记的位置。（这里直接看例子吧，秒懂的)

Also, the lanes are around the center of sight, which we encourage the autonomous driving vehicle to focus on the current lane and left/right lanes. These lanes are essential for the control of the car.
此外，车道在视线中心附近，我们鼓励自动驾驶车辆专注于当前车道和左/右车道。 这些车道对于控制汽车至关重要。

举个例子,
```
{
  "lanes": [
        [-2, -2, -2, -2, 632, 625, 617, 609, 601, 594, 586, 578, 570, 563, 555, 547, 539, 532, 524, 516, 508, 501, 493, 485, 477, 469, 462, 454, 446, 438, 431, 423, 415, 407, 400, 392, 384, 376, 369, 361, 353, 345, 338, 330, 322, 314, 307, 299],
        [-2, -2, -2, -2, 719, 734, 748, 762, 777, 791, 805, 820, 834, 848, 863, 877, 891, 906, 920, 934, 949, 963, 978, 992, 1006, 1021, 1035, 1049, 1064, 1078, 1092, 1107, 1121, 1135, 1150, 1164, 1178, 1193, 1207, 1221, 1236, 1250, 1265, -2, -2, -2, -2, -2],
        [-2, -2, -2, -2, -2, 532, 503, 474, 445, 416, 387, 358, 329, 300, 271, 241, 212, 183, 154, 125, 96, 67, 38, 9, -2, -2, -2, -2, -2, -2, -2, -2, -2, -2, -2, -2, -2, -2, -2, -2, -2, -2, -2, -2, -2, -2, -2, -2],
        [-2, -2, -2, 781, 822, 862, 903, 944, 984, 1025, 1066, 1107, 1147, 1188, 1229, 1269, -2, -2, -2, -2, -2, -2, -2, -2, -2, -2, -2, -2, -2, -2, -2, -2, -2, -2, -2, -2, -2, -2, -2, -2, -2, -2, -2, -2, -2, -2, -2, -2]
       ],
  "h_samples": [240, 250, 260, 270, 280, 290, 300, 310, 320, 330, 340, 350, 360, 370, 380, 390, 400, 410, 420, 430, 440, 450, 460, 470, 480, 490, 500, 510, 520, 530, 540, 550, 560, 570, 580, 590, 600, 610, 620, 630, 640, 650, 660, 670, 680, 690, 700, 710],
  "raw_file": "path_to_clip"
}
```
`-2` in `lanes` means on some h_sample, there is no existing lane marking. The first existing point in the first lane is `(632, 280)`.

`-2` in `lanes` 意思是在当前竖直位置 h_sample, 没有存在的车道标记. 第一个 lane 的第一个点是 `(632, 280)`.

## Evaluation

For each prediction of a clip, please organize the result as the same format of label data.
Also, you need to output the `lanes` according to the `h_samples` in the `test_tasks.json` for evaluation. It means we are going to evaluate points on specific image heights.

__Format__

```
{
  'raw_file': str. 20th frame file path in a clip.
  'lanes': list. A list of lanes. For each list of one lane, there is only width index on the image.
  'run_time': list of float. The running time for each frame in the clip. The unit is millisecond.
}
```
Remember we expect at most 4/5 lane markings in `lanes` (current lane and left/right lanes). Feel free to output either a extra left or right lane marking when changing lane. We only accept that the number of submitted lanes is no larger than the number of ground-truth lanes plus 2. For example, if the number of lanes in the ground-truth for some image is 4 and you submit 7 lanes, the accuracy for this image is 0. So, please submit the most confident lanes. Besides, the maximum number of lanes in ground-truth is mostly 4, some are 5.

The evaluation formula is
$$
accuracy = \frac{\sum_{clip} C_{clip}}{\sum_{clip} S_{clip}}
$$

where $C_{clip}$ is the number of correct points in the last frame of the `clip`, 
$S_{clip}$ is the number of requested points in the last frame of the `clip`. If the difference between the width of ground-truth and prediction is less than a threshold, the predicted point is a correct one. 

其中 $C_{clip}$ 是`剪辑片段`最后一帧中正确点的数量，$S_{clip}$ 是`剪辑片段`最后一帧中GT的点数。 如果ground-truth和预测的宽度之差小于阈值，则预测点是正确的。

If you some point is out of view or there is no lane markings of some specific `h_sample`, just record the detection as `-2`. We will evaluate the values of all heights in `h_sample`.

如果您的某个点不在视野范围内，或者没有某个特定 `h_sample` 的车道标记，只需将检测记录写为 `-2`。 
我们将评估 `h_sample` 中所有高度的值。

Based on the formula above, we will also compute the rate of false positive and false negative for your test results. False positive means the lane is predicted but not matched with any lane in ground-truth. False negative means the lane is in the ground-truth but not matched with any lane in the prediction.


$$\text{假阳性误报率: } FP = \frac{F_{pred}}{N_{pred}}$$

$$\text{假阴性误报率: } FN = \frac{M_{pred}}{N_{gt}}$$

where $F_{pred}$ is the number of wrong predicted lanes, $N_{pred}$ is the number of all predicted lanes. $M_{pred}$ is the number of missed ground-truth lanes in the predictions, $N_{gt}$ is the number of all ground-truth lanes.

比赛还要求您算法的运行时间。 我们不按运行时间排名。 但是，太慢的算法（例如使用单个 GPU 时低于 5 fps）将被视为无预测车道。


### 4. Prizes

The prizes for the winners are following. Please review the [rules](http://benchmark.tusimple.ai/#/term) for terms and conditions to receive a prize.

1. First place prize: $ 1000
2. Second place prize: $ 500
3. Third place prize: $ 250 

We rank only by the `accuracy` we methioned above. `FP` and `FN` help competitors to improve their algorithm.

