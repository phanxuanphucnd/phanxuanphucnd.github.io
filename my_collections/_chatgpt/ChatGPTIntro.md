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

### Capability vs Alignment trong LLMs

<div align="center">
    <img src="media/ChatGPTIntro/image0.png" width=500>
</div>

Đầu tiền chúng ta cần hiểu 2 khái niệm *Capability* và *Alignment* trong machine learning. Khái niệm *Capability* đề cập 
tới khả năng của một model để thực hiện một hoặc nhiều task cụ thể. Khả năng của một model cụ thể được đánh giá bởi khả 
năng tối ưu objective-function (hàm tối ưu/ hàm mục tiêu) của nó tốt như thế nào? Ví dụ như, một model được thiết kế để 
dự đoán giá của thị trường  chứng khoán phải có một hàm mục tiêu nào đó đo được độ chính xác các dự đoán của model. Nếu 
model có khả năng dự đoán chính xác sự biến động giá chứng khoán theo thời gian, thì được xem là có khả năng *capability* 
cao cho task này.

Mặt khác, *Alignment* liên quan tới **những gì mà chúng ta thực sự muốn model làm** so với **những gì mà model đang được 
huấn luyện để làm**. Chúng ta sẽ đặt câu hỏi là "các hàm mục tiêu mà model sử dụng để huấn luyện có phù hợp với 
ý định của chúng ta không?" và mức độ mà các hàm mục tiêu đó và hành vi model đưa ra so với kỳ vọng của con người là như 
thế nào? Một ví dụ cụ thể đơn giản là chúng ta ây dựng một model phân loại chim để phân loại một con chim liệu nó là 
chim sẻ hay chim họa mi. Thông thường, chúng ta sử dụng log loss là hàm mục tiêu (để đo được sự phân biệt giữa phân bố xác 
suất dự đoán của mô hình và phân bố của nhãn thực) mặc dù mục tiêu cuối cùng là độ chính xác phân loại cao (accuracy). 
Vì thế, model sẽ có xu hướng low log loss, tức là *capability* của model là cao, nhưng accuracy kém trên tập test. Trong 
thực tế, log loss không hoàn toàn tương quan tới độ accuracy trong các task phân loại. Đây là một ví dụ sai lệch (misalignment) 
giữa hàm mục tiêu được tối ưu trong quá trình huấn luyện nhưng lại căn chỉnh kém mục tiêu cuối cùng.

<div align="center">
<b><i>LLMs như GPT-3 là dạng misaligned</i></b>
</div>


