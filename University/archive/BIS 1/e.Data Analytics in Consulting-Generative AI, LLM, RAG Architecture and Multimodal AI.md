### Generative AI
Fascinating field with many application in data management and beyond. 
Different type of generative models:
- Variational Autoencoders(VAEs)
- Generative Adversarial Networks(GAN)
- Autoregressive Model
Generative AI can be used across different domain such as text generation, image generation, music generation, etc... 
We will see how different is the implementation of generative AI(from model deployment to prompt creation).
Needs to take care of the ethical and societal implications.
 Generative AI refers to a branch of artificial intelligence focused on developing systems capable of generating new data samples that resemble, or are indistinguishable from, samples in the training dataset. Unlike discriminative models, which are designed to classify input data into predefined categories or predict specific outputs, generative models aim to learn the underlying structure of the data and generate new instances that capture its distribution.
In essence, generative AI seeks to mimic the creativity and generative capabilities of humans by learning from examples and producing new content that exhibits similar characteristics. These models are trained on large datasets and can be used to create realistic images, text, audio, video, and other types of data.
#### Application of Generative AI
- IMAGE GENERATION AND EDITING: example is DALL-E, use GANs to designs realistics images
- TEXT GENERATION AND SUMMARIZATION: example is CHATGPT and is used toi generate text and summaries, translate some texts in other languages
- CONTENT CREATION AND AUGMENTATION: can generate new content or augment an existing one
- DATA SYNTHESIS AND SIMULATION: used to generate synthetic data for training models/to test activivties
#### Prompt Generation
Prompt engineering refers to the process of designing and crafting prompts or inputs that are used to interact with language models or AI systems in order to achieve desired outputs.
A prompt is what you are telling the AI to generate. The same prompt will give different results in base of the model interpelled.
To have the best response your prompt has to be Specific(Clearly state what you want the response to focus on), Provide Context(Give GPT some background information to work with, provide relevant details and/or examples), Ask Open-Ended Questions(Encourage detailed responses by asking open-ended question), Use Keywords(Incorporate key words or phrases related to your topic to guide GPT in generating relevant responses), Avoid Ambiguity(Make sure your prompts are clear and unambiguous to minimize misunderstandings. Use straightforward language and avoid complex or convoluted sentences), Engage GPT-X(Create prompts that encourage GPT to actively participate in the conversation), Provide Feedback(If you're not satisfied with the initial response, provide feedback or additional information to help GPT better understand your intent), Experiment and Iterate(Don't be afraid to experiment with different prompts and approaches to see what works best)  
#### ETHICS AND SOCIAL IMPLICATIONS
- Misinformation and Fake Content: Generative AI models can be used to create highly realistic fake images, videos, and text. This raises concerns about the spread of misinformation, fake news, and the potential for malicious actors to manipulate public opinion or deceive individuals.
- Privacy Concerns: Generative AI models trained on large datasets may inadvertently memorize and reproduce sensitive or private information, posing risks to individuals' privacy. This raises questions about data security, consent, and the need for robust privacy-preserving techniques.
- Bias and Fairness: Like other machine learning models, generative AI systems can perpetuate and amplify existing biases present in the training data. This can lead to unfair or discriminatory outcomes, reinforcing societal inequalities. Addressing bias and ensuring fairness in generative AI is crucial for promoting equity and justice.
- Intellectual Property and Copyright: The generation of synthetic content by AI models raises questions about intellectual property rights and copyright infringement. Determining ownership and attribution of generated content can be challenging, particularly when it closely resembles existing works.
- Security Risks: Generative AI models can also be exploited for malicious purposes, such as generating realistic phishing emails, deepfakes for impersonation, or creating counterfeit documents. These pose significant cybersecurity risks and may undermine trust in digital communication and verification systems.
- Identity Theft and Fraud: With the ability to generate realistic images and text, there's a potential for generative AI to be used in identity theft, fraud, or other criminal activities. This highlights the importance of robust authentication and verification mechanisms to combat such threats.
- Regulatory and Legal Challenges: The rapid advancement of generative AI technology has outpaced regulatory frameworks and legal norms, leading to uncertainties around accountability, liability, and governance. Policymakers face challenges in adapting existing laws and regulations to address the unique characteristics of generative AI.
- Impact on Creative Industries: Generative AI has the potential to disrupt traditional creative industries by automating content generation tasks. While this can lead to increased efficiency and innovation, it may also result in job displacement and economic disruptions for professionals in these sectors.
### LLM
A Large Language Model (LLM) is a type of artificial intelligence model designed to understand and generate human-like text based on the input it receives. These models are trained on vast amounts of text data, typically sourced from the internet or other large corpora, to learn the intricate patterns and structures of human language. 
Large language models are built using deep learning architectures, often based on transformer architectures like OpenAI's GPT (Generative Pre-trained Transformer) series. These models consist of multiple layers of neural networks that process input text in a hierarchical manner, capturing both local and global dependencies within the text. 
Once trained, a large language model can perform a variety of natural language processing
tasks, including text generation, text completion, translation, summarization, question answering, and more. The model's ability to generate coherent and contextually relevant text has led to its widespread use in various applications, including virtual assistants, content generation, chatbots, language translation, and sentiment analysis.
### RAG Architecture
RAG stands for "Retrieval-Augmented Generation." It's a technique used in natural language processing (NLP) that combines retrieval-based methods with generative models to enhance the quality and relevance of generated text.
In RAG, a retriever component is used to search a large database or corpus of text documents to find relevant information related to the input query or context. This retrieved information is then used to augment the generation process of a generative model, such as a language model like GPT (Generative Pre-trained Transformer). By incorporating relevant information retrieved from the corpus, the generative model can produce more accurate, coherent, and contextually relevant responses. With RAG, company data can be used to augment your prompts. Data come from multiple data sources, such as a document repositories, databases, or APIs.

![](https://i.imgur.com/lS5qORS.png)

### Multimodal AI
Multimodal generative AI refers to artificial intelligence systems that can generate content across multiple modalities, such as text, images, and audio, simultaneously. These systems leverage advanced machine learning techniques to understand and generate content based on inputs from different types of data.
- Integration of Multiple Modalities: Unlike traditional generative AI models that focus on one modality (e.g., text generation or image generation), multimodal generative AI models can process and generate content across multiple modalities simultaneously. This enables more comprehensive and expressive generation capabilities.
- Cross-Modal Understanding: Multimodal generative AI models are capable of understanding relationships between different modalities of data. For example, a model may generate a textual description based on an input image or produce an image based on a textual prompt. This cross-modal understanding allows for richer and more contextually relevant content generation
- Training with Multimodal Data: To train multimodal generative AI models, large datasets containing diverse examples of multiple modalities are required. These datasets often include paired examples of text, images, and/or audio, allowing the model to learn correlations and associations between different types of data

