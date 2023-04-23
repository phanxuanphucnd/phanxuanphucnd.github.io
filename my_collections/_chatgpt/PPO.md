---
title: "PPO: Giải thích thuật toán Proximal Policy Opitmization (PPO)"
date: 2023-04-26
cover: /ppo0.jpg
labs: ["VinBigdata", "Hanoi, Vietnam"]
---

**Proximal Policy Optimization (PPO)** hiện nay được xem như là thuật toán SoTA trong `Reinforcement Learning (RL)`. Thuật 
toán được giới thiệu bởi `OpenAI` năm 2017, được xây dựng nằm cân bằng giữa performance và khả năng hiểu/ dễ dàng sử dụng. 
Chất lượng của thuật toán cạnh tranh với các thuật toán khác trong RL trên nhiều benchmark chất lượng, thâm chí còn vượt 
trội trên một số task. Đồng thời, nó đủ đơn giản để chúng ta dễ dàng áp dụng vào thực tế, điều này thường rất khó khăn 
cho mọi thuật toán trong `RL`.

Hiểu theo bề nổi thì sự khác nhau giữa các phương pháp policy gradient truyền thống với `PPO` không quá lớn. Dựa trên 
pseudo-code của 2 thuật toán có thể thấy tưởng chừng như hai thuật toán này giống nhau. Tuy nhiên, đằng sau tất cả là 
một lý thuyết phong phú cần thiết để đánh giá và hiểu đầy đủ về thuật toán. Chúng ta sẽ thảo luận ngắn gọn về các điểm 
chính của các phương pháp **Policy Gradient**, **Natural Policy Gradient** và **Trust Region Policy Optimization (TRPO)** 
để có thể làm bước đệm cho việc hiểu rõ hơn về `PPO`.

### Vanilla Policy Gradient

Để hiểu rõ về PPO chúng ta cần phải hiểu thật rõ về phương pháp Policy Gradients. Nếu bạn còn chưa có kiến thức về phần 
này bạn có thể đọc tham khảo bài viết này: [https://phanxuanphucnd.github.io/chatgpt/PolicyGradients](https://phanxuanphucnd.github.io/chatgpt/PolicyGradients). 

Trong RL, một policy `π` hiểu đơn giản là một hàm mà trả về một hành động (action) khả thi cho trước 1 trạng thái (state) `s`.



### Natural Policy Gradient


### Trust Region Policy Optimization (TRPO)


### Proximal Policy Optimization (PPO)

#### PPO - Variant with Adaptive KL Penalty

#### PPO - Variant with Clipped Objective

#### PPO2


### Kết luận


### References 

[1] Natural Gradient Works Efficiently in Learning, 1998, [https://ieeexplore.ieee.org/abstract/document/6790500](https://ieeexplore.ieee.org/abstract/document/6790500).

[2] Towards Delivering a Coherent Self-Contained Explanation of Proximal Policy Optimization, 2021, [https://fse.studenttheses.ub.rug.nl/25709/1/mAI_2021_BickD.pdf](https://fse.studenttheses.ub.rug.nl/25709/1/mAI_2021_BickD.pdf).

[3] A Natural Policy Gradient, 2001, [https://proceedings.neurips.cc/paper/2001/file/4b86abe48d358ecf194c56c69108433e-Paper.pdf](https://proceedings.neurips.cc/paper/2001/file/4b86abe48d358ecf194c56c69108433e-Paper.pdf).

[4] Trust Region Policy Optimization, 2015, [https://proceedings.mlr.press/v37/schulman15.pdf](https://proceedings.mlr.press/v37/schulman15.pdf).

[5] Proximal policy optimization algorithms, 2017, [https://arxiv.org/pdf/1707.06347.pdf](https://arxiv.org/pdf/1707.06347.pdf).

