# TuSimple Competitions for CVPR2017

> Note: We do not have a calibration file(校准文件) for Lane Detection Challenge.

- [Lane Detection Challenge](doc/lane_detection/readme.md)
- [Velocity Estimation Challenge](doc/velocity_estimation/readme.md)
- [Download ground truths](https://github.com/TuSimple/tusimple-benchmark/issues/3)


## Lane Detection Challenge Evaluate

可以直接传入GT和pred的json文件进行评估：
```
cd evaluate
python lane.py ../example/label_data_0313.json ../example/label_data_0313.json # 前者是 pred 后者是 GT
```