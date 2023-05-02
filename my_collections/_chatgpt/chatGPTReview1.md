---
title: "Tại sao các bản "sao chép" của GPT-3 fail? Chúng ta nên sử dụng GPT-3.5/ chatGPT trong những bài toán nào?"
date: 2023-05-02
cover: /chatgpt.png
labs: ["Hanoi, Vietnam"]
---


Tại sao các bản sao được public trên công đồng của GPT-3 fail? Thật sự thì những nhiệm vụ nào thì nên sử dụng GPT-3.5/ chatGPT?

Source: https://phanxuanphucnd.github.io/

Trong bài viết này, tôi chia sẻ dựa trên quan điểm của riêng cá nhân tôi. Tôi sẽ viết một số tóm tắt và suy nghĩ của mình về hai câu hỏi trên, sau khi tôi đã nghiên cứu chi tiết trong một loạt các bài báo về GPT-3, PaLM, BLOOM, OPT, FLAN-T5/PaLM, HELM và nhiều hơn nữa. 

Câu hỏi đầu tiên: "tại sao các bản sao được cộng đồng reproduce lại GPT-3 fail?" là một câu hỏi quan trọng, đặc biệt đối với những người muốn "sao chép" lại GPT-3/ chatGPT của riêng họ. Câu hỏi thứ hai quan trọng đối với những người muốn sử dụng GPT-3.5/ InstrucT GPT/ chatGPT.


## Tại sao các bản "sao chép" của GPT-3/ GPT-3.5/ Instruct GPT/ chatGPT (gọi ngắn gọn là GPT-3) fail?

Ở đây, tôi xác định "fail" là không đạt được hiệu suất được báo cáo trong bài báo gốc về GPT-3, với kích thước mô hình tương tự hoặc thậm chí lớn hơn. Theo tiêu chí này, GPT-3 và PaLM (540B) là thành công, nhưng cả hai mô hình này đều không được công khai, trong khi tất cả các mô hình công khai khác (ví dụ như OPT-175B và BLOOM-176B) đều "thất bại" đến một mức độ nào đó. Nhưng vẫn có thể học được nhiều bài học từ những "thất bại" này. Lưu ý rằng nếu chúng ta đã thử nhiều cài đặt khác nhau nhiều lần, cộng đồng công khai có thể cuối cùng sẽ sao chép lại được GPT-3. Nhưng cho đến bây giờ, chi phí vẫn quá cao để huấn luyện thậm chí một phiên bản khác của OPT-175B. Bởi vì huấn luyện một lượt của một mô hình lớn như vậy yêu cầu phải chạy ít nhất trong 2 tháng trên ~1000 GPU A100 80G.


Mặc dù một số bài báo như OPT-175B và GLM-130B cho rằng chúng đạt được hiệu suất tương đương hoặc vượt trội hơn so với GPT-3 gốc trong một số nhiệm vụ, nhưng vẫn còn đang đặt nghi vấn trong một loạt các nhiệm vụ mà GPT-3 đã được đánh giá. Hơn nữa, kết quả API GPT3 mới nhất của OpenAI vẫn tốt hơn rất nhiều so với các mô hình mã nguồn mở như vậy, theo trải nghiệm của hầu hết người dùng trong một loạt các nhiệm vụ và đánh giá HELM. Mặc dù mô hình cơ sở có thể sử dụng instruction tuning (InstructGPT), các phiên bản tương tự được instruction-tuned của OPT (OPT-IML) và BLOOM (BLOOMZ) vẫn kém hơn rất nhiều so với InstructGPT và FLAN-PaLM (Instruction-tuned PaLM).

Theo chi tiết trong các bài báo, có nhiều lý do có thể giải thích vì sao OPT-175B và BLOOM-176B thất bại so với GPT-3 và PaLM thành công. Tôi phân loại chúng thành hai phần: `dữ liệu huấn luyện (pretraining data)` và `chiến lược huấn luyện (training strategy)`.


### Pretraining data (dữ liệu huấn luyện)

Đầu tiên, hãy xem cách mà GPT-3 chuẩn bị và sử dụng dữ liệu huấn luyện. GPT-3 được huấn luyện trên 300B tokens, trong đó 60% là từ Common Crawl đã được lọc, các thành phần khác bao gồm webtext2 (corpus để huấn luyện GPT2), Books1, Books2 và Wikipidia. Các phiên bản sau của GPT-3 cũng được huấn luyện trên dữ liệu mã nguồn (ví dụ như Github Code). Tỷ lệ mỗi phần không tỷ lệ thuận với kích thước tập dữ liệu gốc. Thay vào đó, các dữ liệu có chất lượng cao được lấy mẫu nhiều hơn. Có ba điều bí ẩn có thể làm cho việc thu thập dữ liệu tương tự khó khăn đối với cộng đồng open-source, dẫn đến các "thất bại" của OPT-175B và BLOOM-176B:

1. Điều đầu tiên là một bộ phân loại tốt để **lọc bỏ các dữ liệu chất lượng thấp**, được sử dụng để xây dựng tập dữ liệu tiền huấn luyện của cả GPT-3 và PaLM. Nhưng bước này không được thực hiện bởi OPT hoặc BLOOM. Một số nghiên cứu đã chỉ ra rằng việc tiền huấn luyện một mô hình với ít dữ liệu chất lượng cao có thể vượt trội hơn so với những mô hình có nhiều dữ liệu chất lượng khác nhau hơn. Tuy nhiên, đa dạng dữ liệu cũng rất quan trọng như đã nêu ở điểm thứ ba. Do đó, một người nên cẩn thận với sự cân bằng giữa đa dạng và chất lượng dữ liệu.

2. Thứ hai là việc **loại bỏ dữ liệu trùng lặp trong quá trình tiền huấn luyện**. Việc loại bỏ trùng lặp có thể ngăn mô hình tiền huấn luyện xem cùng một điểm dữ liệu nhiều lần và quá khớp/nhớ chúng, dẫn đến khả năng tổng quát hóa tốt hơn. GPT-3 và PaLM thực hiện loại bỏ trùng lặp ở mức document (document-level), cũng được sử dụng bởi OPT. Nhưng vẫn có rất nhiều sự trùng lặp trong tập dữ liệu Pile được sử dụng bởi OPT sau bước này, như được chỉ ra trong bài báo của OPT, điều này có thể dẫn đến hiệu suất thấp hơn. (Thú vị là một số bài báo gần đây cho thấy việc loại bỏ dữ liệu trùng lặp trong quá trình tiền huấn luyện không quan trọng như chúng ta từng nghĩ để tránh quá khớp.)

3. Thứ ba là **đa dạng về dữ liệu tiền huấn luyện**. Điều này bao gồm đa dạng về domain, đa dạng về hình thức/ form(ví dụ: text, code, table) và đa dạng về ngôn ngữ. Bộ dữ liệu Pile corpus được sử dụng bởi OPT-175B tuyên bố có tính đa dạng tốt hơn, nhưng bộ dữ liệu ROOTS được sử dụng bởi BLOOM có quá nhiều tập dữ liệu kiểu academic, thiếu sự đa dạng của dữ liệu Common Crawl và có thể dẫn đến hiệu suất kém hơn của BLOOM. So với đó, GPT-3 có tỷ lệ dữ liệu đa dạng và đa lĩnh vực (general-domain) từ Common Crawl nhiều hơn, đó cũng là một trong những lý do tại sao GPT-3 là một trong những mô hình cơ sở đầu tiên dẫn đến Chatbot tổng quát được công nhận tốt - ChatGPT.

Note: Mặc dù dữ liệu đa dạng thường quan trọng đối với việc huấn luyện LLM có khả năng tổng quát, tuy nhiên phân phối dữ liệu huấn luyện cụ thể thực sự ảnh hưởng rất nhiều đến hiệu suất của LLM trên các downstream task. Ví dụ, BLOOM và PaLM có tỷ lệ dữ liệu đa ngôn ngữ cao hơn, dẫn đến hiệu suất tốt hơn của chúng trong một số tác vụ đa ngôn ngữ (Multi-lingual tasks) và dịch máy (Machine Translation). OPT có nhiều dữ liệu huấn luyện về conversation (ví dụ như Reddit), điều này có thể dẫn đến hiệu suất tốt của nó trong các tác vụ dialogue. PaLM có một tỷ lệ rất lớn các cuộc trò chuyện trên các mạng xã hội, điều này có thể dẫn đến hiệu suất vượt trội của nó trên các tác vụ và tập dữ liệu trả lời câu hỏi đa dạng (Question Answering). Ngoài ra, PaLM và các phiên bản sau của GPT-3 có một tỷ lệ lớn dữ liệu code, điều này tăng khả năng của chúng trong các tác vụ lập trình và có thể khả năng nội suy (CoT). Một hiện tượng thú vị là BLOOM vẫn cho thấy khả năng lập trình và CoT kém mặc dù dữ liệu code được sử dụng trong quá trình huấn luyện của nó, điều này có thể cho thấy rằng dữ liệu code đơn lẻ không đảm bảo khả năng lập trình và CoT của một mô hình.


Tóm lại, một số bài báo đã chỉ ra sự quan trọng của ba điểm sau: 
- giảm trùng để tránh ghi nhớ/ overfiting, 
- lọc dữ liệu để có dữ liệu chất lượng cao hơn,
- và đa dạng hóa dữ liệu để đảm bảo khả năng tổng quát của LLM. 

Tuy nhiên, chi tiết về cách PaLM và GPT-3 tiền xử lý dữ liệu hoặc dữ liệu tiền huấn luyện gốc chưa được tiết lộ, điều này khiến cộng đồng công chúng khó tái tạo lại chúng.


### Training strategy (chiến lược huấn luyện)

Ở đây, chiến lược huấn luyện bao gồm các framework để huấn luyện (training framework), độ dài thời gian huấn luyện (training duration), kiến ​​trúc mô hình/ thiết lập huấn luyện (model architecture/ training setups) và các kỹ thuật trong quá trình huấn luyện, với mục đích đảm bảo tính ổn định và hội tụ tốt hơn khi huấn luyện các mô hình rất lớn. Nói chung, vấn đề bùng nổ loss, divergences trong quá trình tiền huấn đã được thấy rộng rãi với các nguyên nhân không rõ ràng, do đó nhiều kỹ thuật cho quá trình huấn luyện và kiến ​​trúc mô hình đã được đề xuất để giảm thiểu vấn đề này. Nhưng một số trong số chúng ở OPT và BLOOM không phải là các giải pháp tối ưu, điều này có thể dẫn đến hiệu suất thấp hơn của chúng. GPT-3 không đề cập rõ ràng cách giải quyết vấn đề này.

1. Training framework: Một mô hình lớn hơn 175B thường yêu cầu ZeRO-fashion data parallel (distributed optimizer) và model parallel (bao gồm tensor parallel, pipeline parallel và sequence parallel). OPT sử dụng FSDP để triển khai kỹ thuật ZeRO và Megatron-LM cho kỹ thuật model parallel. BLOOM sử dụng Deepspeed cho kỹ thuật ZeRO và Megatron-LM cho kỹ thuật model parallel. PaLM sử dụng Pathways, một hệ thống data parallel và model parallel dựa trên TPU. Chi tiết về hệ thống huấn luyện GPT-3 vẫn chưa được biết, nhưng ít nhất họ đã sử dụng kỹ thuật model parallel theo một cách nào đó (một số người nói rằng nó sử dụng Ray). Các hệ thống huấn luyện và phần cứng khác nhau có thể dẫn đến một số hiện tượng khác nhau trong quá trình huấn luyện. Rõ ràng, một số thiết lập huấn luyện TPU được đưa ra trong bài báo của PaLM có thể không phù hợp cho việc huấn luyện GPU trong các mô hình khác. Một hệ quả quan trọng của hardwares và training framework là người ta có thể sử dụng bfloat16 để lưu trữ weights của mô hình, activations và các thành phần khác. Điều này đã được chứng minh là một yếu tố rất quan trọng để đảm bảo ổn định khi training, vì bfloat16 có thể biểu diễn phạm vi số dấu chấm động lớn hơn, giải quyết vấn đề *loss spikes* trong quá trình training. Trong TPU, bfloat16 là tùy chọn mặc định, điều này có thể là một bí mật vì sao PaLM thành công. Nhưng trên GPU, người ta trước đây chủ yếu sử dụng float16. OPT sử dụng float16, điều này có thể là một yếu tố gây ra sự không ổn định. BLOOM xác định vấn đề này và cuối cùng sử dụng bfloat16 trên GPU A100, nhưng nó không mang lại kết quả của setting này, do đó sử dụng additional layer normalization sau embedding đầu tiền, được sử dụng để giải quyết sự không ổn định trong các thử nghiệm sơ bộ bằng float16. Tuy nhiên, layer normalization này đã được chứng minh là tệ hơn cho việc zero-shot, điều này có thể là một yếu tố gây ra thất bại của BLOOM.

2. Modifications during training procedure: Những điều chỉnh trong quá trình huấn luyện thì OPT đã thực hiện rất nhiều điều chỉnh trong khi đang huấn luyện và restart training từ một checkpoint gần nhất, bao gồm thay đổi clip gradient norm và learning rate, chuyển sang sử dụng vanilla SGD và sau đó quay lại Adam, resetting dynamic loss scalar, chuyển sang phiên bản mới hơn của Megatron. Những điều chỉnh trong quá trình huấn luyện như thế này có lẽ là một trong những lý do OPT thất bại. So với đó, PaLM gần như không thực hiện bất kỳ điều chỉnh nào trong quá trình huấn luyện. Chỉ đơn giản là re-started training từ checpoint xấp xỉ 100 steps trước khi gặp vấn đề spike, và skip khoảng 200-500 batches data. Việc restarting đơn giản như vậy có thể thành công kỳ diệu trong PaLM, bởi vì tính xác định theo bit của nó trong đó việc lấy mẫu được hoàn thành trong quá trình xây dựng pretraining data và do nhiều modify trong kiến trúc models và training setup để đạt được tính ổn định tốt hơn. Những modify đó trong PaLM được nói ở điểm tiếp theo.

3. Model Architecture/Training Setup: Để làm cho quá trình training ổn định hơn, PaLM đã thực hiện một số điều chỉnh đối với model architecture và training setup, bao gồm sử dụng version được sửa đổi của Adafactor làm optimizer, scaling pre-softmax output logits, sử dụng auxiliary loss để khuyến khích softmax normalizer gần với 0, sử dụng khởi tạo khác nhau cho kernel weights và embeddings, không sử dụng bias trong dense kernel và lay norms, và không sử dụng dropout trong quá trình pretraining. Lưu ý rằng còn có nhiều bài học quý giá khác trong GLM-130B về cách huấn luyện cho LLMs để stable, như sử dụng DeepNorm dựa trên Post-LN thay vì Pre-LN, và Embedding Layer Gradient Shrink. Hầu hết các tinh chỉnh mô hình không được áp dụng trong OPT và BLOOM, điều này có thể gây ra sự không ổn định và thất bại của các mô hình đó.

4. Training duration: Như được thể hiện trong bảng dưới đây, số lượng token được sử dụng trong quá trình tiền huấn luyện của GPT-3 tương tự như OPT và BLOOM, trong khi PaLM đã sử dụng nhiều token hơn trong quá trình pretraining. Ngoài ra, pretraining corpus của PaLM và GPT-3 đều lớn hơn tập dữ liệu pretraining corpus của BLOOM và OPT. Do đó, pretraining trên nhiều token với tập corpus chất lượng tốt hơn có thể là một yếu tố quan trọng của thành công của GPT-3 và PaLM.

| | Pretraining Corpus Tokens | Tokens Seen During Pretraining |
| --- | --- | --- |
| PaLM | 780B | 770B |
| GPT-3 | 500B | 300B |
| OPT | 180B | 300B |
| BLOOM | 341B | 366B |

Có một vài yếu tố khác, có thể không quan trọng cho tính ổn định trong quá trình huấn luyện, nhưng vẫn có thể ảnh hưởng đến hiệu suất mô hình.

- Thứ nhất, cả PaLM và GPT-3 đều sử dụng kích thước batch được tăng dần (từ nhỏ đến lớn) trong quá trình huấn luyện, đã được chứng minh là hiệu quả để huấn luyện LLM tốt hơn, trong khi OPT và BLOOM sử dụng kích thước batch không đổi. 

- Thứ hai, OPT sử dụng activation ReLU, trong khi PaLM sử dụng activation SwiGLU, và GPT-3/BLOOM sử dụng activation GeLU, điều này thường dẫn đến hiệu suất tốt hơn khi huấn luyện LLMs. 

- Cuối cùng, để mô hình hóa các chuỗi dài (long-sequence) tốt hơn, PaLM sử dụng RoPE embeddings và BLOOM sử dụng ALiBi embeddings, trong khi GPT-3 và OPT sử dụng positional embeddings được học trong quá trình huấn luyện, điều này có thể ảnh hưởng đến hiệu suất với long sequence.



Về câu hỏi thứ 2 được đề cập ngay từ ban đầu, tôi sẽ tiếp tục cập nhật sớm nhất.

