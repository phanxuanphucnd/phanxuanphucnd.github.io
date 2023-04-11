---
title: "ChatGPT: bản chất ChatGPT hoạt động như thế nào?"
date: 2022-12-23
cover: /chatgpt.jpg
labs: ["AssemblyAI", "San Francisco, California"]
---

ChatGPT là một Large Language Model (LLM) mới nhất của OpenAI và cho thấy được sự cải thiện đáng kể với mô hình tiền 
nhiệm của nó GPT-3. Tương tự như nhiều LLMs, ChatGPT có khả năng sinh văn bản (text) theo nhiều phong cách khác nhau và 
cho nhiều mục đích khác nhau, nhưng ChatGPT cho thấy được khả năng về độ chính xác, chi tiết và mạch lạc hơn rất đáng kể. 
ChatGPT đang là xu hướng, nó như đại diện cho thế hệ tiếp theo của LLMs, tập trung mạnh vào sự tương tác trong hội thoại 
(interative conversations).

OpenAI họ đã sử dụng kết hợp cả Supervised Learning (học giám sát) và Reinforment Learning (học tăng cường) để tinh 
chỉnh (fine-tune) ChatGPT, **nhưng Reinforment Learning mới là thành phần làm cho ChatGPT trở nên độc đáo hơn tất cả**. 
Kỹ thuật cụ thể được gọi là **Reinforment Learning from Human Feedback (RLHF)**, tức là học tăng cường từ những dữ liệu 
feedback của con người, sử dụng vòng lặp feedback của con người trong quá trình huấn luyện để giảm thiểu việc đưa ra các phản hồi 
có hại, không trung thực hay là sai lệch tri thức.


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
thực tế, log loss không hoàn toàn tương quan tới độ accuracy trong các task phân loại. Đây là một ví dụ sai lệch 
(misalignment) giữa hàm mục tiêu được tối ưu trong quá trình huấn luyện nhưng lại căn chỉnh kém mục tiêu cuối cùng.

<div align="center">
    <b><i>LLMs như GPT-3 là dạng misaligned</i></b>
</div>


Các LLMs chẳng hạn như GPT3, LLaMA, BLOOM, PaLM, ... được huấn luyện trên một lượng rất lớn dữ liệu text từ internet và 
có khả năng sinh văn bản giống như con người, nhưng không phải lúc nào LLMs cũng sinh ra được đầu ra như kỳ vọng của con 
người hoặc có tính chính xác. Trong thực tế, hàm mục tiêu của chúng là phân phối xác suất trên các chuỗi từ (word sequences)
hoặc trên chuỗi token (token sequences) mà cho phép dự đoán chính xác từ tiếp theo trong chuỗi.

Tuy nhiên, trong các ứng dụng thực tế, các model này nhằm để thực hiện một số dạng kiểu nhận dạng có giá trị, và có sự 
khác biệt rõ ràng giữa cách huấn luyện với cách mà chúng ta muốn sử dụng model. Mặc dù về mặt toán học, việc sử dụng 
phân bố thống kê chuỗi các từ có thể là một lựa chọn rất hiệu quả cho language model, nhưng con người chúng ta tạo ra 
ngôn ngữ bằng cách lựa chọn chuỗi text phù hợp nhất với tình huống nhất định bằng cách sử dụng nền tảng tri thức và 
quy tắc hay phép tắc thông thường để hướng dẫn quá trình này. Đây có thể là vấn đề khi language models được sử dụng 
trong các ứng dụng mà đòi hỏi mức độ tin cậy và độ tin tưởng cao chẳng hạn như hệ thống hội thoại (dialog systems) hoặc 
trợ lý ảo cá nhân thông minh (intelligent personal assistants).

Mặc dù những model phức tạp này được huấn luyện trên lượng dữ liệu khổng lồ đã trở nên cực kỳ hiệu quả trong vài năm 
gần đây, nhưng khi được sử dụng trong các hệ thống sản phẩm thực tế mà giúp cuộc sống con người trở nên dễ dàng hơn thì 
chúng thường không đạt được khả năng này. Một số vấn đề *alignment* trong LLMs thường thấy rõ ở dạng:

- Thiếu hữu ích: tức là mô hình không tuân theo hướng dẫn cụ thể của người dùng.
- Phi thực tế: model tạo ra những sự thật không tồn tại hoặc sai sự thật.
- Thiếu khả năng diễn giải: con người khó hiểu cách mà model đưa ra một quyết định hoặc dự đoán cụ thể.
- Bias hoặc toxic trong output: một model dược huấn luyện với dữ liệu bị bias/ toxic có thể tái tạo điều đó trong đầu ra.

Nhưng vấn đề *alignment* này bắt nguồn từ đâu? Có phải ngay từ cách mà language models được huấn luyện vốn dễ bị 
*misalignment*?

### Tại sao các chiến lược huấn luyện language model có thể tạo ra vấn đề *misalignment*

`Next token prediction` và `masked language modeling` là một trong số các kỹ thuật cốt lõi được sử dụng để huấn luyện 
language models, chẳng hạn như `transformers`. 

Trong cách tiếp cận đầu tiên, model có đầu vào là chuỗi các từ (hoặc 
token) và dự đoán từ tiếp theo trong chuỗi. Ví dụ, chuỗi đầu vào của model là: "Con mèo đang nằm trên ". Model phải dự 
đoán từ tiếp theo là "giường", "ghế", "bàn", "sân" chẳng hạn. Bởi vì xác suất xuất hiện từ tiếp theo của các từ đó trong ngữ 
cảnh phía trước cao. Trên thực tế, language model có thể ước tính khả năng xuất hiện của mỗi từ (trong vốn từ vựng của 
nó) có thể xuất hiện tiếp theo dựa trên chuỗi từ trước đó. 

Cách tiếp cận theo `masked language modeling` là một biến thể của `next token prediction`, trong đó một số từ trong 
chuỗi đầu vào được thay thế bởi một token đặc biệt, ví dụ như [MASK]. Model có nhiệm vụ dự đoán chính xác từ sẽ được 
thêm vào vị trí được mask. Ví dụ, chuỗi đầu vào của model là: "Con [MASK] đang nằm trên ghế ." thì model phải dự đoán 
được từ được mask là "mèo", "chó", hoặc "thỏ" chẳng hạn.




