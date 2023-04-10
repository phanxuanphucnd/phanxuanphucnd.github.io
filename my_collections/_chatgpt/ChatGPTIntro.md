---
title: "ChatGPT: How ChatGPT actually works"
date: 2022-12-23
cover: /chatgpt.jpg
labs: ["AssemblyAI", "San Francisco, California"]
---

ChatGPT là một Large Language Model (LLM) mới nhất của OpenAI và cho thấy được sự cải thiện đáng kể với mô hình tiền 
nhiệm của nó GPT-3. Tương tự như nhiều LLMs, ChatGPT có khả năng sinh văn bản (text) theo nhiều phong cách khác nhau và 
cho nhiều mục đích khác nhau, nhưng ChatGPT cho thấy được khả năng về độ chính xác, chi tiết và mạch lạc hơn rất đáng kể.
ChatGPT đang là xu hướng, nó như đại diện cho thế hệ tiếp theo của LLMs, tập trung mạnh vào tương tác hội thoại 
(interative conversations).

OpenAI họ đã sử dụng kết hợp cả Supervised Learning (học giám sát) và Reinforment Learning (học tăng cường) để tinh 
chỉnh (fine-tune) ChatGPT, **nhưng Reinforment Learning mới là thành phần làm cho ChatGPT trở nên độc đáo hơn tất cả**. 
Kỹ thuật cụ thể được gọi là **Reinforment Learning from Human Feedback (RLHF)**, tức là học tăng cường từ những dữ liệu 
feedback của con người, sử dụng vòng lặp feedback của con người trong quá trình huấn luyện để giảm thiểu kết quả có hại, 
không trung thực hay là sai lệch tri thức.

Chúng ta sẽ xem xét các giới hạn của GPT-3 và cách họ xuất phát cho quá trình đào tạo, trước khi tìm hiểu cách thức 
hoạt động của RLHF và hiểu cách ChatGPT sử dụng RLHF để khắc phục những vấn đề này. Tôi sẽ kết luận bằng cách xem xét 
một số hạn chế của các diễn giải ChatGPT này.

<div align="center">
    <img src="media/ChatGPTIntro/image0.png" width=500>
</div>

### Capability vs Alignment trong LLMs



