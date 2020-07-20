---
title: "Performance and Capacity Engineering - Facebook"
excerpt: "Solving the problem of connecting the world"
---

## Experience with Cloud Computing and Capacity
Data Lake Connector that democratizes data to all engineers.

## General Infrastructure Engineering
N/A

##












##### Checked out [Bill Jia's talk](https://engineering.fb.com/category/core-data/?post=video) at "Performance @Scale 2018".

> *"The highest throughput, with acceptable latency, in the smallest footprint"* -- Performance and Capacity Engineering @ Facebook

> *"Without data, and with analysis of data, you're not working on performance. You're working on something else"* -- Bill Jia, Facebook

Consider: architecture, code base, infrastructure, hardware

##### Looked at the [ Data Center Performance adn Capacity Engineer job post](https://www.facebook.com/careers/jobs/681688192609406/)
- Provide deep visibility into power, performance and health.
- Help optimize capacity usage (run simulations to determine utilization parameters)
- Identify bottle necks
- Develop simulation models and tools to monitor data center capacity performance and utilization. Write monitoring, reporting, data-mining tools to do performance and capacity-related tests and analysis.
- MySQL, Hadoop.




##### Read [Bill Jia's Publication](https://ieeexplore.ieee.org/document/8675201), *Machine Learning at Facebook: Understanding Inference at the Edge*.
- "Machine Learning is used by most Facebook services"
- Ranking posts for News Feed, content understanding, AR/VR, speech recognition.
- RNNs, decision trees, logistic regression
- Desire to bring that to edge
- Optimizations include: model architecture search, weight compression, quantization, algorithmic complexity reduction, and microarchitecture.
- Optimizations enable edge inference to run on mobil CPUs. Only a small fraction of inference currently run on mobile GPU's.
- Use of PyTorch and Caffe2
- Two internal packages NNPACK and QNNPACK

##### Learned about [Flame Charts](http://www.brendangregg.com/flamegraphs.html) by Brendan Gregg
- Way of seeing what parts are consuming the most resources.
- Hierarchical; within that part what is consuming the most.






#### Some relevant notes:
- CloudComputing
- CapacityEngineering
- OpenShift
- Kubernetes
- Hadoop
- APIs
