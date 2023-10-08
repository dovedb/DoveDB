[![Slack chat][slack-img]](#get-in-touch)
[![Unit Tests][ci-img]](https://github.com/dovedb/DoveDB)

<img src="./figs/DoveDBlatestlogo.png" width="250">

# DoveDB - a Declarative and Low-Latency Video Database

DoveDB 🕊️ is a database for intelligent video data management and analysis. Drawing inspiration from the world's most advanced vision AI methods, DoveDB represents a significant leap forward in open-source research and development in the field of video data handling.

<!-- <img src="./figs/framework.png" width="520"> -->

## Video Demo
Here is a video to introduce our DoveDB system. 

[![Demo video](./figs/video_play.png)](https://www.youtube.com/watch?v=N139dEyvAJk)

## Quick Start

Here are some common operations for DoveDB:

1. Run DoveDB Server
```bash
python run_server.py
```

2. Upload video
```bash
python video_upload.py
```

3. Start web interface
First, make sure that Node.js is installed on your local machine. Then, use the following command:
```bash
npm run start:dev
```
Open your web browser and access `127.0.0.1:5000` to access the web interface.

## Publications

1. *DoveDB: A Declarative and Low-Latency Video Database*. VLDB 2023 demo. [[pdf]](https://www.vldb.org/pvldb/vol16/p3906-zhang.pdf)

2. *Human-in-the-Loop Vehicle ReID*. AAAI 2023. [[pdf]](https://github.com/dovedb/DoveDB/blob/main/Documentation/hitl_aaai.pdf) [[bibtex]](https://github.com/dovedb/DoveDB/blob/main/Documentation/hitl_aaai.pdf)

3. *Towards One-Size-Fits-Many: Multi-Context Attention Network for Diversity of Entity Resolution Tasks*. TKDE 2022. [[pdf]](https://ieeexplore.ieee.org/abstract/document/9360523/) [[bibtex]](https://dblp.org/rec/journals/tkde/ZhangLWTC22.bib?param=1)

4. *Co-movement Pattern Mining from Videos*. VLDB 2024 [[pdf]](https://browse.arxiv.org/pdf/2308.05370.pdf) [[bibtex]](https://dblp.org/rec/journals/corr/abs-2308-05370.html?view=bibtex) 


## Contribute
We greatly value your contributions and are committed to making your involvement with DoveDB as straightforward and transparent as possible. 

<img src="./figs/contributers.png" width="400">

## License

**Apache-2.0 License**: Our open-source software is made available under the Apache-2.0 license, which is approved by the Open Source Initiative (OSI). This open-source license encourages collaboration and knowledge sharing and is particularly suitable for students and enthusiasts who want to contribute and utilize our software.

## Get in Touch

Have questions, suggestions, bug reports? Reach the project community via these emails:

* xiaoziyang.xzy@gmail.com
* lizepeng@zju.edu.cn

[ci-img]: https://github.com/jaegertracing/jaeger/workflows/Unit%20Tests/badge.svg?branch=main
[slack-img]: https://img.shields.io/badge/slack-join%20chat%20%E2%86%92-brightgreen?logo=slack
